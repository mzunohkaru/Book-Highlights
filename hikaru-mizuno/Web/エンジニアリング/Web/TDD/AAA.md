```typescript
// 🎯 AAAパターン（Arrange-Act-Assert）の完璧な例

test('商品リストが正しく表示される', () => {
  // 📝 Arrange（準備）: テストに必要なものを用意
  const testData = [
    { type: 'laptop', title: 'MacBook Pro' },
    { type: 'phone', title: 'iPhone 15' }
  ]
  
  // 🎬 Act（実行）: 実際にテストする動作を実行
  const wrapper = mount(ItemList, {
    props: { items: testData }
  })
  
  // ✅ Assert（確認）: 期待した結果になっているか確認
  expect(wrapper.findAll('[data-testid^="item-"]')).toHaveLength(2)
  expect(wrapper.text()).toContain('MacBook Pro')
  expect(wrapper.text()).toContain('iPhone 15')
})

// 🔥 このパターンを守ると...
// - テストが読みやすい
// - バグを見つけやすい  
// - 他の人も理解しやすい

// ❌ 悪い例：ごちゃごちゃ
test('なんかテスト', () => {
  const wrapper = mount(ItemList, { props: { items: [{ type: 'one', title: 'test' }] } })
  expect(wrapper.find('[data-testid="item-list"]').exists()).toBe(true)
  expect(wrapper.text()).toContain('test')
  // 何をテストしてるかわからない...
})
```