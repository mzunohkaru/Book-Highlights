> 公開したAPIのユースケースを考えること

# エンドポイントの基本的な設計

> どんな機能を持つ URI なのかが一目でわかる

1. 短く入力しやすい
2. 人間が呼んで理解できる
3. 大文字小文字が混在していない（基本は全て小文字）
4. 改造しやすい（Hackableな）
5. サーバー側のアーキテクチャが反映されていない
6. ルールが統一されている

# リクエスト

## クエリパラメータによる検索

取得数と取得位置のクエリパラメータ
1. limit=50 & offset=100
	- 例)  ：<u>100アイテム目から、50アイテム</u>を取得
	- 利点：自由度が高い
2. per_page=50 & page=3

絶対位置
- 「指定したIDよりも前」「指定した日時よりも前」という方法

絞り込みパラメータ
- パラメータ複数は、<u>全て指定可能</u>仕様
	- http://api.example.com/v1/search?<u>first-name=Clair&last-name=Standish&school-name&Tokyo</u>
	- http://api.example.com/v1/blog<u>?tag=new+yourk+yankees</u>
- パラメータが単一の場合は、”q”というパラメータ
	- http://api.example.com/v1/search?<u>q=ken</u>
		- q = query
		- ユーザー情報の中で文書情報が含まれるフィールド（メールアドレスなど）全て


>  自分の情報へのエイリアスとして、me,self が使用される。ユーザーIDを指定する代わりにこのキーワードを指定すると現在のアクセストークンに紐づいた「自分」のnユーザー情報を取得できる

## クエリパラメータとパスの使い分け

**基本的な設計思想**
パスパラメータ（Path Parameters）
- リソースの階層構造を表現
- RESTful設計における「リソースの一意識別」
- キャッシュしやすい（URLが決定論的）

クエリパラメータ（Query Parameters）
- フィルタリング・検索条件
- オプション的な情報
- 動的な条件

**具体的な使い分けルール**
パスパラメータを使う場合
1. 必須の識別子
    ```
    GET /orders/ORD-123
    DELETE /users/456
    ```
2. 階層構造
    ```
    GET /companies/apple/departments/engineering/employees
    ```
3. バージョニング
    ```
    GET /v1/users/123
    GET /v2/users/123
    ```

クエリパラメータを使う場合
1. 省略可能
	- 省略すればデフォルト値が利用される
    ```
    GET /products?brand=sony&price_max=50000
    ```
2. ソート・形式指定
    ```
    GET /reports?sort=date&format=pdf
    ```
3. オプション機能
    ```
    GET /users/123?include=profile,settings
    ```

**なぜアクセストークンは、クエリパラメータやパスパラメータではダメなのか？**
1. ログに残る
```
❌ 危険：ログに残る
GET /api/users?access_token=secret123

❌ 危険：リファラーヘッダーに露出
GET /api/users/secret123/profile

❌ アクセスログにトークンが記録される
192.168.1.1 - - [25/Dec/2023] "GET /api/data?token=abc123" 200

❌ URL全体がプロキシログに記録
GET /api/data?token=secret123

✅ 安全：ヘッダーはログに残りにくい
Authorization: Bearer secret123
```
2. キャッシュの問題
```
// ❌ トークンごとに別のキャッシュエントリ
GET /api/products?access_token=user1_token
GET /api/products?access_token=user2_token

// ✅ 同じリソースは同じキャッシュキー
GET /api/products
Authorization: Bearer user1_token
```

## OAuth2

仕組み
![](oauth2.png)
【補足】
なぜ直接アクセストークンを渡さないのか？
- 問題点
	- ブラウザのアドレスバーやリファラーヘッダーに露出
	- ブラウザ履歴に残存
	- 第三者による傍受リスク
	- 長期間有効なトークンが公開される
- 認可コードを使う利点
	- 短時間で無効化（通常10分以内）
	- 一回限りの使用
	- クライアント認証と組み合わせ使用
	- ブラウザ経由でも比較的安全

アクセストークンの保持場所
- Webアプリケーション
	- サーバーサイド
		- セッションストレージやデータベース（推奨）
	- クライアントサイド
		- HttpOnlyクッキー（XSS対策）
	- 避けるべき
		- localStorage（XSS攻撃のリスク）
- SPA
	- メモリ内保持（ページリロード時は再取得）
	- HttpOnlyクッキー（可能な場合）

### Grant Type

