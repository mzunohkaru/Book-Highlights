# AWS Management Services

AWS の Management サービス群について説明します。

## ![](セキュリティサービス一覧.jpeg)

## CloudFormation

![](CloudFormation.jpeg)

### 概要

プログラミング言語やテキストファイルを使用して AWS リソースを自動で構築するサービスです。Infrastructure as Code (IaC) の代表的なサービスで、JSON や YAML 形式でインフラストラクチャを定義できます。

### 特徴

- 同じテンプレートを利用することにより、どのアカウントでも同じリソースが構築できる
- 設定ミスが減少し、作業時間の短縮が期待できる
- バージョン管理による変更履歴の追跡
- ロールバック機能による安全な運用

### アーキテクチャパターン

#### 1. ネストされたスタック

```yaml
# 親テンプレート
Resources:
  NetworkStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.bucket.com/network-template.yaml
      Parameters:
        VpcCidr: 10.0.0.0/16

  ApplicationStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.bucket.com/app-template.yaml
      Parameters:
        VpcId: !GetAtt NetworkStack.Outputs.VpcId
```

#### 2. クロススタック参照

```yaml
# ネットワークスタック
Outputs:
  VpcId:
    Value: !Ref VPC
    Export:
      Name: !Sub "${AWS::StackName}-VpcId"

# アプリケーションスタック
Parameters:
  VpcId:
    Type: String
    Default: !ImportValue NetworkStack-VpcId
```

### ベストプラクティス

#### テンプレート設計

- **単一責任の原則**: 1 つのスタックには関連するリソースのみを含める
- **パラメータ化**: 環境固有の値はパラメータとして外部化
- **出力値の活用**: 他のスタックで使用するリソース ID を Outputs で公開

#### セキュリティ

- **IAM ロールの最小権限**: CloudFormation に必要最小限の権限のみを付与
- **機密情報の管理**: Secrets Manager や Parameter Store との連携
- **ドリフト検出**: 手動変更の検出と修正

#### 実装例: セキュアな VPC 構築

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "Secure VPC with public/private subnets"

Parameters:
  Environment:
    Type: String
    AllowedValues: [dev, staging, prod]
    Default: dev

  VpcCidr:
    Type: String
    Default: 10.0.0.0/16
    AllowedPattern: ^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.){3}([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])(\/(1[6-9]|2[0-8]))$

Mappings:
  EnvironmentConfig:
    dev:
      InstanceType: t3.micro
      FlowLogsRetention: 7
    prod:
      InstanceType: t3.small
      FlowLogsRetention: 30

Resources:
  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: !Ref VpcCidr
      EnableDnsHostnames: true
      EnableDnsSupport: true
      Tags:
        - Key: Name
          Value: !Sub "${Environment}-vpc"

  FlowLogsRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: Allow
            Principal:
              Service: vpc-flow-logs.amazonaws.com
            Action: sts:AssumeRole
      Policies:
        - PolicyName: CloudWatchLogPolicy
          PolicyDocument:
            Version: "2012-10-17"
            Statement:
              - Effect: Allow
                Action:
                  - logs:CreateLogGroup
                  - logs:CreateLogStream
                  - logs:PutLogEvents
                Resource: "*"

  VPCFlowLog:
    Type: AWS::EC2::FlowLog
    Properties:
      ResourceType: VPC
      ResourceId: !Ref VPC
      TrafficType: ALL
      LogDestinationType: cloud-watch-logs
      LogGroupName: !Sub "/aws/vpc/flowlogs/${Environment}"
      DeliverLogsPermissionArn: !GetAtt FlowLogsRole.Arn
      Tags:
        - Key: Environment
          Value: !Ref Environment

Outputs:
  VpcId:
    Description: VPC ID
    Value: !Ref VPC
    Export:
      Name: !Sub "${AWS::StackName}-VpcId"
```

### トラブルシューティング

#### よくあるエラーと対処法

1. **リソース作成失敗**: Dependencies を確認し、適切な `DependsOn` を設定
2. **権限エラー**: IAM ポリシーの見直しとリソースレベルの権限確認
3. **ドリフト検出**: 手動変更を元に戻すか、テンプレートを更新

#### デバッグテクニック

```bash
# スタックイベントの確認
aws cloudformation describe-stack-events --stack-name my-stack

# リソースドリフトの検出
aws cloudformation detect-stack-drift --stack-name my-stack

# 変更セットでの事前確認
aws cloudformation create-change-set --stack-name my-stack \
  --template-body file://template.yaml --change-set-name preview-changes
```

### 参考資料

- [CTC の CloudFormation 解説](https://www.ctc-g.co.jp/solutions/cloud/column/article/07.html#:~:text=CloudFormation%E3%81%AF%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0%E8%A8%80%E8%AA%9E%E3%82%84,%E3%81%99%E3%82%8B%E3%81%93%E3%81%A8%E3%81%8C%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82)
- [YouTube プレイリスト](https://www.youtube.com/playlist?list=PL2nCE2iR-lpkInfBOJbEu1tMB3Ld2PHZc)
- [AWS CloudFormation ベストプラクティス](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/best-practices.html)

---

## CloudWatch

![](CloudWatch.jpeg)

### 概要

AWS 上で提供されるあらゆるサービスを監視するためのサービスです。メトリクス、ログ、イベントを統合的に管理し、システムの可観測性を向上させます。

### CloudWatch の主要コンポーネント

#### CloudWatch メトリクス

各リソースから収集したデータを時系列でまとめたものです。

**標準メトリクス vs カスタムメトリクス**

- **標準メトリクス**: AWS サービスが自動的に送信（無料）
- **カスタムメトリクス**: アプリケーションから送信（有料）

**実装例: カスタムメトリクス送信**

```typescript
import {
  CloudWatchClient,
  PutMetricDataCommand,
} from "@aws-sdk/client-cloudwatch";

const cloudwatch = new CloudWatchClient({ region: "ap-northeast-1" });

async function putCustomMetric(
  metricName: string,
  value: number,
  unit: string = "Count"
): Promise<void> {
  try {
    const command = new PutMetricDataCommand({
      Namespace: "MyApp/Performance",
      MetricData: [
        {
          MetricName: metricName,
          Value: value,
          Unit: unit,
          Timestamp: new Date(),
          Dimensions: [
            {
              Name: "Environment",
              Value: "production",
            },
            {
              Name: "Service",
              Value: "order-service",
            },
          ],
        },
      ],
    });

    await cloudwatch.send(command);
    console.log(`メトリクス ${metricName} を送信しました`);
  } catch (error) {
    console.error(`メトリクス送信エラー: ${error}`);
  }
}

// 使用例
await putCustomMetric("OrdersProcessed", 150);
await putCustomMetric("ResponseTime", 250, "Milliseconds");
```

#### CloudWatch エージェント

追加のメトリクスを収集したい時に使用するサービスです。

**設定例: 詳細なシステムメトリクス収集**

```json
{
  "agent": {
    "metrics_collection_interval": 60,
    "run_as_user": "cwagent"
  },
  "metrics": {
    "namespace": "CWAgent",
    "metrics_collected": {
      "cpu": {
        "measurement": [
          "cpu_usage_idle",
          "cpu_usage_iowait",
          "cpu_usage_user",
          "cpu_usage_system"
        ],
        "metrics_collection_interval": 60,
        "totalcpu": false
      },
      "disk": {
        "measurement": ["used_percent"],
        "metrics_collection_interval": 60,
        "resources": ["*"]
      },
      "diskio": {
        "measurement": [
          "io_time",
          "read_bytes",
          "write_bytes",
          "reads",
          "writes"
        ],
        "metrics_collection_interval": 60,
        "resources": ["*"]
      },
      "mem": {
        "measurement": ["mem_used_percent"],
        "metrics_collection_interval": 60
      },
      "netstat": {
        "measurement": ["tcp_established", "tcp_time_wait"],
        "metrics_collection_interval": 60
      },
      "swap": {
        "measurement": ["swap_used_percent"],
        "metrics_collection_interval": 60
      }
    }
  },
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/application.log",
            "log_group_name": "/aws/ec2/application",
            "log_stream_name": "{instance_id}",
            "timezone": "UTC"
          }
        ]
      }
    }
  }
}
```

#### CloudWatch ログ

- ログを文字列として保存
- ログに特定の文字列があった際にアラームを出すことができる

**ログ分析のベストプラクティス**

```bash
# ログインサイトクエリ例: エラー率の計算
fields @timestamp, @message
| filter @message like /ERROR/
| stats count() as error_count by bin(5m)

