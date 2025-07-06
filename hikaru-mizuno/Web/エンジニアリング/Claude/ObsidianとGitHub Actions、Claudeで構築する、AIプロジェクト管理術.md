---
URL: https://zenn.dev/acntechjp/articles/c36f7827aec2b1
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/05408ee68329677c16ef
Tags: []
Last updated: 2025-06-17
---
#### Highlights & Notes

> システムアーキテクチャ

> ローカルのObsidianが「知の源泉」となり、それをGitHub Actionsが吸い上げ、Claudeが分析し、GitHub Issuesという「ダッシュボード」に届ける流れ

> Step 1: Obsidian - 知の拠点を構築する

> 情報を一元管理するObsidian

> フォルダ構成

> 📁 01_Projects/: 案件ごとのノートを格納📁 02_Resources/: 顧客情報やメンバー情報など、案件をまたぐ情報を格納📁 Members/: メンバーのプロフィールノート📁 03_Meetings/: 議事録ノート📁 04_Assignments/: 誰がどの案件にアサインされているかの「関係性」を定義するノート📁 05_Daily/: 案件に紐づかない日々のタスクやメモを記録するデイリーノート📁 _Templates/: 各ノートのひな形を保存

> テンプレートとDataviewによる情報のリンク

> Step 2: GitHub Actions & Claude - 全自動ToDoアナリストを召喚する

> ワークフローファイルの作成

> プロンプトファイルの作成

> 必要な設定

> リポジトリの Settings > Secrets and variables > Actions で、以下のシークレットを登録します。ANTHROPIC_API_KEY: あなたのAnthropic APIキー

> 拡張性: プロンプトを工夫すれば、「今週の進捗レポート」や「停滞している案件のアラート」など、様々なレポートを自動生成できます。

> タスクの自動棚卸し: 毎日Claudeがすべてのノートを巡回し、やるべきことを教えてくれます。


