# Kubernetesを完全に理解する


こんにちは、しぶやです。\
学生時代の研究から始まり、社会人になってからも愛用しているDockerについて、気合をいれてまとめようと思います。

## TL;DR

*
*
*

## 0　はじめに

基本概念
k8s

## 1　アーキテクチャ

### 1.1　Master Node
### 1.2　Worker Node

## 2　主要コンポーネント

### 2.1　container runtime

### 2.2　kube-apiserver

### 2.3　etcd


### 2.4　kube-scheduler

### 2.5　kube-controller-manager

### 2.6　cloud-controller-manager

### 2.7　kubelet
### 2.8　kube-proxy
### 2.9　CoreDNS
### 2.10　CNI plugin

## 3　Kubernetesの仕組み

### 3.1　Pod間通信（CNI、Overlay vs Underlay）

### 3.2　Service（ClusterIP, NodePort, LoadBalancer）

### 3.3　kube-proxyの動作とiptables/ipvs

### 3.4　Control PlaneとNode間の通信ポート

### 3.5　etcdとの通信、TLSと認証の詳細

## 4　Kubernetesセキュリティ

### 4.1　RBACとABAC

### 4.2　etcdの暗号化（EncryptionConfiguration）

### 4.3　API認証方式（Bearer Token, X.509, OpenID Connect）

### 4.4　ネットワークポリシーとその強制

### 4.5　Admission Controller（例: PodSecurity, OPA Gatekeeper）

### 4.6　SecretsとServiceAccountの管理

### 4.7　Node / Pod セキュリティ（PSP, SecurityContext, AppArmor）

## 5　デプロイ戦略

### 5.1　マニフェスト管理（kubectl, Kustomize, Helm）

### 5.2　クラスタ監視（Prometheus, Grafana, kube-state-metrics）

### 5.3　ログ管理（Fluentd / Loki）

### 5.4　アップグレード戦略（Control PlaneとNodeのバージョン差）

### 5.5　高可用構成（HA Control Plane, etcd quorum）

### 5.6　Backup & Disaster Recovery（etcd snapshot、Velero）

## 6　ユースケース



## 7　メリット/デメリット

### 7.1　メリット

柔軟なスケーラビリティ
自動化による効率化
IaC

### 7.2　デメリット

学習コスト
運用コスト
オーバーヘッド

## 8　まとめ

kubernetes thread model
kunernetes atacck tree


## 参考

[1] [docker](https://www.docker.com/)
[2] [コンテナセキュリティ　コンテナ化されたアプリケーションを保護する要素技術 ](https://www.amazon.co.jp/%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3-%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E5%8C%96%E3%81%95%E3%82%8C%E3%81%9F%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%92%E4%BF%9D%E8%AD%B7%E3%81%99%E3%82%8B%E8%A6%81%E7%B4%A0%E6%8A%80%E8%A1%93-Liz-Rice/dp/4295016403)

