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



{{< image src="preparation.jpeg" width="800px" height="600px" caption="準備" >}}
{{< image src="diord.jpeg" width="800px" height="600px" caption="ダイオードの準備" >}}
{{< image src="diord-2.jpeg" width="800px" height="600px" caption="ダイオードのはんだ付け" >}}
{{< image src="key-socket.jpeg" width="800px" height="600px" caption="キーソケットのはんだ付け" >}}
{{< image src="micon.jpeg" width="800px" height="600px" caption="マイコンのはんだ付け" >}}
{{< image src="cleaner.jpeg" width="800px" height="600px" caption="フラックスクリーナーで掃除" >}}
{{< image src="sensor.jpeg" width="800px" height="600px" caption="トラックボール用のセンサーと水平ロータリーエンコーダの作成" >}}
{{< image src="frame.jpeg" width="800px" height="600px" caption="フレームの組み立て" >}}
{{< image src="xiable.png" width="800px" height="600px" caption="Xiao BLEをブートローダーモードで起動" >}}
{{< image src="xiaoble_write.png" width="800px" height="600px" caption="ファームウェアの書き込み１" >}}
{{< image src="xiaoble_write2.png" width="800px" height="600px" caption="ファームウェアの書き込み２" >}}
{{< image src="bluetoothctl.png" width="800px" height="600px" caption="Bluetooth接続１" >}}
{{< image src="bluetoothctl-2.png" width="800px" height="600px" caption="Bluetooth接続２" >}}


## 参考

[1] [roBa 【トラックボール付きワイヤレス自作キーボードキット】](https://booth.pm/ja/items/6010869)\
[2] [Build Guide for roBa](https://github.com/kumamuk-git/roBa/blob/main/doc/v3/buildguide_v3.md)\
[3] [分離式＆トラックボール搭載 自作キーボード Keyball39 制作工程 まとめ](https://www.youtube.com/watch?v=MsvfFNaBjTs)\
[4] [自作キーボード作ってみた Keyball44編 | Keyball44 : Custom Mechanical Keyboard Build](https://www.youtube.com/watch?v=XyoEP3h4iB8)\
[5] [【永久保存版】はんだ付けのやり方を解説します【はんだづけの原理, DIP部品, 表面実装】【イチケン電子基礎シリーズ】RX-802AS](https://www.youtube.com/watch?v=dQ7AUjb1tkA)

