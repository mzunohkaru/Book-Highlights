### Metadata

- Title: RAGの検索性能を90％も低下させるテキストの落とし穴
- URL: https://zenn.dev/knowledgesense/articles/ff2c528acf6b04
- Last Updated on: 2025-10-13



### Highlights & Notes

- Embeddingにも限界があり一般的には「固有名詞に弱い」と言われています
- Embeddingの学習内で利用されてこなかった単語の意味をEmbeddingが理解できないため
## 性能を引き下げる文章の特徴
- 位置バイアス
	- 入力したクエリに対する回答が、どの位置にあるかによって類似度がどのように変化するかを示したグラフ
	- 文章の先頭にある状態が最も類似度の高い状態で、そこから精度が減少していく
![](https://storage.googleapis.com/zenn-user-upload/ca2f7e36fe8f-20250317.png)

- 単語バイアス
	- 「United States」を含むクエリで検索した場合、対象のドキュメントが「United States」なのか「US」なのかで、大きな差が生まれる

- 文章量バイアス
	- 内容の質よりも文章量で評価が変わってしまう傾向がある
	- 保管されている文章の長さによる類似度の比較結果
	![](https://storage.googleapis.com/zenn-user-upload/28b98b6f8610-20250317.png)

- 精度への影響
	- 正解を含むが関係のない文章も含む正解ドキュメントと、正解を含まないがEmbeddingが一致していると導きたくなる特徴を持つ不正解ドキュメントを比較して、正解ドキュメントを類似していると判断できるかを実験
	- 圧倒的に不正解ドキュメントを取得する傾向がある

