---
URL: https://tech-education-nav.com/contents/educational-materials/nuxt-js/nuxt-testing-guide
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/fdf71f3d4ac8cc646e84
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> @nuxt/test-utilsを通じて

> 単体テスト（Unit Testing）: コンポーネントや関数単位でのテスト。エンドツーテスト（E2E Testing）: ユーザー視点でのアプリケーション全体の動作確認。

> 単体テストの実施

> 必要なライブラリのインストール

> 環境構築と設定

> vitest.config.tsの作成

> Nuxt特有の設定

> テスト専用の環境変数を設定する場合、.env.testを作成

> mountSuspended: Nuxt環境内でのVueコンポーネントのマウント。renderSuspended: テストライブラリを使用したコンポーネントのレンダリング。mockNuxtImport: Nuxtの自動インポート機能のモック。

> テスト用ユーティリティ

> エンドツーテストの実施

> エンドツーテスト（E2E）は、ユーザー視点でのアプリケーション全体の動作を確認するテスト

> 必要なライブラリのインストール

> @nuxt/test-utils playwright

> @nuxt/test-utils vitest @vue/test-utils happy-dom

> Playwright設定

> テストベストプラクティス

> テスト後に状態をクリアする。グローバルな状態を適切にモック。

> 状態のリセット

> 自動化されたCI/CDの活用

> CI/CDパイプラインでテストを実行する


