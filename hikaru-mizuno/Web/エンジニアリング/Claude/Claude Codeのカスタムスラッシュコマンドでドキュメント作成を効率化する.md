---
URL: https://zenn.dev/driller/articles/06f916dc73a514
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/045b8825c5497b491984
Tags: []
Last updated: 2025-06-17
---
#### Highlights & Notes

> Claude Codeのカスタムスラッシュコマンドとは

> Claude Codeは、.claude/commands/ディレクトリに配置したMarkdownファイルを自動的に読み込み、カスタムスラッシュコマンドとして利用できる機能

> 主な利点

> プロジェクト固有の操作を標準化: 複雑なコマンドラインや設定手順を単一のコマンドに集約チーム全体での作業統一: 同じコマンドを使うことで、メンバー間の作業方法を統一学習コストの削減: 複雑なツールの使い方を覚える必要がなく、シンプルなコマンドで操作可能自動化の促進: 手動で行っていた複数ステップの作業を自動化コマンドのMarkdown化: スクリプトではなく自然言語で手順を記述することで、理解しやすく保守しやすい形式で管理

> ファイル構成

> .claude├── commands  # Claude Codeのカスタムスラッシュコマンド定義│   ├── sphinx-build.md│   ├── sphinx-create.md│   └── sphinx-update.md├── docs│   ├── CLAUDE.md  # システムのドキュメントとガイド│   └── config  # Sphinx設定ファイル（テーマと拡張機能）│       ├── extensions-config.md│       └── theme-settings.md

> 使用方法

> プロジェクトのCLAUDE.mdにルールを記述

> 3. プロジェクト情報の設定

> 1. Claude Codeで設定を取り込み

> 4. プロジェクトのCLAUDE.mdにルールを記述

> プロジェクト名やauthorsがなければ、ディレクトリの名前やgit/OSのユーザ名から取得します。

> プロジェクトルートのCLAUDE.mdファイルに、ドキュメント作成のルールを記述

> 5. Claude Codeでコマンドを実行

> # 初期化/project:sphinx-create

> # 設定更新/project:sphinx-update

> # ビルド/project:sphinx-build

> 応用のポイント

> コマンドの標準化: 言語に関係なく、/project:doc-create、/project:doc-build等の統一命名設定の外部化: プロジェクト固有の設定を.claude/config/に分離


