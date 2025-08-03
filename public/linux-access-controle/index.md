# Linuxのアクセス制御を完全に理解する


<!--
Todo:
- ユースケース
    - nginxをcapabilityで動かす
- Docker
    - 結局Dockerが最適じゃね？
- 参考
-->


こんにちは、しぶやです。\
今回はLinuxのアクセス制御を「完全に理解」していきたいと思います。

## TL;DR

* Linuxの権限管理の基本を復習
* capabilityを利用することで最小権限の原則に従える
* SELinuxを利用すれば、権限分掌が可能


## 0　はじめに

Linuxに限った話ではないですが、ファイルについて理解することは非常に重要だと思います。
というのも、以下のような有名な考え方があります。

> Linuxでは全てをファイルとして扱うようになっています。by [LPI-JAPAN](https://lpi.or.jp/lpic_all/linux/intro/intro10.shtml)

僕がLinuxを好きである一番の理由です。
Linuxがかっこ良いからというわけではなく、すべてをファイルとして扱ってくれるLinuxは理解しやすいのです。
Windows等は複雑すぎて僕にはまだ理解できない部分がほとんどです。(いずれ勉強したい。。)

話を戻して、本記事ではLinuxにおけるアクセス制御、すなわちファイルのアクセス制御について「完全に理解」していきたいと思います。
「少し分かる」レベルの人は軽く読み飛ばしてください。


※ 本記事では以下の環境で動作検証をしています。
* OS：Ubuntu 24.04.1 LTS
* ルートファイルシステム：ext4
* ホスト：AWS EC2

## 1　Linuxにおけるアクセス制御の基本

まずは、基本の復習からです。

### 1.1　基本のパーミッション

Linuxにおけるファイルパーミッションは以下のような形をしています。

* drwxrwxrwx

これは、以下の4つに分けられます。

* d + rwx + rwx + rwx (= drwxrwxrwx)
    * d---------\
    ファイルの種類を表す。
        * \- ... ファイル
        * l ... リンク
        * d ... ディレクトリ
        * etc.
    * -rwx------\
    所有ユーザの権限を表す。
        * \- ... 権限なし
        * r ... 読み込み権限
        * w ... 書き込み権限
        * x ... 実行権限
    * ----rwx---\
    所有グループの権限を表す。
    読み方は所有ユーザと同様。
    * -------rwx\
    その他のユーザの権限を表す。
    読み方は所有ユーザと同様。

それでは、実際のファイルを見てみましょう。

{{< image src="ping.png" width="800px" height="600px" caption="pingの権限" >}}

上記から、echoは管理者のみが書き換えも可能、他は読み込みと実行のみが可能です。
書き換えを行うとどうなるでしょうか？

{{< image src="ping_rewrite.png" width="800px" height="600px" caption="書き込みの権限エラーその1" >}}

権限がないと怒られました。

詳細に見てみましょう。システムコールを追ってみます。

{{< image src="ping_rewrite_syscall.png" width="800px" height="600px" caption="書き込みの権限エラーその2" >}}

"openat(AT_FDCWD, "/usr/bin/ping", O_WRONLY|O_CREAT|O_TRUNC, 0666) = -1 EACCES (Permission denied)"
から、書き込み権限でファイルを開こうとしたときに権限エラーが出ていることが分かります。

ファイル実行の場合はどうでしょうか。
Linuxにおいて、基本的にはファイルの実行に読み込み権限が必要です。では、「実行権限は読み込み権限を包含しているのでしょうか？」
「読み込み権限なしに、実行権限だけを持つようなパターンはあるのでしょうか？」
これについては、次節で見ていきたいと思います。

ここでは、アクセス権限意外の部分を復習していきましょう。

{{< image src="ping.png" width="800px" height="600px" caption="（再掲）pingの権限" >}}

ファイルの主要な属性には以下のようなものがあります。

* リンク数
* 所有者、所有グループ
* 最終更新日時
* サイズ
* ファイル名

アクセス権限の横にある数字はリンク数です。
Windowsでいうショートカットに近いものです。

実際にリンクファイルを作成してみましょう。

{{< image src="pinglink.png" width="800px" height="600px" caption="リンクファイル" >}}

ここでは、実際にpinglinkというリンクファイルを作成しています。
リンクファイルを作成したことにより、"/usr/bin/ping"のリンク数が2になっていることが分かります。
また、pinglink経由でpingが実行できていることが分かります。\
※ リンクファイルの作成に"sudo"が必要な理由は本記事を最後まで読むと分かるはずです。

{{< admonition note "シンボリックリンクとハードリンク" >}}
Linuxのリンクにはシンボリックリンクとハードリンクがあります。
Windowsのショートカットに近いものはシンボリックリンクの方です。
* シンボリックリンク
    * ファイルやディレクトリへのパス情報
    * 参照先が消えるとリンクが使えなくなる。
* ハードリンク
    * 元ファイルと同じinodeを指す\
    → もとのファイルが消えても、ハードリンクからはデータにアクセス可能\
    → リンク先の変更は元ファイルにも影響\
    ※ inodeについては、2.2 VFSで詳しく話します。
{{< /admonition >}}


{{< admonition note "chmod, chown" >}}
以下の2つはよく使うコマンドですね。
* chmod
    * ファイルのアクセス権限を変更する。
    * 例：chmod ./hoge.txt 644\
    ※ "hoge.txt"を所有者のみ読み込み書き込みok、所有グループ、その他のユーザは読み込みのみokに変更
* chown
    * ファイルの所有者を変更する。
    * 例：chown hoge:ghoge ./hoge.txt\
    ※ "hoge.txt"の所有者を"hoge"に、所有グループを"ghoge"に変更
{{< /admonition >}}

{{< admonition tip >}}
chmodで存在しないユーザを指定したらどうなのでしょう。

{{< image src="chmod_error1.png" width="800px" height="600px" caption="⇓" >}}
{{< image src="chmod_error2.png" width="800px" height="600px" caption="chmodのエラー" >}}

"invalid user: hoge"と怒られました。
どうやら"systemd"がpasswdとかを利用して管理しているuserdbでユーザのチェックを行っているようです。

{{< /admonition >}}

### 1.2　その他パーミッション

その他のパーミッションについて見ていきます。

* setuid, setgid

setuid, setgidは実行時に所有者の権限で実行するためのものです。
passwdを例に見ていきましょう。

{{< image src="passwd.png" width="800px" height="600px" caption="passwdの権限" >}}

setuidがセットされていると、ファイルの所有者の実行権限が"s"になります。
これにより、ファイル所有者の権限でファイルを実行でき、ファイル所有者がアクセスできる全てのファイルにアクセスできるようになります。

{{< admonition Note >}}
setuidがセットされているファイルを実行した場合、所有者の権限を得られることが分かりました。
しかし、自分自身が持つ権限はどうなるのでしょうか？
言い換えると、これまでアクセスできていた自分のファイルにはアクセスできる状態を保ってくれるのでしょうか？

実際に見てみましょう。
{{< image src="setuid.png" width="800px" height="600px" caption="setuid" >}}

ここでは、ユーザ"hoge"がもつ"owned_by_hoge.txt"とユーザ"ubuntu"がもつ"owned_by_ubuntu.txt"をそれぞれ600のパーミッションで用意しています。
また、"cat"のバイナリを用意し、setuidを付与しています。

"cat"経由でそれぞれのファイルを開いた際、"ubuntu"が所有する"owned_by_ubuntu.txt"は開けなくなっていることが分かります。
つまり、setuidで他のユーザ権限で実行した際は、完全に権限が移行され、自分自身の権限はなくなるということです。

※ この仕組みはシンプルで分かりやすいですね。
{{< /admonition >}}


{{< admonition tip RUIDとEUID>}}

LinuxのプロセスはプロセスIDの他にRUIDとEUIDというものをもっています。
* RUID ... 実ユーザID
* EUID ... 実効ユーザID

setuidがセットされたファイルを実行した場合は、RUIDは実行ユーザのユーザID、EUIDは借りた権限の持ち主のユーザIDです。
EUIDがプロセスの実行ユーザとなり、その権限でプロセスが動きます。

{{< image src="eudid.png" width="800px" height="600px" caption="RUIDとEUID" >}}

{{< /admonition >}}

setuidは便利な反面、脆弱性があると権限昇格に利用される恐れがあり非常に危険な存在です。
それでは、なぜsetuidを利用するのでしょうか？実行権限だけ付与すればよいのではないでしょうか？

この答えは、**実行プロセスに管理者権限が必要だから**です。以下がその例です。
* passwd\
passwdは実行後、"/etc/shadow"や"/etc/passwd"の書き込みが発生します。
しかし、通常ユーザではこれらのファイルへの書き込み権限がありません。
そのため、setuidを行った"passwd"コマンドにより、passwdコマンド経由でroot権限を利用してそれらのファイルの書き換えができるようになります。

{{< admonition note "uid・gid" >}}
uid, gidはユーザやグループのための一意の識別子です。
ユーザの情報は"/etc/passwd"に、グループの情報は"/etc/group"にあります。

{{< image src="etc_passwd.png" width="800px" height="600px" caption="/etc/passwd" >}}
{{< image src="etc_group.png" width="800px" height="600px" caption="/etc/group" >}}

読み方としては、以下です。
* /etc/passwd ... ユーザー名:パスワード(※):UID:GID:コメント:ホームディレクトリ:ログインシェル
* /etc/group ... グループ名:パスワード(※):GID:グループメンバー
ユーザIDは1000以上が割り当てられます。rootは0です。
{{< /admonition >}}

{{< admonition note "shadow" >}}
先程の"/etc/passwd"には実際のパスワードは入っていませんでした。
これは、"/etc/passwd"はすべてのユーザから読み込まれてしまうため、パスワードが他のユーザに漏洩してしまうからです。
実際のパスワードは"/etc/passwd"に保存されています。

{{< image src="etc_shadow.png" width="800px" height="600px" caption="/etc/shadow" >}}

読み方は以下です。
* ユーザー名:暗号化パスワード:最終変更日:最小日数:最大日数:警告日数:無効日数:有効期限

{{< /admonition >}}


* スティッキービット

スティッキービットはディレクトリに適用するものです。
スティッキービットが適用されたディレクトリはすべてのユーザーがファイルの作成・書き込み可能です。
スティッキービットの特徴は**ファイルの削除は所有者のみが可能**ということです。
/tmpなどはステッキービットが適用されている例です。
実際にスティッキービットを試してみましょう。

{{< image src="tmp.png" width="800px" height="600px" caption="/tmpのスティッキービット" >}}

スティッキービットに作成されたファイルの権限は"---t"になります。

{{< image src="tmp_error.png" width="800px" height="600px" caption="スティッキービットの効果" >}}

また、ファイル所有者以外のユーザはファイルの読み込み、書き込みは可能ですが、削除をしようとすると怒られます。

{{< admonition tip >}}
そもそも、ファイルの削除権限とは何なのでしょうか？
書き込み権限とは異なるのでしょうか？

ファイルの削除に必要な権限は**ファイルが置かれたディレクトリの書き込み権限と実行権限があるか**です。

{{< /admonition >}}


{{< admonition tip >}}
スティッキービットはファイルの**削除は不可能ですが、書き込みは可能**です。
ここで、他のユーザにファイル内容をすべて削除されてしまうと、ファイルを削除されたような状態になります。
スティッキービットは一体何のためにあるのでしょうか？

結論、スティッキービットは誤って他人のファイルを削除することを防止するためのものです。
ファイルの中身を書き換えられてしまうことに変わりはないので、それを防ぐには通常のアクセス権管理が必要です。
{{< /admonition >}}

### 1.4　sudo

1.3でsetuid, setgidを学んだところで、sudoも復習しておきましょう。

sudoは管理者権限でコマンドを実行するために利用されます。
sudoの管理を行っているのは"/etc/sudoers"です。実際に見てみましょう。

{{< image src="sudoers.png" width="800px" height="600px" caption="sudoers" >}}

読み方は以下です。
* alice ALL=(ALL:ALL) ALL
    * alice ... 権限を与えるユーザー
    * ALL ... すべてのホストでコマンドを実行可能
    * (ALL:ALL)
        * 1つ目の ALL ... sudo 実行時に任意のユーザーに切り替え可能
        * 2つ目の ALL ... sudo 実行時に任意のグループに切り替え可能
    * ALL ... すべてのコマンド実行可能

それでは、sudoを試してみましょう。

{{< image src="sudo.png" width="800px" height="600px" caption="sudo" >}}

所有者???


{{< admonition tip >}}
"/etc/sudoers"を編集する際は、"visudo"コマンドが推奨されています。
これには以下のような機能があるからのようです。
* 保存時に構文エラーのチェックを行う
* 同時編集を禁止する
* （"/etc/sudoers"自体の）パーミッションの自動保護
* "/etc/sudoers"の破壊防止（rootに戻れなくなることを防止）
{{< /admonition >}}

### 1.5　コピー時の動作

1.1~1.3ででファイルのアクセス権限について復習しました。
では、ファイルをコピーした際はどのような挙動をするのでしょうか。

デフォルトのcpコマンドの動作は以下です。
* コピー元の読み取り権限が必要
* コピー元のアクセス権の設定をコピー
* 所有者、所有グループはコピーを行った人のものに変更

"-p"オプションをつけると、アクセス権に加え、所有者やタイムスタンプなども完全にコピーされます。

{{< image src="cp_ping.png" width="400px" height="300px" caption="copy" >}}

リンクファイル作成時はどうでしょうか？

* シンボリックリンク\
シンボリックリンク自体は独立したファイルとして扱われます。※ シンボリックリンクはただのパスのようなものであるため。
    * 所有者はリンク作成者
    * パーミッションは"lrwxrwxrwx"\
    リンク先のファイルを操作する際にそちらに左右されるため、リンクファイル自体にパーミッションをもたせる必要がない。
* ハードリンク
    * 所有者、パーミッション等はコピー元ファイルと同じ

{{< image src="link-permission.png" width="400px" height="300px" caption="link" >}}


## 2　Linuxにおけるアクセス制御の深堀

それではディープダイブしていきましょう。
そもそもLinuxカーネルくんは、どこでファイルのパミッションを管理し、ファイルアクセスをしているのでしょうか？

### 2.1　ファイルシステム

まずはファイルシステムです。[wiki](https://ja.wikipedia.org/wiki/%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0)の定義を見てみましょう。
>ファイルシステム（英: file system、filesystem）は、コンピュータのリソースを操作するための、オペレーティングシステム (OS) が持つ機能の一つ

定義の通り、ファイルシステムはOSの機能の一つとして、インストール時に選択できたら、複数のファイルシステムが組み込まれていたりします。

{{< image src="fstree.png" width="400px" height="300px" caption="Linuxカーネルに組み込まれたファイルシステム" >}}


LPI Japanではファイルシステについて、以下のように解説あります。
>そもそもデータを扱うということはどういうことなのでしょうか。
>通常OSを操作する上において、データを操作するということはファイルを操作することとほぼ同等の意味を持つかと思います。
>このため通常はデータが物理的にどのように格納されているかを意識する必要もなく、逆に意識できない作りになっています。
>これはOSの機能であるデータの抽象化によってもたらされる結果です。
>実際に物理的な記憶装置に格納されているデータを素で扱おうとすると、 HDDのどこのセクタのどの部分とどの部分を取り出して、データをメモリ上に読み込み、メモリに格納できないものは後からまた読む・・・といった大変面倒なことをする必要があります。
>現在のHDDの容量を考えてみると、何百ギガバイト（テラになりつつありますが・・・）という巨大なデータを格納できるような物体です。
>その中に入っている何かのデータを自力で探しだし、利用することは人間が行う作業として成立しないものだともいえます。
>このような物体に存在するデータを人間にわかりやすいファイルというものに抽象化、可視化し、データを永続的に管理しやすくするという役目を持つのがファイルシステムの基本的な考え方です。

一言でいうとファイルの管理をしてくれているようです。
詳細には、ファイルをどのように格納するかや、どのようにアクセスするかなど難しい話が色々あると思います。

具体的なファイルシステムを見ていきましょう。
* Linux
    * ext4\
    Linuxで最も利用されています。
    * ext3\
    ext4の前のやつです。
    * btreefs\
    ファイルシステム自体にスナップショットやraidの機能があります。安定性については議論があるらしい。
    * xfs\
    大容量のファイルシステムむけに最適化されているらしい。

* Windows
    * NTFS\
    現在、Windowsでデフォルトで利用されているファイルシステムです。セキュリティであったり、ロギングであったり非常に高機能なイメージです。
    * FAT32\
    USBとかで使われていらやつです。ファイルサイズが4GBまでの制約があります。\
    * exFat\
    FAT32を改良したもので、大きなファイルもサポートしています。

* MacOS
    * APFS\
    現在のmacosで利用されているもの。
    * HFS\
    APFSの前に利用されていたもの。

ファイルシステムごとにデータの格納方法だったり、管理方法が異なります。
ファイルシステムについては詳しくないですが、どこかで詳しく調べて記事にできたらと思います

### 2.2　VFS

2.1でファイルシステムにはいくつか選択肢があることがわかりました。なんなら人によっては自作している人もいます。

ここで、ファイルシステムを変えるたびにOSに大きな変更があっては困らないでしょうか？できるだけ疎結合を意識したくなります。ここで登場するのがVFSです。

VFSとは、Virtual File Systemの略です。

LPIJapanの説明を見てみましょう。
>VFSは下位の物理的な媒体を抽象化し、データ、デバイスを含む全てのコンピュータリソースに対して統一的なファイルアクセスという入出力インターフェイスを提供します。
>これによりリソースの差異を気にすることなく、様々な対象に対して統一のアクセスを行うことができます。
>さらにVFSにはファイルシステムの抽象化という役割もあり、これによってことなる複数のファイルシステムに対して透過的にアクセスすることが可能になっており、ファイルシステムの種類を意識せずに利用することができます

すなわち、カーネルとファイルシステムの間に入り、カーネルからのアクセスを抽象化し、ファイルシステムが異なってもカーネルに統一的なアクセスを提供します。
ファイルシステム以外にも、様々な入出力を抽象化し、統一的なアクセスを可能としてくれてます。

実際にVFSではどのように管理されているのでしょうか。

VFSにおいて、重要な役割を果たしているのが"struct inode"です。
"sturuct inode"にはファイルの所有者やタイムスタンプなどオブジェクトの基本情報が格納されます。
実際に"struct inode"を見てみましょう。

```
/*
 * Keep mostly read-only and often accessed (especially for
 * the RCU path lookup and 'stat' data) fields at the beginning
 * of the 'struct inode'
 */
struct inode {
	umode_t			i_mode;
	unsigned short		i_opflags;
	kuid_t			i_uid;
	kgid_t			i_gid;
	unsigned int		i_flags;

#ifdef CONFIG_FS_POSIX_ACL
	struct posix_acl	*i_acl;
	struct posix_acl	*i_default_acl;
#endif

	const struct inode_operations	*i_op;
	struct super_block	*i_sb;
	struct address_space	*i_mapping;

#ifdef CONFIG_SECURITY
	void			*i_security;
#endif

	/* Stat data, not accessed from path walking */
	unsigned long		i_ino;
	/*
	 * Filesystems may only read i_nlink directly.  They shall use the
	 * following functions for modification:
	 *
	 *    (set|clear|inc|drop)_nlink
	 *    inode_(inc|dec)_link_count
	 */
	union {
		const unsigned int i_nlink;
		unsigned int __i_nlink;
	};
	dev_t			i_rdev;
	loff_t			i_size;
	time64_t		i_atime_sec;
	time64_t		i_mtime_sec;
	time64_t		i_ctime_sec;
	u32			i_atime_nsec;
	u32			i_mtime_nsec;
	u32			i_ctime_nsec;
	u32			i_generation;
	spinlock_t		i_lock;	/* i_blocks, i_bytes, maybe i_size */
	unsigned short          i_bytes;
	u8			i_blkbits;
	enum rw_hint		i_write_hint;
	blkcnt_t		i_blocks;


.....(省略).......


	void			*i_private; /* fs or device private pointer */
} __randomize_layout;
```
[include/linux/fs.h](https://github.com/torvalds/linux/blob/master/include/linux/fs.h)


"i\_mode"はファイルの種類とパミッションを、"i\_uid"や"i\_gid"はファイルの所有者などを表しています。
このように、VFSが提供する"struct inode"はオブジェクトに関する統一的な基本情報を保持しています。



ここで、ファイルシステムの関係についても説明したいと思います。
これまで、inodeのことを"struct inode"と呼んでいました。
これはinodeにはVFSがもつ"struct inode"とファイルシステムがもつinodeがあるからです。
ファイルシステムが持つinodeの方はデータブロックの位置情報やファイルシステム固有の情報などより詳細な情報を持っています。
実際にext4のファイルシステムを見てみましょう。
```
/*
 * Structure of an inode on the disk
 */
struct ext4_inode {
	__le16	i_mode;		/* File mode */
	__le16	i_uid;		/* Low 16 bits of Owner Uid */
	__le32	i_size_lo;	/* Size in bytes */
	__le32	i_atime;	/* Access time */
	__le32	i_ctime;	/* Inode Change time */
	__le32	i_mtime;	/* Modification time */
	__le32	i_dtime;	/* Deletion Time */
	__le16	i_gid;		/* Low 16 bits of Group Id */
	__le16	i_links_count;	/* Links count */
	__le32	i_blocks_lo;	/* Blocks count */
	__le32	i_flags;	/* File flags */
	union {
		struct {
			__le32  l_i_version;
		} linux1;
		struct {
			__u32  h_i_translator;
		} hurd1;
		struct {
			__u32  m_i_reserved1;
		} masix1;
	} osd1;				/* OS dependent 1 */
	__le32	i_block[EXT4_N_BLOCKS];/* Pointers to blocks */
	__le32	i_generation;	/* File version (for NFS) */
	__le32	i_file_acl_lo;	/* File ACL */
	__le32	i_size_high;
	__le32	i_obso_faddr;	/* Obsoleted fragment address */

... （省略）...

	__le16	i_extra_isize;
	__le16	i_checksum_hi;	/* crc32c(uuid+inum+inode) BE */
	__le32  i_ctime_extra;  /* extra Change time      (nsec << 2 | epoch) */
	__le32  i_mtime_extra;  /* extra Modification time(nsec << 2 | epoch) */
	__le32  i_atime_extra;  /* extra Access time      (nsec << 2 | epoch) */
	__le32  i_crtime;       /* File Creation time */
	__le32  i_crtime_extra; /* extra FileCreationtime (nsec << 2 | epoch) */
	__le32  i_version_hi;	/* high 32 bits for 64-bit version */
	__le32	i_projid;	/* Project ID */
};
```
[]()

VFSがもつ"struct inode"とファイルシステムのinodeの関係としては、VFSの"struct inode"がファイルシステムのinodeから情報を取得する感じです。
これにより、カーネルくんはVFSの"struct inode"への統一的なアクセスが可能となります。

ここで、"struct inode"はアクセス権限を"imode"として持っています。
ファイルへアクセスする際はこの"imode"の情報を利用し、マスクをかけることで権限のチェックを行っているのです！

他にもスーパーブロックなど様々な話がありますが、ファイルシステムについての別記事で詳しく書こうと思います。

## 2.3　カーネルの権限管理

1.2でsetuidの危険性について説明しました。
同時に、setuidの必要性についても説明しました。

最小権限の原則や権限分離を守りつつアクセス制御をしたいところですが、どうやってアクセス制御を行うのが良いのでしょうか？

本説では*capability*について説明します。

2.1, 2.2ではinodeについて説明しました。
inodeではimodeで権限情報を持っています。
ここで説明するcapabilityや3章で説明するaclは拡張領域を利用してより細かい権限管理を行います。

capabilitiesについてマニュアルでは以下のように書かれています。

> For the purpose of performing permission checks, traditional UNIX
> implementations distinguish two categories of processes:
> privileged processes (whose effective user ID is 0, referred to as
> superuser or root), and unprivileged processes (whose effective
> UID is nonzero).  Privileged processes bypass all kernel
> permission checks, while unprivileged processes are subject to
> full permission checking based on the process's credentials
> (usually: effective UID, effective GID, and supplementary group
> list).
>
> Starting with Linux 2.2, Linux divides the privileges
> traditionally associated with superuser into distinct units, known
> as capabilities, which can be independently enabled and disabled.
> Capabilities are a per-thread attribute.

[capabilities(7) — Linux manual page](https://man7.org/linux/man-pages/man7/capabilities.7.html)

capabilityは従来のルート権限を分割したものです。
ルート権限という強く広い権限を与えるではなく、capabilityによる一部の強い権限を与えることにより、最小権限の原則に従うことができます。

実際に例を見てみましょう。以下はubuntuにもともと入っている"ping"です。

{{< image src="ppping.png" width="800px" height="600px" caption="pingの権限" >}}

特にsetuidが付与されているわけでもなく、一般ユーザとして実行できるように見えます。
実際に、pingは一般ユーザでも実行できると思います。

では、このコマンドを自分のものとしてコピーして実行してみましょう。
一般ユーザとして実行できているファイルなので、コピーができれば問題なく動作するはずです。

{{< image src="ping_caps_error.png" width="800px" height="600px" caption="ソケットエラー" >}}

エラーになりました。
ローソケットの操作が許可されていないと怒られており、"capability"か"setuid"がないと言われています。

これは、"ping"がローソケットを利用しており、その権限がないためエラーが出ているのです。
では、もともと入っていた"ping"を見てみましょう。

{{< image src="ping_caps.png" width="800px" height="600px" caption="pingのcapability" >}}

"cap_net_raw=ep"とあり、ローソケットを利用するためのcapabilityが割り当てられていることが分かります。\
※ e=Effectve, p=Permitted を意味してます。

これにより、setuidを利用せずにローソケットの利用しており、pingに脆弱性があったとしても権限昇格などの危険性が緩和されます。

## 3　その他のアクセス制御

### acl

aclはこれまでの1章で復習した標準の権限管理より細かいアクセス制御を実現できます。
具体的には、「その他のユーザーが」ではなく、「誰が」を設定できます。

{{< image src="facl.png" width="800px" height="600px" caption="aclのインストール" >}}

標準ではaclが入っていなかったのでインスールしました。

実際に使ってみましょう。

{{< image src="setfacl.png" width="800px" height="600px" caption="aclの設定" >}}

"root_secret.txt"にユーザ"hoge"だけ読み込みと書き込みができるように設定しました。

### SELinux

Linuxのアクセス制御は、以下の2種類があります。
* 任意アクセス制御
* 強制アクセス制御

これまで紹介してきたものはすべて任意アクセス制御です。
"selinux"は強制アクセス制御に分類されます。

> SELinux (Security-Enhanced Linux) は、システムにアクセスできるユーザーを管理者がよりきめ細かく制御するための、Linux® システム向けセキュリティ・アーキテクチャです。もともとは、Linux Security Modules (LSM) を使用した Linux カーネルへの一連のパッチとして、アメリカ国家安全保障局 (NSA) によって開発されました。

[SELinux (Security-Enhanced Linux) とは](https://www.redhat.com/ja/topics/linux/what-is-selinux)

SELinuxもcapabilityやaclのように標準的な権限管理より細かい設定が可能です。
任意アクセス制御と最も異なる点は、「rootであっても、すべてを自由に操作できるとは限らない」ことです。
すなわち、rootという中央集権を排除し、**権限分掌**を行うことができます。

SELinuxについては、深く学びたいので別の記事で個別に深堀ることとします。

## 4　ユースケース

### ACL

その他のユーザを細かく分ける。
グループとかたくさん作ってやると便利

### Capability

Nginxとかを例にsudoとの差を示す
取られたときの影響範囲をどれだけ抑えられるか
実際は大変な話。ベストプラクティス集みたいなものをつくれれば

## 5　まとめ

今回はLinuxのアクセス制御について学習しました。
capabilityやSELinuxを利用することで、より細かい制御を行うことができ、最小権限の原則を実現できます。
実際のユースケースまで見えていない部分があるので、追って勉強しようと思います。



## 参考

[1] []()
[2] []()
[3] []()

