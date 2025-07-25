---
weight: 4
title: "NATを完全に理解する"
date: 2025-04-13T00:00:00+09:00
lastmod: 2025-04-13T00:00:00+09:00
draft: true
author: "しぶや"
authorLink: "https://github.com/shibuya-s-eg"
description: "NATを完全に理解する"
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


## 1. はじめに
- IPv4アドレス枯渇問題とNATの誕生背景
- プライベートネットワークとグローバルネットワークの分離
- 本記事のゴール：NATを仕組み・動作・背景から完全に理解する

## 2. NATの基本概念
### 2.1 プライベートIPアドレスとグローバルIPアドレス
### 2.2 NATとは何か？
### 2.3 RFC1918とアドレス空間の整理

## 3. NATの種類
### 3.1 Static NAT（1対1変換）
### 3.2 Dynamic NAT（プールからの変換）
### 3.3 PAT / NAPT（ポートアドレス変換）
### 3.4 各方式の比較とユースケース

## 4. NATの仕組みと動作
### 4.1 パケット送信時の変換
### 4.2 パケット受信時の逆変換とセッション維持
### 4.3 NATテーブルの仕組み（変換エントリ、タイムアウト）

## 5. NATと通信プロトコルの関係
### 5.1 TCPとNATの相互作用
### 5.2 UDPとNATの課題
### 5.3 FTP・SIPなどアプリケーション層プロトコルへの影響

## 6. NAT越え技術（NAT Traversal）
### 6.1 STUNの仕組みと動作例
### 6.2 TURNによる中継方式
### 6.3 ICEによるP2P接続最適化
### 6.4 WebRTCとの関係

## 7. NATとセキュリティ
### 7.1 NATが提供する“セキュリティ”の正体
### 7.2 NATとファイアウォールの違いと共存
### 7.3 VPNやP2P通信への影響

## 8. NATとIPv6
### 8.1 IPv6ではなぜNATが不要なのか
### 8.2 NAT66/NPTv6という考え方
### 8.3 NAT64・DNS64とIPv4-IPv6共存環境

## 9. NATの課題と限界
### 9.1 アプリケーション層での問題と対策
### 9.2 多段NAT（ダブルNAT）によるトラブル
### 9.3 NATループバックの必要性と挙動

## 10. パケットレベルでのNAT観察
### 10.1 tcpdump/Wiresharkでの変化の観察
### 10.2 NATテーブルの実例：Linux（conntrack）、Ciscoなど
### 10.3 NAT変換処理のソースコード/実装への入り口

## 11. まとめと考察
### 11.1 NATの本質は「接続制御」である
### 11.2 NATと今後のネットワーク設計
### 11.3 学びを活かすには：構築・可視化・実験



## 参考

[1] []()
