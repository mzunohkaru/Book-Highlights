# IAM

## 概要

### IAM（Identity and Access Management）

AWS リソースへのアクセス権限を管理するサービス

**重要概念:**

- **認証（Authentication）**: ユーザーが誰であるかを確認する
- **認可（Authorization）**: 認証されたユーザーが何をできるかを制御する
- **最小権限の原則**: 必要最小限の権限のみを付与する
- **職務分離**: 異なる役割に対して適切な権限を分離する

## 基本概念

### ルートユーザー

アカウントの所有者であり、アカウントに対する全ての操作権限を持っています

**🚨 セキュリティベストプラクティス:**

- ルートユーザーは初期設定以外では使用しない
- MFA（多要素認証）を必ず設定する
- アクセスキーは作成しない
- 定期的にアクティビティをモニタリングする

### IAM ユーザー
![](98C192AE-3E69-4AB8-8D98-8FFFC880E873-1.png.jpeg)


個別の人間またはアプリケーション用のアカウント

**認証方法:**

- **プログラマティックアクセス**: アクセスキー + シークレットキー
- **マネジメントコンソールアクセス**: ユーザー名 + パスワード + MFA

**運用のポイント:**

- 人間のユーザーには MFA を必須とする
- アクセスキーの定期的なローテーション
- 不要になったユーザーの即座な削除
- 命名規則の統一（例: firstname.lastname、service-name-role）

### グループ
![](A354B93B-480A-4D17-9CD3-9658B06E3EC9.png.jpeg)


IAM ユーザーの集まり

**効率的なグループ運用:**

- 職務別グループ（例: Developers, DataScientists, Operations）
- 環境別グループ（例: ProductionAccess, StagingAccess）
- プロジェクト別グループ（例: ProjectA-Team, ProjectB-Team）

### ポリシー
![](033D1AB6-1A88-4A33-B662-A83761ABF1F3.png.jpeg)


#### AWS 管理ポリシー

AWS によって事前に準備がされている IAM ポリシー

**よく使用される管理ポリシー:**

- `PowerUserAccess`: 開発者向け（IAM 管理以外のフルアクセス）
- `ReadOnlyAccess`: 読み取り専用アクセス
- `AmazonS3FullAccess`: S3 への完全アクセス
- `AmazonEC2ReadOnlyAccess`: EC2 への読み取り専用アクセス

#### カスタマー管理ポリシー

利用者によって手動作成を行うことが出来る IAM ポリシー

