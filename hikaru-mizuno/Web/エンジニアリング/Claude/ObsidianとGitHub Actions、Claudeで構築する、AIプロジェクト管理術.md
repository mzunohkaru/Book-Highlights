# ObsidianとGitHub Actions、Claudeで構築する、AIプロジェクト管理術

![](https://res.cloudinary.com/zenn/image/upload/s--bEHRj6pd--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Obsidian%25E3%2581%25A8GitHub%2520Actions%25E3%2580%2581Claude%25E3%2581%25A7%25E6%25A7%258B%25E7%25AF%2589%25E3%2581%2599%25E3%2582%258B%25E3%2580%2581AI%25E3%2583%2597%25E3%2583%25AD%25E3%2582%25B8%25E3%2582%25A7%25E3%2582%25AF%25E3%2583%2588%25E7%25AE%25A1%25E7%2590%2586%25E8%25A1%2593%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_34:%25E3%2583%259E%25E3%2583%2583%25E3%2582%25B5%25E3%2583%25B3%2520%2528Masanori%2520Yos...%2Cx_220%2Cy_108/bo_3px_solid_rgb:d6e3ed%2Cg_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2U2ZDA4MDY5ODcuanBlZw==%2Cr_20%2Cw_90%2Cx_92%2Cy_102/g_south_west%2Ch_34%2Cl_default:og-publication-pro-mark-xcosax%2Cw_34%2Cx_217%2Cy_158/co_rgb:6e7b85%2Cg_south_west%2Cl_text:notosansjp-medium.otf_30:Accenture%2520Japan%2520%2528%25E6%259C%2589%25E5%25BF%2597%2529%2Cx_255%2Cy_160/bo_4px_solid_white%2Cg_south_west%2Ch_50%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzU5MzkwNDBmYjYuanBlZw==%2Cr_max%2Cw_50%2Cx_139%2Cy_84/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: ObsidianとGitHub Actions、Claudeで構築する、AIプロジェクト管理術
- URL: https://zenn.dev/acntechjp/articles/c36f7827aec2b1
- Last Updated on: 2025-06-17



### Highlights & Notes

- システムアーキテクチャ
- ローカルのObsidianが「知の源泉」となり、それをGitHub Actionsが吸い上げ、Claudeが分析し、GitHub Issuesという「ダッシュボード」に届ける流れ
- Step 1: Obsidian - 知の拠点を構築する
- 情報を一元管理するObsidian
- フォルダ構成
- 📁 01_Projects/: 案件ごとのノートを格納
	📁 02_Resources/: 顧客情報やメンバー情報など、案件をまたぐ情報を格納
	📁 Members/: メンバーのプロフィールノート
	📁 03_Meetings/: 議事録ノート
	📁 04_Assignments/: 誰がどの案件にアサインされているかの「関係性」を定義するノート
	📁 05_Daily/: 案件に紐づかない日々のタスクやメモを記録するデイリーノート
	📁 _Templates/: 各ノートのひな形を保存
- テンプレートとDataviewによる情報のリンク
- Step 2: GitHub Actions & Claude - 全自動ToDoアナリストを召喚する
- ワークフローファイルの作成
- プロンプトファイルの作成
- 必要な設定
- リポジトリの Settings > Secrets and variables > Actions で、以下のシークレットを登録します。
	ANTHROPIC_API_KEY: あなたのAnthropic APIキー
- 拡張性: プロンプトを工夫すれば、「今週の進捗レポート」や「停滞している案件のアラート」など、様々なレポートを自動生成できます。
- タスクの自動棚卸し: 毎日Claudeがすべてのノートを巡回し、やるべきことを教えてくれます。