# レスポンス時間の分析
fields @timestamp, responseTime
| filter @type = "REPORT"
| stats avg(responseTime), max(responseTime), min(responseTime) by bin(5m)

# 特定のユーザーのアクション追跡
fields @timestamp, userId, action
| filter userId = "user123"
| sort @timestamp desc
| limit 100
```

#### CloudWatch アラーム

1 つ、または複数のメトリクスが特定の値になった時にアラームを出すことができるサービスです。

**実装例: 複合アラーム設定**

```yaml
# CloudFormation での複合アラーム定義
HighErrorRateAlarm:
  Type: AWS::CloudWatch::Alarm
  Properties:
    AlarmName: HighErrorRate
    AlarmDescription: API エラー率が 5% を超えた場合
    MetricName: 4XXError
    Namespace: AWS/ApiGateway
    Statistic: Sum
    Period: 300
    EvaluationPeriods: 2
    Threshold: 5
    ComparisonOperator: GreaterThanThreshold
    AlarmActions:
      - !Ref SNSTopicArn
    Dimensions:
      - Name: ApiName
        Value: MyAPI

CompositeAlarm:
  Type: AWS::CloudWatch::CompositeAlarm
  Properties:
    AlarmName: SystemHealthAlarm
    AlarmRule: !Sub |
      ALARM(${HighErrorRateAlarm}) OR 
      ALARM(${HighLatencyAlarm}) OR 
      ALARM(${LowMemoryAlarm})
    AlarmActions:
      - !Ref CriticalSNSTopicArn
```

#### CloudWatch イベント（EventBridge）

AWS リソースの変更や API の特定のイベントをトリガーとしてアクションの実行を自動化できるサービスです。

**実装例: 自動スケーリング**

```json
{
  "Rules": [
    {
      "Name": "AutoScaleOnHighCPU",
      "EventPattern": {
        "source": ["aws.cloudwatch"],
        "detail-type": ["CloudWatch Alarm State Change"],
        "detail": {
          "state": {
            "value": ["ALARM"]
          },
          "alarmName": ["HighCPUUtilization"]
        }
      },
      "Targets": [
        {
          "Id": "1",
          "Arn": "arn:aws:lambda:region:account:function:auto-scale",
          "InputTransformer": {
            "InputPathsMap": {
              "instance": "$.detail.configuration.metrics[0].metricStat.metric.dimensions.InstanceId"
            },
            "InputTemplate": "{\"action\": \"scale-out\", \"instanceId\": \"<instance>\"}"
          }
        }
      ]
    }
  ]
}
```

#### CloudWatch ダッシュボード

メトリクス、アラーム、イベントを自分の好きなようにカスタマイズして表示できるサービスです。

**実装例: API ダッシュボード**

```typescript
import {
  CloudWatchClient,
  PutDashboardCommand,
} from "@aws-sdk/client-cloudwatch";

async function createApiDashboard(): Promise<void> {
  const cloudwatch = new CloudWatchClient({ region: "ap-northeast-1" });

  const dashboardBody = {
    widgets: [
      {
        type: "metric",
        x: 0,
        y: 0,
        width: 12,
        height: 6,
        properties: {
          metrics: [
            ["AWS/ApiGateway", "Count", "ApiName", "MyAPI"],
            [".", "4XXError", ".", "."],
            [".", "5XXError", ".", "."],
          ],
          period: 300,
          stat: "Sum",
          region: "ap-northeast-1",
          title: "API Gateway Requests and Errors",
        },
      },
      {
        type: "metric",
        x: 0,
        y: 6,
        width: 12,
        height: 6,
        properties: {
          metrics: [["AWS/ApiGateway", "Latency", "ApiName", "MyAPI"]],
          period: 300,
          stat: "Average",
          region: "ap-northeast-1",
          title: "API Gateway Latency",
        },
      },
    ],
  };

  const command = new PutDashboardCommand({
    DashboardName: "API-Performance-Dashboard",
    DashboardBody: JSON.stringify(dashboardBody),
  });

  await cloudwatch.send(command);
}
```

### 監視戦略とベストプラクティス

#### 4 つの黄金シグナル

1. **レイテンシ**: リクエストの応答時間
2. **トラフィック**: リクエスト数/秒
3. **エラー**: エラー率
4. **サチュレーション**: リソース使用率

#### コスト最適化

- **メトリクス保持期間の調整**: 不要な長期保存を避ける
- **ログフィルタリング**: 重要なログのみを保存
- **リアルタイム監視の最適化**: 必要最小限のアラームのみ設定

### 参考資料

- [富士通の CloudWatch 解説](https://www.fujitsu.com/jp/products/software/resources/feature-stories/cloud-operation/aws-monitoring/#:~:text=CloudWatch%20%E3%81%A8%E3%81%AF%E3%80%81AWS%E4%B8%8A,%E5%BF%85%E8%A6%81%E4%B8%8D%E5%8F%AF%E6%AC%A0%E3%81%AA%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E3%81%A7%E3%81%99%E3%80%82)
- [BOLD の CloudWatch 解説](https://www.bold.ne.jp/engineer-club/aws-cloudwatch#-CloudWatch)
- [CloudWatch ベストプラクティス](https://docs.aws.amazon.com/ja_jp/AmazonCloudWatch/latest/monitoring/cloudwatch_architecture.html)

---

## CloudTrail

![](CloudTrail.jpeg)

### 概要

AWS アカウント内での操作ログを記録するサービスです。

### 主な機能

- どのユーザーがいつ、何に対して、何をしたのかを記録
- ログファイルの改ざんや削除が行われていないかの整合性の検証
- ログデータは自動で 90 日間保存される

---

## AWS Config

![](Config.jpeg)

### 概要

AWS リソースの設定変更を監視し、コンプライアンスチェックを行うサービスです。ガバナンス、リスク管理、コンプライアンス（GRC）の要件を満たすための重要なツールです。

### 主な機能

- 設定の変更や関連性の変化を自動的に検知
- 社内のガイドラインに沿ったコンプライアンスの確認
- セキュリティ分析、変更管理、トラブルシューティングを容易に実行
- リソース間の依存関係の可視化

### 実装例

#### カスタム Config ルール

```typescript
import {
  ConfigServiceClient,
  PutConfigRuleCommand,
  PutEvaluationsCommand,
} from "@aws-sdk/client-config-service";

interface ConfigRule {
  ConfigRuleName: string;
  Description: string;
  Source: {
    Owner: string;
    SourceIdentifier: string;
  };
  InputParameters?: string;
}

async function createSecurityConfigRules(): Promise<void> {
  const config = new ConfigServiceClient({ region: "ap-northeast-1" });

  // S3バケットの公開アクセス禁止ルール
  const s3PublicAccessRule: ConfigRule = {
    ConfigRuleName: "s3-bucket-public-access-prohibited",
    Description: "S3バケットの公開アクセスが禁止されているかチェック",
    Source: {
      Owner: "AWS",
      SourceIdentifier: "S3_BUCKET_PUBLIC_ACCESS_PROHIBITED",
    },
    InputParameters: JSON.stringify({
      excludedPublicBuckets: "static-website-bucket,public-logs-bucket",
    }),
  };

  // EC2インスタンスのIMDSv2強制ルール
  const ec2Imdsv2Rule: ConfigRule = {
    ConfigRuleName: "ec2-imdsv2-check",
    Description: "EC2インスタンスでIMDSv2が有効になっているかチェック",
    Source: {
      Owner: "AWS",
      SourceIdentifier: "EC2_IMDSV2_CHECK",
    },
  };

  // セキュリティグループの過度なアクセス許可チェック
  const securityGroupRule: ConfigRule = {
    ConfigRuleName: "incoming-ssh-disabled",
    Description:
      "セキュリティグループでSSH（ポート22）が0.0.0.0/0から許可されていないかチェック",
    Source: {
      Owner: "AWS",
      SourceIdentifier: "INCOMING_SSH_DISABLED",
    },
  };

  const rules = [s3PublicAccessRule, ec2Imdsv2Rule, securityGroupRule];

  for (const rule of rules) {
    try {
      const command = new PutConfigRuleCommand({ ConfigRule: rule });
      await config.send(command);
      console.log(`作成完了: ${rule.ConfigRuleName}`);
    } catch (error) {
      console.error(`作成エラー ${rule.ConfigRuleName}: ${error}`);
    }
  }
}

