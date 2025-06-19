# DB

## AWS RDS サービス

### RDS

**概要**

- AWS の管理されたリレーショナルデータベースサービス
- MySQL、PostgreSQL、MariaDB、Oracle、SQL Server をサポート
- 自動バックアップ、パッチ適用、監視機能を提供

**参考資料（公式 PDF）**

- [AWS BlackBelt RDS](https://d1.awsstatic.com/webinars/jp/pdf/services/20180425_AWS-BlackBelt_RDS.pdf)

#### RDS インスタンスクラス選択指針

**汎用目的 (General Purpose)**

- `db.t3.micro` ~ `db.t3.2xlarge`: 開発・テスト環境、軽負荷なワークロード
- `db.m5.large` ~ `db.m5.24xlarge`: バランスの取れた本番ワークロード

**メモリ最適化 (Memory Optimized)**

- `db.r5.large` ~ `db.r5.24xlarge`: インメモリキャッシュ、リアルタイム分析
- `db.x1e.xlarge` ~ `db.x1e.32xlarge`: 大容量メモリが必要なアプリケーション

**コンピューティング最適化 (Compute Optimized)**

- `db.c5.large` ~ `db.c5.24xlarge`: CPU 集約的なワークロード

#### ストレージタイプの選択

**汎用 SSD (gp2/gp3)**

- バースト可能な IOPS（最大 3,000 IOPS）
- コストパフォーマンスが良い
- 中小規模のワークロードに適している

**プロビジョンド IOPS SSD (io1/io2)**

- 一貫した高 IOPS を提供（最大 64,000 IOPS）
- ミッションクリティカルなアプリケーション向け
- IOPS とストレージ容量を独立して設定可能

**磁気ストレージ (Magnetic)**

- 下位互換性のために提供（新規作成は非推奨）
- 低頻度アクセスのワークロード

### RDS リードレプリカ

- 読み取り専用の DB
- 非同期レプリケーションを使用
- 読み取りトラフィックの分散によるパフォーマンス向上

#### リードレプリカのベストプラクティス

**パフォーマンス最適化**

- アプリケーションレベルでの読み書き分離
- 接続プールの適切な設定
- レプリケーション遅延の監視

**監視項目**

- `ReplicaLag`: レプリケーション遅延時間
- `DatabaseConnections`: 接続数
- `ReadIOPS/WriteIOPS`: I/O 操作数

**制限事項**

- 最大 5 つのリードレプリカまで
- 異なる AZ、リージョンへの配置可能
- 自動バックアップが有効である必要がある

**参考資料**

- [RDS リードレプリカの理解と活用](https://www.stylez.co.jp/columns/understand_and_utilize_the_features_of_amazon_rds/#:~:text=%E3%83%AA%E3%83%BC%E3%83%89%E3%83%AC%E3%83%97%E3%83%AA%E3%82%AB%E3%81%AF%E8%AA%AD%E3%81%BF%E5%8F%96%E3%82%8A%E5%B0%82%E7%94%A8,%E3%81%A7%E8%A1%8C%E3%82%8F%E3%82%8C%E3%82%8B%E7%82%B9%E3%81%A7%E3%81%99%E3%80%82)

### RDS Proxy
![](50B3D620-C125-4103-A9AF-123166295908.png.jpeg)

**概要**

- 接続プーリング機能
- 接続の確立・切断に伴うオーバーヘッドを削減
- 多数の同時接続を効率的に管理

#### RDS Proxy の技術的メリット

**接続管理の最適化**

- 最大 66% の接続時間短縮
- コネクションプールサイズの動的調整
- アイドル接続の自動切断

**高可用性の向上**

- フェイルオーバー時間を最大 79% 短縮
- アプリケーションレベルでの透過的な切り替え
- 接続状態の保持

**セキュリティ強化**

- AWS Secrets Manager との統合
- 資格情報の自動ローテーション
- VPC 内でのプライベート接続

#### RDS Proxy の認証機能

**IAM 認証**

- データベースサーバーがネイティブのユーザー名/パスワード認証しか対応していない場合でも、RDS Proxy への接続は IAM 認証を利用可能
- トークンベースの認証（15 分間有効）
- 細かい権限制御が可能

**TLS/SSL 対応**

- RDS Proxy とアプリケーション間の接続で TLS1.2 を利用可能
- 強制的な暗号化接続の設定
- 証明書の自動管理

#### 使用すべきケース

**Lambda 関数との組み合わせ**

- サーバーレスアプリケーションでの接続数制御
- 短時間で大量の接続が発生する場合

**マイクロサービスアーキテクチャ**

- 複数サービスからの DB 接続の集約
- 接続数の上限管理

### フェイルオーバーによる高可用性

- 元のデータベースインスタンスが使用できなくなったときに、別のインスタンスに置き換える高可用性の機能

#### Multi-AZ 配置

**同期レプリケーション**

- プライマリとスタンバイ間でデータを同期
- 自動フェイルオーバー（通常 1-2 分）
- アプリケーションからは単一エンドポイント

**フェイルオーバートリガー**

- プライマリ DB インスタンスの障害
- アベイラビリティゾーンの停止
- プライマリ DB インスタンスのネットワーク接続性の問題
- コンピューティングユニットの障害

**監視とアラート**

- CloudWatch メトリクス
- RDS イベント通知
- SNS との統合

**参考資料**

- [フェイルオーバーの詳細](https://www.sunnycloud.jp/column/20210502-01/)
- [RDS Proxy のアーキテクチャ](https://dev.classmethod.jp/articles/do-you-really-need-rds-proxy/)

### Aurora

**概要**

- AWS クラウド向けに構築されたリレーショナルデータベース
- MySQL および PostgreSQL と互換性
- 最大 5 倍（MySQL）、3 倍（PostgreSQL）のパフォーマンス

#### Aurora の技術的特徴

**ストレージアーキテクチャ**

- 分散型ストレージシステム
- 自動的な 6 つのコピー（3 つの AZ にそれぞれ 2 つ）
- 自動修復機能
- 10GB から 128TB まで自動拡張

**Aurora Serverless**

- オンデマンドの自動スケーリング
- 使用量に基づく課金
- 開発・テスト環境、断続的なワークロードに最適

#### Aurora Global Database

**グローバル展開**

- 複数リージョンでの展開
- 1 秒未満のクロスリージョンレプリケーション
- RPO < 1 秒、RTO < 1 分

**パフォーマンス最適化**

- 読み取り専用ワークロードの地理的分散
- ローカル読み取りによるレイテンシ削減

**参考資料**

- [Amazon Aurora 紹介](https://qiita.com/kuromame1020611/items/83643f14f0dd6ecd9dde)

---

## パフォーマンス最適化戦略

### クエリ最適化

#### 実行計画の分析

**EXPLAIN の活用**

```sql
-- PostgreSQL の場合
EXPLAIN (ANALYZE, BUFFERS) SELECT * FROM users WHERE email = 'user@example.com';

-- MySQL の場合
EXPLAIN FORMAT=JSON SELECT * FROM users WHERE email = 'user@example.com';
```

**Performance Insights の活用**

- Top SQL 文の特定
- 待機イベントの分析
- データベース負荷の可視化

#### インデックス戦略

**複合インデックスの設計原則**

1. 選択性の高いカラムを先頭に配置
2. WHERE 句の条件に合わせた順序
3. カバリングインデックスの検討

**インデックスの種類と使い分け**

- B-tree インデックス：範囲検索、等価検索
- Hash インデックス：等価検索のみ（PostgreSQL）
- GIN/GiST インデックス：全文検索、配列データ

### 接続管理とプーリング

#### 接続プールの設定

**アプリケーションレベル**

```python
# Python の例（SQLAlchemy）
engine = create_engine(
    'postgresql://user:pass@host/db',
    pool_size=20,
    max_overflow=30,
    pool_pre_ping=True,
    pool_recycle=3600
)
```

**推奨設定値**

- pool_size: CPU コア数 × 2 〜 4
- max_overflow: pool_size の 1.5 〜 2 倍
- connection_timeout: 5-10 秒

#### 監視すべきメトリクス

**RDS CloudWatch メトリクス**

- `DatabaseConnections`: 現在の接続数
- `CPUUtilization`: CPU 使用率
- `ReadLatency/WriteLatency`: I/O レイテンシ
- `FreeableMemory`: 使用可能メモリ

---

## 運用ベストプラクティス

### バックアップ戦略

#### 自動バックアップの設定

**バックアップウィンドウ**

- アプリケーションの負荷が低い時間帯に設定
- 通常 30 分程度の時間枠を確保
- Multi-AZ 配置時はスタンバイからバックアップ

**ポイントインタイムリカバリ (PITR)**

- 最大 35 日間の保持期間
- 1 秒単位での復旧ポイント指定
- トランザクションログの連続バックアップ

#### 手動スナップショット

**用途**

- メジャーアップデート前
- 大規模なデータ変更前
- 長期保存が必要なデータ

**命名規則の例**

```
{環境}-{DB名}-{YYYY-MM-DD}-{目的}
prod-userdb-2024-01-15-before-migration
```

### セキュリティベストプラクティス

#### ネットワークセキュリティ

**VPC 配置**

- プライベートサブネットへの配置
- セキュリティグループでの厳格なアクセス制御
- NACLs での追加保護

**接続暗号化**

- 保存時暗号化（KMS キー管理）
- 転送時暗号化（SSL/TLS）
- RDS Proxy 経由での暗号化接続

#### アクセス管理

**IAM データベース認証**

```bash
# IAM 認証を使用した接続例
aws rds generate-db-auth-token \
    --hostname mydb.cluster-xxx.amazonaws.com \
    --port 5432 \
    --region us-east-1 \
    --username db_user
```

**最小権限の原則**

- アプリケーション専用ユーザーの作成
- 必要最小限の権限付与
- 定期的な権限レビュー

### 監視とアラート

#### CloudWatch アラーム設定

**必須アラーム**

```yaml
# CPU 使用率
CPUUtilization:
  threshold: 80%
  evaluation_periods: 2

# 接続数
DatabaseConnections:
  threshold: 80% of max_connections

# 空きメモリ
FreeableMemory:
  threshold: < 512MB
```

#### Performance Insights

**活用ポイント**

- Top SQL の定期的な確認
- 待機イベントの分析
- 負荷パターンの把握

---

## データベース関連用語集

### パフォーマンス関連

#### プロビジョンド IOPS

- 1 秒間の入力や出力がどのくらいできるのかを指定する機能
- I/O 集約的なワークロードで一貫したパフォーマンスを保証
- ストレージ容量とは独立して IOPS を設定可能

#### レプリケーション

- 複製品
- データベースの複製を作成・同期する仕組み

**レプリケーションの種類**

- **同期レプリケーション**: データの整合性重視、レイテンシが高い
- **非同期レプリケーション**: パフォーマンス重視、データ損失のリスクあり
- **準同期レプリケーション**: 中間的なアプローチ

#### キャッシュ戦略

**Redis/ElastiCache の活用**

- ルックアサイドパターン
- ライトスルーパターン
- ライトビハインドパターン

**アプリケーションレベルキャッシュ**

- クエリ結果キャッシュ
- オブジェクトキャッシュ
- セッションキャッシュ

### システム運用関連

#### DR 環境

- 災害に備えたシステムや体制
- Disaster Recovery（災害復旧）環境

**DR 戦略の種類**

- **コールドスタンバイ**: 低コスト、復旧時間長
- **ウォームスタンバイ**: 中程度のコスト、中程度の復旧時間
- **ホットスタンバイ**: 高コスト、即座の復旧

#### RTO と RPO

**RTO (Recovery Time Objective)**

- システム復旧までの目標時間
- ビジネス要件に基づいて設定

**RPO (Recovery Point Objective)**

- データ損失許容時間
- バックアップ頻度を決定する要因

### データベース設計・管理

#### データベース制約

- 格納されるデータへの制約条件
- 例：Not Null 制約の項目に Null を許容しない

**制約の種類**

- **主キー制約 (PRIMARY KEY)**: 一意性と Not Null を保証
- **外部キー制約 (FOREIGN KEY)**: 参照整合性を保証
- **ユニーク制約 (UNIQUE)**: 重複を防止
- **チェック制約 (CHECK)**: 値の範囲や条件を制御

#### データベースインデックス

- テーブル内の特定の列を認識できる値
- データの検索速度を向上させるため、データ本体とは別に作成する索引データ

**インデックス設計の原則**

- 高頻度の検索条件にインデックスを作成
- 更新頻度とのトレードオフを考慮
- インデックスのメンテナンスコストを把握

#### データベースプロシージャ

- データベースに対する複数の処理を 1 つのプログラムにまとめたもの
- 一度の呼び出しでまとめて実行可能
- プロシージャの登録・保存はデータベースで行う

**ストアドプロシージャのメリット**

- ネットワークトラフィックの削減
- ビジネスロジックの集約
- セキュリティの向上

#### データベースビュー

- 1 つ以上の表から任意のデータを選択し、カスタマイズしたもの
- SELECT で作成した一時的な表
- 仮想表とも呼ばれる

**ビューの活用場面**

- 複雑な JOIN クエリの簡素化
- セキュリティ向上（列の制限）
- データの論理的な整理

#### データベースオブジェクト

- データの論理構造（スキーマ）の集まりに対応付けられたものと、そうでないものがある

**主要なデータベースオブジェクト**

- テーブル、インデックス、ビュー
- ストアドプロシージャ、ファンクション
- トリガー、シーケンス

#### スキーマ

- データベースやユーザーによって所有され、そのユーザーと同じ名前を持つ
- ユーザーはそれぞれ 1 つのスキーマを所有する
- スキーマオブジェクトの作成と操作は SQL で行える

**スキーマ設計のベストプラクティス**

- 環境別スキーマの分離
- 権限管理の簡素化
- 名前空間の整理

---

## トラブルシューティング

### よくある問題と対処法

#### 接続数上限エラー

**症状**

```
ERROR: too many connections for role "username"
FATAL: remaining connection slots are reserved
```

**対処法**

1. 現在の接続数確認

```sql
SELECT count(*) FROM pg_stat_activity;
```

2. アイドル接続の確認

```sql
SELECT state, count(*) FROM pg_stat_activity GROUP BY state;
```

3. 接続プールの設定見直し
4. RDS Proxy の導入検討

#### スロークエリの対処

**特定方法**

1. Performance Insights での確認
2. ログ分析

```sql
-- PostgreSQL でスロークエリログを有効化
ALTER SYSTEM SET log_min_duration_statement = 1000;
SELECT pg_reload_conf();
```

**最適化手順**

1. 実行計画の確認
2. インデックスの見直し
3. クエリの書き換え
4. パーティショニングの検討

#### デッドロックの解決

**検出と分析**

```sql
-- PostgreSQL でデッドロック情報を確認
SELECT * FROM pg_locks WHERE NOT granted;
```

**予防策**

- 一貫したロック順序の維持
- トランザクション時間の短縮
- 適切な分離レベルの設定

---

## 実践的な設定例

### 本番環境推奨設定

#### RDS パラメータグループ

**PostgreSQL の場合**

```ini
# 接続設定
max_connections = 200
shared_preload_libraries = 'pg_stat_statements'

# メモリ設定
shared_buffers = {DBInstanceClassMemory/4}
effective_cache_size = {DBInstanceClassMemory*3/4}

# ログ設定
log_statement = 'mod'
log_min_duration_statement = 5000
log_lock_waits = on

# チェックポイント設定
checkpoint_completion_target = 0.9
wal_buffers = 16MB
```

**MySQL の場合**

```ini
# InnoDB 設定
innodb_buffer_pool_size = {DBInstanceClassMemory*3/4}
innodb_log_file_size = 512MB
innodb_flush_log_at_trx_commit = 1

# クエリキャッシュ
query_cache_type = 1
query_cache_size = 128MB

# ログ設定
slow_query_log = 1
long_query_time = 5
log_queries_not_using_indexes = 1
```

#### セキュリティグループ設定

```yaml
# アプリケーション層からのアクセス
Type: Custom TCP
Protocol: TCP
Port Range: 5432  # PostgreSQL の場合
Source: sg-xxxxx  # アプリケーションサーバーのSG

# 管理用アクセス（踏み台サーバー経由）
Type: Custom TCP
Protocol: TCP
Port Range: 5432
Source: sg-yyyyy  # 踏み台サーバーのSG
```

### モニタリング設定例

#### CloudWatch ダッシュボード

```json
{
  "widgets": [
    {
      "type": "metric",
      "properties": {
        "metrics": [
          ["AWS/RDS", "CPUUtilization", "DBInstanceIdentifier", "mydb"],
          [".", "DatabaseConnections", ".", "."],
          [".", "FreeableMemory", ".", "."]
        ],
        "period": 300,
        "stat": "Average",
        "region": "ap-northeast-1",
        "title": "RDS メトリクス"
      }
    }
  ]
}
```

#### アラート設定例

```yaml
HighCPUAlarm:
  Type: AWS::CloudWatch::Alarm
  Properties:
    AlarmName: RDS-HighCPU
    MetricName: CPUUtilization
    Namespace: AWS/RDS
    Statistic: Average
    Period: 300
    EvaluationPeriods: 2
    Threshold: 80
    ComparisonOperator: GreaterThanThreshold
    AlarmActions:
      - !Ref SNSTopic
```
