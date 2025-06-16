# 数十名規模で Devin を1ヶ月トライして見えてきた点

![](https://res.cloudinary.com/zenn/image/upload/s--0eA9BmqU--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:%25E6%2595%25B0%25E5%258D%2581%25E5%2590%258D%25E8%25A6%258F%25E6%25A8%25A1%25E3%2581%25A7%2520Devin%2520%25E3%2582%25921%25E3%2583%25B6%25E6%259C%2588%25E3%2583%2588%25E3%2583%25A9%25E3%2582%25A4%25E3%2581%2597%25E3%2581%25A6%25E8%25A6%258B%25E3%2581%2588%25E3%2581%25A6%25E3%2581%258D%25E3%2581%259F%25E7%2582%25B9%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_34:sue738%2Cx_220%2Cy_108/bo_3px_solid_rgb:d6e3ed%2Cg_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzJkZmVkY2VkZTAuanBlZw==%2Cr_20%2Cw_90%2Cx_92%2Cy_102/co_rgb:6e7b85%2Cg_south_west%2Cl_text:notosansjp-medium.otf_30:GLOBIS%2520Tech%2Cx_220%2Cy_160/bo_4px_solid_white%2Cg_south_west%2Ch_50%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzgyOGZjYzdiNjIuanBlZw==%2Cr_max%2Cw_50%2Cx_139%2Cy_84/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: 数十名規模で Devin を1ヶ月トライして見えてきた点
- URL: https://zenn.dev/globis/articles/7733191f62d1e7
- Last Updated on: 2025-06-16



### Highlights & Notes

- おすすめの使い方
- 1. コードレビュー・PR関連の活用
- 「コードレビューでは特に効果を発揮している印象です。小規模なリファクタはスムーズに行えるため、かなりありがたいですね…！」
	「PRレビューでは非常に有用だと感じています。確認すべき観点を knowledge に蓄積することで、確認漏れや同じミスを防げると考えています。」
	「コードレビューを代わりにしてくれる。」
	「PRのセルフレビューの観点ヌケモレ・QAやチームメンバーと議論のベースとしたりしてみてます。」
- PRの作成支援や説明生成にも活用
- 「シンプルなプルリクエストを作る -> とくに開発中に既存実装を少し変えたい箇所が出てきた時などは、実装中の手を止めずに既存実装の修正ができてコンテキストスイッチが少なく済みます。」
- 2. Issue作成・情報整理
- 「Github等のissueから内容を抽出してもらう。また、直近で変更部分があるか、どこが変更されたか差分を抽出してもらう等。」
- 「Slack のスレッドを元に、取り決めた内容を Issue に起票する。」
- 「PdMなので、issueの作成がおすすめです。中身の確認はもちろん必要ですが、自分で手で作らずとも、たたきを作ってくれるので作ってくれたものを確認/修正するだけで良いので、手間が減りました。」
- Issue分析や知識共有にも貢献
- 「GitHub Issue の分析をさせる。」
- 「hodai リポジトリは複数の開発チームが関わっているため、チーム間の knowledge 共有が Devin を通じて自然に行われる点は大きなメリットだと思います。」
- 3. 単純作業の自動化
- 「基本的に『投げっぱなしでやってもらえる作業』という方向で使っています。」
- 「プログラマーとして使う場合、単純な繰り返し作業などを依頼すると、自分の時間を使わずにチェックが進められる。」
	「人間がするまでもない、細かい開発業務を裏で進めてPRを上げさせる。」
- 4. コーディング支援
- コーディングタスクの支援にも一定の活用が見られましたが、まだ十分に使いこなせていないという声も。
- Devinのここがいまいちという点
- 1. 進捗の可視性とレスポンスの遅さ
- 2. 高度な実装の難しさ
- 3. ナレッジの蓄積・管理が必要
- 4. プロンプト設計の難しさ
- 「意外と指示にないことを実行することがあり、プロンプトの工夫が必要」
- 「プロンプトを投げても、少しでも抽象的な要素があると正しく実装されない」
- 7. PR管理の不安定さ
- 「指示をしていないのにPR内のレビューコメントに反応し、不要なcommitを積んでしまう」
	「他プロダクトのリポジトリに誤ってpushすることがあり、制御が大変」
