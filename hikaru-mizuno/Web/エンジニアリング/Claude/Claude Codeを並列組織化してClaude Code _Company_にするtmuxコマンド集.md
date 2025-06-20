# Claude Codeを並列組織化してClaude Code "Company"にするtmuxコマンド集

![](https://res.cloudinary.com/zenn/image/upload/s--AroizRzh--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Claude%2520Code%25E3%2582%2592%25E4%25B8%25A6%25E5%2588%2597%25E7%25B5%2584%25E7%25B9%2594%25E5%258C%2596%25E3%2581%2597%25E3%2581%25A6Claude%2520Code%2520%2522Company%2522%25E3%2581%25AB%25E3%2581%2599%25E3%2582%258Btmux%25E3%2582%25B3%25E3%2583%259E%25E3%2583%25B3%25E3%2583%2589%25E9%259B%2586%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:kazuph%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzFjNmY3ODBlZDYuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: Claude Codeを並列組織化してClaude Code "Company"にするtmuxコマンド集
- URL: https://zenn.dev/kazuph/articles/beb87d102bd4f5
- Last Updated on: 2025-06-17



### Highlights & Notes

- claude --dangerously-skip-permissions
- dangerouslyオプションは自己責任
- ベストプラクティス
- 1. 明確な役割分担
	pane番号を必ず伝える
	担当タスクを具体的に指示
	エラー時の報告方法を明記
- 2. 効率的なコミュニケーション
	ワンライナー形式での報告徹底
	[pane番号]プレフィックス必須
	具体的なエラー内容の報告
- 3. トークン使用量管理
	定期的な/clear実行
	大量トークン消費の監視
	ccusageでの使用量確認
- 4. エラー対処
	Web検索による解決策調査を指示
	具体的エラー内容の共有
	成功事例の横展開
- タスク割り当て方法
- 基本テンプレート
- 並列タスク割り当て例
- 報連相システム
- 部下からメインへの報告形式
- 部下は以下のワンライナーで報告
- トークン管理
- /clearコマンドの実行
- 実行タイミングの判断基準
- タスク完了時（新しいタスクに集中させるため）
	トークン使用量が高くなった時（ccusageで確認）
	エラーが頻発している時（コンテキストをリセット）
	複雑な作業から単純な作業に切り替える時
- 並列/clear
- 状況確認コマンド
- pane状況確認
- 全pane一括確認
- なぜ必要
- 部下が応答しない時（フリーズ、エラー状態の確認）
	報告内容の詳細確認（エラーメッセージの全文確認）
	作業状況の客観的把握（進捗の可視化）
	トラブルシューティング時（ログの確認）
