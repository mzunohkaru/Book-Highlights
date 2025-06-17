# Claude Codeが固まった時の「claude -c」を知った件

![](https://res.cloudinary.com/zenn/image/upload/s--angG0kxi--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Claude%2520Code%25E3%2581%258C%25E5%259B%25BA%25E3%2581%25BE%25E3%2581%25A3%25E3%2581%259F%25E6%2599%2582%25E3%2581%25AE%25E3%2580%258Cclaude%2520-c%25E3%2580%258D%25E3%2582%2592%25E7%259F%25A5%25E3%2581%25A3%25E3%2581%259F%25E4%25BB%25B6%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:nobu%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzUwZTE4MWI3ZWQuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: Claude Codeが固まった時の「claude -c」を知った件
- URL: https://zenn.dev/ness98981010/articles/2025-06-10-claude-code-continue-option
- Last Updated on: 2025-06-17



### Highlights & Notes

- -cオプション
- 直前の会話のコンテキストを保持したまま会話を続けられる
- 地味に便利な機能
- メモリ機能
- # 入力の最初に#をつけるとメモリに保存される
- 複数行入力
- # \ で改行できる
- # Option+Enter でも複数行入力可能
- スラッシュコマンド
- /compact    # 会話を圧縮（長い会話をまとめる）
	/doctor     # Claude Codeの動作状況をチェック
	/vim        # Vimキーバインドを有効化
	/memory     # メモリファイルを直接編集
- 便利なフラグ
- claude --output-format json    # JSON形式で出力（スクリプト向け）
	claude --verbose              # デバッグ用の詳細出力
	claude --max-turns 5          # 自動実行の最大ターン数制限
