# Claude CodeのCLAUDE.md自動改善プログラムについて

![](https://res.cloudinary.com/zenn/image/upload/s--qwFkLk6F--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Claude%2520Code%25E3%2581%25AECLAUDE.md%25E8%2587%25AA%25E5%258B%2595%25E6%2594%25B9%25E5%2596%2584%25E3%2583%2597%25E3%2583%25AD%25E3%2582%25B0%25E3%2583%25A9%25E3%2583%25A0%25E3%2581%25AB%25E3%2581%25A4%25E3%2581%2584%25E3%2581%25A6%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:uuta%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2IxMWUwNjhiOTEuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: Claude CodeのCLAUDE.md自動改善プログラムについて
- URL: https://zenn.dev/yutti/articles/claude-code-auto-claude
- Last Updated on: 2025-06-17



### Highlights & Notes

- 全部のProjectで横断的に使用する場合
	配置場所: ~/.claude/commands/
	Prefix: /user:
- 個別のProjectで使用する場合
	配置場所: .claude/commands/
	Prefix: /project:
- Commandを作成
- 実行
- /project:reflectionを実行
- たとえば、docs/配下にあるdocumentを読み込んでくださいというようなpromptをいちいち入れるよりも、/project:readdocsなどのcommandを最初から用意しておくほうが良い
