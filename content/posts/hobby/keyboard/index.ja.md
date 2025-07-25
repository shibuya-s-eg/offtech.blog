---
weight: 4
title: "自作キーボードデビューする"
date: 2025-04-30T00:00:00+09:00
lastmod: 2025-04-30T00:00:00+09:00
draft: false
author: "しぶや"
authorLink: "https://github.com/shibuya-s-eg"
description: "自作キーボードデビューする"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"

tags: ["Hobby"]
categories: ["Hobby"]
subcategories: ["Keyboard"]

lightgallery: true
---

<!--
Todo:
- TLDR

-->


こんにちは、しぶやです。
最近、キーボードの自作を始めたので、その様子を紹介したいと思います。


## TL;DR

* キーボードを初めて自作しました。
* 初心者でも頑張れば練習なしで作れます。
* 諸々合わせると結構お金かかりました。


## 1　はじめに

### 1.1　経緯

まず、今回自作キーボードを始めようとした理由ですが、大きくは以下の点があります。
* オタクっぽくでカッコ良いから。
* 40％キーボードを使いたかったから。
* 電子工作を勉強したかったから。

3つ目に関しては、回路を全て読めるようになるというよりは、ひとまず各部品やその役割を抑えることや、どのように基盤に取り付けられるのかを学ぶことくらいのイメージです。
いずれは回路も読めるようになりたいですが、今回はスピードを重視してキーボードを動かすことに注力したいと思います。

### 1.2　キーボード選定

キーボードを選定するにあたり以下の条件がありました。
* はんだ付けから行えること。
* ビルドガイドやyoutubeなど情報が充実していること。
* 40%キーボードであること。

特に2点目はかなり大きかったです。
ビルドガイドはもちろんYoutubeで同種のキーボード自作動画がたくさん存在したため、はんだ付けも視覚的な参考しながら進められました。

今回、僕が選定したキーボードは[roba]()です。
robaは完全無線化された40%の分割キーボードで、トラックボールも搭載されています。
robaについては、こちらの動画で詳しく紹介されています。

キーボード自作という観点では、[ビルドガイド]()が充実していることに加え、[Keyball]()という似たキーボードの自作動画がたくさん存在するため参考情報には困らないです。

本当に自分に合うかは分からないですが、Boothですぐ売り切れてしまう人気なキーボードなのでこちらで自作キーボードデビューをすることに決めました。

## 2 準備

キーボードに加え、はんだ付けに必要な道具も一式揃えました。
なお、robaはセット内容以外に追加で必要なものがあるため、そちらは別途購入する必要があります。

{{< image src="preparation.jpeg" width="800px" height="600px" caption="準備" >}}

キースイッチとキーキャップは全く知識がなかったので、ひとまず安いやつを購入しました。
事前に勉強しようと思い、電子工作キットを購入しましたが使う場面はなかったです。
後述しますが、テスターは必須だと思います。
加えて、コテは良いやつを買った方が良いです。
はんだ付け時に電子部品に熱を取られてしまってから温まるまでに時間がかかってしまい効率がかなり落ちます。

## 3 はんだ付け

いよいよ、はんだ付けをしていきます。

まずは、ダイオードのはんだ付けです。
ダイオードには向きがありますが、プリント基板に印が付いているため注意していれば間違えることはないと思います。
事前にマスキングテープに貼り付けて向きも固定しておくことで、スムーズにはんだ付けできると思います。

{{< image src="diord.jpeg" width="800px" height="600px" caption="ダイオードの準備" >}}

以下の流れではんだ付けを行いました。
1. (片側に)フラックスを塗る。
2. (片側の)基盤の装着部分を温める。
3. (片側に)はんだを盛る。
4. はんだを溶かしながらダイオードをスライドさせ、ダイオードを片側で固定する。
5. 反対側も1〜4を繰り返す。
6. 最初に固定した方のはんだを溶かし、再度調整する。はんだが足りていなければ追加する。
7. フラックスクリーナーで掃除する。
8. テスターで抵抗を測る。

