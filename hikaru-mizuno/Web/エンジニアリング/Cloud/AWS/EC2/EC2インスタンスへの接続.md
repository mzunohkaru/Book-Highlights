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

