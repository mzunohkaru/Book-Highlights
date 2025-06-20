# 目次

- [概要](#概要)
  - [主要な構成要素](#主要な構成要素)
- [インスタンスタイプの選択指針](#インスタンスタイプの選択指針)
  - [ファミリー別特性](#ファミリー別特性)
  - [サイジングのベストプラクティス](#サイジングのベストプラクティス)
- [踏み台サーバー（Bastion Host）](#踏み台サーバーbastion-host)
  - [セキュリティ設定のベストプラクティス](#セキュリティ設定のベストプラクティス)
  - [Session Manager との比較](#session-manager-との比較)
- [AMI（Amazon Machine Image）](#amiAmazon-machine-image)
  - [カスタム AMI 作成のメリット](#カスタム-ami-作成のメリット)
  - [AMI 作成・管理のベストプラクティス](#ami-作成管理のベストプラクティス)
  - [バージョニング戦略](#バージョニング戦略)
- [Compute Savings Plans](#compute-savings-plans)
  - [Reserved Instances との比較](#reserved-instances-との比較)
  - [選択指針](#選択指針)
- [キャパシティー予約](#キャパシティー予約)
  - [使用場面](#使用場面)
  - [設定例](#設定例)
- [プレイスメントグループ](#プレイスメントグループ)
  - [3 つのタイプと使い分け](#3-つのタイプと使い分け)
- [EC2 Auto Scaling](#ec2-auto-scaling)
  - [スケーリングポリシーの詳細設定](#スケーリングポリシーの詳細設定)
  - [ヘルスチェック設定](#ヘルスチェック設定)
- [起動テンプレート](#起動テンプレート)
  - [User Data の活用](#user-data-の活用)
  - [バージョン管理戦略](#バージョン管理戦略)
- [ターゲットの追跡](#ターゲットの追跡)
  - [カスタムメトリクス活用](#カスタムメトリクス活用)
  - [実用的なターゲット値設定](#実用的なターゲット値設定)
- [Elastic Beanstalk](#elastic-beanstalk)
  - [デプロイ戦略](#デプロイ戦略)
  - [設定ファイル例（.ebextensions）](#設定ファイル例ebextensions)
  - [ウェブサーバー環境](#ウェブサーバー環境)
  - [ワーカー環境](#ワーカー環境)
- [セキュリティ設定](#セキュリティ設定)
  - [セキュリティグループ設計パターン](#セキュリティグループ設計パターン)
  - [IAM ロール設定](#iam-ロール設定)
- [パフォーマンス最適化](#パフォーマンス最適化)
  - [EBS 最適化](#ebs-最適化)
  - [ネットワーク最適化](#ネットワーク最適化)
  - [CPU クレジット管理](#cpu-クレジット管理)
- [コスト最適化戦略](#コスト最適化戦略)
  - [スポットインスタンス活用](#スポットインスタンス活用)
  - [リソース使用量監視](#リソース使用量監視)
- [監視と運用](#監視と運用)
  - [CloudWatch メトリクス設定](#cloudwatch-メトリクス設定)
  - [バックアップ戦略](#バックアップ戦略)
- [トラブルシューティング](#トラブルシューティング)
  - [インスタンス起動失敗](#インスタンス起動失敗)
  - [パフォーマンス問題](#パフォーマンス問題)
  - [ネットワーク接続問題](#ネットワーク接続問題)
  - [ログ分析](#ログ分析)
- [ベストプラクティス総括](#ベストプラクティス総括)
  - [セキュリティ](#セキュリティ)
  - [可用性](#可用性)
  - [パフォーマンス](#パフォーマンス)
  - [コスト効率](#コスト効率)

## 概要

・仮想サーバーを構築できる AWS の中核サービス
・オンデマンドでスケーラブルなコンピューティングリソースを提供
・物理サーバーの購入・管理が不要

**ソース**  
[https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/concepts.html](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/concepts.html)

### 主要な構成要素

- **インスタンス**: 仮想サーバー
- **AMI**: Amazon Machine Image（テンプレート）
- **EBS**: Elastic Block Store（ストレージ）
- **セキュリティグループ**: ファイアウォール機能
- **キーペア**: SSH 認証用の公開鍵・秘密鍵

## インスタンスタイプの選択指針

### ファミリー別特性

- **汎用（t3, m5）**: バランス取れた性能、Web サーバーやアプリケーションサーバー
- **コンピューティング最適化（c5）**: CPU 集約的な処理、計算処理、ゲームサーバー
- **メモリ最適化（r5, x1）**: インメモリデータベース、リアルタイム解析
- **ストレージ最適化（i3, d2）**: NoSQL データベース、データウェアハウス
- **高速ネットワーク（ena）**: HPC、機械学習

### サイジングのベストプラクティス

1. **右寄せ**: 小さいインスタンスから開始して必要に応じてスケールアップ
2. **モニタリング**: CloudWatch で CPU、メモリ、ネットワーク使用率を監視
3. **コスト効率**: t3 インスタンスのバーストクレジットを活用

```bash
# インスタンス情報確認
aws ec2 describe-instances --instance-ids i-1234567890abcdef0

# インスタンスタイプ変更
aws ec2 modify-instance-attribute --instance-id i-1234567890abcdef0 --instance-type "{\"Value\":\"t3.medium\"}"
```

## 踏み台サーバー（Bastion Host）

・踏み台サーバーにログインし、踏み台サーバーから、さらに RDP や SSH などで目的のサーバーにログインする、といったステップで接続を行います

**ソース**  
[https://www.manageengine.jp/products/Password_Manager_Pro/jump-server-configuration.html#:~:text=%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%82%92%E8%B8%8F%E3%81%BF%E5%8F%B0%E3%81%AB%E7%9B%AE%E7%9A%84,%E9%96%A2%E4%BF%82%E3%81%AA%E3%81%8F%E5%88%A9%E7%94%A8%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82](https://www.manageengine.jp/products/Password_Manager_Pro/jump-server-configuration.html#:~:text=%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%82%92%E8%B8%8F%E3%81%BF%E5%8F%B0%E3%81%AB%E7%9B%AE%E7%9A%84,%E9%96%A2%E4%BF%82%E3%81%AA%E3%81%8F%E5%88%A9%E7%94%A8%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82)

### セキュリティ設定のベストプラクティス

1. **SSH キー管理**: 定期的なローテーション、パスフレーズ設定
2. **セキュリティグループ**: 最小権限の原則（必要な IP のみ許可）
3. **ポート制限**: SSH は 22 番以外のカスタムポート使用を検討
4. **ログ監視**: CloudTrail での接続ログ監視

### Session Manager との比較

- **踏み台サーバー**: 従来型、SSH キー管理が必要
- **Session Manager**: AWS ネイティブ、IAM 認証、ログ自動記録

```bash
# Session Manager での接続
aws ssm start-session --target i-1234567890abcdef0

# 踏み台サーバー経由の接続
ssh -i ~/.ssh/bastion-key.pem -A ec2-user@bastion-host
ssh ec2-user@private-instance
```

## AMI（Amazon Machine Image）

・事前に設定済みの仮想マシンイメージを利用することにより、初期設定なしで複数のインスタンスを構築することができる

### カスタム AMI 作成のメリット

1. **設定の標準化**: 組織共通の設定やツールを含んだベースイメージ
2. **起動時間短縮**: アプリケーションやミドルウェアがプリインストール済み
3. **セキュリティ強化**: セキュリティパッチ適用済みのイメージ

### AMI 作成・管理のベストプラクティス

```bash
# インスタンスから AMI 作成
aws ec2 create-image --instance-id i-1234567890abcdef0 --name "MyApp-v1.2.3" --description "Production ready application image"

# AMI の共有設定
aws ec2 modify-image-attribute --image-id ami-12345678 --launch-permission "Add=[{UserId=123456789012}]"

# 古い AMI の削除
aws ec2 deregister-image --image-id ami-old123456
```

### バージョニング戦略

- **命名規則**: `AppName-Environment-Version-Date`
- **自動化**: CI/CD パイプラインでの AMI 作成
- **ライフサイクル管理**: 古いバージョンの自動削除

## Compute Savings Plans

・1 年または 3 年の期間で、一貫したコンピューティング使用量を契約する

**ソース**  
[https://aws.amazon.com/jp/savingsplans/compute-pricing/#:~:text=Savings%20Plans%20%E3%81%AF%E3%80%811%20%E5%B9%B4,%E6%9F%94%E8%BB%9F%E3%81%AA%E6%96%99%E9%87%91%E3%83%A2%E3%83%87%E3%83%AB%E3%81%A7%E3%81%99%E3%80%82](https://aws.amazon.com/jp/savingsplans/compute-pricing/#:~:text=Savings%20Plans%20%E3%81%AF%E3%80%811%20%E5%B9%B4,%E6%9F%94%E8%BB%9F%E3%81%AA%E6%96%99%E9%87%91%E3%83%A2%E3%83%87%E3%83%AB%E3%81%A7%E3%81%99%E3%80%82)

### Reserved Instances との比較

| 項目       | Compute Savings Plans              | Reserved Instances |
| ---------- | ---------------------------------- | ------------------ |
| 柔軟性     | インスタンスファミリー間で移動可能 | 固定的             |
| 割引率     | 最大 66%                           | 最大 75%           |
| 適用範囲   | EC2、Fargate、Lambda               | EC2 のみ           |
| 支払い方式 | 前払い・一部前払い・後払い         | 同様               |

### 選択指針

1. **安定した使用量**: 予測可能なベースライン使用量
2. **コミットメント期間**: 1 年（短期） vs 3 年（長期）
3. **支払い方式**: 前払いで更なる割引

## キャパシティー予約

・EC2 Instance を任意の時間に「起動可能」であるというキャパシティーを予約する権利

**ソース**  
[https://blog.serverworks.co.jp/ec2-capacity-reservations#:~:text=%E3%82%AD%E3%83%A3%E3%83%91%E3%82%B7%E3%83%86%E3%82%A3%E3%83%BC%E4%BA%88%E7%B4%84%E3%81%A8%E3%81%AF%E3%80%81%E8%A9%B2%E5%BD%93%E3%81%99%E3%82%8B%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%97%E3%81%AE,%E3%81%99%E3%82%8B%E6%A8%A9%E5%88%A9%E3%81%AE%E3%81%93%E3%81%A8%E3%81%A7%E3%81%99%E3%80%82](https://blog.serverworks.co.jp/ec2-capacity-reservations#:~:text=%E3%82%AD%E3%83%A3%E3%83%91%E3%82%B7%E3%83%86%E3%82%A3%E3%83%BC%E4%BA%88%E7%B4%84%E3%81%A8%E3%81%AF%E3%80%81%E8%A9%B2%E5%BD%93%E3%81%99%E3%82%8B%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%97%E3%81%AE,%E3%81%99%E3%82%8B%E6%A8%A9%E5%88%A9%E3%81%AE%E3%81%93%E3%81%A8%E3%81%A7%E3%81%99%E3%80%82)

### 使用場面

1. **災害対策**: 障害時の即座な復旧
2. **バッチ処理**: 定期的な大規模処理
3. **コンプライアンス**: 規制要件でのキャパシティ確保

### 設定例

```bash
# キャパシティー予約作成
aws ec2 create-capacity-reservation \
    --instance-type m5.large \
    --instance-platform Linux/UNIX \
    --availability-zone us-west-2a \
    --instance-count 10 \
    --end-date-type unlimited
```

## プレイスメントグループ

![71595A54-7DED-4624-88ED-7F915AC85407.png.jpeg](71595A54-7DED-4624-88ED-7F915AC85407.png.jpeg)

・複数のインスタンスを論理的にグループ化して、パフォーマンスの向上・耐障害性を高める機能

**ソース**  
[https://qiita.com/mzmz\_\_02/items/8651f578601f3a567fa0](https://qiita.com/mzmz__02/items/8651f578601f3a567fa0)

### 3 つのタイプと使い分け

#### 1. Cluster プレイスメントグループ

- **特徴**: 同一 AZ 内の物理的に近い場所に配置
- **用途**: HPC、低レイテンシーが必要なアプリケーション
- **制限**: 単一 AZ のみ、最大 10Gbps ネットワーク

#### 2. Partition プレイスメントグループ

- **特徴**: 異なるハードウェアラックに分散配置
- **用途**: 大規模分散システム（Hadoop、Cassandra、Kafka）
- **利点**: 障害影響の最小化

#### 3. Spread プレイスメントグループ

- **特徴**: インスタンスを異なる物理ハードウェアに分散
- **用途**: 高可用性が重要なアプリケーション
- **制限**: AZ あたり最大 7 インスタンス

```bash
# Cluster プレイスメントグループ作成
aws ec2 create-placement-group --group-name my-cluster --strategy cluster

# プレイスメントグループでインスタンス起動
aws ec2 run-instances --image-id ami-12345678 --count 2 --instance-type c5.xlarge --placement GroupName=my-cluster
```

## EC2 Auto Scaling

・インスタンスに異常が発生したタイミングを検出し、インスタンスを削除して、その代わりに新しいインスタンスを起動する

・必要なときにインスタンスを起動し、不要な場合は削除することで費用を節約

**ソース**  
[https://docs.aws.amazon.com/ja_jp/autoscaling/ec2/userguide/auto-scaling-benefits.html](https://docs.aws.amazon.com/ja_jp/autoscaling/ec2/userguide/auto-scaling-benefits.html)

### スケーリングポリシーの詳細設定

#### 1. ターゲット追跡スケーリング

```json
{
  "TargetValue": 70.0,
  "PredefinedMetricSpecification": {
    "PredefinedMetricType": "ASGAverageCPUUtilization"
  },
  "ScaleOutCooldown": 300,
  "ScaleInCooldown": 300
}
```

#### 2. ステップスケーリング

- **段階的**: 負荷に応じて段階的にスケール
- **高速対応**: 急激な負荷変動に対応

#### 3. 予測スケーリング

- **機械学習**: 過去のパターンから将来の需要を予測
- **事前スケール**: 負荷発生前にインスタンスを準備

### ヘルスチェック設定

```bash
# Auto Scaling グループ作成
aws autoscaling create-auto-scaling-group \
    --auto-scaling-group-name my-asg \
    --launch-template LaunchTemplateName=my-template,Version=1 \
    --min-size 1 \
    --max-size 10 \
    --desired-capacity 2 \
    --vpc-zone-identifier "subnet-12345,subnet-67890" \
    --health-check-type ELB \
    --health-check-grace-period 300
```

## 起動テンプレート

・EC2 インスタンスを起動するときに設定するパラメータ（AMI、インスタンスタイプ、キーペア、セキュリティグループなど）を設定し、テンプレートを定義する

**ソース**  
[https://blog.logical.co.jp/entry/2022/07/08/141000#%E8%B5%B7%E5%8B%95%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%81%A8%E3%81%AF](https://blog.logical.co.jp/entry/2022/07/08/141000#%E8%B5%B7%E5%8B%95%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%81%A8%E3%81%AF)

### User Data の活用

```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd

# アプリケーション設定
echo "<h1>Instance ID: $(curl -s http://169.254.169.254/latest/meta-data/instance-id)</h1>" > /var/www/html/index.html

# CloudWatch Agent インストール
wget https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm
rpm -U ./amazon-cloudwatch-agent.rpm
```

### バージョン管理戦略

1. **デフォルトバージョン**: 本番環境用の安定版
2. **最新バージョン**: 開発・テスト環境用
3. **特定バージョン**: 重要なアプリケーション用

```bash
# 起動テンプレート作成
aws ec2 create-launch-template \
    --launch-template-name my-template \
    --launch-template-data '{
        "ImageId": "ami-12345678",
        "InstanceType": "t3.micro",
        "SecurityGroupIds": ["sg-12345678"],
        "UserData": "IyEvYmluL2Jhc2gKCnl1bSB1cGRhdGUgLXk="
    }'
```

## ターゲットの追跡

・アプリの負荷メトリクスを選択し、ターゲット値を設定する。

設定されたターゲット値を基に、Amazon EC2 Auto Scaling は ASG グループ内の EC2 インスタンスの数を調整

**ソース**  
[https://aws.amazon.com/jp/ec2/autoscaling/faqs/](https://aws.amazon.com/jp/ec2/autoscaling/faqs/)

### カスタムメトリクス活用

```bash
# カスタムメトリクス送信
aws cloudwatch put-metric-data \
    --namespace "MyApp/Performance" \
    --metric-data MetricName=ActiveConnections,Value=150,Unit=Count

# Application Load Balancer メトリクス
aws cloudwatch put-metric-data \
    --namespace "AWS/ApplicationELB" \
    --metric-data MetricName=RequestCountPerTarget,Value=100,Unit=Count
```

### 実用的なターゲット値設定

- **CPU 使用率**: 70-80%（バーストクレジット考慮）
- **ALB リクエスト数**: インスタンスあたり 1000 req/min
- **SQS キュー長**: メッセージあたり 5-10 個

## Elastic Beanstalk

・AWS 上でアプリを動かすために必要なサーバを立てたりネットワークの設定をしたり AutoScalling の設定をしたりデータベースの設定をしたりと複雑な初期設定を自動で行ってくれるサービス

**ソース**  
[https://qiita.com/yShig/items/2120bba6649321623cad](https://qiita.com/yShig/items/2120bba6649321623cad)

### デプロイ戦略

#### 1. All at Once（一括）

- **特徴**: 全インスタンスを同時に更新
- **用途**: 開発・テスト環境
- **ダウンタイム**: あり

#### 2. Rolling（ローリング）

- **特徴**: バッチ単位で段階的に更新
- **用途**: 本番環境（ダウンタイム最小化）
- **ダウンタイム**: なし

#### 3. Rolling with Additional Batch

- **特徴**: 追加インスタンスを起動してローリング
- **用途**: キャパシティ維持が重要な本番環境
- **ダウンタイム**: なし

#### 4. Immutable（イミュータブル）

- **特徴**: 新しい Auto Scaling グループを作成
- **用途**: 重要な本番環境、ロールバック要件
- **ダウンタイム**: なし、高速ロールバック

### 設定ファイル例（.ebextensions）

```yaml
# .ebextensions/01_packages.config
packages:
  yum:
    git: []
    htop: []

# .ebextensions/02_environment.config
option_settings:
  aws:autoscaling:launchconfiguration:
    InstanceType: t3.medium
  aws:elasticbeanstalk:environment:
    EnvironmentType: LoadBalanced
```

### ウェブサーバー環境

**ソース**  
[https://docs.aws.amazon.com/ja_jp/elasticbeanstalk/latest/dg/concepts-webserver.html](https://docs.aws.amazon.com/ja_jp/elasticbeanstalk/latest/dg/concepts-webserver.html)

- **構成**: ALB + Auto Scaling + EC2
- **用途**: Web アプリケーション、API サーバー
- **プラットフォーム**: Java、.NET、PHP、Node.js、Python、Ruby、Go

### ワーカー環境

**ソース**  
[https://docs.aws.amazon.com/ja_jp/elasticbeanstalk/latest/dg/concepts-worker.html](https://docs.aws.amazon.com/ja_jp/elasticbeanstalk/latest/dg/concepts-worker.html)

- **構成**: SQS + Auto Scaling + EC2
- **用途**: バックグラウンド処理、バッチジョブ
- **特徴**: キューベースの処理、自動スケーリング

## セキュリティ設定

### セキュリティグループ設計パターン

#### 1. 階層型セキュリティ

```bash
# Web tier セキュリティグループ
aws ec2 create-security-group --group-name web-tier-sg --description "Web tier security group"
aws ec2 authorize-security-group-ingress --group-id sg-web123 --protocol tcp --port 80 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-id sg-web123 --protocol tcp --port 443 --cidr 0.0.0.0/0

# App tier セキュリティグループ
aws ec2 create-security-group --group-name app-tier-sg --description "App tier security group"
aws ec2 authorize-security-group-ingress --group-id sg-app123 --protocol tcp --port 8080 --source-group sg-web123

# DB tier セキュリティグループ
aws ec2 create-security-group --group-name db-tier-sg --description "Database tier security group"
aws ec2 authorize-security-group-ingress --group-id sg-db123 --protocol tcp --port 3306 --source-group sg-app123
```

### IAM ロール設定

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::my-app-bucket/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

## パフォーマンス最適化

### EBS 最適化

- **EBS 最適化インスタンス**: 専用帯域幅でストレージ性能向上
- **GP3 vs GP2**: GP3 は IOPS とスループットを独立して設定可能
- **プロビジョンド IOPS**: io1/io2 で高 IOPS 要件に対応

### ネットワーク最適化

```bash
# SR-IOV 有効化確認
aws ec2 describe-instance-attribute --instance-id i-1234567890abcdef0 --attribute sriovNetSupport

# Enhanced Networking 有効化
aws ec2 modify-instance-attribute --instance-id i-1234567890abcdef0 --sriov-net-support simple
```

### CPU クレジット管理

```bash
# CPU クレジット残高確認
aws cloudwatch get-metric-statistics \
    --namespace AWS/EC2 \
    --metric-name CPUCreditBalance \
    --dimensions Name=InstanceId,Value=i-1234567890abcdef0 \
    --start-time 2023-01-01T00:00:00Z \
    --end-time 2023-01-02T00:00:00Z \
    --period 3600 \
    --statistics Average
```

## コスト最適化戦略

### スポットインスタンス活用

```bash
# スポット料金履歴確認
aws ec2 describe-spot-price-history \
    --instance-types t3.medium \
    --product-descriptions "Linux/UNIX" \
    --start-time 2023-01-01T00:00:00Z

# スポットインスタンス起動
aws ec2 run-instances \
    --image-id ami-12345678 \
    --count 1 \
    --instance-type t3.medium \
    --instance-market-options MarketType=spot,SpotOptions='{MaxPrice=0.05,SpotInstanceType=one-time}'
```

### リソース使用量監視

- **CloudWatch**: CPU、メモリ、ネットワーク使用率
- **AWS Cost Explorer**: コスト分析とトレンド
- **Trusted Advisor**: コスト最適化の推奨事項

## 監視と運用

### CloudWatch メトリクス設定

```bash
# カスタムメトリクス設定
aws logs create-log-group --log-group-name /aws/ec2/application

# メトリクスフィルター作成
aws logs put-metric-filter \
    --log-group-name /aws/ec2/application \
    --filter-name ErrorCount \
    --filter-pattern "ERROR" \
    --metric-transformations metricName=ApplicationErrors,metricNamespace=MyApp,metricValue=1
```

### バックアップ戦略

```bash
# EBS スナップショット作成
aws ec2 create-snapshot \
    --volume-id vol-1234567890abcdef0 \
    --description "Daily backup $(date)"

# 自動化（cron）
0 2 * * * /usr/local/bin/aws ec2 create-snapshot --volume-id vol-1234567890abcdef0 --description "Daily backup $(date)"
```

## トラブルシューティング

### インスタンス起動失敗

1. **キャパシティ不足**: 別の AZ で再試行
2. **セキュリティグループ**: ルール設定確認
3. **IAM 権限**: インスタンスプロファイル確認
4. **サブネット**: IP アドレス枯渇確認

### パフォーマンス問題

```bash
# システムリソース確認
htop
iostat -x 1
sar -u 1 10

# ネットワーク確認
iftop
netstat -tulpn
ss -tuln
```

### ネットワーク接続問題

```bash
# セキュリティグループ確認
aws ec2 describe-security-groups --group-ids sg-12345678

# ルートテーブル確認
aws ec2 describe-route-tables --filters "Name=association.subnet-id,Values=subnet-12345678"

# NACLs 確認
aws ec2 describe-network-acls --filters "Name=association.subnet-id,Values=subnet-12345678"
```

### ログ分析

```bash
# システムログ
sudo tail -f /var/log/messages
sudo journalctl -f

# アプリケーションログ
tail -f /var/log/httpd/error_log
tail -f /var/log/nginx/error.log
```

## ベストプラクティス総括

### セキュリティ

1. **最小権限**: IAM ロールで必要最小限の権限付与
2. **暗号化**: EBS、EFS の暗号化有効化
3. **パッチ管理**: Systems Manager Patch Manager の活用
4. **ネットワーク分離**: VPC、サブネット、セキュリティグループの適切な設計

### 可用性

1. **マルチ AZ**: 複数の Availability Zone への分散配置
2. **自動復旧**: Auto Scaling による自動復旧設定
3. **ヘルスチェック**: ELB、Auto Scaling のヘルスチェック設定
4. **バックアップ**: 定期的なスナップショット作成

### パフォーマンス

1. **適切なサイジング**: モニタリングに基づくリソース最適化
2. **ストレージ選択**: 用途に応じた EBS タイプ選択
3. **ネットワーク**: Enhanced Networking の活用
4. **キャッシュ**: ElastiCache によるデータベース負荷軽減

### コスト効率

1. **インスタンス選択**: 用途に応じた適切なインスタンスタイプ
2. **予約**: Reserved Instances、Savings Plans の活用
3. **スポット**: 中断可能なワークロードでのスポットインスタンス利用
4. **自動化**: 不要リソースの自動停止・削除
