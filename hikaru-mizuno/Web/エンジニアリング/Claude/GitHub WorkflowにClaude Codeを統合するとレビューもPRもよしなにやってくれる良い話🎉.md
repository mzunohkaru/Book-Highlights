# GitHub WorkflowにClaude Codeを統合するとレビューもPRもよしなにやってくれる良い話🎉

![](https://res.cloudinary.com/zenn/image/upload/s--TocnZGvb--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:GitHub%2520Workflow%25E3%2581%25ABClaude%2520Code%25E3%2582%2592%25E7%25B5%25B1%25E5%2590%2588%25E3%2581%2599%25E3%2582%258B%25E3%2581%25A8%25E3%2583%25AC%25E3%2583%2593%25E3%2583%25A5%25E3%2583%25BC%25E3%2582%2582PR%25E3%2582%2582%25E3%2582%2588%25E3%2581%2597%25E3%2581%25AA%25E3%2581%25AB%25E3%2582%2584%25E3%2581%25A3%25E3%2581%25A6%25E3%2581%258F%25E3%2582%258C%25E3%2582%258B%25E8%2589%25AF%25E3%2581%2584%25E8%25A9%25B1%2520%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_34:%25E3%2581%25AF%25E3%2581%258C%25E3%2581%258F%25E3%2582%2593%2540%25E5%2585%2583%25E8%2596%25AC%25E5%2589%25A4%25E5%25B8%25AB%25E3%2581%25AEFlutter%252F...%2Cx_220%2Cy_108/bo_3px_solid_rgb:d6e3ed%2Cg_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzg5ODUyZjZkYzguanBlZw==%2Cr_20%2Cw_90%2Cx_92%2Cy_102/co_rgb:6e7b85%2Cg_south_west%2Cl_text:notosansjp-medium.otf_30:%25E6%25A0%25AA%25E5%25BC%258F%25E4%25BC%259A%25E7%25A4%25BE%25E3%2583%259E%25E3%2582%25A4%25E3%2583%25B3%25E3%2583%2587%25E3%2582%25A3%25E3%2582%25A2%2520%25E3%2583%2586%25E3%2583%2583%25E3%2582%25AF%25E3%2583%2596%25E3%2583%25AD%25E3%2582%25B0%2Cx_220%2Cy_160/bo_4px_solid_white%2Cg_south_west%2Ch_50%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2YyZDdiMzc4M2QuanBlZw==%2Cr_max%2Cw_50%2Cx_139%2Cy_84/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: GitHub WorkflowにClaude Codeを統合するとレビューもPRもよしなにやってくれる良い話🎉
- URL: https://zenn.dev/minedia/articles/be0005c37f7229
- Last Updated on: 2025-06-17



### Highlights & Notes

- Claude Code を使うと
	@claude レビューして
	修正
	@claude 再レビューして
	GitHub だけで完結
- GitHub上でコミュニケーションが完結する便利さ
- Promptやトリガーもカスタマイズ可能
  - Notes: https://docs.anthropic.com/en/docs/claude-code/github-actions
- # Define the review focus areas
- CLAUDE.mdで挙動をカスタマイズ
- プロジェクトルートにCLAUDE.mdファイルを置くと、Claude の挙動をカスタマイズできます
- CLAUDE.mdは/initで作れます。
- プロジェクト固有のルールに従ったレビューをしてくれるようになる
