# 社内向けにDevinクイックスタートガイドを作った話

![](https://res.cloudinary.com/zenn/image/upload/s--HB1aa_IX--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:%25E7%25A4%25BE%25E5%2586%2585%25E5%2590%2591%25E3%2581%2591%25E3%2581%25ABDevin%25E3%2582%25AF%25E3%2582%25A4%25E3%2583%2583%25E3%2582%25AF%25E3%2582%25B9%25E3%2582%25BF%25E3%2583%25BC%25E3%2583%2588%25E3%2582%25AC%25E3%2582%25A4%25E3%2583%2589%25E3%2582%2592%25E4%25BD%259C%25E3%2581%25A3%25E3%2581%259F%25E8%25A9%25B1%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_34:Masato%2520Inoue%2520%2540%2520tac...%2Cx_220%2Cy_108/bo_3px_solid_rgb:d6e3ed%2Cg_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2E1NTlmNWNjYmUuanBlZw==%2Cr_20%2Cw_90%2Cx_92%2Cy_102/co_rgb:6e7b85%2Cg_south_west%2Cl_text:notosansjp-medium.otf_30:tacoms%25E3%2583%2586%25E3%2583%2583%25E3%2582%25AF%25E3%2583%2596%25E3%2583%25AD%25E3%2582%25B0%2Cx_220%2Cy_160/bo_4px_solid_white%2Cg_south_west%2Ch_50%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2M2ZjIxM2Q5MDYuanBlZw==%2Cr_max%2Cw_50%2Cx_139%2Cy_84/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: 社内向けにDevinクイックスタートガイドを作った話
- URL: https://zenn.dev/tacoms/articles/efeb008ce810f3
- Last Updated on: 2025-06-15



### Highlights & Notes

- ナレッジ機能の活用方針について
- いつ追加するか
- セッションが終わったタイミングでDevinからレコメンドしてくれるタイミングがあるのでその時に追加する
- 特にどのような情報を追加すべきか
- エンジニアとして常識的なレベルでありチームで形式知化するものでもないが、Devinがうまく動いてくれない時
- Githubのリポジトリには存在しないものの、NotionやSlackにある情報で他タスクと共有したい情報
- CIはなるべくローカルで実行させ何度もremoteへpushさせないようにする
- 何もプロンプトで指定しないと、めちゃくちゃ細かくリモートにpushされてしまう事象を発見
- remoteへのpush頻度が多いと、都度CIの待機時間が発生してしまう問題やActionsの費用が増える問題があり
