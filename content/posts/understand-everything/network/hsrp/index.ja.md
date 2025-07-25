---
weight: 4
title: "HSRPを完全に理解する"
date: 2025-04-13T00:00:00+09:00
lastmod: 2025-04-13T00:00:00+09:00
draft: true
author: "しぶや"
authorLink: "https://github.com/shibuya-s-eg"
description: "HSRPを完全に理解する"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"

tags: ["Network"]
categories: ["Understand-Everything"]
subcategories: ["Network"]

lightgallery: true
---

<!--
Todo:
- TLDR

-->


こんにちは、しぶやです。
今回はを完全に理解していきます。


## TL;DR

*
*
*

## 1　はじめに
### 1.1　HSRPとは何か
- Cisco独自のファーストホップ冗長化プロトコルであること
- 対象読者（ネットワーク管理者、学習者）と本記事のゴール
### 1.2　なぜHSRPが必要か
- 通信の可用性と冗長性の確保
- ゲートウェイが単一障害点になる問題

## 2　HSRPの基本構成と仕組み
### 2.1　HSRPグループ
- 複数のルーターで仮想IPを共有
### 2.2　仮想IPアドレスとMACアドレス
- クライアントは仮想IPをデフォルトゲートウェイとして使用
### 2.3　役割（Active/Standby/Listen）
- どのルーターがアクティブで、どのルーターが待機するかの決定

## 3　パケットレベルで見るHSRP
### 3.1　Hello/Deadインターバル
- デフォルト値と意味（Hello: 3秒、Hold: 10秒）
### 3.2　UDP 1985番ポートによるマルチキャスト通信
- 224.0.0.2 宛に送信されるHelloパケットの内容
### 3.3　パケットの構造
- バージョン、グループ番号、優先度、状態など

## 4　優先度とプリエンプト
### 4.1　優先度の設定と意味
- 数値が高い方がActiveルーターに選ばれる
### 4.2　プリエンプトの有効化/無効化
- より高い優先度のルーターが復帰時にActiveに戻るかどうか

## 5　HSRPの状態遷移
### 5.1　初期状態からの遷移フロー
- `Initial → Learn → Listen → Speak → Standby/Active`
### 5.2　各状態の役割と条件

## 6　実機/シミュレーション例
### 6.1　Cisco IOSを使った設定例
### 6.2　Packet TracerやGNS3での検証
- フェイルオーバーが発生する様子のキャプチャ付きで

## 7　HSRPのバージョンと違い
### 7.1　HSRP Version 1 vs Version 2
- IPv6対応、マルチグループ数、アドレスフォーマットなどの差異

## 8　セキュリティと制限事項
### 8.1　HSRPのセキュリティ上の懸念
- 攻撃者がHelloパケットを送って乗っ取るリスク
### 8.2　対策例
- 認証（MD5など）、インターフェースACLの導入

## 9　VRRPやGLBPとの比較
### 9.1　他の冗長化プロトコルとの比較表
- 標準性、負荷分散、可搬性などで整理

## 10　おわりに
### 10.1　まとめと再確認
### 10.2　ネットワーク冗長化の重要性
### 10.3　今後の発展や関連プロトコルの学習へのつなぎ


## 参考

[1] []()
