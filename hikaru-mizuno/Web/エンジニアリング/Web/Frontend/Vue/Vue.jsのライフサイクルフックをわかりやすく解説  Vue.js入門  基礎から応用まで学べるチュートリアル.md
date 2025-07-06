---
URL: https://tech-education-nav.com/contents/educational-materials/vue/vuejs-lifecycle-hooks
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/623b9a8c8f24ed953e1c
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> ライフサイクルの流れ

> 初期化コンポーネントのインスタンスが作成され、状態やイベントがセットアップされます。マウントコンポーネントがDOMに追加され、表示されます。更新リアクティブデータが変更されると、コンポーネントが再レンダリングされます。アンマウントコンポーネントがDOMから削除され、関連するリソースがクリーンアップされます。

> フック名	タイミングonBeforeMount	コンポーネントがDOMにマウントされる前onMounted	コンポーネントがDOMにマウントされた後onBeforeUpdate	データの変更により再レンダリングが始まる前onUpdated	再レンダリングが完了した後onBeforeUnmount	コンポーネントがDOMから削除される前onUnmounted	コンポーネントが完全に削除された後

> onMounted: 初期化処理

> コンポーネントがDOMにマウントされた直後に呼び出され

> データの取得やサードパーティライブラリの初期化を行います

> onUpdated: 更新処理

> リアクティブデータの変更による再レンダリングが完了した後に呼び出され

> DOMの操作やログ記録を行う

> onUnmounted: クリーンアップ処理

> コンポーネントが破棄される直前に呼び出され

> イベントリスナーやタイマーの解除など、不要なリソースをクリーンアップ

> イベントリスナーの登録と解除

> window.addEventListener('resize', updateWidth);
- リサイズイベントリスナーを登録

> window.removeEventListener('resize', updateWidth);
- リスナーを解除し、不要なリソースをクリーンアップ


