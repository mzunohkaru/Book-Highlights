# Claude Codeのカスタムスラッシュコマンドでドキュメント作成を効率化する

![](https://res.cloudinary.com/zenn/image/upload/s--Zit4l8SY--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Claude%2520Code%25E3%2581%25AE%25E3%2582%25AB%25E3%2582%25B9%25E3%2582%25BF%25E3%2583%25A0%25E3%2582%25B9%25E3%2583%25A9%25E3%2583%2583%25E3%2582%25B7%25E3%2583%25A5%25E3%2582%25B3%25E3%2583%259E%25E3%2583%25B3%25E3%2583%2589%25E3%2581%25A7%25E3%2583%2589%25E3%2582%25AD%25E3%2583%25A5%25E3%2583%25A1%25E3%2583%25B3%25E3%2583%2588%25E4%25BD%259C%25E6%2588%2590%25E3%2582%2592%25E5%258A%25B9%25E7%258E%2587%25E5%258C%2596%25E3%2581%2599%25E3%2582%258B%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:driller%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2lPT1dLNnlQWE9BNndvZDFlYkxTQ0FLOGlpX1E3X2hqWmVwbDdJeUE9czI1MC1j%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: Claude Codeのカスタムスラッシュコマンドでドキュメント作成を効率化する
- URL: https://zenn.dev/driller/articles/06f916dc73a514
- Last Updated on: 2025-06-17



### Highlights & Notes

- Claude Codeのカスタムスラッシュコマンドとは
- Claude Codeは、.claude/commands/ディレクトリに配置したMarkdownファイルを自動的に読み込み、カスタムスラッシュコマンドとして利用できる機能
- 主な利点
- プロジェクト固有の操作を標準化: 複雑なコマンドラインや設定手順を単一のコマンドに集約
	チーム全体での作業統一: 同じコマンドを使うことで、メンバー間の作業方法を統一
	学習コストの削減: 複雑なツールの使い方を覚える必要がなく、シンプルなコマンドで操作可能
	自動化の促進: 手動で行っていた複数ステップの作業を自動化
	コマンドのMarkdown化: スクリプトではなく自然言語で手順を記述することで、理解しやすく保守しやすい形式で管理
- ファイル構成
- .claude
	├── commands  # Claude Codeのカスタムスラッシュコマンド定義
	│   ├── sphinx-build.md
	│   ├── sphinx-create.md
	│   └── sphinx-update.md
	├── docs
	│   ├── CLAUDE.md  # システムのドキュメントとガイド
	│   └── config  # Sphinx設定ファイル（テーマと拡張機能）
	│       ├── extensions-config.md
	│       └── theme-settings.md
- 使用方法
- プロジェクトのCLAUDE.mdにルールを記述
- 3. プロジェクト情報の設定
- 1. Claude Codeで設定を取り込み
- 4. プロジェクトのCLAUDE.mdにルールを記述
- プロジェクト名やauthorsがなければ、ディレクトリの名前やgit/OSのユーザ名から取得します。
- プロジェクトルートのCLAUDE.mdファイルに、ドキュメント作成のルールを記述
- 5. Claude Codeでコマンドを実行
- # 初期化
	/project:sphinx-create
- # 設定更新
	/project:sphinx-update
- # ビルド
	/project:sphinx-build
- 応用のポイント
- コマンドの標準化: 言語に関係なく、/project:doc-create、/project:doc-build等の統一命名
	設定の外部化: プロジェクト固有の設定を.claude/config/に分離
