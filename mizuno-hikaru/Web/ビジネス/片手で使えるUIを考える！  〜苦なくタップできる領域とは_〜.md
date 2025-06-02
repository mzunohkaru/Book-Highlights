# 片手で使えるUIを考える！  〜苦なくタップできる領域とは?〜

![](https://res.cloudinary.com/zenn/image/upload/s--IK4hO3df--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:%25E7%2589%2587%25E6%2589%258B%25E3%2581%25A7%25E4%25BD%25BF%25E3%2581%2588%25E3%2582%258BUI%25E3%2582%2592%25E8%2580%2583%25E3%2581%2588%25E3%2582%258B%25EF%25BC%2581%2520%2520%25E3%2580%259C%25E8%258B%25A6%25E3%2581%25AA%25E3%2581%258F%25E3%2582%25BF%25E3%2583%2583%25E3%2583%2597%25E3%2581%25A7%25E3%2581%258D%25E3%2582%258B%25E9%25A0%2598%25E5%259F%259F%25E3%2581%25A8%25E3%2581%25AF%253F%25E3%2580%259C%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_34:Imajo%2520%252F%2520Flutter%2Cx_220%2Cy_108/bo_3px_solid_rgb:d6e3ed%2Cg_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2NmYTM5ODQ3MWIuanBlZw==%2Cr_20%2Cw_90%2Cx_92%2Cy_102/g_south_west%2Ch_34%2Cl_default:og-publication-pro-mark-xcosax%2Cw_34%2Cx_217%2Cy_158/co_rgb:6e7b85%2Cg_south_west%2Cl_text:notosansjp-medium.otf_30:SODA%2520Engineering%2520Blog%2Cx_255%2Cy_160/bo_4px_solid_white%2Cg_south_west%2Ch_50%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2ZlOTVhMjJhYjMuanBlZw==%2Cr_max%2Cw_50%2Cx_139%2Cy_84/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: 片手で使えるUIを考える！  〜苦なくタップできる領域とは?〜
- URL: https://zenn.dev/team_soda/articles/3839de85b1c214
- Last Updated on: 2025-02-13



### Highlights & Notes

- TabBarで実装されているページも考慮されていて、一番左のTabのみでスワイプバックできるようになっています
- swipeable_page_route
  - Notes: https://pub.dev/packages/swipeable_page_route
- ![](https://storage.googleapis.com/zenn-user-upload/7cba0dec9a38-20241024.png)
- 親指ゾーンマップ(片手でタップできる領域を把握する)
- 緑の領域が苦なくタップできる領域
- ボタンを上に置かない
- ![](https://storage.googleapis.com/zenn-user-upload/f020f9da54ec-20241024.png)
- 画面を引っ張って検索する
- ![](https://storage.googleapis.com/zenn-user-upload/bb340fa66c01-20241025.gif)
- タブバーを長押しで検索する
- ![](https://storage.googleapis.com/zenn-user-upload/7dd9eb24e940-20241102.gif)
- スワイプバックのしすぎで親指の付け根が辛い
- 傾けた時と戻る時の角度の閾値を変える
- 傾けた時にボタンが移動する角度は40度ですが、逆に元に戻る時の角度は15度
