---
weight: 4
title: "EIGRPを完全に理解する"
date: 2025-04-13T00:00:00+09:00
lastmod: 2025-04-13T00:00:00+09:00
draft: true
author: "しぶや"
authorLink: "https://github.com/shibuya-s-eg"
description: "EIGRPを完全に理解する"
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

## 1 はじめに
- なぜEIGRPを理解することが重要なのか
- OSPFやRIPとの違いと位置づけ
- 記事のゴール（表面的な説明ではなく、設計思想や内部処理まで掘り下げる）

## 2 EIGRPの概要
### 2.1 EIGRPとは
- EIGRPとは（登場の背景と目的）
- IGPとEGPの違いに触れた上での立ち位置
- Cisco独自 → 現在のオープン化（RFC 7868）

## 3 基本用語と概念
### 3.1 ルータID / ネイバー / ネイバーテーブル
### 3.2 トポロジーテーブル / ルーティングテーブル
### 3.3 DUAL（Diffusing Update Algorithm）
### 3.4 メトリクス要素（帯域幅、遅延、信頼性、負荷）

## 4 EIGRPの動作メカニズム
### 4.1 ネイバー形成
- Helloパケットの交換
- ネイバーテーブルの構築
- マルチキャストとUnicastの使い分け

### 4.2 トポロジーテーブルとDUAL
- トポロジーテーブルに保持される情報
- DUALのアルゴリズム詳細
  - Successor / Feasible Successor
  - Feasibility Condition
  - Loop-freeルーティングの保証

### 4.3 ルート再計算とコンバージェンス
- ルートの追加・削除・変更時の挙動
- フルではなく部分的なアップデート
- スタックではなく非同期での収束

## 5 EIGRPメトリクス計算式の詳細
- デフォルトのK値とその意味
- カスタマイズ可能な点と注意点
- バランス調整と設計への影響

## 6 パケット種別と通信の詳細
### 6.1 Hello / Update / Query / Reply / ACK
### 6.2 各パケットのフォーマット・やり取り
### 6.3 Wiresharkによるキャプチャ例と解説

## 7 実装と設定例（Cisco IOSベース）
- EIGRP設定コマンドの例と解説
- ネイバー確認、テーブルの確認方法
- Passive interfaceや認証設定など実用的Tips

## 8 デザイン思想と設計上の工夫
- なぜDUALを使ったのか
- OSPFやRIPと比較した設計意図
- スケーラビリティと冗長性へのアプローチ

## 9 セキュリティと課題
- 認証（MD5やSHA）
- 攻撃手法の例（ネイバー偽装など）
- 対策とベストプラクティス

## 10 まとめと今後の位置づけ
- EIGRPの現在の使われ方
- 他プロトコルとの棲み分け
- 学習リソース・RFCリンク



## 参考

[1] []()
