# Claude Sonnet 4に産業レベルのコードを書かせるプロンプトのハック

![](https://res.cloudinary.com/zenn/image/upload/s--ZjP4ztdJ--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Claude%2520Sonnet%25204%25E3%2581%25AB%25E7%2594%25A3%25E6%25A5%25AD%25E3%2583%25AC%25E3%2583%2599%25E3%2583%25AB%25E3%2581%25AE%25E3%2582%25B3%25E3%2583%25BC%25E3%2583%2589%25E3%2582%2592%25E6%259B%25B8%25E3%2581%258B%25E3%2581%259B%25E3%2582%258B%25E3%2583%2597%25E3%2583%25AD%25E3%2583%25B3%25E3%2583%2597%25E3%2583%2588%25E3%2581%25AE%25E3%2583%258F%25E3%2583%2583%25E3%2582%25AF%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:ShintaroAmaike%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzVmNWYzMmE0MWIuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: Claude Sonnet 4に産業レベルのコードを書かせるプロンプトのハック
- URL: https://zenn.dev/shintaroamaike/articles/d5437c8afd54c4
- Last Updated on: 2025-06-17



### Highlights & Notes

- システムプロンプトは、AIモデルの振る舞いを制御する基本的な指示のこと
- 「deep dive」（深掘り）
	「comprehensive」（包括的な）
	「analyze」（分析する）
	「evaluate」（評価する）
	「research」（調査する）
- システムプロンプトの特徴に従い、タスクの詳細、技術要件、コアコンポーネント、
	品質基準、実装ガイドライン、必須機能、コード品質、出力形式、追加指示のセクションをで
	それぞれ期待する設計に落とし込み指示をする
- 産業レベルの実装を要求し、単なる実験コードではないと指示する。
	具体的に使用してほしいライブラリの指定やコーディングルールを意識させる
	マルチリンガル対応や、GPU利用など実際に想定される要素を包含
	さらに、テストや設定管理といった観点を意識させる
	具体的なファイル構造を指定することで、体系的な出力を誘導する
- 必要に応じてテストコードを実装させるようにプロンプトを修正する
