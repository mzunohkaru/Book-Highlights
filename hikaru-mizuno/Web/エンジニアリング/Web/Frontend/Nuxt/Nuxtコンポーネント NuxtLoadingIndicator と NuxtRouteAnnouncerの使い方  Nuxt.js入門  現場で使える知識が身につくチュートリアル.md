---
URL: https://tech-education-nav.com/contents/educational-materials/nuxt-js/nuxt-loading-indicator-route-announcer-guide
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/23c0817ff5ab16c0339f
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> ページ間の遷移時にプログレスバーを表示する

> NuxtLoadingIndicator

> カスタマイズ性: プロパティ（color、errorColor、height、duration、throttle、estimatedProgress）を利用して、デザインや動作を柔軟に調整可能です。カスタムコンテンツ: デフォルトの表示だけでなく、スロットを利用してカスタムHTMLやコンポーネントを埋め込むこともできます。useLoadingIndicator Composable: 内部インスタンスへのアクセスが可能で、独自の開始/終了イベントをトリガーするカスタム実装も可能です。

>  <NuxtLoadingIndicator> を app.vue やレイアウトファイルに追加し、ページ遷移時にプログレスバーを表示させる

> 例1: 基本的

> 例2: プロパティでカスタマイズ

> ページ遷移が発生すると、プログレスバーが上部に表示され

> 例では、ローディングバーの色、エラー時の色、高さ、表示時間、そしてスロットを用いたカスタム表示を設定しています

> NuxtRouteAnnouncer

> ルート変更時に画面上に非表示要素としてページタイトルを追加し、スクリーンリーダーなどの支援技術へ対して変更内容をアナウンスする

> アクセシビリティ向上: ページ遷移時に画面外に隠されたアナウンス用テキストを自動的に更新し、スクリーンリーダーに通知します。カスタム表示: スロットを利用して、アナウンスメッセージの表示形式を自由にカスタマイズ可能です。プロパティ:atomic: true にすると、全内容の更新をアナウンス。false なら変更部分のみを通知（デフォルトは false）。politeness: off（アナウンスなし）、polite（静かにアナウンス）、assertive（即座にアナウンス）。（デフォルトは polite）

> atomic や politeness プロパティを設定して、どの程度アナウンスを行うかを制御

> 例3: プロパティを利用したカスタマイズ


