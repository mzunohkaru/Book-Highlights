---
URL: https://zenn.dev/neotechpark/articles/da5ab456bf5a46
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/650b5c8edb4f87625231
Tags: []
Last updated: 2025-06-17
---
#### Highlights & Notes

> 同じリポジトリで複数のタスクを同時に実行する

> 「Git Worktree」

> 「1つのリポジトリのブランチを、それぞれ別のフォルダとしてPC上に作成できる」 というGitの標準機能

> 実際の使い方

> ![](https://storage.googleapis.com/zenn-user-upload/37fe74f42690-20250611.png)

> 実際のディレクトリ構造は

> 物理的にフォルダが分かれているだけ

> 各ブランチをVSCodeで開ける！最強の並列開発環境の完成

> ![](https://storage.googleapis.com/zenn-user-upload/8b6a1def5462-20250611.png)

> 同時並列開発を成功させるための「６つ」のヒント

> Worktreeで新しいフォルダを作っても、Node.jsのパッケージ（node_modules）などは共有されません。そのため、各フォルダで環境構築をやり直す必要があります。

> Dockerで動かしているなら、ローカルに1つコンテナを立てておけば、各Worktreeから共通で利用できます。

> 1. 環境構築はシンプルに

> 複雑な環境構築は避け、できるだけシンプルな構成を心がける

> 2. 指示用のMarkdownファイルを用意する

> 実装してほしいこと、修正してほしいこと、守ってほしいルールなどを詳しく書いておけば、Claudeはそれを忠実に実行してくれます。

> （これらはGitの管理対象外にしたいので、.gitignoreにISSUE.mdなどを追加しておきましょう）

> それぞれのタスク（Worktree）ごとに、ISSUE.mdのような指示書ファイルを作成します。

> 3. ポートを分ければ同時デバッグも可能

> 4. 開発環境の混乱を避ける「Peacock」

> 機能追加（赤色）バグ修正（青色）リファクタリング（緑色）

> タスクの種類ごとに色分け

> 5. 完了通知を確実に受け取る

> CLAUDE.md（共通指示書）に以下のルールを追記する

> 【重要】すべてのタスクが完了したら、**必ず最後に**以下のコマンドをターミナルで実行してください。```bashecho -e "\a"```

> 6. コンフリクトのリスクをどう下げるか

> こまめに main を取り込む


