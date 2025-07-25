---
weight: 4
title: "攻撃手法をまとめる"
date: 2025-04-13T00:00:00+09:00
lastmod: 2025-04-13T00:00:00+09:00
draft: true
author: "しぶや"
authorLink: "https://github.com/shibuya-s-eg"
description: "攻撃手法をまとめる"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"

tags: ["Security"]
categories: ["Understand-Everything"]
subcategories: ["Security"]

lightgallery: true
---

<!--
Todo:
- TLDR
* サイドチャネル攻撃
    * タイミング攻撃
    * 故障利用攻撃
    * 電力解析攻撃
    * 破壊攻撃

* AIへの攻撃

* IoT
    * タイポスワっティング
    * ホモグラフ攻撃
    * 好き民具
    * 電磁シールド
    * ステガノグラフィ
    * テンペスト攻撃
    * wi-peep

* Webセキュリティ
    * 攻撃
        * SQLインジェクション
        * クロスサイトスクリプティング
        * クロスサイトリクエストフォージェリ
        * HTTPヘッダインジョクション
        * メールヘッダインジェクション
        * ディレクトリトラバーサル
        * サーバーサイドリクエストフォージェリ
        * OSコマンドインジェクション
        * セッションハイジャック
        * マンインサミドル
        * Pass The Hash
        * rat
        * MITM
            * トランザクション署名
        * モバイルコード
        * 水飲み場攻撃
        * emotet
        * monero
        * LOTL
        * ｸｲｯｼﾝｸﾞ

    * 防御
        * CSRFトークン
        * samesite属性
        * EV SSL見分け
        * HSTS
        * Keep-Alive
        * キャッシュコントロール
        * トランザクション認証
            * これって同じ秘密鍵的なものをサーバでも持っている感じ?
        * X XSS Protection
        * Client Security Policy
        * Chache Controle: no chacheとか
        * セッションフィくせーション
        * cookie monster bug
        * domain属性
        * url rewriting
        * ドメインフロンティング
        * 2.8.1
        * content ecurity policy
        * traceメソッド
        * クリックジャッキング, samesite
        * x frame option
        * PUA
        * ゾーニング

* DoS
    * synflood
    * http post flood
    * ボットネット
    * optimisticack, duplicate ack, partial ack: firewallとかの送信順序をどうこうするやつ
    * smurf攻撃
    * メールボム

* NMAP
    * ステルススキャン

* BOF
    * DEP
        * return-to-libc
    * アドレス空間ランダム化
    * PIE
    * 整数オーバーフロー
    * BOFの仕組み
    * stack guard
    * ssp
    * カナリア
    * automatic fotification

* ルートキット




* mtm
    * arpスプーフィング
    * tcp, udp, web, 認証

-->







こんにちは、しぶやです。
今回はを完全に理解していきます。


## TL;DR

*
*
*

## 0　はじめに

## 1　サイバー攻撃基礎

### 1.1　攻撃の目的

### 1.2　分類

### 1.3　ゼロデイ

### 1.4　近年の傾向

## 2　サイドチャネル攻撃

### 2.1　タイミング攻撃
### 2.2　故障利用攻撃
### 2.3　電力解析攻撃
### 2.4　TEMPEST攻撃
### 2.5　破壊攻撃

## 3　認証/認可への攻撃

### 3.1　Man In The Middle攻撃
### 3.2　ARPスプーフィング
### 3.3　Pass the Hash
### 3.4　認証バイパス
### 3.5　セッションハイジャック
### 3.6　URLリライト
### 3.7　ドメインフロンティング


## 4　Webへの攻撃

### 4.1　SQLインジェクション
### 4.2　XSS
### 4.3　CSRF
### 4.4　HTTPヘッダインジェクション
### 4.5　OSコマンドインジェクション
### 4.6　SSRF
### 4.7　クリックジャッキング
### 4.8　Traceメソッドの悪用
### 4.9　TypoSaqatting
### 4.10　ホモグラフ

## 5　DoS/DDoS

### 5.1　SYN Flood
### 5.2　HTTP POST Flood
### 5.3　Smurf攻撃
### 5.4　メールボム
### 5.5　Slowloris/RUDY
### 5.6　Optimistic ACK

## 6　バッファオーバーフロ

### 6.1　バッファオーバーフロー

## 7　マルウェア

### 7.1　コンピュータウイルス
### 7.2　ワーム
### 7.3　トロイの木馬
### 7.4　ランサムウェア

## 8　スキャン

### 8.1　ポートスキャン

### 8.2　脆弱性スキャン

## 9　メールへの攻撃

## 10　DNSへの攻撃

## 11　AIへの攻撃

### 11.1　データ汚染攻撃
### 11.2　モデル転用
### 11.3　バックドア攻撃
### 11.4　モデルサプライチェーンの悪用
### 11.5　ソフトウェアのバグの悪用
### 11.6　訓練データの逆算
### 11.7　メンバシップ推定攻撃
### 11.8　訓練データの窃取
### 11.9　モデル複製
### 11.10　敵対的サンプルの作成

## 12　その他

### 12.1　マイニング
### 12.2　Rootkit
### 12.3　RAT/バックドア
### 12.4　PUA
### 12.5　LOTL







## 参考

[1] []()