// カスタムルール Lambda 関数例
interface ConfigEvent {
  configurationItem: {
    resourceType: string;
    resourceId: string;
    tags?: Record<string, string>;
    configuration?: any;
    configurationItemCaptureTime: string;
  };
  ruleParameters?: Record<string, any>;
  resultToken: string;
}

async function lambdaHandler(
  event: ConfigEvent
): Promise<{ statusCode: number; body: string }> {
  const { configurationItem, ruleParameters = {} } = event;
  const { resourceType, resourceId } = configurationItem;

  let complianceType = "COMPLIANT";
  let annotation = "";

  try {
    if (resourceType === "AWS::EC2::Instance") {
      // EC2インスタンスの特定タグ存在チェック
      const tags = configurationItem.tags || {};
      const requiredTags = ["Environment", "Owner", "Project"];

      const missingTags = requiredTags.filter((tag) => !(tag in tags));

      if (missingTags.length > 0) {
        complianceType = "NON_COMPLIANT";
        annotation = `必須タグが不足: ${missingTags.join(", ")}`;
      } else {
        annotation = "必須タグが全て設定されています";
      }
    } else if (resourceType === "AWS::S3::Bucket") {
      // S3バケットの暗号化チェック
      const configuration = configurationItem.configuration;
      const encryption = configuration?.serverSideEncryptionConfiguration;

      if (!encryption) {
        complianceType = "NON_COMPLIANT";
        annotation = "サーバーサイド暗号化が設定されていません";
      } else {
        annotation = "サーバーサイド暗号化が設定されています";
      }
    }
  } catch (error) {
    complianceType = "NOT_APPLICABLE";
    annotation = `評価エラー: ${error}`;
  }

  // 評価結果をConfigに送信
  const config = new ConfigServiceClient({ region: "ap-northeast-1" });
  const putEvaluationsCommand = new PutEvaluationsCommand({
    Evaluations: [
      {
        ComplianceResourceType: resourceType,
        ComplianceResourceId: resourceId,
        ComplianceType: complianceType,
        Annotation: annotation,
        OrderingTimestamp: new Date(
          configurationItem.configurationItemCaptureTime
        ),
      },
    ],
    ResultToken: event.resultToken,
  });

  await config.send(putEvaluationsCommand);

  return {
    statusCode: 200,
    body: JSON.stringify(`評価完了: ${resourceId} - ${complianceType}`),
  };
}
```

#### 修復アクション（Remediation）

```yaml
# CloudFormation での自動修復設定
ConfigRemediationConfiguration:
  Type: AWS::Config::RemediationConfiguration
  Properties:
    ConfigRuleName: !Ref SecurityGroupRule
    TargetType: SSM_DOCUMENT
    TargetId: AWSConfigRemediation-RemoveUnrestrictedSourceInSecurityGroup
    TargetVersion: 1
    Parameters:
      AutomationAssumeRole:
        StaticValue: !GetAtt RemediationRole.Arn
      GroupId:
        ResourceValue: RESOURCE_ID
    Automatic: true
    MaximumAutomaticAttempts: 3

RemediationRole:
  Type: AWS::IAM::Role
  Properties:
    AssumeRolePolicyDocument:
      Version: "2012-10-17"
      Statement:
        - Effect: Allow
          Principal:
            Service: ssm.amazonaws.com
          Action: sts:AssumeRole
    ManagedPolicyArns:
      - arn:aws:iam::aws:policy/service-role/AmazonSSMAutomationRole
    Policies:
      - PolicyName: ConfigRemediationPolicy
        PolicyDocument:
          Version: "2012-10-17"
          Statement:
            - Effect: Allow
              Action:
                - ec2:AuthorizeSecurityGroupIngress
                - ec2:RevokeSecurityGroupIngress
                - ec2:DescribeSecurityGroups
              Resource: "*"
```

---

## Compute Optimizer

![](ComputeOptimizer.jpeg)

### 概要

AWS リソースの使用量を分析し、コスト最適化の推奨事項を提供するサービスです。

### 対象サービス

- EC2
- EBS
- Lambda

これらのサービスに限り、コスト削減が可能です。

---

## WorkMail & WorkDocs

![](WorkMail.jpeg)

### 概要

AWS が提供するビジネス向けコラボレーションサービスです。

---

## Amazon Connect

![](Connect.jpeg)

### 概要

クラウド型のコールセンター機能を提供するサービスです。

---

## Amazon SNS (Simple Notification Service)

![](SNS.jpeg)

### 概要

メッセージ配信とプッシュ通知を提供するサービスです。pub/sub（publish/subscribe）パターンを実装し、1 つのメッセージを複数の宛先に同時配信できます。

### 主な機能

- AWS のサービスへの通知
- SMS 送信
- スマートフォンへのプッシュ通知
- Email 配信
- HTTP/HTTPS エンドポイントへの配信

### アーキテクチャパターン

#### ファンアウトパターン

```typescript
import {
  SNSClient,
  CreateTopicCommand,
  SubscribeCommand,
  PublishCommand,
} from "@aws-sdk/client-sns";
import {
  SQSClient,
  CreateQueueCommand,
  GetQueueAttributesCommand,
} from "@aws-sdk/client-sqs";

interface OrderData {
  id: string;
  customer_id: string;
  items: any[];
  total: number;
  created_at: string;
}

async function setupFanoutArchitecture(): Promise<string> {
  const sns = new SNSClient({ region: "ap-northeast-1" });
  const sqs = new SQSClient({ region: "ap-northeast-1" });

  // SNS トピック作成
  const createTopicCommand = new CreateTopicCommand({
    Name: "OrderProcessingTopic",
    Attributes: {
      DisplayName: "注文処理トピック",
      DeliveryPolicy: JSON.stringify({
        healthyRetryPolicy: {
          numRetries: 3,
          minDelayTarget: 20,
          maxDelayTarget: 20,
          numMinDelayRetries: 0,
          numMaxDelayRetries: 0,
          backoffFunction: "linear",
        },
      }),
    },
  });

  const topicResponse = await sns.send(createTopicCommand);
  const topicArn = topicResponse.TopicArn!;

  // 複数のSQSキューを作成してサブスクライブ
  const services = ["inventory", "payment", "shipping", "notification"];

  for (const service of services) {
    // SQSキューの作成
    const createQueueCommand = new CreateQueueCommand({
      QueueName: `${service}-processing-queue`,
      Attributes: {
        VisibilityTimeoutSeconds: "300",
        MessageRetentionPeriod: "1209600", // 14日間
        RedrivePolicy: JSON.stringify({
          deadLetterTargetArn: `arn:aws:sqs:region:account:${service}-dlq`,
          maxReceiveCount: 3,
        }),
      },
    });

    const queueResponse = await sqs.send(createQueueCommand);
    const queueUrl = queueResponse.QueueUrl!;

    const getQueueAttributesCommand = new GetQueueAttributesCommand({
      QueueUrl: queueUrl,
      AttributeNames: ["QueueArn"],
    });

    const queueAttrs = await sqs.send(getQueueAttributesCommand);
    const queueArn = queueAttrs.Attributes!.QueueArn;

    // SNSトピックにSQSキューをサブスクライブ
    const subscribeCommand = new SubscribeCommand({
      TopicArn: topicArn,
      Protocol: "sqs",
      Endpoint: queueArn,
      Attributes: {
        FilterPolicy:
          service !== "notification"
            ? JSON.stringify({ service: [service] })
            : JSON.stringify({}),
      },
    });

    await sns.send(subscribeCommand);
  }

  return topicArn;
}

