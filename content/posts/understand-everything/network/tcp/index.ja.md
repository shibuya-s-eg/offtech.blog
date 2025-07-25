---
weight: 4
title: "TCPを完全に理解する"
date: 2025-04-13T00:00:00+09:00
lastmod: 2025-04-13T00:00:00+09:00
draft: true
author: "しぶや"
authorLink: "https://github.com/shibuya-s-eg"
description: "TCPを完全に理解する"
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
- なぜTCPを深く理解する必要があるのか
- TCPとUDPの違い（導入として軽く）

## 2 TCPの基本概念
### 2.1 コネクション指向とは
### 2.2 信頼性、順序性、重複排除
### 2.3 ストリーム通信とセグメントの違い

## 3 通信の流れ
### 3.1 3-wayハンドシェイクの詳細
- SYN, SYN-ACK, ACKのパケット構造とフラグ
- ISN（Initial Sequence Number）の役割
- なぜ3回必要か（セキュリティや信頼性の観点から）
### 3.2 データ転送とウィンドウ制御
### 3.3 4-wayハンドシェイクによる切断処理

## 4 TCPヘッダ構造の完全理解
### 4.1 各フィールドの意味と用途（例：Sequence Number, Acknowledgment Number, Window Size）
### 4.2 フラグ（SYN, ACK, FIN, RST, PSH, URG）の役割
### 4.3 オプションフィールド（MSS, Window Scaling, SACKなど）

## 5 信頼性と順序制御の仕組み
### 5.1 シーケンス番号と確認応答
### 5.2 再送制御（タイムアウト再送、Fast Retransmit）
### 5.3 スライディングウィンドウとフロー制御
### 5.4 Nagleアルゴリズムと遅延ACK

## 6 輻輳制御（Congestion Control）
### 6.1 なぜ必要か（ネットワーク負荷の観点）
### 6.2 アルゴリズムの種類と仕組み
- Slow Start
- Congestion Avoidance
- Fast Retransmit / Fast Recovery
### 6.3 CUBICやBBRなどの現代的アルゴリズムにも触れる

## 7 実際のパケット観察と解析
### 7.1 `tcpdump` や Wireshark を使ったトレース解析例
### 7.2 3-wayハンドシェイクや再送のパケット例
### 7.3 異常系（RST送信、遅延ACK、再送）の観察

## 8 セキュリティの観点から見るTCP
### 8.1 SYN Flood攻撃とSYN Cookie
### 8.2 TCPセッションハイジャック
### 8.3 TCPのセキュリティ強化（TCP-AO、TLSとの関係）

## 9 TCPの設計思想と限界
### 9.1 「信頼性をアプリケーションに任せない」という考え方
### 9.2 TCPが抱える課題（モバイル・低レイテンシ通信・NAT環境など）

## 10 まとめと応用への展望
### 10.1 理解したことの総括
### 10.2 応用分野（Web通信、ファイル転送、SSHなど）
### 10.3 TCPの将来：QUICへのバトンタッチ？

## 補遺（Appendix）
### A.1 RFC一覧（RFC 793, 1122, 1323, 2018, 5681, etc.）
### A.2 用語解説
### A.3 TCP関連ツール紹介



## 参考

[1] []()
