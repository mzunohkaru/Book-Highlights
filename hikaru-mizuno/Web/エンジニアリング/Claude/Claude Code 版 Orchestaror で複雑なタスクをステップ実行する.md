---
URL: https://zenn.dev/mizchi/articles/claude-code-orchestrator
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/d9af4ac464801e08b4ed
Tags: []
Last updated: 2025-06-17
---
#### Highlights & Notes

> .claude/commands

> .claude/commands/*.md に Markdown ファイルを配置すると、それらがコマンドとして利用可能になる

> .claude/└── commands/    ├── orchestrator.md      # 複雑なタスクの分解実行    └── commit-with-check.md # テスト後のコミット

> コマンドは /project:コマンド名 の形式で実行できる

> 効いたのはこれらの指示

> 最初に大雑把にコードを調べ、サブタスクの分割とステップを計画するサブタスクの実行ステップ内は並列化するステップごとに、一つ前のタスクの実行結果から現在のサブタスク計画が妥当か再考するそのままだと計画が破綻した時に手戻りが大きい。要はアジャイルっぽくする