**ポリシー作成のベストプラクティス:**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::my-bucket/*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "203.0.113.0/24"
        }
      }
    }
  ]
}
```

**ポリシー設計のポイント:**

- 明示的な Deny は Allow より優先される
- 条件（Condition）を活用した細かい制御
- ワイルドカード（\*）の慎重な使用
- リソース ARN の具体的な指定

**参考:** [IAM ポリシーの詳細](https://www.idaten.ne.jp/portal/page/out/secolumn/multicloud/column005.html#:~:text=IAM%E3%83%9D%E3%83%AA%E3%82%B7%E3%83%BC%E3%81%A8%E3%81%AF%E3%80%81%E3%80%8C%E3%81%A9%E3%81%AE,IAM%E3%83%9D%E3%83%AA%E3%82%B7%E3%83%BC%E3%81%A8%E5%91%BC%E3%81%B3%E3%81%BE%E3%81%99%E3%80%82)

#### リソースベースのポリシー

S3 や ECR などのリソースにアタッチします

**リソースベースポリシーの活用例:**

- S3 バケットポリシー: 特定の IP からのアクセス制限
- Lambda リソースポリシー: API Gateway からの呼び出し許可
- SNS トピックポリシー: クロスアカウントアクセス

**参考:** [リソースベースポリシーについて](https://qiita.com/Shoma0210/items/17193f254180396fb8e1)

### ロール
![](FECA6855-1D67-43B8-B749-348D3BB15376.png.jpeg)

![](51AC4E26-D378-42C5-9D5A-9C8D3AC5EEEF.png.jpeg)


AWS サービスにアクセス権限を付与して、その AWS サービスが IAM ユーザのように行動できるようにする

ロールは主に「(EC2 等の) インスタンス」（リソース）に割り当てられるケースが多い

**ロールの主要な使用パターン:**

1. **サービスロール**: EC2、Lambda、ECS などで使用
2. **クロスアカウントロール**: 他の AWS アカウントからのアクセス
3. **Web Identity ロール**: Google、Facebook、Amazon Cognito からの認証
4. **SAML ロール**: オンプレミス AD との連携

**信頼ポリシーの例:**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

## セキュリティベストプラクティス

### MFA（Multi-Factor Authentication）

**推奨設定:**

- 仮想 MFA デバイス（Google Authenticator、Authy）
- ハードウェア MFA デバイス（企業環境）
- SMS は非推奨（SIM スワップ攻撃のリスク）

### アクセスキー管理

**ローテーション戦略:**

```bash
# 古いキーの無効化前に新しいキーでのテスト
aws configure set aws_access_key_id NEW_ACCESS_KEY
aws configure set aws_secret_access_key NEW_SECRET_KEY
aws sts get-caller-identity  # 動作確認
```

### CloudTrail との連携

**監査ログの重要性:**

- すべての API 呼び出しをログ記録
- 不正アクセスの早期発見
- コンプライアンス要件への対応

## 実践的な運用パターン

### 環境別権限管理

```
Production Account
├── Production-FullAccess (運用チーム)
├── Production-ReadOnly (監査・監視)
└── Production-DeployOnly (CI/CD)

Development Account
├── Development-FullAccess (開発チーム)
└── Development-ReadOnly (ステークホルダー)
```

### アプリケーション用ロール設計

```
WebApplication-Role
├── S3 バケットへの読み書き
├── RDS への接続
├── CloudWatch へのログ出力
└── Parameter Store からの設定値取得
```

## トラブルシューティング

### よくあるエラーと対処法

#### AccessDenied エラー

```bash
# 現在の権限を確認
aws sts get-caller-identity
aws iam simulate-principal-policy \
    --policy-source-arn arn:aws:iam::123456789012:user/username \
    --action-names s3:GetObject \
    --resource-arns arn:aws:s3:::bucket-name/object-key
```

#### AssumeRole 失敗

**チェックポイント:**

1. 信頼ポリシーの設定確認
2. 外部 ID の一致確認
3. MFA 要件の確認
4. IP 制限の確認

### IAM Policy Simulator の活用

**デバッグに有効なツール:**

- 権限の事前テスト
- 複雑なポリシーの検証
- アクセス拒否の原因特定

## セキュリティ・認証関連サービス

### AWS WAF

定義する条件に基づきウェブリクエストを許可、ブロック、または監視 (カウント) するルールを設定し、ウェブアプリケーションを攻撃から保護するのを助ける Web アプリケーションファイアウォール

**実装例:**

```json
{
  "Name": "RateLimitRule",
  "Statement": {
    "RateBasedStatement": {
      "Limit": 1000,
      "AggregateKeyType": "IP"
    }
  },
  "Action": {
    "Block": {}
  }
}
```

**参考:** [AWS WAF FAQ](https://aws.amazon.com/jp/waf/faq/#:~:text=AWS%20WAF%20%E3%81%AF%E3%81%8A%E5%AE%A2%E6%A7%98%E3%81%8C,%E5%8A%A9%E3%81%91%E3%82%8B%20Web%20%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB%E3%81%A7%E3%81%99%E3%80%82)

### Amazon Cognito

AWS などに構築した、Web アプリやモバイルアプリに認証・認可機能を提供するサービス

#### ユーザープール

セルフサービスと管理者主導の両方によるユーザー作成、管理、認証を行うユーザーディレクトリ

**設定のポイント:**

- パスワードポリシーの強化
- MFA の強制有効化
- カスタム属性の設計
- トリガー関数による拡張

#### アイデンティティプール

認証されたユーザーまたは匿名ユーザーに AWS リソースへのアクセスを許可する

**実装パターン:**

```javascript
// Cognito Identity Pool からの一時的な認証情報取得
AWS.config.credentials = new AWS.CognitoIdentityCredentials({
  IdentityPoolId: "ap-northeast-1:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  RoleArn: "arn:aws:iam::123456789012:role/Cognito_AppAuth_Role",
});
```

**参考:** [Amazon Cognito ドキュメント](https://docs.aws.amazon.com/ja_jp/cognito/latest/developerguide/what-is-amazon-cognito.html)

### AWS KMS（Key Management Service）

データへのアクセスをコントロールする暗号化キーを一元管理できます

**暗号化の実装例:**

```python
import boto3

kms = boto3.client('kms')

# データの暗号化
response = kms.encrypt(
    KeyId='alias/my-key',
    Plaintext=b'sensitive data'
)
ciphertext = response['CiphertextBlob']

# データの復号化
response = kms.decrypt(CiphertextBlob=ciphertext)
plaintext = response['Plaintext']
```

**キー管理のベストプラクティス:**

- カスタマー管理キーの使用
- キーの定期的なローテーション
- 用途別のキー分割
- 削除保護の有効化

**参考:** [AWS KMS FAQ](https://aws.amazon.com/jp/kms/faqs/#:~:text=AWS%20KMS%20%E3%81%AF%E3%80%81%E6%9A%97%E5%8F%B7%E5%8C%96,%E5%8C%96%E3%82%92%E7%AE%A1%E7%90%86%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82)

### AWS Shield

DDoS 攻撃から保護するマネージド型のサービス

**Shield Standard vs Advanced:**

- **Standard**: 無料、L3/L4 攻撃の基本的な保護
- **Advanced**: 有料、L7 攻撃の保護、24/7 サポート、コスト保護

### IAM Identity Center

従業員 ID を安全に作成または接続し、AWS アカウントとアプリケーション全体の従業員アクセスを一元的に管理する

組織の規模や種類を問わず、AWS で従業員の認証と認可を行うための推奨されるアプローチ

**エンタープライズでの活用:**

- Active Directory との統合
- SAML 2.0 による SSO
- 許可セットによる権限管理
- アカウント間の一元アクセス制御

**参考:** [IAM Identity Center](https://aws.amazon.com/jp/iam/identity-center/#:~:text=AWS%20IAM%20%E3%82%A2%E3%82%A4%E3%83%87%E3%83%B3%E3%83%86%E3%82%A3%E3%83%86%E3%82%A3%E3%82%BB%E3%83%B3%E3%82%BF%E3%83%BC%E3%81%AF,%E6%8E%A8%E5%A5%A8%E3%81%95%E3%82%8C%E3%82%8B%E3%82%A2%E3%83%97%E3%83%AD%E3%83%BC%E3%83%81%E3%81%A7%E3%81%99%E3%80%82)

### AWS Certificate Manager（ACM）

AWS 内で SSL/TLS 証明書の発行や発行した SSL/TLS 証明書の自動更新ができるサービス

**運用のポイント:**

```bash
# ドメイン検証の自動化
aws acm request-certificate \
    --domain-name example.com \
    --subject-alternative-names "*.example.com" \
    --validation-method DNS
```

**証明書管理のベストプラクティス:**

- ワイルドカード証明書の活用
- 自動更新の設定確認
- 証明書の有効期限監視
- CloudFront との連携

**参考:** [AWS Certificate Manager について](https://business.ntt-east.co.jp/content/cloudsolution/column-63.html#:~:text=ACM%E3%81%A8%E3%81%AF%E3%80%81AWS%E5%86%85,%E3%81%AE%E5%89%8A%E6%B8%9B%E3%82%82%E5%9B%B3%E3%82%8C%E3%81%BE%E3%81%99%E3%80%82)

## 高度な権限管理パターン

### 時間ベース権限制御

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "*",
      "Condition": {
        "DateGreaterThan": {
          "aws:CurrentTime": "2024-01-01T00:00:00Z"
        },
        "DateLessThan": {
          "aws:CurrentTime": "2024-12-31T23:59:59Z"
        }
      }
    }
  ]
}
```

### 条件付きアクセス制御

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": ["ap-northeast-1", "ap-northeast-3"]
        },
        "Bool": {
          "aws:ViaAWSService": "false"
        }
      }
    }
  ]
}
```

## コンプライアンスと監査

### 定期的な権限レビュー

**チェックリスト:**

- [ ] 90 日以上使用されていないアクセスキーの確認
- [ ] 過剰な権限を持つユーザーの特定
- [ ] 不要になったロールの削除
- [ ] MFA 未設定ユーザーの確認

### Access Analyzer の活用

```bash
# 外部からアクセス可能なリソースの検出
aws accessanalyzer list-findings \
    --analyzer-arn arn:aws:access-analyzer:region:account:analyzer/analyzer-name \
    --filter '{"status": {"eq": ["ACTIVE"]}}'
```

## 認証関連用語

### IdP（Identity Provider）

クラウドサービスへの認証を管理する統合認証システム

**主要な IdP:**

- Active Directory Federation Services (ADFS)
- Okta
- Auth0
- Google Workspace
- Azure AD

### SP（Service Provider）

認証によりアクセスできるようになるクラウドサービス自体

**SAML フローの理解:**

1. ユーザーが SP にアクセス
2. SP が IdP にリダイレクト
3. IdP でユーザー認証
4. SAML レスポンスで SP に戻る
5. SP がアクセス許可

**参考:** [IdP と SP について](https://www.splashtop.co.jp/knowhow/28/)

## 実装例とコードサンプル

### Terraform での IAM 管理

```hcl
# IAM ロールの作成
resource "aws_iam_role" "ec2_role" {
  name = "ec2-s3-access-role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
      }
    ]
  })
}

# ポリシーのアタッチ
resource "aws_iam_role_policy_attachment" "s3_access" {
  role       = aws_iam_role.ec2_role.name
  policy_arn = "arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess"
}
```

### Python での権限チェック

```python
import boto3
from botocore.exceptions import ClientError

def check_s3_permissions(bucket_name, key_name):
    s3 = boto3.client('s3')

    try:
        s3.head_object(Bucket=bucket_name, Key=key_name)
        return True
    except ClientError as e:
        if e.response['Error']['Code'] == '403':
            print(f"Access denied to {bucket_name}/{key_name}")
            return False
        else:
            print(f"Error: {e}")
            return False
```

## 参考リソース

### 公式ドキュメント

- [AWS IAM 概要 - SoftBank](https://www.softbank.jp/biz/blog/cloud-technology/articles/202212/aws-iam/#:~:text=%E9%96%A2%E9%80%A3%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9-,IAM%20%E3%81%A8%E3%81%AF,%E3%83%9D%E3%83%AA%E3%82%B7%E3%83%BC%E3%81%A7%E8%A1%8C%E3%82%8F%E3%82%8C%E3%81%BE%E3%81%99%E3%80%82)
- [AWS IAM 初心者向けガイド](https://o2mamiblog.com/aws-iam-beginner/)
- [AWS IAM 解説動画プレイリスト](https://www.youtube.com/playlist?list=PL2nCE2iR-lpmJH4FZLlwjIEMBxiafXVDb)

### 実践的なリソース

- [AWS IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [IAM Policy Simulator](https://policysim.aws.amazon.com/)
- [AWS Config Rules for IAM](https://docs.aws.amazon.com/config/latest/developerguide/iam-rules.html)
- [CloudFormation IAM Templates](https://github.com/awslabs/aws-cloudformation-templates)

### ツールとユーティリティ

- [IAM Policy Generator](https://awspolicygen.s3.amazonaws.com/policygen.html)
- [Parliament (Policy Linter)](https://github.com/duo-labs/parliament)
- [Prowler (Security Auditing)](https://github.com/prowler-cloud/prowler)
- [ScoutSuite (Security Assessment)](https://github.com/nccgroup/ScoutSuite)
