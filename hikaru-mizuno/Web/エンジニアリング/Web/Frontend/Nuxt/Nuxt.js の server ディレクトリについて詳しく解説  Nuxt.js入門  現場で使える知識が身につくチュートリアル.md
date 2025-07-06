---
URL: https://tech-education-nav.com/contents/educational-materials/nuxt-js/nuxtjs-server-directory
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/cb2713916d189bc1325c
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> api/: /api プレフィックス付きのルートを定義。

> routes/: /api プレフィックスなしのルートを定義。

> middleware/: すべてのリクエスト前に実行されるカスタムミドルウェアを登録。

> server/ディレクトリの基本構造

> カスタムサーバールート

> ユーザーが直接アクセスするためのURLを作成する場合に適しています

> サーバーミドルウェア

> リクエストの検証やログ記録など、リクエスト処理前に実行されるロジックを定義

> ファイル名に .get、.post などのサフィックスを付け

> server/api/test.get.ts

> server/api/test.post.ts

> 動的パラメータを使用するには、ファイル名に角括弧を使用

> 動的パラメータ

> HTTP メソッドの対応

> クエリパラメータの取得

> ボディデータの取得

> const query = getQuery(event);

> const body = await readBody(event);

> エラーハンドリング

> throw createError({

> キャッチオールルート

> すべてのパスにマッチするルートを定義するには、ファイル名に [...] を使用

> Nitro のストレージ機能を使用して Redis と統合する
- RAMにデータを保存
キー・バリュー型
---
用途
- 主に一時的なデータの保存に使われる（※永続化設定可能）
キャッシュ
セッション管理
リアルタイム機能：リアルタイム通知などで、一時的なデータの保存


