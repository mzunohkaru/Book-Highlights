---
URL: https://tech-education-nav.com/contents/educational-materials/vue/vuejs-teleport-basics
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/ac4ecb7aafe4fe1f3c76
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> DOM内の異なる場所に「テレポート（転送）」する

> なぜTeleportが必要なのか？

> モーダルダイアログツールチップ通知バナードロップダウンメニューフルスクリーンオーバーレイ

> 実際のDOMでは別の場所に配置したい場合

>     <!-- モーダルの内容はbody直下にテレポートされる -->    <teleport to="body">

> Teleportの使い方

> モーダルの内容は<teleport to="body">によってコンポーネントツリーから切り離され、bodyタグの直下に挿入され

> Teleportの特性

> Teleportはあくまでレンダリング結果（HTML）のみを移動させるため、コンポーネントのプロパティ、イベント、スロット、双方向バインディングなどのVueの機能は全て正常に動作する

> 複数のテレポートが同じターゲットを指定した場合、それらは順番に追加される

> テレポート先が存在しない場合、Vueはコンソールに警告を出し、コンテンツはテレポートされない

> Teleportのメリット

> コンポーネントの論理的なカプセル化を維持しながら、DOM上の配置を柔軟に制御できるz-indexやoverflow、positionなど、CSSの配置に関する問題を簡単に解決できるアクセシビリティの向上（モーダルやダイアログを適切な場所に配置できる）コードの意図が明確になり、DOMの最終的な構造が理解しやすくなる


