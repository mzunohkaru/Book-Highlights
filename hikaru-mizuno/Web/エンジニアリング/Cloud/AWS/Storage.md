# AWS Storage サービス

## S3 (Simple Storage Service)

- 耐久性が高く大量のデータを保存しやすい
- オブジェクトストレージサービス

### S3 Storage Class

- 用途によって選択できる複数のストレージの種類が存在します
- [参考資料](https://qiita.com/shimajiri/items/01ab61a08b58c2cb8acf)

### S3 オブジェクト

- ファイルのこと

### S3 バケット

- オブジェクトを保存するコンテナ

## EBS (Elastic Block Store)

- EC2 のストレージとして使用
- 処理速度を高速化したいデータの保存に適している
- [参考資料：S3 vs EBS](https://bcblog.sios.jp/what-is-amazon-ebs/)

### EBS Volumes

- ファイルシステムの構築やデータベースを実行できる

### EBS Volumes gp2

- SSD ボリューム

### EBS Snapshot

- EBS Volumes をバックアップする

## EFS (Elastic File System)

- EC2 インスタンスやオンプレミスのサーバとも同時にアクセス可能なファイルストレージサービス
- [参考資料：EFS vs EBS](https://recipe.kc-cloud.jp/archives/11974/)

## FSx for Windows

- Windows Server 上に構築されたフルマネージド型の共有ストレージ
- 構築基盤に Microsoft Windows ファイルシステムを採用している
- 複数の Windows サーバにマウントできるストレージ
- 参考資料：
  - [NTT 東日本ビジネス](https://business.ntt-east.co.jp/content/cloudsolution/column-68.html)
  - [NURO Biz](https://biz.nuro.jp/column/aws-mama-024/)

## ファイルシステム

- ファイルを階層構造に格納してラベルをつけ、必要なときに使えるように管理
- ファイルを操作するためのインターフェースも提供している