// メッセージ送信例
async function publishOrderEvent(
  topicArn: string,
  orderData: OrderData
): Promise<void> {
  const sns = new SNSClient({ region: "ap-northeast-1" });

  const message = {
    orderId: orderData.id,
    customerId: orderData.customer_id,
    items: orderData.items,
    totalAmount: orderData.total,
    timestamp: orderData.created_at,
  };

  // 各サービス向けに属性付きで配信
  for (const service of ["inventory", "payment", "shipping"]) {
    const publishCommand = new PublishCommand({
      TopicArn: topicArn,
      Message: JSON.stringify(message),
      MessageAttributes: {
        service: {
          DataType: "String",
          StringValue: service,
        },
      },
      Subject: `注文処理: ${service}サービス`,
    });

    await sns.send(publishCommand);
  }

  // 通知サービス向け（フィルターなし）
  const notificationPublishCommand = new PublishCommand({
    TopicArn: topicArn,
    Message: JSON.stringify({
      ...message,
      notificationType: "order_confirmation",
    }),
    MessageAttributes: {
      service: {
        DataType: "String",
        StringValue: "notification",
      },
    },
    Subject: "注文確認通知",
  });

  await sns.send(notificationPublishCommand);
}
```

---

## Amazon SQS (Simple Queue Service)

![](SQS.jpeg)

### 概要

メッセージキューイングサービスです。非同期処理、システム間の疎結合、負荷の平準化を実現する重要なコンポーネントです。

### メリット

キューイングを行うことで、送信側は受信側の状態にかかわらずメッセージの送信が可能になり、システム全体で安定した連携ができます。

### キューの種類

#### Standard Queue

- **高スループット**: 秒間ほぼ無制限の API 呼び出し
- **At-Least-Once 配信**: メッセージが少なくとも 1 回は配信される
- **順序保証なし**: メッセージの順序は保証されない

#### FIFO Queue

- **順序保証**: メッセージが送信順に配信される
- **Exactly-Once 処理**: 重複排除により、同じメッセージが複数回処理されない
- **スループット制限**: 秒間 3,000 メッセージ（バッチング使用時は 30,000 メッセージ）

### 実装パターン

#### ワーカープール パターン

```typescript
import {
  SQSClient,
  ReceiveMessageCommand,
  DeleteMessageCommand,
  Message,
} from "@aws-sdk/client-sqs";

interface SQSMessage {
  Body?: string;
  ReceiptHandle?: string;
}

class SQSWorkerPool {
  private sqs: SQSClient;
  private queueUrl: string;
  private workerCount: number;
  private isRunning: boolean = false;

  constructor(queueUrl: string, workerCount: number = 5) {
    this.sqs = new SQSClient({ region: "ap-northeast-1" });
    this.queueUrl = queueUrl;
    this.workerCount = workerCount;
  }

  async start(): Promise<void> {
    this.isRunning = true;

    // 複数ワーカーを並行実行
    const workers = Array.from({ length: this.workerCount }, (_, i) =>
      this.workerLoop(`worker-${i}`)
    );

    await Promise.all(workers);
  }

  stop(): void {
    this.isRunning = false;
  }

  private async workerLoop(workerId: string): Promise<void> {
    while (this.isRunning) {
      try {
        // メッセージの受信（ロングポーリング）
        const command = new ReceiveMessageCommand({
          QueueUrl: this.queueUrl,
          MaxNumberOfMessages: 10,
          WaitTimeSeconds: 20, // ロングポーリング
          VisibilityTimeoutSeconds: 300,
        });

        const response = await this.sqs.send(command);
        const messages = response.Messages || [];

        for (const message of messages) {
          try {
            // メッセージ処理
            await this.processMessage(message, workerId);

            // 処理完了後にメッセージ削除
            const deleteCommand = new DeleteMessageCommand({
              QueueUrl: this.queueUrl,
              ReceiptHandle: message.ReceiptHandle!,
            });

            await this.sqs.send(deleteCommand);
          } catch (error) {
            console.error(`Worker ${workerId}: メッセージ処理エラー: ${error}`);
            // エラー時は可視性タイムアウト後に再処理される
          }
        }
      } catch (error) {
        console.error(`Worker ${workerId}: 受信エラー: ${error}`);
        await new Promise((resolve) => setTimeout(resolve, 5000)); // エラー時は少し待機
      }
    }
  }

  private async processMessage(
    message: Message,
    workerId: string
  ): Promise<void> {
    const body = JSON.parse(message.Body!);
    console.log(`Worker ${workerId}: 処理中 - ${JSON.stringify(body)}`);

    // ビジネスロジックの実行
    // 例: 画像処理、データ変換、外部API呼び出しなど
    await new Promise((resolve) => setTimeout(resolve, 2000)); // 処理時間のシミュレーション

    console.log(`Worker ${workerId}: 処理完了 - ${JSON.stringify(body)}`);
  }
}

// デッドレターキューの設定
function setupDlqWithCloudFormation(): object {
  const template = {
    AWSTemplateFormatVersion: "2010-09-09",
    Resources: {
      ProcessingQueue: {
        Type: "AWS::SQS::Queue",
        Properties: {
          QueueName: "order-processing-queue",
          VisibilityTimeoutSeconds: 300,
          MessageRetentionPeriod: 1209600,
          RedrivePolicy: {
            deadLetterTargetArn: { "Fn::GetAtt": ["DeadLetterQueue", "Arn"] },
            maxReceiveCount: 3,
          },
        },
      },
      DeadLetterQueue: {
        Type: "AWS::SQS::Queue",
        Properties: {
          QueueName: "order-processing-dlq",
          MessageRetentionPeriod: 1209600,
        },
      },
      DLQAlarm: {
        Type: "AWS::CloudWatch::Alarm",
        Properties: {
          AlarmName: "DLQ-Messages-Available",
          AlarmDescription: "DLQにメッセージが蓄積されています",
          MetricName: "ApproximateNumberOfMessages",
          Namespace: "AWS/SQS",
          Statistic: "Average",
          Period: 300,
          EvaluationPeriods: 1,
          Threshold: 1,
          ComparisonOperator: "GreaterThanOrEqualToThreshold",
          Dimensions: [
            {
              Name: "QueueName",
              Value: { "Fn::GetAtt": ["DeadLetterQueue", "QueueName"] },
            },
          ],
        },
      },
    },
  };
  return template;
}
```

#### バッチ処理パターン

```typescript
import {
  SQSClient,
  ReceiveMessageCommand,
  DeleteMessageBatchCommand,
} from "@aws-sdk/client-sqs";

async function batchProcessor(): Promise<void> {
  const sqs = new SQSClient({ region: "ap-northeast-1" });
  const queueUrl = "https://sqs.region.amazonaws.com/account/queue-name";

  while (true) {
    // 最大10件のメッセージを一括受信
    const command = new ReceiveMessageCommand({
      QueueUrl: queueUrl,
      MaxNumberOfMessages: 10,
      WaitTimeSeconds: 20,
    });

    const response = await sqs.send(command);
    const messages = response.Messages || [];

    if (messages.length === 0) {
      continue;
    }

    // バッチで処理
    const messageData = messages.map((msg) => JSON.parse(msg.Body!));
    const batchResults = await processMessageBatch(messageData);

    // 成功したメッセージのみ削除
    const deleteEntries = [];
    for (let i = 0; i < messages.length; i++) {
      if (batchResults[i]) {
        deleteEntries.push({
          Id: i.toString(),
          ReceiptHandle: messages[i].ReceiptHandle!,
        });
      }
    }

    if (deleteEntries.length > 0) {
      const deleteBatchCommand = new DeleteMessageBatchCommand({
        QueueUrl: queueUrl,
        Entries: deleteEntries,
      });

      await sqs.send(deleteBatchCommand);
    }
  }
}

async function processMessageBatch(messages: any[]): Promise<boolean[]> {
  const results: boolean[] = [];

  for (const message of messages) {
    try {
      // 実際のビジネスロジック
      await heavyProcessing(message);
      results.push(true);
    } catch (error) {
      console.error(`処理エラー: ${error}`);
      results.push(false);
    }
  }

  return results;
}

async function heavyProcessing(message: any): Promise<void> {
  // 重い処理のシミュレーション
  await new Promise((resolve) => setTimeout(resolve, 1000));
}
```

### パフォーマンス最適化

#### ベストプラクティス

1. **ロングポーリング**: WaitTimeSeconds を 1-20 秒に設定
2. **バッチ処理**: 一度に複数メッセージを処理
3. **適切な可視性タイムアウト**: 処理時間に応じた設定
4. **デッドレターキュー**: 失敗メッセージの適切な処理

```python
# パフォーマンス最適化の例
def optimized_message_processing():
    sqs = boto3.client('sqs')

    # 設定の最適化
    optimized_params = {
        'WaitTimeSeconds': 20,        # ロングポーリング
        'MaxNumberOfMessages': 10,    # バッチ受信
        'VisibilityTimeoutSeconds': 300,  # 処理時間に応じた設定
        'MessageAttributeNames': ['All'],  # 必要な属性のみ取得
        'AttributeNames': ['All']
    }

    return optimized_params
