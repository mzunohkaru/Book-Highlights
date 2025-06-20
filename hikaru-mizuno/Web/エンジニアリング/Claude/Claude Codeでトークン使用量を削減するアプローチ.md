# Claude Codeでトークン使用量を削減するアプローチ

![](https://res.cloudinary.com/zenn/image/upload/s--sr5xVJDJ--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Claude%2520Code%25E3%2581%25A7%25E3%2583%2588%25E3%2583%25BC%25E3%2582%25AF%25E3%2583%25B3%25E4%25BD%25BF%25E7%2594%25A8%25E9%2587%258F%25E3%2582%2592%25E5%2589%258A%25E6%25B8%259B%25E3%2581%2599%25E3%2582%258B%25E3%2582%25A2%25E3%2583%2597%25E3%2583%25AD%25E3%2583%25BC%25E3%2583%2581%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:driller%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2lPT1dLNnlQWE9BNndvZDFlYkxTQ0FLOGlpX1E3X2hqWmVwbDdJeUE9czI1MC1j%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: Claude Codeでトークン使用量を削減するアプローチ
- URL: https://zenn.dev/driller/articles/ff6a50ae228b2b
- Last Updated on: 2025-06-17



### Highlights & Notes

- トークン使用量を意識した効率的な使い方
- 1. コード生成の最適化
- 具体的な指示を出す
- 段階的な開発
- 2. コンテキスト管理の最適化
- 必要最小限のファイル共有
- repomixツールの活用
- プロジェクト構造の明確な可視化
	不要なコメントや空行の除去によるトークン削減
	XML形式でのClaude最適化された出力
- 部分的更新の活用
- ファイル全体を再生成するのではなく、変更が必要な部分のみの修正を依頼
- 関連するもののみに絞り込み
- 3. 事前設計によるトークン節約
- 難しいタスクでのループ: 複雑な問題に対して同じアプローチを繰り返し、解決に至らない
	遠回りな解決方法: 最適ではない実装を試み続ける
	設計レベルの議論が困難: アーキテクチャや技術選定の深い議論には向かない場合がある
	レート制限の制約: プロプランではトークン使用量に制限があるため、無駄な試行錯誤は避けたい
- 最も効果的な戦略の一つが、Claude Code で実装する前に通常の Claude との対話で設計を固めること
- Claude Code の限界
- 効果的な役割分担
- 【通常のClaude】設計相談・アーキテクチャ検討・問題解決戦略
	↓
	【Claude Code】明確な指示による一発実装
- Claude Code で得た知見の活用
- Claude Code で問題にぶつかった際は、そのログを通常の Claude に相談することも有効
- エラー対応の効率化
- エラーが発生した場合は、エラーメッセージと関連コードを同時に提示して一回で解決できるようにします
- 6. 実践的なワークフロー
- Step 1: 設計相談（通常のClaude）
- Step 2: 実装（Claude Code）
- Step 3: 問題発生時の対処（必要に応じて）
- テストの同時生成
- 実装と同時にテストコードの生成を依頼することで、効率的
- 使用するライブラリとバージョンを事前に明確にします。
- 7. 構造化された知見管理システムの活用
