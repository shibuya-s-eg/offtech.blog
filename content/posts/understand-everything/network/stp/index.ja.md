---
weight: 4
title: "STPを完全に理解する"
date: 2025-04-13T00:00:00+09:00
lastmod: 2025-04-13T00:00:00+09:00
draft: true
author: "しぶや"
authorLink: "https://github.com/shibuya-s-eg"
description: "STPを完全に理解する"
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
- ループがネットワークにもたらす問題（ブロードキャストストーム、MACテーブルの混乱など）
- レイヤ2スイッチの特徴とループの危険性
- STPの目的と登場背景

## 2　STPの基本概念
### 2.1　ブリッジとスイッチの役割
### 2.2　フレームの流れとブロードキャストドメイン
### 2.3　STPの目的：ループ防止と冗長経路の制御

## 3　STPの仕組みとプロセス
### 3.1　BPDU（Bridge Protocol Data Unit）とは
### 3.2　ルートブリッジ選出の仕組み（Bridge ID, Priority, MACアドレス）
### 3.3　各ポートの役割と決定プロセス
- ルートポート（RP）
- 指定ポート（DP）
- ブロッキングポート（BP）

## 4　STPの動作詳細
### 4.1　STPステート遷移（Blocking → Listening → Learning → Forwarding）
### 4.2　BPDUの交換とタイマー（Hello Time, Forward Delay, Max Age）
### 4.3　ルートブリッジ変更時の再収束（Topology Change）

## 5　STPの通信とパケットレベルの詳細
### 5.1　BPDUフォーマットとフィールド
### 5.2　通信のキャプチャ例（Wiresharkによる分析）

## 6　STPの進化とバリエーション
### 6.1　RSTP（Rapid Spanning Tree Protocol）
- STPとの違い
- ポートロール（Alternate, Backupなど）
- 高速収束の仕組み
### 6.2　MSTP（Multiple Spanning Tree Protocol）
- VLANグループ化とインスタンス化
- MSTインスタンスの設計と利点

## 7　STPに関する設計と運用の考え方
### 7.1　ルートブリッジの明示的な設定
### 7.2　冗長性設計と収束時間のバランス
### 7.3　再収束時の影響と対策
### 7.4　STP無効化とループ防止策（PortFast, BPDU Guardなど）

## 8　STPに関するセキュリティとリスク
### 8.1　BPDUインジェクション攻撃
### 8.2　ルートブリッジの乗っ取りリスク
### 8.3　BPDU GuardやRoot Guardの実装とその意味

## 9　STPをめぐる実践的なトラブルシューティング
### 9.1　トポロジ変化の原因を追う
### 9.2　収束遅延やループの検出
### 9.3　実際の事例から学ぶSTPの落とし穴

## 10　おわりに：STPの理解がネットワークを強くする
- STPを知ることで得られるネットワーク設計力
- モダンネットワーク（SDN、ファブリックなど）との接続点



## 参考

[1] []()