```

### SNS + SQS 連携パターン

```python
def event_driven_architecture():
    """イベント駆動アーキテクチャの実装例"""

    # 注文イベントをSNSで配信し、各サービスがSQSで受信
    services_config = {
        'inventory': {
            'queue': 'inventory-processing-queue',
            'filter': {'eventType': ['inventory_update']}
        },
        'payment': {
            'queue': 'payment-processing-queue',
            'filter': {'eventType': ['payment_required']}
        },
        'shipping': {
            'queue': 'shipping-processing-queue',
            'filter': {'eventType': ['shipping_required']}
        }
    }

    return services_config
```

---

## Amazon MQ

![](MQ.jpeg)

### 概要

マネージド型のメッセージブローカーサービスです。

### 特徴

- 非同期でスケールに強いサービスを構築できる
- 一気にデータが来た場合、そのままサーバーが処理すると固まることがある
- そんなときに、一時的にメッセージを格納して管理するのがメッセージブローカーサービス

---

## Amazon EventBridge

![](EventBridge.jpeg)

### 概要

イベント駆動型アプリケーションを構築するためのサーバーレスサービスです。

### 特徴

- イベントを使用することでアプリケーションコンポーネントを接続できる
- イベント駆動型アプリを簡単に構築できる

---

## API Gateway

![](ApiGateway.jpeg)

### 概要

RESTful API と WebSocket API を作成、配布、保守、監視、保護できるサービスです。マイクロサービスアーキテクチャやサーバーレスアプリケーションの重要なコンポーネントとして機能します。

### 主な機能

- API の作成、配布、保守、監視、保護
- 認証・認可の統合管理
- レート制限とスロットリング
- リクエスト/レスポンスの変換
- キャッシュ機能

### アーキテクチャパターン

#### 1. マイクロサービス API 統合

```yaml
# CloudFormation での API Gateway 定義
ApiGateway:
  Type: AWS::ApiGateway::RestApi
  Properties:
    Name: MicroservicesAPI
    Description: マイクロサービス統合API
    EndpointConfiguration:
      Types:
        - REGIONAL
    Policy:
      Version: "2012-10-17"
      Statement:
        - Effect: Allow
          Principal: "*"
          Action: "execute-api:Invoke"
          Resource: "*"
          Condition:
            IpAddress:
              "aws:SourceIp":
                - "10.0.0.0/16" # VPC内からのアクセスのみ許可

UserServiceResource:
  Type: AWS::ApiGateway::Resource
  Properties:
    RestApiId: !Ref ApiGateway
    ParentId: !GetAtt ApiGateway.RootResourceId
    PathPart: users

UserServiceMethod:
  Type: AWS::ApiGateway::Method
  Properties:
    RestApiId: !Ref ApiGateway
    ResourceId: !Ref UserServiceResource
    HttpMethod: GET
    AuthorizationType: AWS_IAM
    Integration:
      Type: HTTP_PROXY
      IntegrationHttpMethod: GET
      Uri: https://user-service.internal.example.com/users
      RequestParameters:
        integration.request.header.X-API-Key: "'secret-key'"
```

#### 2. サーバーレス API

```typescript
import {
  APIGatewayProxyEvent,
  APIGatewayProxyResult,
  Context,
} from "aws-lambda";

interface User {
  id: string;
  name: string;
  email: string;
}

interface APIResponse {
  statusCode: number;
  headers: Record<string, string>;
  body: string;
}

export async function lambdaHandler(
  event: APIGatewayProxyEvent,
  context: Context
): Promise<APIGatewayProxyResult> {
  // CORSヘッダー設定
  const headers = {
    "Content-Type": "application/json",
    "Access-Control-Allow-Origin": "*",
    "Access-Control-Allow-Headers":
      "Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token",
    "Access-Control-Allow-Methods": "GET,POST,PUT,DELETE,OPTIONS",
  };

  try {
    // HTTPメソッドとパスの取得
    const httpMethod = event.httpMethod;
    const path = event.path;

    // リクエストボディの解析
    let body: any = {};
    if (event.body) {
      body = JSON.parse(event.body);
    }

    // パスパラメータの取得
    const pathParams = event.pathParameters || {};

    // クエリパラメータの取得
    const queryParams = event.queryStringParameters || {};

    // ビジネスロジック実行
    let result: any;

    if (httpMethod === "GET" && path === "/users") {
      result = await getUsers(queryParams);
    } else if (httpMethod === "GET" && path === "/users/{id}") {
      result = await getUser(pathParams.id!);
    } else if (httpMethod === "POST" && path === "/users") {
      result = await createUser(body);
    } else if (httpMethod === "PUT" && path === "/users/{id}") {
      result = await updateUser(pathParams.id!, body);
    } else if (httpMethod === "DELETE" && path === "/users/{id}") {
      result = await deleteUser(pathParams.id!);
    } else {
      return {
        statusCode: 404,
        headers,
        body: JSON.stringify({ error: "Not Found" }),
      };
    }

    return {
      statusCode: 200,
      headers,
      body: JSON.stringify(result),
    };
  } catch (error) {
    console.error(`Error: ${error}`);
    return {
      statusCode: 500,
      headers,
      body: JSON.stringify({ error: "Internal Server Error" }),
    };
  }
}

// ビジネスロジック関数例
async function getUsers(queryParams: Record<string, string>): Promise<User[]> {
  // DynamoDB等からユーザー一覧を取得する実装
  return [
    { id: "1", name: "John Doe", email: "john@example.com" },
    { id: "2", name: "Jane Smith", email: "jane@example.com" },
  ];
}

async function getUser(id: string): Promise<User | null> {
  // 特定ユーザーの取得
  return { id, name: "John Doe", email: "john@example.com" };
}

async function createUser(userData: Partial<User>): Promise<User> {
  // ユーザー作成
  const newUser: User = {
    id: Math.random().toString(36),
    name: userData.name!,
    email: userData.email!,
  };
  return newUser;
}

async function updateUser(id: string, userData: Partial<User>): Promise<User> {
  // ユーザー更新
  return {
    id,
    name: userData.name || "Updated Name",
    email: userData.email || "updated@example.com",
  };
}

async function deleteUser(id: string): Promise<{ message: string }> {
  // ユーザー削除
  return { message: `User ${id} deleted successfully` };
}
```

### セキュリティとベストプラクティス

#### 認証・認可パターン

```yaml
# Cognito User Pool 認証
CognitoAuthorizer:
  Type: AWS::ApiGateway::Authorizer
  Properties:
    Name: CognitoAuthorizer
    Type: COGNITO_USER_POOLS
    IdentitySource: method.request.header.Authorization
    RestApiId: !Ref ApiGateway
    ProviderARNs:
      - !GetAtt UserPool.Arn

# カスタム Lambda 認証
CustomAuthorizer:
  Type: AWS::ApiGateway::Authorizer
  Properties:
    Name: CustomAuthorizer
    Type: TOKEN
    IdentitySource: method.request.header.Authorization
    RestApiId: !Ref ApiGateway
    AuthorizerUri: !Sub
      - "arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${AuthFunction}/invocations"
      - AuthFunction: !GetAtt AuthorizerFunction.Arn
    AuthorizerResultTtlInSeconds: 300
```

#### カスタム認証 Lambda 実装例

```python
import json
import jwt
import os

def lambda_handler(event, context):
    """カスタム認証 Lambda"""
    token = event['authorizationToken']
    method_arn = event['methodArn']

    try:
        # JWTトークンの検証
        payload = jwt.decode(
            token,
            os.environ['JWT_SECRET'],
            algorithms=['HS256']
        )

        # ユーザー情報の取得
        user_id = payload['user_id']
        role = payload.get('role', 'user')

        # ポリシーの生成
        effect = 'Allow' if role in ['admin', 'user'] else 'Deny'

        policy = {
            'principalId': user_id,
            'policyDocument': {
                'Version': '2012-10-17',
                'Statement': [
                    {
                        'Action': 'execute-api:Invoke',
                        'Effect': effect,
                        'Resource': method_arn
                    }
                ]
            },
            'context': {
                'userId': user_id,
                'role': role
            }
        }

        return policy

    except Exception as e:
        print(f"Authorization failed: {str(e)}")
        raise Exception('Unauthorized')
```

#### レート制限とスロットリング

```yaml
# API Usage Plan 設定
UsagePlan:
  Type: AWS::ApiGateway::UsagePlan
  Properties:
    UsagePlanName: StandardPlan
    Description: 標準プラン
    ApiStages:
      - ApiId: !Ref ApiGateway
        Stage: prod
    Throttle:
      BurstLimit: 100 # バースト時の上限
      RateLimit: 50 # 持続的なレート制限
    Quota:
      Limit: 10000 # 日次クォータ
      Period: DAY

