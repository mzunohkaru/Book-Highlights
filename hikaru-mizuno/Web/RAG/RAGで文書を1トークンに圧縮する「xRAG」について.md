### Metadata

- Title: RAGで文書を1トークンに圧縮する「xRAG」について
- URL: https://zenn.dev/knowledgesense/articles/2b6aa64f27ea89
- Last Updated on: 2025-10-13



### Highlights & Notes


- xRAGを使うメリットは、コンテキストを圧縮できるのでRAGの回答速度が早くなること、トークン数を節約できるので安くなること
	- 論文: https://arxiv.org/abs/2405.13792
- xRAGでやっていることは、GPT-4oのようなLLMで「画像読み込み」を可能にしている手法と似ています
- RAGで取得してきた文書のベクトルデータを、「LLMが解釈しやすい別のベクトルデータ」に変換してから、LLMに渡すという手法
- 最終的にLLMに渡すドキュメントのデータがベクトルデータなので、1トークンで済む
![](https://storage.googleapis.com/zenn-user-upload/d967a8caee73-20240525.png)
### 「変換器」の作り方
	1. 普通のRAGの通り「関連テキスト+質問」をLLMに渡した場合のLLMの回答
	2. 「関連テキストをProjectorで変換したベクトル+質問」をLLMに渡した場合のLLMの回答
	この2つで違いがなるべく出ないように、Projectorモデル（変換器）を構築