## アーキテクチャ


## 手順

1. 踏み台サーバー構築
```shell
# MySQLのインストール
sudo dnf update -y
sudo dnf remove -y mariadb-libs
sudo dnf install yum-utils -y
sudo dnf -y localinstall  https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
sudo dnf install -y mysql-community-client
mysql --version

# MySQLに接続するコマンド
# [RDSエンドポイント] = training-udemy-instance.chs08coam2o7.us-east-2.rds.amazonaws.com
mysql -h [RDSエンドポイント] -u [DBマスターユーザー] -p [DB Name]

# SQL
CREATE TABLE ...
# 文字コード設定
SET NAMES utf8mb4;
# SQL
INSERT INTO ...
```

2. RDS構築
3. アプリケーションサーバー構築
```shell
# dockerのインストール
sudo dnf install -y docker
sudo systemctl enable --now docker
sudo usermod -aG docker ec2-user
source ~/.bashrc
docker info

# docker-composeのインストール
DOCKER_CONFIG=${DOCKER_CONFIG:-/usr/local/lib/docker}
sudo mkdir -p $DOCKER_CONFIG/cli-plugins
sudo curl -SL https://github.com/docker/compose/releases/download/v2.17.3/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
source ~/.bashrc
docker compose version

# ローカルディレクトリをEC2インスタンスに転送するコマンド
# pemファイル = TrainingKeyPair.pem
# ローカルディレクトリパス = Desktop/udemy
# パブリック ip v4 = 18.118.120.115
scp -r -i ~/.ssh/[pemファイル] ~/[ローカルディレクトリパス] ec2-user@[パブリック ip v4]:/home/ec2-user/
```
