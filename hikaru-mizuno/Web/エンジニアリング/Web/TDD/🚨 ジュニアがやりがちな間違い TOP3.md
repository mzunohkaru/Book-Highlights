```typescript
// ❌ 間違い1: 実装の詳細をテストしてしまう
test('ダメな例：実装に依存したテスト', () => {
  const wrapper = mount(ItemList)
  
  // これはダメ！内部実装に依存している
  expect(wrapper.vm.items).toBeDefined()
  expect(wrapper.vm.$data).toEqual({})
  
  // CSSクラスに依存（デザインが変わったら壊れる）
  expect(wrapper.find('.item-list-container').exists()).toBe(true)
})

// ✅ 正解1: ユーザーが見るものをテスト
test('良い例：ユーザー視点でテスト', () => {
  const items = [{ type: 'one', title: 'タイトル1' }]
  const wrapper = mount(ItemList, { props: { items } })
  
  // ユーザーが見るものをテスト
  expect(wrapper.text()).toContain('タイトル1')
  expect(wrapper.find('[data-testid="item-list"]').exists()).toBe(true)
})

// ❌ 間違い2: テストケースが複雑すぎる
test('ダメな例：1つのテストで複数のことをテスト', () => {
  const wrapper = mount(ItemList)
  
  // 空の状態をテスト
  expect(wrapper.find('[data-testid="empty-message"]').exists()).toBe(true)
  
  // 途中でpropsを変更
  wrapper.setProps({ items: [{ type: 'one', title: 'test' }] })
  
  // データありの状態をテスト
  expect(wrapper.find('[data-testid="item-list"]').exists()).toBe(true)
  
  // ローディング状態もテスト
  wrapper.setProps({ loading: true })
  expect(wrapper.find('[data-testid="loading"]').exists()).toBe(true)
  
  // 1つのテストで色々やりすぎ！
})

// ✅ 正解2: 1つのテストで1つのことだけ
test('良い例：空データの時の表示', () => {
  const wrapper = mount(ItemList, { props: { items: [] } })
  expect(wrapper.find('[data-testid="empty-message"]').exists()).toBe(true)
})

test('良い例：データありの時の表示', () => {
  const items = [{ type: 'one', title: 'test' }]
  const wrapper = mount(ItemList, { props: { items } })
  expect(wrapper.find('[data-testid="item-list"]').exists()).toBe(true)
})

test('良い例：ローディング時の表示', () => {
  const wrapper = mount(ItemList, { props: { loading: true } })
  expect(wrapper.find('[data-testid="loading"]').exists()).toBe(true)
})

// ❌ 間違い3: テスト名が曖昧
test('ItemListのテスト', () => {
  // 何をテストしてるかわからない...
})

test('データのテスト', () => {
  // 何のデータ？どんなテスト？
})

// ✅ 正解3: テスト名で何をテストするか明確に
test('空のデータが渡された時、空メッセージが表示される', () => {
  // テスト名を読めば何をテストしてるかすぐわかる！
})

test('商品データ3件が渡された時、リストに3件表示される', () => {
  // 具体的で分かりやすい！
})

test('ローディング中の時、ローディングメッセージが表示される', () => {
  // ユーザーの行動と結果が明確！
})
```