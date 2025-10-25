### Metadata

- Title: MCPサーバーを自作する中でつまずいたポイント
- URL: https://zenn.dev/moneyforward/articles/6deaa22428a109
- Last Updated on: 2025-10-23



### Highlights & Notes

### Toolsを多く提供しすぎてMCPクライアントが全部のツールを認識できない
- 公式ドキュメントに Limitations として明記されている
	- Some MCP servers, or user’s with many MCP servers active, may have many tools available for Cursor to use. Currently, Cursor will only send the first 40 tools to the Agent.
### 対策
- 「将来的に自然言語でテストを書いてもらうためにも、これらは必要だろう」という当初成し遂げたかったユースケースを満たすのに十分なToolのみを提供

### MCPクライアントに返却するデータ量が多くてコンテキストウインドウの制限に抵触する
- 原因として、シンプルに返却するデータの情報量が多い
### 対策
- そもそも大きくなるであろう情報は含まずに返す
	- 例えばGitHub MCP でも list_issues には本文などの長くなり得る情報が含まれていない
	- 1件の詳細系のToolではそのような情報を含むことができる
- MCPクライアントに返却する情報を最適化する
	- Figma-Context-MCPではペイロードを最適化して返却している
- シンプルにページネーション・フィルターを実装する
	- GitHub MCPの例ですが、やはり一覧系のToolにはページネーションにあたる実装がされている
