# Devin に Dependabot PR をレビューしてもらおう

![](https://res.cloudinary.com/zenn/image/upload/s--2CJuJAxQ--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Devin%2520%25E3%2581%25AB%2520Dependabot%2520PR%2520%25E3%2582%2592%25E3%2583%25AC%25E3%2583%2593%25E3%2583%25A5%25E3%2583%25BC%25E3%2581%2597%25E3%2581%25A6%25E3%2582%2582%25E3%2582%2589%25E3%2581%258A%25E3%2581%2586%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_34:%25E9%25BB%2592%25E6%259B%259C%2Cx_220%2Cy_108/bo_3px_solid_rgb:d6e3ed%2Cg_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzhhNjFkZTAyMDUuanBlZw==%2Cr_20%2Cw_90%2Cx_92%2Cy_102/co_rgb:6e7b85%2Cg_south_west%2Cl_text:notosansjp-medium.otf_30:%25E3%2583%25AA%25E3%2583%25BC%25E3%2583%258A%25E3%2583%25BC%25E3%2583%2586%25E3%2583%2583%25E3%2582%25AF%25E3%2583%2596%25E3%2583%25AD%25E3%2582%25B0%2Cx_220%2Cy_160/bo_4px_solid_white%2Cg_south_west%2Ch_50%2Cl_fetch:aHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2lncWhoYi1Kck9NZndzdHpxZm15LVJyY25iS080TmlkTk9BMkZwT1E9czI1MC1j%2Cr_max%2Cw_50%2Cx_139%2Cy_84/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: Devin に Dependabot PR をレビューしてもらおう
- URL: https://zenn.dev/leaner_dev/articles/20250218-devin-review-bump-prs
- Last Updated on: 2025-06-17



### Highlights & Notes

- Dependabot PR のレビュー
- マージするには大抵の場合以下のレビューを実施する必要があり
- 更新されるバージョンのリリースノートを確認し、破壊的な変更があるか確認する
	で破壊的な変更がある場合は、影響があるかプロジェクトのソースコードを確認する
	で影響があると確認できた場合は、既存の挙動を保つようソースコードを修正する
- 大抵の Pull Request は 1. で「破壊的変更なし、マージしてヨシ！」となる
- # 指示
	[リポジトリ名]の Pull Request を順に確認し、 Dependabot や Renovate などによるライブラリバージョン更新のものについてレビューを行ってください。(その他の Pull Request は何もしないでください。またすでに Devin によるレビューコメントがついている場合はスキップしてください)
	レビューでは、以下の作業を行ってください。
	1. ライブラリバージョン更新に伴う変更点を確認し、破壊的変更がないかを確認する
	2. 変更点確認に使用した情報ソースへのリンクと、変更内容のサマリ、動作に影響がないかを簡単にまとめてPull Requestにコメントする
	すべての Pull Request で作業が完了したら、さらに以下を行ってください。
	レビュー対象のうち、破壊的変更があり動作に影響があると判断した各 Pull Request それぞれについて、正しく動作するよう修正を行ってください。この際、 HEAD ブランチから新しいブランチを切り、 HEAD ブランチに向けた Pull Request を新たに作ってください
- # 制約事項
	 * すべてのコミュニケーションやPull Requestコメントは日本語で行ってください。
	 * 調査や修正において、調査や修正作業が3回連続で進展しない場合はその旨をコメントし、次のPull Requestのレビューや修正に進んでください。
	 * 現在どのPull Requestに対してレビューや修正などの作業をしているか、セッションで都度報告してください(調査や修正の詳細は不要なので、調査・修正の開始・終了のみを対象としているPull Requestのリンクのみを付記して教えて下さい)
- Devin の出番
- つまるところ、人間がやる「リリースノートの確認」「影響があるかの判断と修正」をお願いしています。
- 「ブラウザを操作できるので、各ライブラリのリリースノートを見たうえでリンクを辿って詳細を追える」「プロジェクトコードを見られるので、影響判断や修正ができる」という両方を簡単な指示でできるのが Devin のメリット
- 以下のようなところもあり、まだプロンプトの調整や Knowledge の追加が必要そう
- Rails など、ある程度影響の広い修正だと頓珍漢な修正になることがある
	Ruby のバージョン変更で Docker Image Tag の修正を見逃すなど、影響範囲を適切に判断できないときがある
	たまにインデント修正を繰り返すなど謎のドツボにハマったループで ACU を使い潰してレビューが途中で終わる
