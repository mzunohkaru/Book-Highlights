---
URL: https://zenn.dev/driller/articles/2a23ef94f1d603
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/709e2d921912bec57c3e
Tags: []
Last updated: 2025-06-17
---
#### Highlights & Notes

> 知見管理システム

> 基本的なディレクトリ構造

> project-root/├── CLAUDE.md                    # Claude Codeのメイン設定└── .claude/    ├── context.md              # プロジェクトの背景・制約    ├── project-knowledge.md    # 技術的知見・パターン    ├── project-improvements.md # 改善履歴・教訓    ├── common-patterns.md      # 頻用コマンドパターン    ├── debug-log.md           # 重要なデバッグ記録    └── debug/                 # 一時的なデバッグファイル        ├── sessions/          # セッション別ログ        ├── temp-logs/         # 作業中の一時ファイル        └── archive/           # 解決済み問題の保管

> CLAUDE.md - メイン設定ファイル

> プロジェクトの概要とClaudeへの指示を記載

> .claude/context.md - プロジェクトコンテキスト

> .claude/project-knowledge.md - 技術的知見

> ## アーキテクチャ決定

> ## 実装パターン

> ## 避けるべきパターン

> ## ライブラリ選定

> ## 概要

> ## 制約条件

> ## 技術選定理由

> # プロジェクト概要

> ### `.claude/context.md`- プロジェクトの背景、目的、制約条件- 技術スタック選定理由- ビジネス要件や技術的制約

> .claude/project-improvements.md - 改善履歴

> .claude/common-patterns.md - 頻用パターン

> # よく使用するパターン

> ## コンポーネント生成

> デバッグ支援

> claude analyze error log and suggest fixes for [error-description]

> デバッグ情報の管理

> 永続化ルール

> 以下の条件を満たす問題は debug-log.md に記録

> 解決に30分以上要した問題再発の可能性が高い問題チーム全体で共有すべき知見

> 記録フォーマット

> ## [YYYY-MM-DD] 問題の概要**症状**: エラーメッセージや異常動作**環境**: OS, Node.js, ブラウザバージョン**再現手順**: 具体的な操作手順**試行錯誤**: 試した方法とその結果**最終解決方法**: 確実に動作する解決手順**根本原因**: 問題の本質的な原因**予防策**: 同じ問題を避けるための対策

> 効果的なプロンプトのコツ

> 1. 文脈を明確に提供

> 2. 具体的な成果物を指定

> 3. 過去の知見を活用

> 以下の要件でAPIエンドポイントを作成してください：- RESTful設計- TypeScriptの型定義も含める- エラーハンドリング込み- テストコードも生成- .claude/project-knowledge.md のパターンに従う

> このプロジェクトはReact + TypeScriptのWebアプリです。現在、ユーザー認証機能を実装中で、JWTトークンを使用しています。.claude/context.md の制約条件を考慮して、以下の機能を実装してください...

> .claude/project-improvements.md の「ページ表示速度改善」の教訓を踏まえて、商品検索機能を実装してください。

> コマンド
- https://docs.anthropic.com/ja/docs/claude-code/tutorials#%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%A0%E3%82%B9%E3%83%A9%E3%83%83%E3%82%B7%E3%83%A5%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B

> 新たな知見が得られたら、次のファイルを更新してください- `.claude/context.md`: 背景・制約・技術選定理由- `.claude/project-knowledge.md`: 実装パターン・設計決定- `.claude/project-improvements.md`: 試行錯誤・改善記録- `.claude/common-patterns.md`: 定型実装・コマンドパターン

> 運用時の重要なコマンド

> /init コマンドの活用

> 実行すべきタイミング

> claude /init

> 初回導入時: .claude/ ディレクトリの作成と初期セットアップ知見ファイル大幅更新後: project-knowledge.md や context.md に重要な変更を加えた際新メンバー参加時: 最新のプロジェクトコンテキストを確実に反映定期メンテナンス後: 月1回程度の知見整理やファイル構造変更後

> /init を実行することで、Claude Codeがプロジェクトの最新状態を正確に把握し、一貫性のある提案ができるようになります


