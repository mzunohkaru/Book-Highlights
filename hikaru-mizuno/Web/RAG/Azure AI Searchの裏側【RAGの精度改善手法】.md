### Metadata

- Title: Azure AI Searchの裏側【RAGの精度改善手法】
- URL: https://zenn.dev/galirage/articles/azure_ai_search_rag_improvement
- Last Updated on: 2025-10-22



### Highlights & Notes
- RAGの全体構成として、L0（Query）、L1（Recall）、L2（Ranking）、L3（Synthetic）の4つの階層
	- L0（Query）は、検索クエリの書き換えをします。
	- L1（Recall）は、関連文書の検索をします。
	- L2（Ranking）は、高精度の再ランキングをします。
	- L3（Synthetic）は、最終的な回答を生成します。