# API Key の作成
ApiKey:
  Type: AWS::ApiGateway::ApiKey
  Properties:
    Name: ProductionKey
    Description: 本番環境用APIキー
    Enabled: true

# Usage Plan と API Key の関連付け
UsagePlanKey:
  Type: AWS::ApiGateway::UsagePlanKey
  Properties:
    KeyId: !Ref ApiKey
    KeyType: API_KEY
    UsagePlanId: !Ref UsagePlan
```

### パフォーマンス最適化

#### キャッシュ設定

```yaml
# メソッドレベルキャッシュ
CachedMethod:
  Type: AWS::ApiGateway::Method
  Properties:
    RestApiId: !Ref ApiGateway
    ResourceId: !Ref Resource
    HttpMethod: GET
    AuthorizationType: NONE
    Integration:
      Type: AWS_PROXY
      IntegrationHttpMethod: POST
      Uri: !Sub "arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${LambdaFunction}/invocations"
      CacheKeyParameters:
        - method.request.querystring.category
        - method.request.header.Accept-Language
    RequestParameters:
      method.request.querystring.category: false
      method.request.header.Accept-Language: false
    MethodResponses:
      - StatusCode: 200
        ResponseParameters:
          method.response.header.Cache-Control: true
```

#### モニタリングとログ設定

```yaml
# CloudWatch Logs 設定
ApiGatewayCloudWatchRole:
  Type: AWS::IAM::Role
  Properties:
    AssumeRolePolicyDocument:
      Version: "2012-10-17"
      Statement:
        - Effect: Allow
          Principal:
            Service: apigateway.amazonaws.com
          Action: sts:AssumeRole
    ManagedPolicyArns:
      - arn:aws:iam::aws:policy/service-role/AmazonAPIGatewayPushToCloudWatchLogs

ApiGatewayAccount:
  Type: AWS::ApiGateway::Account
  Properties:
    CloudWatchRoleArn: !GetAtt ApiGatewayCloudWatchRole.Arn

# ステージでのログ有効化
Stage:
  Type: AWS::ApiGateway::Stage
  Properties:
    StageName: prod
    RestApiId: !Ref ApiGateway
    DeploymentId: !Ref Deployment
    MethodSettings:
      - ResourcePath: "/*"
        HttpMethod: "*"
        LoggingLevel: INFO
        DataTraceEnabled: true
        MetricsEnabled: true
```

### よくある構成

**API Gateway + Lambda = サーバーレス REST API**
**API Gateway + ECS = コンテナ化されたマイクロサービス**
**API Gateway + Cognito = セキュアな認証機能付き API**

### トラブルシューティング

#### よくある問題と解決法

1. **CORS エラー**: プリフライトリクエストの OPTIONS メソッド設定
2. **タイムアウト**: Lambda 統合の場合は 29 秒、HTTP 統合の場合は 30 秒が上限
3. **ペイロードサイズ制限**: 10MB までの制限に注意

```python
# CORS対応のOPTIONSハンドラー
def handle_options(event, context):
    return {
        'statusCode': 200,
        'headers': {
            'Access-Control-Allow-Origin': '*',
            'Access-Control-Allow-Headers': 'Content-Type,X-Amz-Date,Authorization,X-Api-Key',
            'Access-Control-Allow-Methods': 'GET,POST,PUT,DELETE,OPTIONS'
        },
        'body': ''
    }
```

### 実装参考資料

- [Zenn 記事：API Gateway + Lambda 実装](https://zenn.dev/astrologian/articles/862d50d4ceacdd)
- [Qiita 記事 1：API Gateway 実装](https://qiita.com/tamura_CD/items/46ba8a2f3bfd5484843f)
- [Qiita 記事 2：API Gateway 実装](https://qiita.com/miyuki_samitani/items/f01f1bd49334f97fe84c)
- [AWS API Gateway ベストプラクティス](https://docs.aws.amazon.com/ja_jp/apigateway/latest/developerguide/api-gateway-basic-concept.html)

---

## AppSync

![](AppSync.jpeg)

### 概要

GraphQL API を構築するためのマネージドサービスです。

### 主な機能

DynamoDB や Lambda、その他のデータソースとの安全な接続に必要な作業を自動的に処理します。

---

## Amazon SES (Simple Email Service)

![](SES.jpeg)

### 概要

メール送信サービスです。

### 主なユースケース

- メールマガジン配信
- サービスのメール認証機能
- 完了メールの送信

---

## AWS Secrets Manager

![](SecretsManager.jpeg)

### 概要

機密情報を安全に管理するサービスです。パスワードローテーション、細かいアクセス制御、監査機能を提供し、セキュリティのベストプラクティスを実現します。

### 管理対象

- データベースの認証情報
- API キー
- その他の機密情報
- SSL/TLS 証明書
- OAuth トークン

### メリット

Secrets Manager に認証情報のキーなどを保存、取得することで、アプリケーションに認証情報を保存する必要がなくなります。

### 実装パターン

#### Lambda での秘密情報取得

```typescript
import {
  SecretsManagerClient,
  GetSecretValueCommand,
} from "@aws-sdk/client-secrets-manager";

interface DatabaseCredentials {
  host: string;
  username: string;
  password: string;
  database: string;
  port: number;
}

interface ApiSecrets {
  stripe_secret_key: string;
  [key: string]: any;
}

class SecretsManager {
  private client: SecretsManagerClient;
  private cache: Map<string, any> = new Map();

  constructor(regionName: string = "ap-northeast-1") {
    this.client = new SecretsManagerClient({ region: regionName });
  }

  async getSecret(secretName: string, useCache: boolean = true): Promise<any> {
    // キャッシュから取得
    if (useCache && this.cache.has(secretName)) {
      return this.cache.get(secretName);
    }

    try {
      const command = new GetSecretValueCommand({ SecretId: secretName });
      const response = await this.client.send(command);
      const secretData = JSON.parse(response.SecretString!);

      // キャッシュに保存
      if (useCache) {
        this.cache.set(secretName, secretData);
      }

      return secretData;
    } catch (error: any) {
      if (error.name === "DecryptionFailureException") {
        throw error;
      } else if (error.name === "InternalServiceErrorException") {
        throw error;
      } else if (error.name === "InvalidParameterException") {
        throw error;
      } else if (error.name === "InvalidRequestException") {
        throw error;
      } else if (error.name === "ResourceNotFoundException") {
        throw new Error(`秘密情報が見つかりません: ${secretName}`);
      } else {
        throw error;
      }
    }
  }

  async getDatabaseCredentials(
    secretName: string
  ): Promise<DatabaseCredentials> {
    const credentials = await this.getSecret(secretName);

    return {
      host: credentials.host,
      username: credentials.username,
      password: credentials.password,
      database: credentials.dbname,
      port: credentials.port || 5432,
    };
  }
}

// Lambda関数での使用例
export async function lambdaHandler(event: any, context: any): Promise<any> {
  const secretsManager = new SecretsManager();

  try {
    // データベース接続情報取得
    const dbCreds = await secretsManager.getDatabaseCredentials(
      "prod/myapp/database"
    );

    // データベース接続（Node.js用PostgreSQLクライアント例）
    // import { Client } from 'pg';
    // const client = new Client({
    //   host: dbCreds.host,
    //   database: dbCreds.database,
    //   user: dbCreds.username,
    //   password: dbCreds.password,
    //   port: dbCreds.port
    // });
    // await client.connect();

    // API キー取得
    const apiSecrets: ApiSecrets = await secretsManager.getSecret(
      "prod/myapp/external-apis"
    );
    const stripeKey = apiSecrets.stripe_secret_key;

    // ビジネスロジック実行
    const result = await processBusinessLogic(dbCreds, stripeKey);

    return {
      statusCode: 200,
      body: JSON.stringify(result),
    };
  } catch (error) {
    console.error(`エラー: ${error}`);
    return {
      statusCode: 500,
      body: JSON.stringify({ error: "Internal Server Error" }),
    };
  }
}

async function processBusinessLogic(
  dbCreds: DatabaseCredentials,
  stripeKey: string
): Promise<any> {
  // ビジネスロジックの実装
  return { message: "Success", timestamp: new Date().toISOString() };
}
```

#### 自動ローテーション設定

```yaml
# CloudFormation での自動ローテーション設定
DatabaseSecret:
  Type: AWS::SecretsManager::Secret
  Properties:
    Name: !Sub "${Environment}/myapp/database"
    Description: "データベース認証情報"
    GenerateSecretString:
      SecretStringTemplate: '{"username": "admin"}'
      GenerateStringKey: "password"
      PasswordLength: 32
      ExcludeCharacters: '"@/\'

