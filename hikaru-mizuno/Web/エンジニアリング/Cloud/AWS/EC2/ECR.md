## 概要
ECRにローカルプロジェクトをプッシュ

### 手順
1. プロジェクトを構築
	- 補足
		- リポジトリ
			- https://github.com/mzunohkaru/Nuxt-Sample-Post
2. ECRでプライベートリポジトリを作成
3. プロジェクトをECRにプッシュ
```shell
# Dockerイメージを作成
docker-compose build

# ECRにログイン
# ECR<リポジトリ<プッシュコマンドを表示 から確認
aws ecr get-login-password --region [リージョン名] | docker login --username AWS --password-stdin [アカウントID].dkr.ecr.[リージョン名].amazonaws.com

# イメージを正しくタグ付け
# プロジェクト名 = Nuxt-Sample-Post
docker tag [プロジェクト名]:latest [アカウントID].dkr.ecr.[リージョン名].amazonaws.com/[リポジトリ名]:latest

# プッシュ再実行
docker push [アカウントID].dkr.ecr.[リージョン名].amazonaws.com/[リポジトリ名]:latest
```
