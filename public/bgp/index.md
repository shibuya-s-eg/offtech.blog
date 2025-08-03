# BGPを完全に理解する


<!--
Todo:
- TLDR

-->


こんにちは、しぶやです。
今回はをBGP ( Border Gateway Protocol )を完全に理解していきます。


## TL;DR

*
*
*

## 1　はじめに
###　1.1　BGPとは

BGPとは、Border Gateway Protocolの略で、ビー・ジー・ピーと呼ばれます。

BGPはAS間で経路情報を交換するEGPの一種です。
とはいえ、「そもそもASってなんだよ」な人もいると思います。

ASについては「インターネットの構造を完全に理解する」をおすすめします！

本説では、BGPは**ASと呼ばれる単位のネットワーク同士で経路情報を交換するプロトコル**くらいに認識しておいて、ASについて先に学んでいきましょう。


### 2.2　BGPとIGPとの違い
### 2.3 AS（Autonomous System）の概念

## 3 BGPの基本構造とメッセージ
### 3.1 BGPピアの確立（TCP 179）
### 3.2 メッセージタイプ
- Open / Keepalive / Update / Notification
### 3.3 属性（Attributes）の仕組み
- ORIGIN / AS_PATH / NEXT_HOP / LOCAL_PREF / MED
### 3.4 AS_PATHによるループ防止

## 4 経路選択の仕組み
### 4.1 経路選定ロジックの流れ
### 4.2 BGP Decision Processの詳細
### 4.3 Policyベースルーティングとその利用

## 5 BGPの動作とFSM（状態遷移）
### 5.1 状態遷移の全体像
- Idle → Connect → Active → OpenSent → OpenConfirm → Established
### 5.2 Keepaliveとセッション維持
### 5.3 セッションの障害と回復

## 6 iBGPとeBGPの違いと技術
### 6.1 iBGPとeBGPの使い分け
### 6.2 iBGP Full-meshの問題
### 6.3 Route ReflectorとConfederation
### 6.4 eBGP multi-hopとnext-hop-self

## 7 実践的なBGP設定例
### 7.1 シンプルな設定例（Cisco/Juniper）
### 7.2 Prefix-ListとRoute-Mapによる制御
### 7.3 Redistributionとその注意点

## 8 BGPのセキュリティ
### 8.1 BGPが狙われる理由
### 8.2 Prefix Hijackingの仕組みと事例
### 8.3 RPKIによる経路検証
### 8.4 セッション保護手段（MD5認証など）

## 9 BGPとインターネット運用
### 9.1 トランジットとピアリングの考え方
### 9.2 IX（Internet Exchange）との関係
### 9.3 フィルタリングとマルチホーム設計

## 10 トラブルシューティングと監視
### 10.1 よくある問題とチェックポイント
### 10.2 showコマンドやdebug活用
### 10.3 BGP Monitoring Protocol（BMP）とBGP-LS

## 11 BGPの進化と未来
### 11.1 Segment Routing（SR-BGP）の概要
### 11.2 EVPNとMPLSの関係
### 11.3 SDNとの統合（BGP in SDN）

## 12 まとめと参考文献
### 12.1 本記事の振り返り
### 12.2 関連RFC一覧
### 12.3 検証環境・ツール紹介（GNS3 / EVE-NG / FRR など）




## 参考

[1] []()

