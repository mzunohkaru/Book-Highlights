# 目次

- [概要](#概要)
  - [VPC の基本概念](#vpc-の基本概念)
  - [設計時の考慮事項](#設計時の考慮事項)
- [ネットワーク基盤](#ネットワーク基盤)
  - [Route Table](#route-table)
  - [Internet Gateway](#internet-gateway)
  - [NAT Gateway](#nat-gateway)
  - [EIP](#eip)
- [ネットワークインターフェース](#ネットワークインターフェース)
  - [ENI](#eni)
- [セキュリティ](#セキュリティ)
  - [Security Group](#security-group)
  - [Network ACL](#network-acl)
- [VPC 接続](#vpc-接続)
  - [VPC Peering](#vpc-peering)
  - [VPC Endpoint](#vpc-endpoint)
  - [PrivateLink](#privatelink)
- [モニタリング・ログ](#モニタリングログ)
  - [VPC FlowLog](#vpc-flowlog)
- [ロードバランシング](#ロードバランシング)
  - [ELB（ロードバランサー）](#elbロードバランサー)
  - [ALB（Application Load Balancer）](#albapplication-load-balancer)
  - [NLB（Network Load Balancer）](#nlbnetwork-load-balancer)
  - [リスナー](#リスナー)
  - [ターゲット](#ターゲット)
  - [ターゲットグループ](#ターゲットグループ)
  - [スティッキーセッション](#スティッキーセッション)
  - [HTTPS リダイレクト](#https-リダイレクト)
- [コンテンツ配信](#コンテンツ配信)
  - [CloudFront ディストリビューション](#cloudfront-ディストリビューション)
  - [オリジン](#オリジン)
  - [Behavior](#behavior)
  - [エッジロケーション](#エッジロケーション)
  - [OAI](#oai)
  - [OAC](#oac)
- [DNS・ドメイン管理](#dnsドメイン管理)
  - [Route 53, ルーティングポリシー](#route-53-ルーティングポリシー)
  - [DNS レコード 8 種類](#dns-レコード-8-種類)
  - [エイリアスレコード](#エイリアスレコード)
- [ハイブリッド・オンプレ接続](#ハイブリッドオンプレ接続)
  - [Direct Connect](#direct-connect)
  - [Site to Site VPN](#site-to-site-vpn)
  - [Transit Gateway](#transit-gateway)
- [その他のサービス](#その他のサービス)
  - [Global Accelerator](#global-accelerator)
- [用語集](#用語集)
  - [サイジング](#サイジング)
  - [プロトコル](#プロトコル)
  - [SSL](#ssl)
  - [RDP 接続](#rdp-接続)
- [実践的な設計パターン](#実践的な設計パターン)
  - [3 層 Web アプリケーション](#3-層-web-アプリケーション)
  - [マイクロサービス アーキテクチャ](#マイクロサービス-アーキテクチャ)
  - [ハイブリッドクラウド構成](#ハイブリッドクラウド構成)
- [運用ベストプラクティス](#運用ベストプラクティス)
  - [コスト最適化](#コスト最適化)
  - [セキュリティベストプラクティス](#セキュリティベストプラクティス)
  - [災害復旧とバックアップ](#災害復旧とバックアップ)
  - [パフォーマンス最適化](#パフォーマンス最適化)
- [トラブルシューティング手順](#トラブルシューティング手順)
  - [接続問題の診断](#接続問題の診断)
  - [よくある問題と解決方法](#よくある問題と解決方法)
  - [監視とアラート設定](#監視とアラート設定)

## 概要

Amazon VPC（Virtual Private Cloud）は、AWS クラウド内に論理的に分離された仮想ネットワーク環境を提供するサービスです。企業のオンプレミス環境をクラウドに移行する際の基盤となる重要なサービスです。

### VPC の基本概念

- **論理的分離**: 他の AWS アカウントから完全に分離された専用ネットワーク空間
- **IP アドレス範囲**: RFC 1918 準拠のプライベート IP アドレス範囲を使用（10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16）
- **リージョン単位**: VPC は単一の AWS リージョン内に作成され、複数の AZ にまたがることができる
- **最大 5 つの VPC**: デフォルトでは 1 リージョンあたり 5 つまで（上限緩和申請可能）

### 設計時の考慮事項

**CIDR ブロック設計**

```
例: 10.0.0.0/16 (65,536 個の IP アドレス)
├── Public Subnet (AZ-1a): 10.0.1.0/24 (256 個)
├── Private Subnet (AZ-1a): 10.0.2.0/24 (256 個)
├── Public Subnet (AZ-1c): 10.0.3.0/24 (256 個)
└── Private Subnet (AZ-1c): 10.0.4.0/24 (256 個)
```

**マルチ AZ 設計のベストプラクティス**

- 最低 2 つの AZ にリソースを分散配置
- 各 AZ に Public/Private サブネットのペアを作成
- 将来の拡張を考慮した IP アドレス設計

ソース（全てを網羅している）
[https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77)

---

## ネットワーク基盤

### Route Table

![341C69F4-63B3-45EB-A7B0-7C9155922452.png.jpeg](341C69F4-63B3-45EB-A7B0-7C9155922452.png.jpeg)

- VPC 内を流れる通信は、宛先の IP アドレスをルートテーブルと照らし合わせてどこに通信を流すかを決めます。
- 1 つのサブネットには、1 つのルートテーブルしか関連付けることができません。

**ルートテーブルの種類と用途**

1. **Main Route Table（メインルートテーブル）**

   - VPC 作成時に自動生成される
   - 明示的に関連付けされていないサブネットに自動適用
   - 通常はプライベート通信のみ許可

2. **Custom Route Table（カスタムルートテーブル）**
   - 特定の用途に応じて作成
   - Public Subnet 用、Private Subnet 用など

**実践的なルート設定例**

```
# Public Subnet 用ルートテーブル
Destination     Target
10.0.0.0/16    Local          # VPC 内通信
0.0.0.0/0      igw-xxxxxxxx   # インターネット通信

# Private Subnet 用ルートテーブル
Destination     Target
10.0.0.0/16    Local          # VPC 内通信
0.0.0.0/0      nat-xxxxxxxx   # NAT Gateway 経由
192.168.1.0/24 vgw-xxxxxxxx   # VPN Gateway 経由
```

**ルート優先度**

- より具体的（プレフィックス長が長い）ルートが優先される
- 同じ宛先の場合、以下の順序で優先: Local > Static > Propagated

**トラブルシューティング**

- VPC Flow Logs でトラフィックパスを確認
- Route Table の Propagated routes をチェック
- Security Group と NACL の設定も併せて確認

### Internet Gateway

- VPC とインターネットとの間の通信を可能にするためのもの

### NAT Gateway

![781AAE8A-75EF-46F2-9F25-5E9B2F5E7ECB.png.jpeg](781AAE8A-75EF-46F2-9F25-5E9B2F5E7ECB.png.jpeg)

- プライベートサブネット内のインスタンスが VPC 外部ネットワークへ接続するために必要なサービス

**NAT Gateway の特徴**

- **高可用性**: AWS 管理サービスで自動的な冗長化
- **パフォーマンス**: 最大 45 Gbps の帯域幅
- **料金**: 時間課金 + データ処理料金（$0.045/GB）

**NAT Gateway vs NAT Instance**

| 項目                 | NAT Gateway          | NAT Instance             |
| -------------------- | -------------------- | ------------------------ |
| 可用性               | AWS 管理（高可用性） | 手動管理が必要           |
| 帯域幅               | 最大 45 Gbps         | インスタンスタイプに依存 |
| メンテナンス         | 不要                 | OS パッチ等が必要        |
| セキュリティグループ | 不要                 | 設定が必要               |
| 料金                 | 高め                 | 低め（EC2 料金のみ）     |

**冗長化構成のベストプラクティス**

```
# マルチ AZ での NAT Gateway 配置
AZ-1a: NAT Gateway (Public Subnet) → Private Subnet (Route Table A)
AZ-1c: NAT Gateway (Public Subnet) → Private Subnet (Route Table B)
```

**コスト最適化**

- 開発環境では単一 NAT Gateway で構成
- 本番環境では各 AZ に NAT Gateway を配置
- NAT Instance は EC2 料金のみでコスト削減可能（管理コストとのトレードオフ）

### EIP

- インターネットからアクセス可能なパブリックな静的な IPv4 アドレス
- 時間が経つと変更されているようなことはありません

---

## ネットワークインターフェース

### ENI

- 仮想プライベートクラウド（VPC）内のリソースがネットワークに接続するための仮想的なネットワークアダプタ
- EC2 インスタンスを起動すると、自動的にプライマリネットワークインターフェイスが作成されます

**主な特徴**

1. **プライマリ IP アドレス**:

   - 各ネットワークインターフェイスには、VPC 内のサブネットから割り当てられたプライマリ IP アドレスがあります。

2. **セカンダリ IP アドレス**:

   - 必要に応じて、複数のセカンダリ IP アドレスを割り当てることができます。

3. **MAC アドレス**:

   - 各ネットワークインターフェイスには一意の MAC アドレスが割り当てられます。

4. **セキュリティグループ**:

   - ネットワークインターフェイスには、セキュリティグループを関連付けることができます。これにより、インバウンドおよびアウトバウンドのトラフィックを制御します。

5. **アタッチメント**:
   - ネットワークインターフェイスは、EC2 インスタンスなどのリソースにアタッチ（接続）することができます。1 つのインスタンスに複数のネットワークインターフェイスをアタッチすることも可能です。

**利用シナリオ**

- **高可用性と冗長性**:

  - 複数のネットワークインターフェイスを使用して、異なるサブネットやアベイラビリティゾーンにまたがる冗長構成を作成できます。

- **異なるセキュリティグループの適用**:

  - 異なるネットワークインターフェイスに異なるセキュリティグループを適用することで、同じインスタンスに対して異なるセキュリティポリシーを適用できます。

- **トラフィックの分離**:
  - 異なるネットワークインターフェイスを使用して、異なる種類のトラフィック（例：管理トラフィックとデータトラフィック）を分離することができます。

**参考資料**

- [YouTube プレイリスト](https://www.youtube.com/playlist?list=PL2nCE2iR-lpmb9AR1ka1hq6WWORZ_crp2)

---

## セキュリティ

### Security Group

- EC2 インスタンスなどに適用するファイアウォール機能

**Security Group の特徴**

- **ステートフル**: 戻りトラフィックは自動的に許可
- **ALLOW ルールのみ**: 明示的な DENY ルールは設定不可
- **インスタンスレベル**: EC2、RDS、ELB などに直接アタッチ
- **変更の即座反映**: ルール変更は即座に有効化

**実践的な設定例**

```bash
# Web Server 用 Security Group
Inbound Rules:
- HTTP (80)    Source: 0.0.0.0/0
- HTTPS (443)  Source: 0.0.0.0/0
- SSH (22)     Source: Management SG

# Database 用 Security Group
Inbound Rules:
- MySQL (3306) Source: Web Server SG
- MySQL (3306) Source: Management SG

# Management 用 Security Group
Inbound Rules:
- SSH (22)     Source: Corporate IP Range
- RDP (3389)   Source: Corporate IP Range
```

### Network ACL

![EE7CC3D9-7A64-4E02-9BCC-58913DEAA220.png.jpeg](EE7CC3D9-7A64-4E02-9BCC-58913DEAA220.png.jpeg)

- サブネット単位で設定するファイアウォール機能
- どちらも VPC 内のネットワーク転送を細かく制御する

**Network ACL の特徴**

- **ステートレス**: 戻りトラフィックも明示的に許可が必要
- **ALLOW/DENY ルール**: 明示的な拒否ルールが設定可能
- **サブネットレベル**: サブネット内全リソースに適用
- **ルール番号順**: 小さい番号から順に評価

**Security Group vs Network ACL 比較表**

| 項目             | Security Group | Network ACL    |
| ---------------- | -------------- | -------------- |
| 適用レベル       | インスタンス   | サブネット     |
| ステート         | ステートフル   | ステートレス   |
| ルールタイプ     | ALLOW のみ     | ALLOW/DENY     |
| 評価順序         | 全ルール評価   | 番号順に評価   |
| 戻りトラフィック | 自動許可       | 明示的設定必要 |

**設定方針**

- **基本方針**: セキュリティグループで細かい制御、NACL は補完的に使用
- **多層防御**: Security Group（許可）+ NACL（拒否）の組み合わせ
- **コンプライアンス**: 厳格な要件がある場合は NACL で明示的拒否
- **トラブルシューティング**: エフェメラルポート（1024-65535）の考慮が重要

**実践的な NACL 設定例**

```bash
# Public Subnet NACL
Inbound Rules:
100  HTTP (80)     0.0.0.0/0      ALLOW
110  HTTPS (443)   0.0.0.0/0      ALLOW
120  SSH (22)      Corporate IP   ALLOW
130  Ephemeral     0.0.0.0/0      ALLOW  # 戻りトラフィック用
*    ALL           0.0.0.0/0      DENY

Outbound Rules:
100  ALL           0.0.0.0/0      ALLOW
```

**参考資料**

- [Security Group と Network ACL の違い](https://ex-ture.com/blog/2021/07/14/securitygroup-and-networkacl/)
- [設定ベストプラクティス](https://qiita.com/tireidev/items/aa67e2829ff64d6aeb74)

---

## VPC 接続

### VPC Peering

![C6A4714E-7CE1-4BB2-9988-5ACBF2590F20.png.jpeg](C6A4714E-7CE1-4BB2-9988-5ACBF2590F20.png.jpeg)

- 2 つの VPC 間で相互に通信できます

**参考資料**

- [AWS 公式ドキュメント](https://docs.aws.amazon.com/ja_jp/vpc/latest/userguide/vpc-peering.html)

### VPC Endpoint

![AF0A0E6F-2725-40FA-8898-42615A218144.png.jpeg](AF0A0E6F-2725-40FA-8898-42615A218144.png.jpeg)

- VPC 内のインスタンスと VPC 外のサービスをプライベート接続で通信できる

#### インターフェイスエンドポイント

#### ゲートウェイエンドポイント

![E75FF9E2-81C1-4DF7-A8AC-482E049FAF27.png.jpeg](E75FF9E2-81C1-4DF7-A8AC-482E049FAF27.png.jpeg)

**参考資料**

- [VPC Endpoint 詳細解説](https://qiita.com/miyuki_samitani/items/9d9f7a0c417cb75a6c85)

### PrivateLink

![0768D28A-5C47-49A5-BC91-77D352F2D067.png.jpeg](0768D28A-5C47-49A5-BC91-77D352F2D067.png.jpeg)

- VPC 及び AWS サービス、オンプレのプライベート接続をインターネットに公開することなく提供できる
- VPC peering は双方向の通信が可能にするもので、PrivateLink はある VPC から別の VPC へ通信を行うための一方向の通信を行うもの

**参考資料**

- [AWS PrivateLink 公式ページ](https://aws.amazon.com/jp/privatelink/)

---

## モニタリング・ログ

### VPC FlowLog

- VPC のネットワークインターフェイスとの間で行き来する IP トラフィックに関する情報をキャプチャできる

**VPC Flow Logs の活用**

**キャプチャ対象**

- VPC レベル: VPC 内の全 ENI
- Subnet レベル: サブネット内の全 ENI
- ENI レベル: 特定の ENI のみ

**ログフォーマット（バージョン 2）**

```
<version> <account-id> <interface-id> <srcaddr> <dstaddr> <srcport> <dstport> <protocol> <packets> <bytes> <windowstart> <windowend> <action> <flowlogstatus>
```

**実践的な活用例**

1. **セキュリティ分析**

   ```bash
   # 拒否されたトラフィックの分析
   SELECT srcaddr, dstaddr, srcport, dstport, protocol, action
   FROM flowlogs
   WHERE action = 'REJECT'
   GROUP BY srcaddr, dstaddr, srcport, dstport, protocol
   ```

2. **トラフィック分析**
   ```bash
   # 上位の通信先分析
   SELECT dstaddr, SUM(bytes) as total_bytes
   FROM flowlogs
   WHERE action = 'ACCEPT'
   GROUP BY dstaddr
   ORDER BY total_bytes DESC
   ```

**コスト最適化**

- S3 への保存: 長期保存でコスト削減
- CloudWatch Logs: リアルタイム分析用
- カスタムフォーマット: 必要なフィールドのみ記録

**トラブルシューティング手順**

1. Flow Logs で通信可否を確認
2. Security Group のルールをチェック
3. NACL のルールをチェック
4. Route Table の設定を確認

**参考資料**

- [AWS 公式ドキュメント](https://docs.aws.amazon.com/ja_jp/vpc/latest/userguide/flow-logs.html)

---

## ロードバランシング

### ELB（ロードバランサー）

- 外部からのリクエストを複数のサーバーに振り分け負荷分散する

**ELB の種類と選択指針**

| 項目         | ALB                  | NLB              | GLB                |
| ------------ | -------------------- | ---------------- | ------------------ |
| OSI レイヤー | L7 (HTTP/HTTPS)      | L4 (TCP/UDP)     | L3/L4              |
| 用途         | Web アプリケーション | 高パフォーマンス | ファイアウォール等 |
| 静的 IP      | ❌                   | ✅               | ✅                 |
| SSL 終端     | ✅                   | ✅               | ❌                 |

### ALB（Application Load Balancer）

- HTTP・HTTPS プロトコルのレイヤー７（アプリケーション層）に対応する単一ロードバランサー

**ALB の高度な機能**

1. **パスベースルーティング**

   ```
   example.com/api/*     → API Server Target Group
   example.com/images/*  → Static Content Target Group
   example.com/*         → Web Server Target Group
   ```

2. **ホストベースルーティング**

   ```
   api.example.com       → API Target Group
   www.example.com       → Web Target Group
   admin.example.com     → Admin Target Group
   ```

3. **ヘッダベースルーティング**
   - User-Agent に基づくモバイル/デスクトップ振り分け
   - API バージョン管理

**実践的な設定例**

```bash
# Blue/Green デプロイメント
Blue Target Group (Weight: 90)
Green Target Group (Weight: 10)

# カナリアリリース
Production Target Group (Weight: 95)
Canary Target Group (Weight: 5)
```

### NLB（Network Load Balancer）

- TCP や UDP のレイヤー 4（トランスポート層）で動作して、リクエストの内容に基づいてターゲットにルーティングします

**NLB の特徴と利点**

- **超高性能**: 数百万 RPS を処理可能
- **静的 IP**: 各 AZ で固定 IP アドレス
- **低レイテンシ**: マイクロ秒オーダーの低遅延
- **プリザーブ**: クライアント IP の保持

**NLB の使用ケース**

1. **ゲームサーバー**: 低レイテンシが要求される
2. **IoT デバイス**: TCP/UDP 通信
3. **レガシーアプリ**: HTTP 以外のプロトコル
4. **Firewall Integration**: 静的 IP が必要

**接続ドレイン設定**

```bash
# ALB: Connection Draining (300秒デフォルト)
# 既存接続を維持しながら新規接続を停止

# NLB: Deregistration Delay (300秒デフォルト)
# TCP 接続の優雅な終了
```

**参考資料**

- [ELB 詳細解説](https://cloudnavi.nhn-techorus.com/archives/3640)
- [ELB 実践ガイド](https://dev.classmethod.jp/articles/elb-explanation-try/)

### リスナー

![E5F22332-5B3F-4D57-83DE-6D07CED782CA.jpeg](E5F22332-5B3F-4D57-83DE-6D07CED782CA.jpeg)

- 外部からアクセスするプロトコルやポートの設定

### ターゲット

![850FCDB6-8C17-4B87-9425-9319A7224E84.jpeg](850FCDB6-8C17-4B87-9425-9319A7224E84.jpeg)

- 接続先のこと
- 接続先は、ターゲットグループでターゲットの種類として設定した、インスタンス・IP・Lambda のどれかに限られる

### ターゲットグループ

![04DC9BCC-EF57-4C63-9798-A5FDE5C66E02.jpeg](04DC9BCC-EF57-4C63-9798-A5FDE5C66E02.jpeg)

- ロードバランシングする対象群のこと
- ターゲットに対してどのようにリクエストを振り分けるかというルーティングの設定可能

**参考資料**

- [ALB 概念解説](https://www.kanzennirikaisita.com/posts/aws-alb-concepts/)

### スティッキーセッション

- セッションが続いている間は同じクライアントを同じサーバへ誘導する機能

**参考資料**

- [スティッキーセッション詳細](https://tech.uzabase.com/entry/2022/09/30/115417)

### HTTPS リダイレクト

- HTTP → HTTPS のリダイレクト

**参考資料**

- [AWS 公式ガイド](https://repost.aws/ja/knowledge-center/elb-redirect-http-to-https-using-alb)

---

## コンテンツ配信

### CloudFront ディストリビューション

![FAE4308A-3AC1-45E6-A888-246B0656ACCE.png.jpeg](FAE4308A-3AC1-45E6-A888-246B0656ACCE.png.jpeg)

- ユーザーへのウェブコンテンツの配信を高速化するサービス

**参考資料**

- [AWS 公式ドキュメント](https://docs.aws.amazon.com/ja_jp/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)

### オリジン

- CloudFront がエッジロケーションを通じて配信するコンテンツのリクエストを送信するロケーション

### Behavior

- CloudFront でコンテンツをキャッシュする方法を制御するルールを設定し、キャッシュの有効性を判断できる

**参考資料**

- [Origins and Behaviors](https://docs.aws.amazon.com/ja_jp/whitepapers/latest/best-practices-wordpress/origins-and-behaviors.html)

### エッジロケーション

- ユーザーのデータをユーザーの最寄りのエッジロケーションにコピーをキャッシュしておきます

**参考資料**

- [エッジロケーション詳細](https://www.sunnycloud.jp/column/20221013-01/)

### OAI

- ユーザーは S3 バケットから直接ではなく、CloudFront 経由でのみファイルにアクセスできるようにするための特別なユーザー

**参考資料**

- [OAI 詳細解説](https://zenn.dev/sheero/articles/b20feaf7808178)

### OAC

- CloudFront + S3 の構成を新しく作成する場合には、Origin Access Control (OAC)を使用すると良さそう
- セキュリティ面の強化や、今後のことも考えると OAI から OAC に移行した方が良い

**参考資料**

- [OAI から OAC への移行](https://blog.serverworks.co.jp/cloudfront-oai-to-oac)

---

## DNS・ドメイン管理

### Route 53, ルーティングポリシー

![D1538C61-0148-4C1F-9712-5B0EE036D937.jpeg](D1538C61-0148-4C1F-9712-5B0EE036D937.jpeg)

- ドメイン登録機能 + DNS ルーティング + DNS ヘルスチェック
- ルーティングする。例、中国からのリクエストを中国のリージョンのリソース（S3 や ALB）に振り分けることが可能

**参考資料**

- [Route 53 詳細ガイド](https://www.sunnycloud.jp/column/20210602-01/)
- [YouTube プレイリスト](https://www.youtube.com/playlist?list=PL2nCE2iR-lpmNwujBZMi9cxPf2GVtYnEN)

### DNS レコード 8 種類

![90FED434-52A3-47E9-AC34-853F4670A0F8.png.jpeg](90FED434-52A3-47E9-AC34-853F4670A0F8.png.jpeg)

**参考資料**

- [DNS レコード詳細](https://maildata.jp/blog/blog-2023-02-01.html)

### エイリアスレコード

![6BB28DA7-2D32-4B3B-9FFA-9FF33AEBFF03.png.jpeg](6BB28DA7-2D32-4B3B-9FFA-9FF33AEBFF03.png.jpeg)

- AWS サービスの DNS 名をドメイン名に紐付け、直接 IP アドレスを応答してくれる

**参考資料**

- [エイリアスレコード詳細](https://blog.serverworks.co.jp/route53-zone-apex-cname-alias-record)

---

## ハイブリッド・オンプレ接続

### Direct Connect

**AWS Direct Connect の特徴**

- **専用線接続**: インターネットを経由しない安定した接続
- **帯域幅**: 1Gbps, 10Gbps, 100Gbps の専用帯域
- **低レイテンシ**: 一貫した高品質な接続
- **コスト削減**: 大容量データ転送時のコスト効率

**接続オプション**

1. **Dedicated Connection（専用接続）**

   - 1Gbps, 10Gbps, 100Gbps
   - 単一顧客専用の物理ポート

2. **Hosted Connection（ホスト型接続）**
   - 50Mbps ～ 10Gbps
   - パートナー経由での共有接続

**Virtual Interface (VIF) の種類**

```bash
# Public VIF
AWS Public Services (S3, DynamoDB, etc.) への接続

# Private VIF
Single VPC への接続

# Transit VIF
Transit Gateway 経由で複数 VPC への接続
```

### Site to Site VPN

**AWS Site-to-Site VPN の構成要素**

1. **Customer Gateway**: オンプレミス側の VPN デバイス
2. **Virtual Private Gateway**: AWS 側の VPN ゲートウェイ
3. **VPN Connection**: 2 つの IPSec トンネル

**冗長化構成**

```bash
# 推奨構成（冗長化）
Customer Gateway A ←→ VPN Tunnel 1 ←→ VGW
Customer Gateway B ←→ VPN Tunnel 2 ←→ VGW

# 各トンネルは異なる AZ に配置
Tunnel 1: AZ-1a
Tunnel 2: AZ-1c
```

**パフォーマンス特性**

- **帯域幅**: 最大 1.25 Gbps（トンネルあたり）
- **レイテンシ**: インターネット経由のため変動あり
- **料金**: 時間課金 + データ転送料金

### Transit Gateway

**AWS Transit Gateway の利点**

- **中央集約**: 複数 VPC、オンプレミスの接続を一元管理
- **スケーラビリティ**: 最大 5,000 の接続をサポート
- **ルーティング制御**: 柔軟なルーティングポリシー

**典型的な構成**

```bash
# Hub-Spoke モデル
Transit Gateway (Hub)
├── Production VPC
├── Development VPC
├── Shared Services VPC
└── On-premises (VPN/DX)

# セグメンテーション
Production Route Table: Prod VPC ↔ Shared Services VPC
Development Route Table: Dev VPC ↔ Shared Services VPC
```

**コスト最適化**

- **データ処理料金**: $0.02/GB
- **接続時間料金**: $0.05/時間（接続ごと）
- **クロスリージョン**: 追加料金が発生

**ベストプラクティス**

1. **Route Table 分離**: 環境ごとにルートテーブルを分離
2. **セキュリティ**: Security Group で通信制御
3. **監視**: VPC Flow Logs で通信パターンを監視

**参考資料**

- [Transit Gateway 解説動画](https://www.youtube.com/watch?v=mEtluVrgXlk)

---

## その他のサービス

### Global Accelerator

- Cloudfront と似たようなサービス

**比較**

| 項目               | CloudFront          | Global Accelerator                         |
| ------------------ | ------------------- | ------------------------------------------ |
| プロトコル         | HTTP/HTTPS のみ対応 | TCP/UDP のトラフィック・ルーティングに対応 |
| キャッシュ         | 使う                | 使わない                                   |
| クライアント証明書 | 利用しない          | 利用する                                   |

**参考資料**

- [Global Accelerator 詳細比較](https://qiita.com/miyuki_samitani/items/9a320888a833b4aed08c)

---

## 用語集

### サイジング

- 運用するシステムやサービスの規模にあったリソースを用意する

### プロトコル

- データのフォーマットと処理の一連のルール（コンピュータの共通言語）

### SSL

- データ転送を認証、暗号化するプロトコル

### RDP 接続

- リモートデスクトップを実現するための通信プロトコル

---

## 実践的な設計パターン

### 3 層 Web アプリケーション

**典型的な VPC 構成**

```bash
VPC: 10.0.0.0/16

# Public Subnets (Web Tier)
AZ-1a: 10.0.1.0/24 - ALB, NAT Gateway
AZ-1c: 10.0.3.0/24 - ALB, NAT Gateway

# Private Subnets (App Tier)
AZ-1a: 10.0.2.0/24 - EC2 (Web Servers)
AZ-1c: 10.0.4.0/24 - EC2 (Web Servers)

# Database Subnets (DB Tier)
AZ-1a: 10.0.5.0/24 - RDS Primary
AZ-1c: 10.0.6.0/24 - RDS Standby
```

### マイクロサービス アーキテクチャ

**Service Mesh 対応 VPC**

```bash
# Shared Services VPC
Transit Gateway Hub
├── ALB/NLB (Service Discovery)
├── ECS/EKS Clusters
└── Service Mesh (Istio/App Mesh)

# Individual Service VPCs
├── User Service VPC
├── Payment Service VPC
├── Inventory Service VPC
└── Notification Service VPC
```

### ハイブリッドクラウド構成

**企業ネットワーク統合**

```bash
# On-premises: 192.168.0.0/16
# ↓ Direct Connect/VPN
# Transit Gateway
# ├── Production VPC: 10.0.0.0/16
# ├── Development VPC: 10.1.0.0/16
# ├── Management VPC: 10.2.0.0/16
# └── DMZ VPC: 10.3.0.0/16
```

---

## 運用ベストプラクティス

### コスト最適化

1. **NAT Gateway の最適化**

   - 開発環境: 単一 NAT Gateway
   - 本番環境: AZ ごとに配置

2. **VPC Endpoint の活用**

   - S3, DynamoDB: Gateway Endpoint（無料）
   - その他: Interface Endpoint（必要な場合のみ）

3. **Data Transfer 料金の削減**
   - 同一 AZ 内でのデータ転送を優先
   - CloudFront の活用

### セキュリティベストプラクティス

1. **最小権限の原則**

   - Security Group: 必要最小限のポート開放
   - NACL: 明示的な拒否ルール

2. **ネットワーク分離**

   - 環境ごとの VPC 分離
   - サービスごとのサブネット分離

3. **監査とモニタリング**
   - VPC Flow Logs の有効化
   - GuardDuty, Inspector の導入
   - CloudTrail での API 操作記録

### 災害復旧とバックアップ

1. **マルチ AZ 構成**

   - 最低 2 つの AZ にリソース配置
   - AZ 障害時の自動切り替え

2. **クロスリージョン レプリケーション**
   - RDS Cross-Region Automated Backups
   - S3 Cross-Region Replication
   - AMI の定期的なコピー

### パフォーマンス最適化

1. **配置グループ (Placement Groups)**

   - Cluster: 低レイテンシが必要な場合
   - Spread: 高可用性が必要な場合
   - Partition: 分散処理が必要な場合

2. **Enhanced Networking**

   - SR-IOV の有効化
   - 適切なインスタンスタイプの選択

3. **接続性の最適化**
   - Direct Connect: 安定した大容量通信
   - VPC Peering: 低レイテンシ通信
   - Transit Gateway: 複雑なネットワーク統合

---

## トラブルシューティング手順

### 接続問題の診断

**診断フローチャート**

```bash
1. インスタンスに接続できない
   ↓
2. Security Group の確認
   - 必要なポートが開放されているか
   - ソースIP/Security Groupが正しいか
   ↓
3. NACL の確認
   - Inbound/Outbound ルールの確認
   - エフェメラルポートの許可
   ↓
4. Route Table の確認
   - 宛先への適切なルートが存在するか
   - Internet Gateway / NAT Gateway の設定
   ↓
5. VPC Flow Logs の確認
   - トラフィックが実際に流れているか
   - ACCEPT/REJECT の状況確認
```

### よくある問題と解決方法

1. **Internet Gateway 関連**

   ```bash
   問題: Public Subnet のインスタンスにインターネットからアクセスできない
   解決: Route Table に 0.0.0.0/0 → IGW のルートを追加
   ```

2. **NAT Gateway 関連**

   ```bash
   問題: Private Subnet からインターネットにアクセスできない
   解決: Route Table に 0.0.0.0/0 → NAT Gateway のルートを追加
   ```

3. **VPC Peering 関連**
   ```bash
   問題: Peering 接続しているのに通信できない
   解決: 両方の VPC の Route Table にピアリング用ルートを追加
   ```

### 監視とアラート設定

**CloudWatch メトリクス**

- **NAT Gateway**: バイト数、パケット数、エラー数
- **VPN**: トンネル状態、バイト数
- **Transit Gateway**: バイト数、パケット数

**推奨アラート**

```bash
# NAT Gateway 使用量監視
NAT Gateway Data Processing > 100GB/日

# VPN トンネル状態監視
VPN Tunnel State = DOWN

# Security Group 変更監視
CloudTrail: AuthorizeSecurityGroupIngress
```
