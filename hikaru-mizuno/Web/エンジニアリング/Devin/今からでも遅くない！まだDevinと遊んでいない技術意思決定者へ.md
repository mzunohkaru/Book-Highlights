# 今からでも遅くない！まだDevinと遊んでいない技術意思決定者へ

![](https://res.cloudinary.com/zenn/image/upload/s--ntq7cgkr--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:%25E4%25BB%258A%25E3%2581%258B%25E3%2582%2589%25E3%2581%25A7%25E3%2582%2582%25E9%2581%2585%25E3%2581%258F%25E3%2581%25AA%25E3%2581%2584%25EF%25BC%2581%25E3%2581%25BE%25E3%2581%25A0Devin%25E3%2581%25A8%25E9%2581%258A%25E3%2582%2593%25E3%2581%25A7%25E3%2581%2584%25E3%2581%25AA%25E3%2581%2584%25E6%258A%2580%25E8%25A1%2593%25E6%2584%258F%25E6%2580%259D%25E6%25B1%25BA%25E5%25AE%259A%25E8%2580%2585%25E3%2581%25B8%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_34:KosukeAizawa%2Cx_220%2Cy_108/bo_3px_solid_rgb:d6e3ed%2Cg_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2IzZDQxYzc1NzUuanBlZw==%2Cr_20%2Cw_90%2Cx_92%2Cy_102/co_rgb:6e7b85%2Cg_south_west%2Cl_text:notosansjp-medium.otf_30:ourly%2520tech%2520blog%2Cx_220%2Cy_160/bo_4px_solid_white%2Cg_south_west%2Ch_50%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzAxY2FkZDMzMjcuanBlZw==%2Cr_max%2Cw_50%2Cx_139%2Cy_84/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: 今からでも遅くない！まだDevinと遊んでいない技術意思決定者へ
- URL: https://zenn.dev/ourly_tech_blog/articles/5c35ef83b341a1
- Last Updated on: 2025-06-14



### Highlights & Notes

- 個人的なイメージとしては、「一流コンサル出身のジュニアエンジニア」
- Devinとは何者か？
- 1. 変更行数が100～200行前後のゴールが明確なタスク
- Devinはゴールが明確で曖昧さが少ないタスクほど威力を発揮
- 2. ライブラリのアップデート調査
- アップデートに必要な情報(破壊的変更の有無、他ライブラリとの互換性の有無...etc)をきちんと抽出し、めちゃくちゃ見やすい形でGithub Issueにまとめてくれました
- 3. CIがこけたときの単純な修正
- ![](https://storage.googleapis.com/zenn-user-upload/8d82ffea59db-20250317.png)
- 4. 単体テストの実装
- ![](https://storage.googleapis.com/zenn-user-upload/d5a13a7b4cd0-20250317.png)
- TDDで進める際にもIOの型と入力値に対する期待値の情報を渡して先にテストを実装してもらえば、ロジックに集中して実装することができます
- テストや静的解析で失敗したときは原因分析＆修正方針を聞く
- 事前に失敗→原因分析→修正方針の確認というフローを設けておくと安心