8は特に重要で、僕はこれを怠ったため、キーボード組み立て後にキーが反応せず手戻りが発生してしまいました。

{{< image src="diord-2.jpeg" width="800px" height="600px" caption="ダイオードのはんだ付け" >}}

続いて、キーソケットのはんだ付けです。
キーソケットもダイオードと同様にはんだ付けをしていきます。
「割と多めにハンダを持って、溶かしながら押し込む」とはんだ付けが上手くいった感じがします。

{{< image src="key-socket.jpeg" width="800px" height="600px" caption="キーソケットのはんだ付け" >}}

難関のマイコンのはんだ付けです。
(僕はマイコンの一部がはんだ付けできていなかったことで、複数のキーが反応しませんでした。)

僕の場合はマイコン側にはんだが引っ張られ、基盤側に漏れないという問題が発生していました。
どうやら、基盤側をしっかり温めることでこれは解決するようです。

マイコンは左右に複数はんだ付けする部分がありますが、これを一つミスると複数のキーが動かなくなります。
[こちら](https://note.com/pooh_polo/n/n386f7c9355bc)で詳細なマッピングが紹介されているので、トラブルシュートに利用させてもらいました。

{{< image src="micon.jpeg" width="800px" height="600px" caption="マイコンのはんだ付け" >}}

フラックスを塗ってそのままにしておくと、基板が腐食するようです。
そのため、はんだ付け後はフラックスクリーナーで掃除をします。

テスターでテストをするためにもこの工程は必要になると思います。
フラックスが塗ってある状態では、酸化被膜が付着しており上手く通電確認ができないためです。

{{< image src="cleaner.jpeg" width="800px" height="600px" caption="フラックスクリーナーで掃除" >}}

トラックボール用のセンサーとロータリーエンコーダーも作成し、はんだ付けは終了です。

{{< image src="sensor.jpeg" width="800px" height="600px" caption="トラックボール用のセンサーと水平ロータリーエンコーダの作成" >}}

## 4 組み立て

ビルドガイドに沿って組み立てを行いました。
無駄な手戻りが発生するので、組み立てを行う前にテスターで通電確認をしておくことをおすすめします。

{{< image src="frame.jpeg" width="800px" height="600px" caption="フレームの組み立て" >}}


## 5 動作確認

最後に動作確認を行います。
Windowsが手元にないためLinuxに接続をしていきます。

まずは、マイコンにファームウェアを書き込んでいきます。
USB-TypeCを接続し、リセットボタンを2度押すとXiaoBLEがブートローダーモードで起動します。
"/"に出力されるログを確認すると、下記のような記載がありPCがデバイスを認識していることがわかります。

{{< image src="xiable.png" width="800px" height="600px" caption="Xiao BLEをブートローダーモードで起動" >}}

適当なフォルダにマウントを行った上でビルドガイドに従ってファームウェアを書き込みます。
左右両方に対して同様の作業をします。

{{< image src="xiaoble_write.png" width="800px" height="600px" caption="ファームウェアの書き込み１" >}}
{{< image src="xiaoble_write2.png" width="800px" height="600px" caption="ファームウェアの書き込み２" >}}

無事に書き込みが終わりました。

続いて、Bluetooth接続をしていきます。
(GUIがよく分からずCLIで接続しています。)

{{< image src="bluetoothctl.png" width="800px" height="600px" caption="Bluetooth接続１" >}}
{{< image src="bluetoothctl-2.png" width="800px" height="600px" caption="Bluetooth接続２" >}}

無事接続できました。

僕はここで初めて1／4程度のキーが反応していないことが分かり、フレームを外しはんだ付けをし直すことになりました。

## 6　完成
### 6.1　外観

ひとまず、白で統一してみました。
amazonで安いキーキャップをつけただけですが十分かっこよいです。

{{< image src="myroba.png" width="800px" height="600px" caption="my Roba" >}}

amazonで丁度よいサイズの[ケース](https://www.amazon.co.jp/%E3%82%B5%E3%83%B3%E3%83%AF%E3%83%80%E3%82%A4%E3%83%AC%E3%82%AF%E3%83%88-%E3%83%88%E3%83%A9%E3%83%99%E3%83%AB%E3%83%9D%E3%83%BC%E3%83%81-%E3%82%AC%E3%82%B8%E3%82%A7%E3%83%83%E3%83%88%E3%83%9D%E3%83%BC%E3%83%81-%E3%83%A2%E3%83%90%E3%82%A4%E3%83%AB%E3%83%90%E3%83%83%E3%83%86%E3%83%AA%E3%83%BC-200-BAGIN014BK/dp/B0843QXWWZ/ref=asc_df_B0843QXWWZ?mcid=0ec8152148bd3ae1ad5d4a26fe8f4e7c&tag=jpgo-22&linkCode=df0&hvadid=707476841947&hvpos=&hvnetw=g&hvrand=5656379571200932601&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9196647&hvtargid=pla-864105697622&gad_source=1&th=)も買いました。
Xで検索するとrobaの持ち運びケースに丁度よいものがいくつか紹介されているのでそこから選びました。

{{< image src="case-1.png" width="800px" height="600px" caption="Roba用ケース" >}}
{{< image src="case-2.png" width="800px" height="600px" caption="Roba用ケース" >}}


### 6.2　キーマップ

robaのビルドガイド通りに進めると[ここ](https://nickcoutsos.github.io/keymap-editor/)でキーマップを編集することになります。
[こちらの方のキーマップ](https://note.com/bugzzy1013/n/nfc301dfdb1f8)を参考に設定を行い、自分用にアレンジをしていきました

この記事もrobaで書いていますが非常に苦戦しています。
新たなキーマップに慣れるまでかなり時間がかかりそうです。。

### 6.3　その他

その他の打鍵感などは素人すぎてよく分からないため、今後気にしていきたいと考えています。
静音赤軸が良いという噂は聞いています。。

## 6 感想
### 6.1　自作の感想

自作してみての感想は楽しかったにつきます。
はじめてのはんだづけで苦労も多かったですが、上手く動作したときはテンションバク上がりでした。

人気なキーボードを選んだこともあり、ビルドガイドや参考ソースが充実していた点はかなり大きかったです。

一方で、自作に限らずですが、キーマップの編集はかなり骨が折れます。。
使うたびに調整をしたくなりきりがありません。

ここはゆっくり調整していきたいと思います。

### 6.2 robaの感想

まだ使い始めて日が浅いうえに、初の自作キーボードであったのであまり評価ができないのが正直なところです。
ただ、トラックボール登載で完全無線は非常に使いやすいです!
トラックボールを触った際にレイヤーが入れ替わること仕様も個人的には気に入っています。

手を置く台があるとより使いやすいと感じるので試してみようと思います。


## 参考

[1] [roBa 【トラックボール付きワイヤレス自作キーボードキット】](https://booth.pm/ja/items/6010869)\
[2] [Build Guide for roBa](https://github.com/kumamuk-git/roBa/blob/main/doc/v3/buildguide_v3.md)\
[3] [分離式＆トラックボール搭載 自作キーボード Keyball39 制作工程 まとめ](https://www.youtube.com/watch?v=MsvfFNaBjTs)\
[4] [自作キーボード作ってみた Keyball44編 | Keyball44 : Custom Mechanical Keyboard Build](https://www.youtube.com/watch?v=XyoEP3h4iB8)\
[5] [【永久保存版】はんだ付けのやり方を解説します【はんだづけの原理, DIP部品, 表面実装】【イチケン電子基礎シリーズ】RX-802AS](https://www.youtube.com/watch?v=dQ7AUjb1tkA)
[6] [roBaの組み立てで動作不良？パターン別チェックポイントを解説](https://note.com/pooh_polo/n/n386f7c9355bc)
