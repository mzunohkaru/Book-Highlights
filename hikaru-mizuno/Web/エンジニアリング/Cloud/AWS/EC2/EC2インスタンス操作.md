## 概要
SSM経由でEC2インスタンス（パブリック）へSSH接続

### 手順
1. EC2インスタンス起動
	- Amazon Linux2を利用する
		- SSMエージェントがデフォルトでインストールされている
2. EC2インスタンスへ  IAM Role = `AmazonSSMManagedInstanceCore` をアサイン
3. SSH接続
```shell
# TrainingKeyPair.pemに対する権限を設定
chmod 600 ~/.ssh/TrainingKeyPair.pem
# 権限確認
ls -la ~/.ssh/TrainingKeyPair.pem
# SSH接続
ssh -i ~/.ssh/TrainingKeyPair.pem ec2-user@{パブリック IPv4 アドレス}
```
4. エラー「SSMエージェントはオンラインではありません」の対処
	- EC2インスタンスにSSH接続して以下を実行
```shell
# SSMエージェントの状態確認 
sudo systemctl status amazon-ssm-agent 
# SSMエージェントの再起動 
sudo systemctl restart amazon-ssm-agent 
# 自動起動の有効化 
sudo systemctl enable amazon-ssm-agent
```


---
## 概要
踏み台サーバーから、EC2インスタンス（プライベート）へ SSH接続

### 手順
1. EC2インスタンス（踏み台 & プライベート）起動
2. EC2インスタンス（踏み台 & プライベート）へ  IAM Role = `AmazonSSMManagedInstanceCore` をアサイン
3. 踏み台へSSH接続
```shell
# TrainingKeyPair.pemに対する権限を設定
chmod 600 ~/.ssh/TrainingKeyPair.pem
# 権限確認
ls -la ~/.ssh/TrainingKeyPair.pem
# SSH接続
ssh -i ~/.ssh/TrainingKeyPair.pem ec2-user@{パブリック IPv4 アドレス}
```
4. 踏み台からEC2インスタンス（プライベート）に接続
```shell
# SSHエージェントに秘密鍵を登録
ssh-add ~/.ssh/TrainingKeyPair.pem
# 登録確認
ssh-add -l
# SSHエージェントに秘密鍵をEC2インスタンス（プライベート）に転送するための -A オプション
ssh -A -i ~/.ssh/TrainingKeyPair.pem ec2-user@{パブリック IPv4 アドレス}
# EC2インスタンス（プライベート）に接続
ssh ec2-user@{プライベート IPv4 アドレス}
```

例）
```shell
# 踏み台へ接続
(base) mHikaru@~ $ ssh -A -i ~/.ssh/TrainingKeyPair.pem ec2-user@3.147.7.236
# EC2インスタンス（プライベート）に接続
[ec2-user@ip-10-0-100-197 ~]$ ssh ec2-user@10.0.200.176
# EC2インスタンス（プライベート）に接続完了
[ec2-user@ip-10-0-200-176 ~]$ 
```

---
## 概要
 EC2インスタンスでプロジェクトを構築

### 手順
1.  セットアップ
```shell
# パッケージマネージャーの更新
sudo dnf -y update

# Dockerのインストール
sudo dnf install -y docker
sudo systemctl enable --now docker
sudo usermod -aG docker ec2-user
source ~/.bashrc
docker info

# Docker-composeのインストール
DOCKER_CONFIG=${DOCKER_CONFIG:-/usr/local/lib/docker}
sudo mkdir -p $DOCKER_CONFIG/cli-plugins
sudo curl -SL https://github.com/docker/compose/releases/download/v2.17.3/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
source ~/.bashrc
docker compose version
```
2. ローカルファイルをアップロード
	- 補足
		- node_modulesなどのディレクトを削除する
```shell
# ローカルファイルパス = Downloads/chat_prj
# パブリック IP v4 = 18.118.120.115
scp -r -i ~/.ssh/TrainingKeyPair.pem ~/[ローカルファイルパス] ec2-user@[パブリック IP v4]:/home/ec2-user/
```
3. Docker Compose起動
	- 補足
		- リポジトリ
			- https://github.com/mzunohkaru/Nuxt-Sample-Post
		- セキュリティグループ
			- TCP 3000ポートを解放する
```shell
cd chat_prj
docker compose up
```