| アプリタイプ          | 推奨Grant Type              | 理由                         | セキュリティレベル |
| --------------- | ------------------------- | -------------------------- | --------- |
| Webアプリ（サーバーサイド） | Authorization Code        | 最もセキュア、クライアントシークレット保持可能    | 高         |
| SPA             | Authorization Code + PKCE | Implicitの代替、リフレッシュトークン使用可能 | 超高        |
| モバイルアプリ         | Authorization Code + PKCE | パブリッククライアントでも安全            | 超高        |
| サーバー間通信         | Client Credentials        | ユーザー不要、マシン間認証              | 中         |
| IoTデバイス         | Device Authorization      | 入力制限対応                     | 中         |
| レガシー移行          | Resource Owner Password   | 段階的移行（一時的）                 | 低         |
### OIDC

> OIDCはOAuth2.0を拡張して「認証」も行えるようにした仕様

#### 基本的な違い
- OAuth2：「何かをする権限があるか？」を判断する（認可）
- OIDC：「あなたは誰なのか？」を確認する（認証）

#### 役割
エンドユーザーの認証にフォーカスした仕様
- ID Tokenを導入してユーザー情報をやり取りする

1. シングルサインオン（SSO）が実現できる
    - 「Googleアカウントでログイン」「GitHubアカウントでログイン」などが可能
2.  セキュアな認証が標準化される
3. ユーザー情報の安全な取得ができる

#### 主要な登場人物
- OP（OpenID Provider）：認証を提供するサーバー（Google、Auth0など）
- RP（Relying Party）：サービスを提供するアプリケーション
- エンドユーザー：実際にログインする人

#### 処理フロー
![](oidc.png)
#### ID Token
**詳細**
- JWT形式でユーザーの認証情報を含む
- 署名付きなので改ざん検知が可能
**保存場所**
- フロントエンド：セッションストレージ推奨
- バックエンド：セッション管理と連携
- 有効期限：短めに設定（通常1時間以内）

**アクセストークンとIDトークンの使い分け**

| 用途         | 使用トークン   | 目的          |
| ---------- | -------- | ----------- |
| ユーザー情報表示   | IDトークン   | 認証済みユーザーの特定 |
| API呼び出し    | アクセストークン | リソースアクセス権限  |
| UserInfo取得 | アクセストークン | 追加情報の取得     |
###  SSO

![](sso-actor.png)
![](sso-初回認証.png)
![](sso-actor.png)
![](sso-2回目以降のアクセス.png)
![](sso-ログアウト.png)

**SAML Response**

| Response Element   | ルート要素        |
| ------------------ | ------------ |
| Assertion          | 認証・属性情報を含む   |
| Subject            | ユーザー識別情報     |
| Conditions         | 有効性条件（時間制限等） |
| AuthnStatement     | 認証方法・時刻      |
| AttributeStatement | ユーザー属性       |
| Digital Signatures | 完全性保証        |

## ホスト名

ホスト名を分ける
- 例：api.example.com
- 利点：DNSレベルで分割でき、管理しやすい


# レスポンス

## HTTPステータスコードの適切な使用
- 200 OK: データ取得成功（ただし空配列でも200）
- 201 Created: リソース作成成功（Locationヘッダー推奨）
- 204 No Content: 更新・削除成功でレスポンスボディ不要
- 400 Bad Request: クライアント側の入力エラー
- 404 Not Found: リソースが存在しない（権限なしの場合も）
- 422 Unprocessable Entity: バリデーションエラー
> ピッタリくるステータスコードが存在しなかった場合には、"200","400","500"などの"00"で終わるステータスコードを付ける

## 配列レスポンスの考慮
- 配列を直接返すのは避ける（JSONハイジャック対策）
- ページネーション情報を含める
- 総件数、次ページの存在等のメタ情報

## エラーレスポンスの統一
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "入力値に問題があります",
    "details": [
      {
        "field": "email",
        "message": "有効なメールアドレスを入力してください"
      }
    ]
  }
}
```

## パフォーマンスとスケーラビリティ
**レスポンスUX**
- Retry-Afterヘッダ：いつ再試行すべきかを指示する
**レスポンスサイズの最適化**
- <u>必要なフィールドのみ返す（フィールド選択機能）</u>
- 大きなデータはページネーション必須
- 画像URLは絶対パス、適切なサイズ指定
**キャッシュ戦略**
- ETag：データ変更検知とキャッシュ効率化
- Last-Modified：条件付きリクエスト対応
- Cache-Control：適切なキャッシュ期間設定

**Cache-Control**

1. 静的リソースの長期キャッシュ戦略
ファイル内容に基づいてハッシュを生成する事で、ファイル内容の変更を検知できる
```http
# 静的アセット（CSS、JS、画像）
Cache-Control: public, max-age=31536000, immutable

# ファイル名例
/assets/main-a1b2c3d4.css
/assets/bundle-e5f6g7h8.js
```

2. 動的コンテンツの適応的キャッシュ
```http
# HTMLページ
Cache-Control: public, max-age=60, s-maxage=300, stale-while-revalidate=3600