DatabaseSecretRotation:
  Type: AWS::SecretsManager::RotationSchedule
  Properties:
    SecretId: !Ref DatabaseSecret
    RotationLambdaArn: !GetAtt RotationLambda.Arn
    RotationRules:
      AutomaticallyAfterDays: 30

RotationLambda:
  Type: AWS::Lambda::Function
  Properties:
    FunctionName: !Sub "${Environment}-secret-rotation"
    Runtime: python3.9
    Handler: lambda_function.lambda_handler
    Code:
      ZipFile: |
        import boto3
        import json
        import logging
        import os

        def lambda_handler(event, context):
            """RDS パスワード自動ローテーション"""
            
            secret_arn = event['Step1']['SecretArn']
            token = event['Step1']['ClientRequestToken']
            step = event['Step1']['Step']
            
            secrets_client = boto3.client('secretsmanager')
            rds_client = boto3.client('rds')
            
            if step == "createSecret":
                create_secret(secrets_client, secret_arn, token)
            elif step == "setSecret":
                set_secret(secrets_client, rds_client, secret_arn, token)
            elif step == "testSecret":
                test_secret(secrets_client, secret_arn, token)
            elif step == "finishSecret":
                finish_secret(secrets_client, secret_arn, token)
            
            return {"statusCode": 200}

        def create_secret(secrets_client, secret_arn, token):
            """新しいパスワードを生成"""
            import string
            import random
            
            # 現在の秘密情報を取得
            current_secret = secrets_client.get_secret_value(
                SecretId=secret_arn,
                VersionStage="AWSCURRENT"
            )
            
            secret_dict = json.loads(current_secret["SecretString"])
            
            # 新しいパスワードを生成
            password_length = 20
            new_password = ''.join(random.choice(
                string.ascii_letters + string.digits
            ) for _ in range(password_length))
            
            secret_dict["password"] = new_password
            
            # 新しいバージョンを作成
            secrets_client.put_secret_value(
                SecretId=secret_arn,
                ClientRequestToken=token,
                SecretString=json.dumps(secret_dict),
                VersionStages=["AWSPENDING"]
            )

    Role: !GetAtt RotationLambdaRole.Arn
    Environment:
      Variables:
        SECRETS_MANAGER_ENDPOINT: !Sub "https://secretsmanager.${AWS::Region}.amazonaws.com"
```

#### アプリケーション設定管理

```python
class AppConfig:
    """アプリケーション設定管理クラス"""

    def __init__(self, environment='dev'):
        self.environment = environment
        self.secrets_manager = SecretsManager()
        self._config_cache = {}

    def get_config(self, config_type):
        """設定情報の取得"""
        cache_key = f"{self.environment}/{config_type}"

        if cache_key in self._config_cache:
            return self._config_cache[cache_key]

        secret_name = f"{self.environment}/myapp/{config_type}"
        config = self.secrets_manager.get_secret(secret_name)

        self._config_cache[cache_key] = config
        return config

    def get_database_config(self):
        """データベース設定"""
        return self.get_config('database')

    def get_redis_config(self):
        """Redis設定"""
        return self.get_config('redis')

    def get_external_api_config(self):
        """外部API設定"""
        return self.get_config('external-apis')

    def get_encryption_keys(self):
        """暗号化キー"""
        return self.get_config('encryption-keys')

# Django/Flask での使用例
class DatabaseConfig:
    def __init__(self):
        self.app_config = AppConfig(os.environ.get('ENVIRONMENT', 'dev'))

    def get_database_url(self):
        """データベースURL構築"""
        db_config = self.app_config.get_database_config()

        return (
            f"postgresql://{db_config['username']}:"
            f"{db_config['password']}@{db_config['host']}:"
            f"{db_config['port']}/{db_config['database']}"
        )

# 設定の使用
database_config = DatabaseConfig()
DATABASE_URL = database_config.get_database_url()
```

### セキュリティベストプラクティス

#### IAM ポリシー設計

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["secretsmanager:GetSecretValue"],
      "Resource": ["arn:aws:secretsmanager:region:account:secret:prod/myapp/*"],
      "Condition": {
        "StringEquals": {
          "secretsmanager:ResourceTag/Environment": "production",
          "secretsmanager:ResourceTag/Application": "myapp"
        }
      }
    },
    {
      "Effect": "Deny",
      "Action": ["secretsmanager:GetSecretValue"],
      "Resource": ["arn:aws:secretsmanager:region:account:secret:*/admin/*"]
    }
  ]
}
```

### コスト最適化

#### リージョン間レプリケーション

```python
def setup_cross_region_replication():
    """災害復旧のためのクロスリージョンレプリケーション"""

    # プライマリリージョンでの設定
    primary_client = boto3.client('secretsmanager', region_name='ap-northeast-1')

    secret_arn = 'arn:aws:secretsmanager:ap-northeast-1:account:secret:prod/myapp/database'

    # セカンダリリージョンへのレプリケーション設定
    primary_client.replicate_secret_to_regions(
        SecretId=secret_arn,
        AddReplicaRegions=[
            {
                'Region': 'ap-southeast-1',
                'KmsKeyId': 'alias/aws/secretsmanager'
            }
        ]
    )
```

---

## AWS Security Token Service (STS)

![](SecurityTokenService.jpeg)

### 概要

一時的なセキュリティ認証情報を発行するサービスです。

### 主な機能

- 一時的に AWS リソースへアクセスするためのセキュリティ認証情報を発行

### セキュリティメリット

一時的に発行することで、キーの流出リスクを最小限に抑えることができます。

---

## 統合アーキテクチャパターン

### マイクロサービスアーキテクチャ実装例

