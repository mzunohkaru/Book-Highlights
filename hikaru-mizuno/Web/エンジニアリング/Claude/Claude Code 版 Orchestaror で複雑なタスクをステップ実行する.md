# Claude Code 版 Orchestaror で複雑なタスクをステップ実行する

![](https://res.cloudinary.com/zenn/image/upload/s--CmPGRWTS--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Claude%2520Code%2520%25E7%2589%2588%2520Orchestaror%2520%25E3%2581%25A7%25E8%25A4%2587%25E9%259B%2591%25E3%2581%25AA%25E3%2582%25BF%25E3%2582%25B9%25E3%2582%25AF%25E3%2582%2592%25E3%2582%25B9%25E3%2583%2586%25E3%2583%2583%25E3%2583%2597%25E5%25AE%259F%25E8%25A1%258C%25E3%2581%2599%25E3%2582%258B%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:mizchi%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2liclRHT052Z3d3ay1fNGxlcVk4TGNGSlNuX0FoWnpEWVlKaXJNcWc9czI1MC1j%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: Claude Code 版 Orchestaror で複雑なタスクをステップ実行する
- URL: https://zenn.dev/mizchi/articles/claude-code-orchestrator
- Last Updated on: 2025-06-17



### Highlights & Notes

- .claude/commands
- .claude/commands/*.md に Markdown ファイルを配置すると、それらがコマンドとして利用可能になる
- .claude/
	└── commands/
	    ├── orchestrator.md      # 複雑なタスクの分解実行
	    └── commit-with-check.md # テスト後のコミット
- コマンドは /project:コマンド名 の形式で実行できる
- 効いたのはこれらの指示
- 最初に大雑把にコードを調べ、サブタスクの分割とステップを計画する
	サブタスクの実行ステップ内は並列化する
	ステップごとに、一つ前のタスクの実行結果から現在のサブタスク計画が妥当か再考する
	そのままだと計画が破綻した時に手戻りが大きい。要はアジャイルっぽくする