# API レスポンス（頻繁に更新される）
Cache-Control: private, max-age=0, must-revalidate

# API レスポンス（比較的安定）
Cache-Control: public, max-age=300, stale-while-revalidate=900
```
`stale-while-revalidate`キャッシュが期限切れでもバックグラウンドで更新しながら古いコンテンツを提供できる

3. セキュリティを考慮したキャッシュ設定
```http
# 認証が必要なコンテンツ
Cache-Control: private, no-store, max-age=0

# 個人情報を含むAPI
Cache-Control: private, no-cache, no-store, must-revalidate
Pragma: no-cache
Expires: 0
```


## セキュリティ考慮事項

**情報漏洩防止**
- 内部IDの露出回避（UUID使用等）
- スタックトレースの本番環境での非表示
- 権限外データの除外
**CORS設定**
- Origin の適切な制限
- 必要最小限のHTTPメソッド許可
- credentials使用時の注意

**セキュリティ攻撃**
SQLインジェクション攻撃
- 攻撃フロー
``` 
[攻撃者] → [入力フィールド] → [SQL構文構築] → [データベース実行] → [情報漏洩/改ざん] 
```
- 対策
	- プリペアドステートメント
		- ORMフレームワークの活用
	- ホワイトリスト方式による入力検証
		- 正規表現パターンマッチング
		- データ型・長さ制限の厳格な適用
		- 特殊文字のサニタイズ
	- アーキテクチャレベル
		- DBユーザーの権限制限
		- WAF導入

XSS（Cross-Site Scripting）攻撃
- 攻撃フロー
``` 
[攻撃者] → [悪意のあるスクリプト投稿] → [データ保存] → [他ユーザー閲覧] → [スクリプト実行] → [セッション乗っ取り] 
```
- 対策
	- コンテキスト依存エスケープ
		- HTML：`<`, `>`, `&`, `"`, `'`の変換
		- JavaScript：Unicode エスケープ
		- CSS：16進エスケープ
		- URL：パーセントエンコーディング
	- Content-Security-Policy
		- 詳細→セキュリティヘッダーの章

コマンドインジェクション攻撃
- 攻撃フロー
``` 
[攻撃者] → [システムコマンド入力] → [OS実行] → [サーバー制御] → [データ破壊/情報窃取]
```
- 対策
	- 入力検証
		- ファイル名：英数字とピリオドのみ許可
		- パス：相対パス禁止、ディレクトリトラバーサル対策
		- 正規表現：`^[a-zA-Z0-9._-]+$`
	- アーキテクチャ設計
		- ライブラリ関数の直接使用
		- API経由での機能実現
		- コンテナ化による分離

**セキュリティヘッダー**
<必須レベル>
1. Content-Security-Policy
```http
Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; frame-ancestors 'self'
```
- X-Frame-Optionsの代替：`frame-ancestors 'none'`（DENY相当）
- XSS攻撃の大幅な軽減
- リソース読み込み制御

2. Strict-Transport-Security (HSTS)
```http
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```
- HTTPS強制
- 証明書降格攻撃防止

3. X-Content-Type-Options
```http
X-Content-Type-Options: nosniff
```
- MIME type sniffing攻撃防止
- 引き続き有効

<推奨レベル>
4. Referrer-Policy
```http
Referrer-Policy: strict-origin-when-cross-origin
```
- 情報漏洩防止
- プライバシー保護

5. Permissions-Policy
```http
Permissions-Policy: camera=(), microphone=(), geolocation=()
```
- ブラウザ機能制御
- Feature-Policyの後継

6. Cross-Origin-Embedder-Policy (COEP)
```http
Cross-Origin-Embedder-Policy: require-corp
```
- Spectre攻撃対策
- SharedArrayBuffer有効化に必要

7. Cross-Origin-Opener-Policy (COOP)
```http
Cross-Origin-Opener-Policy: same-origin
```
- ウィンドウ間の分離
- サイドチャネル攻撃防止


# 設計変更しやすいWebAPI

## セマンティックバージョニング
**MAJOR (メジャーバージョン)**
- 後方互換性のない変更 (APIの変更など) 
- バグ修正 = 1.0.0 -> 1.0.1 
**MINOR (マイナーバージョン)**
- 後方互換性を保ちつつ、機能が追加された場合にインクリメントします。﻿
- 機能追加 = 1.0.1 -> 1.1.0
**PATCH (パッチバージョン)**
- 後方互換性を保ちつつ、バグが修正された場合にインクリメントします。﻿
- APIの変更 = 1.1.0 -> 2.0.0
