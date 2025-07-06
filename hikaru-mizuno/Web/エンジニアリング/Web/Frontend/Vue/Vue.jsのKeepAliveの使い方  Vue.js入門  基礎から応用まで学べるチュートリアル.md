---
URL: https://tech-education-nav.com/contents/educational-materials/vue/vuejs-keepalive-basics
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/a39a0cc50fdf5c6d295b
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> 通常、Vueでは表示されなくなったコンポーネントはアンマウント（破棄）されますが、<KeepAlive>で囲むことで、非表示になっても状態を維持したまま再利用できる

> 例えばタブ切り替えのあるアプリケーションで、ユーザーが別のタブを選択した後に元のタブに戻ったときに、フォームの入力内容やスクロール位置、選択状態などをそのまま復元できる

>     <keep-alive>      <component :is="options[currentStep].component" />    </keep-alive>

> activated: コンポーネントが表示されたときに呼ばれるフックdeactivated: コンポーネントが非表示になったときに呼ばれるフック

> KeepAliveのキャッシュの仕組み

> mountedやunmountedフックは、コンポーネントが実際にDOMにマウント・アンマウントされるときにしか呼ばれませんが、<KeepAlive>と併用する場合は、これらの特別なフック（activatedとdeactivated）が内部的に呼ばれています。

> 再表示されるたびにonActivatedフックが呼ばれ

> インクルード・エクスクルードによるキャッシュの制御

> キャッシュするコンポーネントを制御するためのincludeとexcludeプロパティ

> すべてのコンポーネントをキャッシュしたくない場合

>     <!-- UserFormのみをキャッシュする -->    <keep-alive include="UserForm">      <component :is="options[currentStep].component" />    </keep-alive>

> 最大キャッシュインスタンス数の制限

> メモリ使用量を抑えるために、キャッシュするコンポーネントの数に上限を設定したい場合

> <KeepAlive>のmaxプロパティを使用する

>     <!-- 最大3つのインスタンスまでキャッシュ -->    <KeepAlive :max="3">      <component :is="pages[currentId - 1]" :id="currentId" />    </KeepAlive>

> 最大3つのページコンポーネントのインスタンスのみがキャッシュされます。4つ目のページを表示すると、最も長く使用されていないページのキャッシュが破棄されます

> コンポーネント定義はリアクティブである必要がない

> キャッシュしたコンポーネントの動的な処理

> 例えば、データの更新やAPIからの再取得などを行う

> activatedフックを使って表示されるたびに


