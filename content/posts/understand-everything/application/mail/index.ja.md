---
weight: 4
title: "メールシステムを完全に理解する"
date: 2025-04-13T00:00:00+09:00
lastmod: 2025-04-13T00:00:00+09:00
draft: true
author: "しぶや"
authorLink: "https://github.com/shibuya-s-eg"
description: "メールシステムを完全に理解する"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"

tags: ["Application"]
categories: ["Understand-Everything"]
subcategories: ["Application"]

lightgallery: true
---

<!--
Todo:
- TLDR
* メール
    * PGP
    * M/MIME
    * SMTP Auth
    * SMTP 587
    * APOP
    * POP3
    * IMAPS
    * SMTP Submission
    * OP25B
    * IP25B
    * メールの仕組み・構成
    * フィッシングについて考える
    * mxレコードとaレコードがどう利用されているのか
-->


こんにちは、しぶやです。
今回はメールシステムを完全に理解していきます。


## TL;DR

*
*
*

## 0　はじめに


## 1　メールの仕組み

### 1.1　メールシステムの構成

### 1.2　メール送受信の流れ

    * 中継するサーバは受信者と送信者のもののみなのか。パブリックな中継がある？

## 2　プロトコル

### 2.1　SMTP

### 2.2　POP3

### 2.3　IMAP


## 3　パケット解析


## 4　セキュリティ

### 4.1　メールヘッダ確認


* メールヘッダインジェクション

### 4.2　送信ドメイン認証

* SPF
* DKIM
* DMARC

### 4.3　送信者認証

### 4.4　暗号化

* IMAPS
* POP3S
* STARTTLS
* S/MIME
* PGP

### 4.5　スパム対策

* OP25B
* IP25B


### 4.6　フィッシング


## 5　冗長化と可用性

### 5.1　MXレコードの冗長構成

### 5.2　キューイングとリトライの仕組み

### 5.3　ローカル配送失敗時の処理


## 6　現代メールシステム

### 6.1　大手メールサービスと特徴

### 6.2　スパムフィルターの仕組み

## 7　メールシステムの応用

### 7.1　メールAPI

### 7.2　メール通知

### 7.3　認証

## 8　まとめ


## 参考

[1] []()
