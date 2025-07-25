---
weight: 4
title: "認証/認可を完全に理解する"
date: 2025-04-13T00:00:00+09:00
lastmod: 2025-04-13T00:00:00+09:00
draft: true
author: "しぶや"
authorLink: "https://github.com/shibuya-s-eg"
description: "認証/認可を完全に理解する"
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



* 認証基礎
    * SAML
    * OAuth
    * OpenID
    * Pass The Hash
    * Cookie認証
    * Kerberos認証
    * リバースプロキシ型認証
    * FIDO
        * UAF
        * CATP 1
        * CATP 2
    * CAPTHA
    * ロックアウト
    * リスクベース
    * JWT?
    * CHAPリプレイ攻撃、なりすまし
    * レインボーテーブル、ソルト
    * パスワードスプレー
    * パスワードリスト
    * クレデンシャルスタッフィング攻撃
    * pass tha hash
    * ケルベロス認証
    * s/key
    * パスキー
    * IDaaS
        * idパスワードはネットワークに流さない。
    * reflesh token
    * SCIM
    * OIDC
        * samlにはないがodicにはユーザからの同意を得る手順がある
    * SMS認証
        * 課金
        * 拒否
        * SIMスワップ
    * バイオメトリクス認証
        * 本人拒否率
        * 他人受け入れ率
        * 代替手段が必要
        * 問題点
    * バイオメトリクス認証
        * 本人拒否率
        * 他人受け入れ率
        * マスターフェイス
        * ウルフ攻撃
    * ekyc
        * EKYCのライブネスチェック
        * 犯収法
            * 顧客の指名・住所・生年月日などの「本人特定事項」や「取引を行う目的」
        * ホ方式、ワ方式
        * ディープフェイク
    * ハードウェアトークン
    * JWT
    * CSMS認証
    * 認証局のかい総合像
    * ISO 42001
    * ジョーアカウント

-->

こんにちは、しぶやです。
今回はを完全に理解していきます。


## TL;DR

*
*
*

## 1 はじめに
### 1.1 認証と認可の違い
### 1.2 なぜ認証・認可が重要なのか
### 1.3 本記事の目的と構成

## 2 認証の基礎
### 2.1 認証とは何か
### 2.2 認証の3要素（知識・所持・生体）
### 2.3 パスワード認証とその限界

## 3 認証方式の種類と仕組み
### 3.1 Cookie認証
### 3.2 JWT（JSON Web Token）
### 3.3 S/Key（ワンタイムパスワード）
### 3.4 ハードウェアトークン
### 3.5 生体認証（バイオメトリクス）
#### 3.5.1 本人拒否率・他人受入率
#### 3.5.2 マスターフェイス・ウルフ攻撃
#### 3.5.3 代替手段と課題
### 3.6 SMS認証
#### 3.6.1 課金・拒否・SIMスワップ問題
### 3.7 パスキーとFIDO
#### 3.7.1 FIDO UAF / CTAP 1 / CTAP 2
### 3.8 Kerberos認証
### 3.9 リバースプロキシ型認証
### 3.10 CSMS認証
### 3.11 eKYC（電子本人確認）
#### 3.11.1 ライブネスチェック
#### 3.11.2 犯収法、ホ方式・ワ方式
#### 3.11.3 ディープフェイク対策

## 4 認可プロトコルとID連携
### 4.1 OAuth 2.0
### 4.2 OpenID Connect（OIDC）
#### 4.2.1 SAMLとの違い（ユーザー同意など）
### 4.3 SAML
### 4.4 IDaaS（ID as a Service）
#### 4.4.1 パスワードがネットワークに流れない仕組み
### 4.5 SCIM（ユーザー管理連携）

## 5 トークンとセキュリティ
### 5.1 アクセストークンとリフレッシュトークン
### 5.2 JWTの構造と検証
### 5.3 トークンの盗難対策

## 6 攻撃手法とその対策
### 6.1 パスワードスプレー攻撃
### 6.2 パスワードリスト攻撃
### 6.3 クレデンシャルスタッフィング攻撃
### 6.4 Pass the Hash
### 6.5 CHAPのリプレイ攻撃・なりすまし
### 6.6 レインボーテーブルとソルト

## 7 防御策とベストプラクティス
### 7.1 ロックアウトポリシー
### 7.2 CAPTCHAの役割
### 7.3 リスクベース認証
### 7.4 マルチファクター認証（MFA）

## 8 認証基盤と信頼モデル
### 8.1 認証局（CA）の役割
### 8.2 PKIとその信頼性
### 8.3 ISO 42001と標準化の流れ

## 9 よくある落とし穴と設計の注意点
### 9.1 ジョーアカウント問題
### 9.2 セッション管理の落とし穴
### 9.3 ユーザー同意の扱いとプライバシー

## 10 おわりに
### 10.1 今後の展望
### 10.2 読者へのメッセージ



## 参考

[1] []()
