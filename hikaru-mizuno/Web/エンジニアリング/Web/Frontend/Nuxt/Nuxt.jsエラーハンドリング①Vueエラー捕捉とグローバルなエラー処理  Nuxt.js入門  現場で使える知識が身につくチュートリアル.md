---
URL: https://tech-education-nav.com/contents/educational-materials/nuxt-js/error-handling-in-nuxt
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/56a21789b1bb4f3466a0
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> 子コンポーネントから発生したエラーを捕捉する

> onErrorCaptured()

> エラーの発生源を正確にキャッチでき

>   // エラーを既に処理済みとするために false を返し、伝播を停止する  return false;

> グローバルエラーハンドリングの設定方法

>  アプリケーション全体に対してエラーハンドリングを行うための仕組み

> app.config.errorHandler: Vue インスタンス内で発生したエラーが、各コンポーネントで個別に捕捉（onErrorCaptured() など）されなかった場合、最終的にこのグローバルエラーハンドラーに流れます。すべての Vue エラーをここで捕捉するため、エラーのロギングや外部のエラー報告サービスへの通知など、共通処理を行うのに適しています。

> vue:error フック: Nuxt 固有のフックで、Vue のエラーがルートレベルまで伝播した場合に呼び出されます。グローバルエラーハンドラーと同様に、エラー報告サービスへ一元的にエラー情報を送信するなど、エラーの統一管理が可能です。

> グローバルエラーハンドリングの実装と検証

> Nuxt アプリケーションの初期化時に実行したい処理や、グローバルな設定（エラーハンドリング、プラグイン登録、フックの設定など）を一元管理でき

> defineNuxtPlugin の役割

> nuxtApp.vueApp.config.errorHandler

> Vue のグローバルなエラーハンドリング機能を利用

> nuxtApp.hook

> Vue 内でエラーが発生したタイミングで呼び出され、上記のグローバルエラーハンドラーと同様にエラー情報を処理


