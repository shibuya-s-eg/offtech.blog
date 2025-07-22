# NASを完全に理解する


こんにちは、しぶやです。\
最近、Synologyのnasを中古で安く仕入れたので、これを機にNASを「完全に理解」していきたいと思います。

## TL;DR

* NASはファイルサーバみたいなもの。
* 標準機能として、RAIDが組めたり、バックアップとれたり、いろいろできる。
* NAS用ソフトが充実している。

## 1　NASの基本

本章では、NASの基本について学んでいきます。

### 1.1　NASとは

NASとは、Network Attached Strorageの略です。その名の通り、ネットワークに接続して利用するストレージです。\
代表的なNASメーカーには以下のようなものがあります。

* Synology
* QNAP
* BUFFALO
* NETGEAR

何はともあれ、現物を見てイメージの具体化をしてみます。

　
* [Synology DiskStation DS223j](https://www.synology.com/ja-jp/products/DS223j)\
小型で静音の省エネ型ストレージ

こちらは、家庭用でも利用されているNASです。Youtubeとかもよく見るサイズのものですね。

　
* [Synology DiskStation DS3622xs+](https://www.synology.com/ja-jp/products/DS3622xs+)\
どんな環境にも適合する、パワフルで大容量のストレージ

こちらは、より高性能な大容量ストレージです。いかつい見た目でかっこよいですね:(fa-brands fa-linux fax):

　
* [Synology FlashStation FS6400](http://synology.com/ja-jp/products/FS6400)\
I/O インテンシブなアプリケーション向けの超高速オールフラッシュサーバー

ラックマウント用のNASもあるようです。

　
* [Synology Plusシリーズ3.5インチSATA HDD](https://www.synology.com/ja-jp/products/drives/hdd/plus-hat)\
家庭やスモールオフィス環境のSynologyシステム専用に作られた高信頼性ハードドライブ
* [Synology Enterpriseシリーズ2.5インチSATA SSD](https://www.synology.com/ja-jp/products/drives/ssd/enterprise-sat)\
SynologyのSSDを使用してストレージ インフラを最新化

NASには上記のようなHDDやSDDを差し込み利用します。**nベイ**というのは、これらを差し込むスロットが何個あるかを表しています。


{{< admonition note "DAS vs NAS vs SAN" >}}
NASと比較されるものとして、**DAS**や**SAN**があります。
* **DAS：Direct Attached Storage**\
ストレージを直接コンピュータに接続する方式です。容易なセットアップや低コストといった特徴があります。
* **NAS：Network Attached Storage**\
本記事で取り上げている通り、ネットワークを通じてアクセス可能なストレージです。ネットワーク経由で複数人で同時にアクセスできる特徴があります。
* **SAN：Storage Area Network**\
専用のネットワーク経由で複数のサーバと接続できるストレージです。
こちらについては、あまり詳しくわかっていませんが、データ転送が高速であったり、ブロックレベルでアクセス可能といった特徴があるようです。
ちなみに、どんなプロトコルで通信が行われているのかについてですが、iSCSI（IP over SCSI）やFibre Channel（FC）、Fibre Channel over Ethernet（FCoE）といったものが利用されているようです。

{{< /admonition >}}


### 1.2　NASの機能

それでは、NASの具体的な機能を見ていきます。

* **ファイル共有**\
ネットワーク経由でファイルの読み書きが可能です。スナップショットをとれたり、バージョン管理機能を持っていたりするものなどもあります。
「具体的に中で何が動いているのか」についてですが、NASはSMBやNFSなど複数のプロトコルをサポートしており、中身は一般的なOSSでファイルサーバのように動くようです。

* **アクセス管理**\
利用者視点ではもちろん機能ですね。ユーザごとに読み取り専用などアクセス権の管理が可能です。

* **RAID**\
RAIDによるデータの冗長性を確保できます。もちろん複数ベイが必要になります。

{{< admonition note "復習：RAID" >}}
RAIDはRedundant Arrays of Inexpensive Disksの略です。複数のディスクを組み合わせて冗長化するやつですね。\
RAIDの主なレベルは以下です。
* **RAID 0**\
冗長化していない通常のモードです。
信頼性は失われますが、書き込む量も増えないため、高速でストレージの利用効率も高いです。

* **RAID 1**\
2台のディスクを利用したシンプルなミラーリングです。データを二重に保存します。

* **RAID 5**\
3台以上のディスクを利用し、データとそのパリティをそれぞれ分散させて保存します。
パリティを保存しているため、容量効率は高いですが、データの復旧には時間がかかってしまいます。

* **RAID 6**\
RAID5 + パリティです。RAID1やRIAD5と異なり、2台のディスクが壊れても復旧が可能です。

* **RAID 10**\
RAID 1 + RAID 0でRAID 10です。RAID 5/6よりも書き込みが高速で、耐障害性も高いです。

{{< /admonition >}}

* **バックアップ**\
バックアップにはPC等のクライアントのバックアップとNAS自身のデータのバックアップの2つ観点があります。
前者は専用のクライアントソフトなどを利用することで、手軽に頻度や対象を指定したバックアップを行えます。
後者は、Google DriveやAWSなど、クラウドストレージへNAS自身のデータをバックアップできます。

{{< admonition tip >}}
NASは標準でRAID機能を持っているものが多いですが、これはあくまで冗長化です。\
操作ミスによるNASデータの削除やランサムウェアによる被害が発生した際はデータを失ってしまいます。\
これらの対策として別のストレージにバックアップを取ることが重要となり、NASは標準でクラウドストレージへのバップアップを行うことができます。
{{< /admonition >}}

データの冗長化ではないですが、通信の冗長化としてLAGが組めるのも良い点ですね。
LANポートを複数持っているので、通信経路の冗長化にもなります。

* **リモートアクセスやVPN**\
専用ソフトを利用することで、インターネット経由で安全にアクセスできる機能を備えています。
インターネット経由で利用することも考えると当たり前に欲しい機能ですね。

* **VMやコンテナの実行**\
高性能のNASの場合は、VMやコンテナを中で動かせるようです。
独自のアプリケーションを中で動かすことで様々なことができるようになります。

* **セキュリティ**\
NAS自身がファイアウォール機能などを持っていることもあります。
また、高性能なものは2段階認証を利用できたり、ウイルススキャン機能を持っていたりします。

* **Webサーバやメディアサーバ**\
サーバ機能を持っているものもあるようです。

* **メンテナンス**\
ファームウェアの自動アップデート機能を持っていたりします。
また、今回Synologyを使ってみてわかりましたが、オンラインからのヘルスチェックサービスなどもあるようです。


{{< admonition note "NASの性能" >}}
NASを買う前は、NASは高性能な高いやつほど読み書き速度が早いと安直に考えていました。
改めて考えてみると、読み書き速度はNASの性能ではなく、HDDやSDDの読み書き速度に依存することがわかります。
※ 正確には、ネットワークの帯域幅であったり、RAIDの構成、CPUの性能などにも依存します。
{{< /admonition >}}


### 1.3　NASのメリット・デメリット

NASのメリットは何と言っても1.2であげたものが**手軽に利用できる**ことです。
外付けHDDだけでは、複数人でデータを共有することはできないですし、ネットワーク経由でデータにアクセスすることもできません。


一方でNASのデメリットは、初期費用が高いことです。高機能なものにもなると、HDD抜きでNAS自身が非常に高いです。

{{< admonition note "所感" >}}
上記で太字で記載しましたが、**NASの本質は標準で様々な機能が入っており、手軽に導入できる**ことかと思います。
NAS自身のOSは専用の組み込みOSであることが多いようですが、その気になればPC1台とRAIDに必要な外付けHDDがあれば1.2で上げた機能をすべて実現できると思います。
これらの導入コストを低くできることがNASのメリットだと考えています。※ 静音だとか性能だとかって話はあると思いますが。
{{< /admonition >}}

### 1.4　外付けストレージ・ファイルサーバとの違い

比較表を書こうと思いましたが、1.3で述べたように、以下のような関係で表すことが適切かと思いました。

* **外付けHDD ⊂ ファイルサーバ ⊂ NAS**

外付けHDDもRAIDが組めるものがありますし、複数台で自分で組むこともできるでしょう。
また、外付けHDDをマウントし、ファイルサーバを建てることもできれば、フアイルサーバに機能を加えNASっぽく運用することもできるでしょう。
これらのことから、それぞれの境界はやや曖昧で、包含関係がしっくりきました。

### 1.5　ユースケース

* **ファイルサーバ**\
家庭用、企業用問わずファイルサーバとしてデータ共有に利用されています。

* **バックアップ**\
バックアップの保存用途でNASが利用されています。

* **その他**\
メディアサーバ等様々な用途で利用されています。IoTの発展に伴い、カメラデータの保存等にも使われているようです。

{{< admonition tip >}}
NASというよりHDDの話です。
よく壊れやすいと言われているHDDについてですが、動かし続けていないとより壊れやすいようです。
具体的な理由はChatGPTが以下のように教えてくれました。
* ヘッドの位置ずれ
* 潤滑油の劣化
* 温度管理の不良
* 結露の問題
* 機械部品の劣化
家とかも誰も住んでいないとすぐだめになったりしますよね:(fa-brands fa-linux fax):
{{< /admonition >}}

## 2　Synologyを触る

今回は[Synology DS120j](https://global.download.synology.com/download/Document/Hardware/DataSheet/DiskStation/20-year/DS120j/jpn/Synology_DS120j_Data_Sheet_jpn.pdf)を中古で安く仕入れたので、こちらを触ります。
1ベイモデルとなっており、RAIDを組めないのが残念ですが、用途としては自宅鯖のバックアップようなのでよいでしょう。

* 製品名：Synology DS120j
* バージョン：DSM 7.2.2
* HDD容量：2TB

OSは初期セットアップ時にダウンロードしました。HDDは別で買うのが面倒だったので、HDD付きを買いました。

{{< image src="synology-exterior.jpeg" width="400px" height="300px" caption="Synology DS120j" >}}


### 2.1　解体

解体（物理）です。

{{< image src="synology-interior.jpeg" width="400px" height="300px" caption="NAS 解体ショー" >}}

思っていたより大きめの基盤が張り付いていました。この辺も最低でも登場人物と役割がわかる程度にはなりたいですが、今は電源やファンの接続部分くらいしか分からないです。
近いうちに勉強する予定があるので別記事に書くとして今回はスルーします。

<div class="image-row">
    {{< image src="synology-hdd0.jpeg" width="400px" height="300px" caption="HDD 表" >}}
    {{< image src="synology-hdd1.jpeg" width="400px" height="300px" caption="HDD 裏" >}}
</div>

NAS用のHDDです。やはりHDDは重いですね。
裏面を見るとよりディスクとヘッドの存在が透けてきてテンションが上がりました。
真ん中にピング色の何かがついていますが、これは何なのでしょう。接着剤？

HDDも解体して中身のディスクとか見たかったですが、使う前に壊してしまうのが怖くなりやめました。壊れたらやってみよう。

### 2.2　初期設定

物理を見たところで、いよいよNASを動かしてみます・

{{< image src="synology-setup0.png" width="400px" height="300px" caption="起動時画面" >}}

<div class="image-row">
    {{< image src="synology-setup1.png" width="400px" height="300px" caption="ファームウェアの選択" >}}
    {{< image src="synology-setup2.png" width="400px" height="300px" caption="ファームウェアのインストール" >}}
</div>

起動するとすぐにファームウェアを要求されました。
[公式](https://www.synology.com/ja-jp/support/download/DS120j?version=7.2#system)からダウンロードし、NASへインストールしていきます。\

Synologyの組み込みOSは**DSM（Disk Station Manager）**というもので.patという拡張子が利用されているようです。

{{< admonition note "HDDのファームウェア" >}}
DSMのダウンロード時に**Synology HDD/SSD オフラインアップデータ**という項目を見つけ、.saという拡張子のファイルが存在することを見つけました。\
これまで意識したことがなかったですが、HDDもRAMやROMを持っており、ファームウェアが動作しているようです。SynologyはDSMからHDDのファームウェアの管理ができるのだとか。\
参考：[【技術顧問の小話 ＃004】HDDのファームウェアとは？](https://www.get-it.ne.jp/column/column_technical-advisor_004/)
{{< /admonition >}}

{{< image src="synology-user-setup.png" width="400px" height="300px" caption="アカウント設定" >}}

続きまして、ユーザの設定です。一般的なOSをインストールしたときと同じような流れですね。

<div class="image-row">
    {{< image src="synology-auto-update.png" width="400px" height="300px" caption="ファームウェアの自動更新設定" >}}
    {{< image src="synology-more-benefits.png" width="400px" height="300px" caption="追加機能" >}}
</div>

NASっぽい設定が始まりました。\
左の画像では、**1.2 NASの機能**でも記載したように、ファームウェアの自動更新ができるようです。選択肢配下。

* 重要なDSMとパッケージのアップデートのみを自動で行う。（推奨）
* DSMとパッケージのアップデートを自動で行う。
* DSMとパッケージのアップデートの通知のみを行い、手動でアップデートを行う。

右の画像では、Synologyのアカウントを作成することでオンラインで追加の機能を提供してくれるようです。以下、詳細です。

* Secure Signin Service\
具体的には以下の方法でサインインができるみたいです。
    * 2要素認証\
    専用アプリにOTPを発行可能。
    * ハードウェアセキュリティキーによる認証\
    USB キー、Windows Hello、または Mac Touch IDで認証可能

* Access your Synology NAS from anywhere\
インターネットからアクセスできるようにするものです。
[仕組み](https://homepaperless.com/quick-connect/#rtoc-1)としては、NAS → Synology中継サーバ、クライアント → Synology中継サーバで接続を行うことで、安全にアクセスできるようにするというものです。
家にインバウンド通信を許可する必要もないことやSynologyが境界を固めてくれていることを考えると非常に便利で安全ですね。中継サーバが乗っ取られていない前提ですが。

* Around-the-clock monitoring and protection\
Synology側のサーバからNASのヘルスチェックだったり、トラブルシュートの提供だったりを行ってくれるらしい。

### 2.2　検証

{{< image src="synology-desktop.png" width="400px" height="300px" caption="Synologyの画面" >}}

Synologyの画面です。**ブラウザで動くアプリケーション**があるという認識はありましたが、**デスクトップlike**なアプリケーションなんですね。
なぜデスクトップlikeにしているのでしょうか。
VM動かしたりとかPCっぽい機能をたくさん利用することを考えるとユーザーフレンドリーなのでしょうか。

<div class="image-row">
    {{< image src="synology-shared-folder.png" width="400px" height="300px" caption="共有フォルダ" >}}
    {{< image src="synology-access-rights.png" width="400px" height="300px" caption="アクセス権" >}}
</div>

試しに共有フォルダを作成してみました。\
Locationに**Volume 1: ext4**とあることから、裏ではボリューム作ってフォーマットしてファイルシステム作成している感じですかね。
物理ボリューム → ボリュームグループ → 論理ボリューム → ファイルシステムみたいなコマンドベースの手順を踏まずにポチポチで完結するのはらくで良いですね。\
アクセス権についてもGUIで簡単に設定できます。

{{< image src="synology-applications.png" width="400px" height="300px" caption="豊富なアプリケーション" >}}

アプリもかなり豊富な種類がありました。


{{< admonition tip "消費電力" >}}

Synology DS120jで消費電力を測ってみました。

<div class="image-row">
    {{< image src="synology-wat2.jpg" width="400px" height="300px" caption="通常時の消費電力" >}}
    {{< image src="synology-wat.jpeg" width="400px" height="300px" caption="書き込み時の消費電力" >}}
</div>

高いのか低いのか勘所がないです。。
アイドル時：4.68W、負荷時：9.81Wと紹介しているサイトがありました。アイドル状態とやらになったらもう少し消費電力を抑えられたのかもしれません。

{{< /admonition >}}


## 3　まとめ

記事の途中でも書きましたが、NASの本質は**標準で様々な機能が入っており、手軽に導入できる**ファイルサーバだと思っています。
以上を踏まえ、以下にまとめます。

* NASはファイルサーバみたいなもの。
* 標準機能として、RAIDが組めたり、バックアップとれたり、いろいろできる。
* NAS用ソフトが充実している。

## 参考

[1] [Synology](https://www.synology.com/)\
[2] [NAS入門: 初心者でもわかるデータ保存・共有・バックアップの極意](https://www.amazon.co.jp/NAS%E5%85%A5%E9%96%80-%E5%88%9D%E5%BF%83%E8%80%85%E3%81%A7%E3%82%82%E3%82%8F%E3%81%8B%E3%82%8B%E3%83%87%E3%83%BC%E3%82%BF%E4%BF%9D%E5%AD%98%E3%83%BB%E5%85%B1%E6%9C%89%E3%83%BB%E3%83%90%E3%83%83%E3%82%AF%E3%82%A2%E3%83%83%E3%83%97%E3%81%AE%E6%A5%B5%E6%84%8F-%E5%A4%96%E4%BB%98%E3%81%91HDD%E3%81%AF%E3%82%82%E3%81%86%E5%8F%A4%E3%81%84%EF%BC%81%E5%AE%B6%E5%BA%AD%E7%94%A8%E3%83%87%E3%83%BC%E3%82%BF%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%81%AE%E6%B4%BB%E7%94%A8%E6%B3%95-%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E3%82%B9%E3%83%88%E3%83%AC%E3%83%BC%E3%82%B8-%E3%82%AF%E3%83%A9%E3%82%A6%E3%83%89%E3%82%B9%E3%83%88%E3%83%AC%E3%83%BC%E3%82%B8-ebook/dp/B0DX1XG5TJ)
[3] [【技術顧問の小話 ＃004】HDDのファームウェアとは？](https://www.get-it.ne.jp/column/column_technical-advisor_004/)
[4] [SynologyNASの外部アクセスQuickConnectの仕組みと設定方法](https://homepaperless.com/quick-connect/#rtoc-1)

