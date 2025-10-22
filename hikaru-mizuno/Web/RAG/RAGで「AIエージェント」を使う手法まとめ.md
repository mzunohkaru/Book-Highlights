### Metadata

- Title: RAGで「AIエージェント」を使う手法まとめ
- URL: https://zenn.dev/knowledgesense/articles/64975fb9377f82
- Last Updated on: 2025-10-13



### Highlights & Notes

- 2024年の論文で「LLMにセルフチェック（Reflection）させると、問題解決能力が向上する」

## Agentic RAG の特徴
- 【設計パターン】
	- Reflection（自己反省）： 出力を評価し、自分で改善
	- Planning（計画）： 与えられたタスクを事前に細分化してから実行
	- Tool Use（ツール利用）： 外部APIやデータベースを活用
	- Multi-Agent Collaboration（マルチエージェント協力）： 複数のエージェントが協調
- 【既存のRAG、ざっくり分類】
	- Naïve RAG： シンプルなRAG
	- Advanced RAG： ハイブリットサーチやリランキングなどによる高度な検索
	- Modular RAG： ツール利用可能にするなどさらに複雑化、コンポーネント化
		- （注: 日本ではこれも含めてAdvanced RAGと呼ばれていた印象です）
	- Graph RAG： グラフデータ構造を利用して検索・推論強化
	- Agentic RAG： AIエージェントを利用
- 【アーキテクチャ】
- シングルエージェント型
	- 1つのエージェント（LLM）が全部をこなす
![](https://storage.googleapis.com/zenn-user-upload/ad23fdc88ae8-20250130.png)

- マルチエージェント型
	- 複数のエージェント（LLM）がデータソースごとに存在
 ![](https://storage.googleapis.com/zenn-user-upload/cdd0ffcb8e36-20250130.png)

- 階層型エージェント
	- 上位エージェントが戦略決定し、下位エージェントが実行
 ![](https://storage.googleapis.com/zenn-user-upload/b02cc102ad67-20250130.png)

- Corrective RAG（セルフチェック付きRAG）
	- 検索結果が十分かどうかセルフチェック
 ![](https://storage.googleapis.com/zenn-user-upload/5536be259fbe-20250130.png)

- Adaptive RAG（適応型RAG）
	- ユーザーの質問内容に応じて、戦略を使い分ける
 ![](https://storage.googleapis.com/zenn-user-upload/d7f8c4bf73a6-20250130.png)

- Graph-Based Agentic RAG
	- グラフDBとAIエージェントを利用したRAG
 ![](https://storage.googleapis.com/zenn-user-upload/a121c1fa5259-20250130.png)