```typescript
import { SNSClient, CreateTopicCommand } from "@aws-sdk/client-sns";
import { SQSClient, CreateQueueCommand } from "@aws-sdk/client-sqs";
import {
  CloudWatchClient,
  PutMetricDataCommand,
} from "@aws-sdk/client-cloudwatch";
import {
  CloudFormationClient,
  CreateStackCommand,
} from "@aws-sdk/client-cloudformation";
import { BackupClient, CreateBackupPlanCommand } from "@aws-sdk/client-backup";

interface ServiceTopology {
  [serviceName: string]: {
    topic_arn: string;
    queue_url: string;
    dlq_url: string;
  };
}

class MicroservicesOrchestrator {
  private sns: SNSClient;
  private sqs: SQSClient;
  private secrets: SecretsManager;
  private cloudwatch: CloudWatchClient;

  constructor() {
    this.sns = new SNSClient({ region: "ap-northeast-1" });
    this.sqs = new SQSClient({ region: "ap-northeast-1" });
    this.secrets = new SecretsManager();
    this.cloudwatch = new CloudWatchClient({ region: "ap-northeast-1" });
  }

  async setupServiceMesh(): Promise<ServiceTopology> {
    const services = [
      "user-service",
      "order-service",
      "payment-service",
      "inventory-service",
      "notification-service",
    ];

    const serviceTopology: ServiceTopology = {};

    for (const service of services) {
      // サービス専用のSNSトピック作成
      const createTopicCommand = new CreateTopicCommand({
        Name: `${service}-events`,
        Attributes: {
          DisplayName: `${
            service.charAt(0).toUpperCase() + service.slice(1)
          } Events`,
          KmsMasterKeyId: "alias/microservices-encryption",
        },
      });

      const topicResponse = await this.sns.send(createTopicCommand);

      // 処理キューとDLQ作成
      const mainQueueCommand = new CreateQueueCommand({
        QueueName: `${service}-processing`,
        Attributes: {
          VisibilityTimeoutSeconds: "300",
          MessageRetentionPeriod: "1209600",
          KmsMasterKeyId: "alias/microservices-encryption",
        },
      });

      const mainQueue = await this.sqs.send(mainQueueCommand);

      const dlqCommand = new CreateQueueCommand({
        QueueName: `${service}-dlq`,
        Attributes: {
          MessageRetentionPeriod: "1209600",
        },
      });

      const dlq = await this.sqs.send(dlqCommand);

      serviceTopology[service] = {
        topic_arn: topicResponse.TopicArn!,
        queue_url: mainQueue.QueueUrl!,
        dlq_url: dlq.QueueUrl!,
      };
    }

    return serviceTopology;
  }
}

class ServiceHealthMonitor {
  private cloudwatch: CloudWatchClient;
  private sns: SNSClient;

  constructor() {
    this.cloudwatch = new CloudWatchClient({ region: "ap-northeast-1" });
    this.sns = new SNSClient({ region: "ap-northeast-1" });
  }

  async setupComprehensiveMonitoring(
    serviceTopology: ServiceTopology
  ): Promise<void> {
    for (const [service, config] of Object.entries(serviceTopology)) {
      // サービス固有メトリクス
      await this.createServiceMetrics(service);

      // SLA監視アラーム
      await this.createSlaAlarms(service);

      // キューの滞留監視
      await this.createQueueMonitoring(service, config.queue_url);
    }
  }

  private async createServiceMetrics(serviceName: string): Promise<void> {
    const metrics = [
      "RequestCount",
      "ErrorRate",
      "ResponseTime",
      "DatabaseConnections",
      "CacheHitRate",
    ];

    for (const metric of metrics) {
      const command = new PutMetricDataCommand({
        Namespace: `Microservices/${serviceName}`,
        MetricData: [
          {
            MetricName: metric,
            Value: 0,
            Unit: metric === "RequestCount" ? "Count" : "Percent",
          },
        ],
      });

      await this.cloudwatch.send(command);
    }
  }

  private async createSlaAlarms(serviceName: string): Promise<void> {
    // SLA監視アラームの実装
    console.log(`SLA監視アラーム作成: ${serviceName}`);
  }

  private async createQueueMonitoring(
    serviceName: string,
    queueUrl: string
  ): Promise<void> {
    // キューの滞留監視の実装
    console.log(`キュー監視設定: ${serviceName} - ${queueUrl}`);
  }
}

// デプロイメント自動化
class InfrastructureAsCodeManager {
  private cloudformation: CloudFormationClient;
  private secrets: SecretsManager;

  constructor() {
    this.cloudformation = new CloudFormationClient({
      region: "ap-northeast-1",
    });
    this.secrets = new SecretsManager();
  }

  async deployMicroserviceStack(
    serviceName: string,
    environment: string
  ): Promise<void> {
    const template = {
      AWSTemplateFormatVersion: "2010-09-09",
      Parameters: {
        ServiceName: { Type: "String", Default: serviceName },
        Environment: { Type: "String", Default: environment },
      },
      Resources: {
        // ECS Fargate サービス
        [`${serviceName}Service`]: {
          Type: "AWS::ECS::Service",
          Properties: {
            Cluster: { Ref: "ECSCluster" },
            TaskDefinition: { Ref: `${serviceName}TaskDefinition` },
            DesiredCount: 2,
            LaunchType: "FARGATE",
            NetworkConfiguration: {
              AwsvpcConfiguration: {
                Subnets: [{ Ref: "PrivateSubnet1" }, { Ref: "PrivateSubnet2" }],
                SecurityGroups: [{ Ref: `${serviceName}SecurityGroup` }],
                AssignPublicIp: "DISABLED",
              },
            },
            LoadBalancers: [
              {
                TargetGroupArn: { Ref: `${serviceName}TargetGroup` },
                ContainerName: serviceName,
                ContainerPort: 8080,
              },
            ],
          },
        },

        // Application Load Balancer
        [`${serviceName}ALB`]: {
          Type: "AWS::ElasticLoadBalancingV2::LoadBalancer",
          Properties: {
            Type: "application",
            Scheme: "internal",
            Subnets: [{ Ref: "PrivateSubnet1" }, { Ref: "PrivateSubnet2" }],
            SecurityGroups: [{ Ref: `${serviceName}ALBSecurityGroup` }],
          },
        },

        // API Gateway統合
        [`${serviceName}ApiGateway`]: {
          Type: "AWS::ApiGateway::RestApi",
          Properties: {
            Name: `${serviceName}-api`,
            EndpointConfiguration: { Types: ["REGIONAL"] },
            Policy: {
              Version: "2012-10-17",
              Statement: [
                {
                  Effect: "Allow",
                  Principal: "*",
                  Action: "execute-api:Invoke",
                  Resource: "*",
                  Condition: {
                    StringEquals: {
                      "aws:PrincipalTag/Environment": environment,
                    },
                  },
                },
              ],
            },
          },
        },
      },
    };

    // スタックのデプロイ
    const stackName = `${serviceName}-${environment}`;

    try {
      const command = new CreateStackCommand({
        StackName: stackName,
        TemplateBody: JSON.stringify(template),
        Parameters: [
          { ParameterKey: "ServiceName", ParameterValue: serviceName },
          { ParameterKey: "Environment", ParameterValue: environment },
        ],
        Capabilities: ["CAPABILITY_IAM"],
        Tags: [
          { Key: "Service", Value: serviceName },
          { Key: "Environment", Value: environment },
          { Key: "ManagedBy", Value: "CloudFormation" },
        ],
      });

      await this.cloudformation.send(command);
      console.log(`スタック作成開始: ${stackName}`);
    } catch (error) {
      console.error(`スタック作成エラー: ${error}`);
    }
  }
}

// 災害復旧とバックアップ
class DisasterRecoveryManager {
  private backup: BackupClient;

  constructor() {
    this.backup = new BackupClient({ region: "ap-northeast-1" });
  }

  async setupMultiRegionBackup(): Promise<string> {
    const backupPlan = {
      BackupPlanName: "microservices-backup-plan",
      Rules: [
        {
          RuleName: "daily-backup",
          TargetBackupVault: "default",
          ScheduleExpression: "cron(0 2 * * ? *)",
          Lifecycle: {
            DeleteAfterDays: 30,
            MoveToColdStorageAfterDays: 7,
          },
          CopyActions: [
            {
              DestinationBackupVault: "cross-region-vault",
              Lifecycle: {
                DeleteAfterDays: 90,
                MoveToColdStorageAfterDays: 30,
              },
            },
          ],
        },
      ],
    };

    const command = new CreateBackupPlanCommand({ BackupPlan: backupPlan });
    const response = await this.backup.send(command);
    return response.BackupPlanId!;
  }
}

// 使用例
async function deployEnterpriseMicroservices(): Promise<void> {
  // オーケストレーター初期化
  const orchestrator = new MicroservicesOrchestrator();
  const serviceTopology = await orchestrator.setupServiceMesh();

  // 監視設定
  const monitor = new ServiceHealthMonitor();
  await monitor.setupComprehensiveMonitoring(serviceTopology);

  // インフラストラクチャデプロイ
  const iacManager = new InfrastructureAsCodeManager();

  for (const service of Object.keys(serviceTopology)) {
    await iacManager.deployMicroserviceStack(service, "production");
  }

  // 災害復旧設定
  const drManager = new DisasterRecoveryManager();
  const backupPlanId = await drManager.setupMultiRegionBackup();

  console.log(`マイクロサービス環境のデプロイ完了`);
  console.log(`サービス数: ${Object.keys(serviceTopology).length}`);
  console.log(`バックアッププランID: ${backupPlanId}`);
}

// 実行
deployEnterpriseMicroservices().catch(console.error);
```

### ベストプラクティス総まとめ

#### セキュリティ

1. **最小権限の原則**: IAM ポリシーは必要最小限の権限のみ
2. **暗号化の徹底**: 保存時・転送時両方の暗号化
3. **監査ログの確保**: CloudTrail による操作ログの記録
4. **秘密情報の分離**: Secrets Manager による認証情報管理

#### 可用性・信頼性

1. **マルチ AZ 構成**: 単一障害点の排除
2. **自動スケーリング**: 負荷に応じたリソースの自動調整
3. **ヘルスチェック**: 定期的な死活監視
4. **データバックアップ**: 定期的なバックアップとリストア訓練

#### パフォーマンス

1. **キャッシュ戦略**: ElastiCache を活用したデータキャッシュ
2. **CDN 活用**: CloudFront による静的コンテンツ配信
3. **データベース最適化**: インデックス設計とクエリ最適化
4. **非同期処理**: SQS/SNS による負荷分散

#### コスト最適化

1. **リソース監視**: Cost Explorer による定期的なコスト分析
2. **適切なインスタンスサイズ**: Compute Optimizer の推奨事項活用
3. **リザーブドインスタンス**: 長期利用リソースのコスト削減
4. **ライフサイクル管理**: S3 のライフサイクルポリシー設定
