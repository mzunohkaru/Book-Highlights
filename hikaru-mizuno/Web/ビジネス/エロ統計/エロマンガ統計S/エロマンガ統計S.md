# エロマンガ統計シリーズ前回までのあらすじ

新たな時代のニッチな需要に誘われて エロマンガ統計、優雅に活躍！

まずは今までのシリーズの復習から。右ページのグラフは上が第 5 作「エロゲ統計」で使用したグラフ、下が前作、「エロマンガ統計 R（前作：エロマンガ統計 Relationship 版）」で使用したグラフです。詳しい解説や分析手法などについてはそちらの本をご参照ください。

エロゲを分析した結論として、以下の仮説が生まれました：

**2 軸 4 象限仮説**

- 横軸：恋愛 ⟷ 暴力
- 縦軸：エロ ⟷ ストーリー性

これらが交差することで生まれる 4 つの象限が、男性向けメディアの基本構造ではないかと考えられます。

エロマンガ統計 R では、その仮説を元に、今度は関係性を中心に分析を行いました。特に社会的な関係（教師と生徒、先輩と後輩、上司と部下などなど）や、性行為の際の主導権（どちらが誘ったのか）が性行為を描く際にどう作用しているのか、という点を分析いたしました。

その結果として見えてきたのが、視点と主導権の関係性です。物語を見つめる視点を持ったキャラクター（モノローグを描かれたり、冒頭で紹介される「感情移入が推奨されているキャラ」）は<u>主導権を相手に握らせていて、受け身的になっている</u>、ということが見えてきました。ここには、能動的に性行為をしたい！というよりも、==性行為をされたい、または性行為をされているキャラクターに感情移入したい、という読者の願望がある==のだと考えられます。

今回は、今までのシリーズの知見を確かめつつ、さらにストーリー全体の構造がどうなっているのか、序盤から中盤、終盤に向かってどのような描写が増え、また減っていくのか、そういった点を統計的に分析していきます。

---

![[hikaru-mizuno/Web/ビジネス/エロ統計/エロマンガ統計S/assets/img-1.jpeg]]

## 調査対象

前作「エロマンガ統計 R（前作：エロマンガ統計 Relationship 版）」で分析した作品をそのまま分析対象とする。

日本出版「雑誌のもくろく」に載っている成人向けマンガ雑誌、男性向けマンガ雑誌、またとらのあな秋葉原店、アニメイト秋葉原店の店頭に並んでいるマンガ雑誌、および Amazon.co.jp のアダルト向けマンガ雑誌のうち「成人向けマーク」が表紙に記されている雑誌および表紙がビニールテープで封印されている雑誌をエロマンガ雑誌と定義する。

エロマンガ雑誌のうち、2011 年 2 月に購入することができる 59 誌から Amazon の売り上げランキング上位 5 誌と、それ以外の 49 誌から 10 誌をランダムサンプリングして集計。

雑誌の中でも、目次に載っていない作品（宣伝のためのマンガなど）および性に関する描写（粘膜の接触もしくは性器の露出）が一切ない作品は除外し、245 作品、4828 ページを調査対象とする。ページごとの分析を行うため、1 ページずつ、各カテゴリーについてあり/なしをカウントした。

### 調査対象雑誌名

**Amazon ランキング上位雑誌（ランキング順）**

- COMIC LO
- コミックメガストア
- COMIC 天魔
- Comic MUJIN
- Comic RIN

**ランダムサンプリング群**

- ANGEL 俱楽部
- COMIC 同吽
- COMIC 華漫
- コミックメガミルク
- バズーカ
- ペンギンクラブ
- ボブリクラブ
- 快楽天
- 純愛果実
- 二次元ドリームマガジン

# 集計方法および分析方法

以下のカテゴリーについて、基本的にあり/なしを判断し、集計していく。例えば「男顔」のカテゴリーであれば、男性キャラの顔が描かれている場合にカウントする。これを各ページごとに行っていく。

分析には、基本的に Excel2010 の基本機能およびアドインソフト「エクセル統計 2008」を使用する。

## 基本項目

### タイトル

目次に載っているタイトルを記録する。

### ページ数

本編が何ページで構成されているかを記録する。

### カラーページ

マンガの一部にカラーページが含まれているかを集計する。全部がカラーでなくとも良い。

## 感情・行動

### 好意：男／好意：女

「愛している」「好きだ」という、相手に対して恋愛的な文脈で好意を寄せている台詞を明確に言った場合をカウントする。薬物等で自由意思が奪われている場合もカウントする。ただし、性行為が好き、性器が好きなど、相手の人格に対する好意ではない表明はカウントしない。

### 嫌悪：男／嫌悪：女

「嫌いだ」「気持ち悪い」など、相手に対して嫌悪の感情を明確に表明した場合にカウントする。「いやぁん」など、本気で嫌がっていないことが文脈から読みとれる場合などはカウントしない。

### 恥：男／恥：女

「恥ずかしい」という台詞を表明した場合、かあああ、という効果と共に赤面した場合をカウントする。ただ単に赤面している場合はカウントしない。（恥と興奮を識別するため）

### 抱擁・男から／抱擁・女から

相手を抱き寄せる行為をカウントする。基本的に相手の前から背中に腕を、もしくは後ろから胸に腕を回して、身体を密着させている状態をカウントする。性行為の最中（座位などで抱き寄せている状態）でもカウントする。ただし、ただ単に相手を拘束している状態（腕を押さえるだけで体が密着していない状態）などはカウントしない。

### キス・男から／キス・女から

唇同士が接触している状態をカウントする。そのキスの主導権があった性別をカウント。双方の合意で行われている場合は男女ともにカウント（典型的には結婚式でのキスなど）。偶然に唇が触れる場合については、集計を試みたが、例が 1 件もなかったため割愛した。

## 拘束・暴力

### 男・身体拘束／女・身体拘束

キャラクターが身体的に拘束されている場合をカウント。ロープ、拘束具、薬物、魔法などを典型とする。性行為を行う本人が手で動きを抑える等の行動は拘束には含まない。ただし、手足を押さえる役など、拘束用の人員を配置している場合は身体拘束に含む。

### 男・精神拘束／女・精神拘束

キャラクターが性行為を行う方向へ精神的に拘束されている場合をカウント。脅迫など、性行為を行わないと不利益になる場合を典型とする。経済拘束と重複する場合もある。

### 男・経済拘束／女・経済拘束

キャラクターが性行為を行うことに対して経済的な拘束を受けている場合をカウント。性行為の対価として金銭が支払われる場合を典型とする。性風俗店でのサービスなど、精神的な拘束は受けていないが経済的に拘束されている場合はありうる。

### 男・薬物／女・薬物

キャラクターが性行為を行うことに対して薬物・催眠・魔法など、相手の自由を奪う手段が用いられている場合をカウント。媚薬、惚れ薬、睡眠薬などを典型とする。

### 攻撃・男へ／攻撃・女へ

キャラクターに対して別のキャラクターから素手あるいは武器を用いて物理的な攻撃が行われた場合をカウント。ダメージについては問わず、ギャグ的な攻撃も含めてカウントする。転倒などの事故についてはカウントしない。

## 身体

### 男顔／女顔

キャラクターの顔が描かれている場合をカウントする。基本的には眼・口が見えている場合を顔としてカウントする。ただし、本来眼がある部分が別のモノ（目隠し・前髪など）で隠されて見えなくなっている場合についても顔が描かれているとカウントする。

### 男下着／女下着

キャラクターの下着が描写されているかどうかをカウントする。パンツおよびブラジャー、またはそれに相当する肌着を身につけている、および着かけ・脱ぎかけの状態をカウント。キャラクターから完全に離れている下着はカウントしない。

### 男胸部／女胸部

キャラクターの胸が描かれている場合をカウント。基本的に乳首、もしくは乳首があるべき場所が露出している場合をカウントする。胸の全部が露出していなくても、乳首が露出している場合は描写されているとカウントする。

### 男性器／女性器

キャラクターの性器・陰毛が描かれている場合、または性器のある部位を描いていると想定されるが、修正等で見えなくなっている場合をカウントする。修正の濃さ・薄さは問わない。

## 性行為

### 男 2 人／男多数／女 2 人／女多数

性行為に参加しているキャラクターの人数をカウントする。ここでの性行為とは、身体の直接接触または道具を用いての接触をしている/しようとしているなど、直接参加している場合を指し、ただ単に見ているだけの状態は含まない。2 人、3 人以上を区別してカウントする。

### 男手 → 女／女手 → 男／男足 → 女／女足 → 男／女手 → 女／女乳 → 男

キャラクターの愛撫行為についてキャラクターの性別、その使った部位、対象の性別をカウントする。触れる場所は性器およびその周辺、尻、肛門、乳首、乳房を対象とし、腕や肩、口、足などは含まない。どの部位に触れていても、手で触れている場合は手、足で触れている場合は足でカウントする。また女性が乳房で相手の性器などを愛撫している場合もカウントしていく。

### 男口 → 女／女口 → 男／女口 → 女

キャラクターの口、唇および舌を使った愛撫行為について、行ったキャラクターの性別、および対象の性別をカウントする。触れる場所は基本的に問わず、腕や足なども含む。ただし、口に対する口の愛撫はキスと判断し、ここには含めない。

### 男玩具 → 女／女玩具 → 男／女玩具 → 女

キャラクターがバイブレーターやローターなどの玩具を使って性器および肛門、乳首などを愛撫した場合をカウントする。玩具でなくても、棒、文具、野菜などを挿入する描写がある場合はカウントする。ただし、生物（触手など）については玩具として扱わない。

### 挿入・男性上位／女性上位／後背位

男性器を女性器に挿入する際の男女の位置関係をそれぞれカウントする。男女が正対し、相対的に男性が覆い被さるような体位になっている場合、いわゆる正常位を男性上位としてカウント。側位については、基本的に男性上位として集計する。相対的に女性が上になっている場合、いわゆる騎乗位を女性上位として扱い、座位は女性上位としてカウントする。また上下の区別なく、男女キャラが同じ方向を向いている場合、女性キャラの背中側から挿入している場合を後背位として扱う。側位も後ろから挿入していると判断できる場合については後背位として集計する。背面座位などの体位については、女性上位と後背位を同時に集計する。

### 挿入・肛門

女性の肛門に男性器を挿入している場合を集計する。体位については問わない。

## 服装

### 学生制服／体操服・ユニフォーム／スクール水着／職服

女性キャラクターの服装について集計を行う。各カテゴリーの服装について、全部または一部を着衣している場合を集計する。上着・スカート・ズボンに類する物を服装と扱い、靴下やアクセサリーについては服装として扱わない（制服を脱いでいき、靴下のみ着用している状態になったら制服着用と見なさない）。セーラー服、ブレザーなど、中高生の制服またはそれに類するものについては学生制服と判断する。ブルマ・スパッツ・短パンまた体育会系部活動のユニフォームなど、学生が運動用に使う衣装は体操服として扱う。スクール水着・競泳水着はスクール水着として扱う。メイド服、看護師の服、スーツなど、その職業に就いていることが明確に分かる服装を職服として集計する。

## 台詞等表現

### 性経験なし言及

作品の中で女性キャラクターに性経験がないことが明示された場合にカウントをする。「処女」「初めて」という言葉での表現を基本とする。慣れていない、などの場合はカウントしないが、破瓜血などが描写され、性経験がないことが明らかな場合はカウントする。

### 低年齢禁忌／既婚禁忌／関係性禁忌／場所禁忌

性行為をしてはいけないという意味で「こんなことをしてはダメだ」や性行為を「してしまっている」などの禁忌に触れている、という表現についてカウントする。低年齢は、「こんな年下の相手と性行為をしてしまっている」「学生なのに…」という禁忌を、既婚は「相手は人妻なのに…」「私には夫が…」という禁忌をカウントする。関係性については、血縁など「兄妹でこんなこと…」また「生徒とこんなこと…」という台詞等をカウントし、場所は「こんな場所で…」という禁忌感情の表現をカウントする。

### 淫乱指摘

女性キャラクターに対して淫乱だ、という指摘が行われた場合にカウントする。「こんなに濡らして…淫乱な女だな！」「かわいい顔なのにこんなに…」などの台詞が対象。ただしモノローグは除く。

### 男性器名称／女性器名称

女性キャラが性器の名称を口にした時にカウントする。男性器は「おちんちん」「ちんこ」「ちんぽ」「ペニス」、女性器は「まんこ」を基本とする。描き文字、写植を問わずにカウント。また、伏せ字で名称の一部が隠れていてもその語が連想できる場合はカウントする。

# 低年齢禁忌/既婚禁忌/関係性禁忌/場所禁忌

性行為をしてはいけないという意味で「こんなことをしてはダメだ」や性行為を「してしまっている」などの禁忌に触れている、という表現についてカウントする。低年齢は、「こんな年下の相手 と性行為をしてしまっている」「学生なのに…」という禁忌を、既婚は「相手は人妻なのに…」「私 には夫が…」という禁忌をカウントする。関係性については、血縁など「兄妹でこんなこと…」ま た「生徒とこんなこと…」という台詞等をカウントし、場所は「こんな場所で…」という禁忌感情 の表現をカウントする。

## 淫乱指摘

女性キャラクターに対して淫乱だ、という指摘が行われた場合にカウントする。「こんなに濡らし て…淫乱な女だな！」「かわいい顔なのにこんなに…」などの台詞が対象。ただしモノローグは除く。

## 男性器名称 / 女性器名称

女性キャラが性器の名称を口にした時にカウントする。男性器は「おちんちん」「ちんこ」「ちんぽ」「ペニス」、女性器は「まんこ」を基本とする。描き文字、写植を問わずにカウント。また、伏せ字 で名称の一部が隠れていてもその語が連想できる場合はカウントする。

## 【エロ表現記号】

## 母乳

女性キャラが胸部から母乳を出している場合をカウントする。妊娠出産の有無を問わない。台詞な どでの言及が無く、汗等と区別がつかない場合はカウントしない。

## 愛液

女性キャラの女性器が愛液で湿っている、という表現がされている場合を集計する。下着越しに垂 れてくる描写や、愛液が指、道具、男性器等に付着している場合も集計する。

## 記号

女性キャラの台詞中にハートマーク（♥）、星マーク（☆）、音符（♪）、などが含まれ、通常の声 ではない、という表現がされている場合をカウントする。描き文字、写植を問わない。

## 描き文字

女性キャラクターの台詞が写植ではなく、手書きまたは手書き風に描かれ喘ぎ声がさらに乱れてい る、という表現についてカウントする。

## アヘ目

女性キャラクターの瞳が焦点の合わない状態で上を向き、瞳のラインの下半分が見えている状態を アヘ目と判断してカウントする。

## 涙目

女性キャラクターの目から涙が流れている、または目尻に溜まっている状態をカウントする。汗等 と区別できない場合はカウントせず、また、潤んでいるだけの状態はカウントしない。

## 否出し

女性キャラクターが否を突きだし、唇の上よりも先に否が出ていると判断できた状態をカウントす る。フェラチオや食事シーンでも集計する。

# 断面図・透過図

挿入シーンなどで、女性器内、肛門内などの身体の内側を描いている表現をカウントする。フェ ラチオなどで喉の奥が透過されている場合も同じようにカウントする。

## 触手・異生物

女性器・女性の肛門・口内に人間ではない生物の一部または全部が侵入している場合をカウント する。獣姦を含み、生物の部位は性器、舌、その他の部位を問わない。また生物の実在、非実在 は問わない。

## 腟内射精 / 口内射精 / 肛門射精 / 体外射精

男性器から精液が射精されたシーンを集計する。射精された瞬間のコマをカウントし、射精後に漏れているだけの状態はカウントしない。射精の瞬間は「ドクッ」「ドビュ」などの擬音で判断 していく。射精した場所によって、膣内、口内、肛門内を集計し、それ以外のものについては体外射精としてカウントする。コンドーム等の避妊具を使った場合は、射精した時の場所、例えば膣内に挿入していた時に射精した場合は膣内射精として集計する。

## 精液

精液が描かれているかどうかを集計する。射精した瞬間でなくても、身体などに付着している状態も合わせて集計を行う。

## <基本的な仮説 〜ツンデレ運動エネルギー仮説〜>

エロマンガ統計シリーズ全体の根底を流れる仮説について紹介しておきます。名付けて ツンデレ運動エネルギー仮説です。`男性向けのエロ表現では、ツンからデレへ移動 する際の運動エネルギーをエロとして消費している、という仮説`です。

簡単に言うと、男性向けのエロが発生するのは「脱ぎかけ」の状態であり、脱ぎきった状態＝それ以上エロ方向へ動かない状態では運動エネルギーが起こらず、エロさを感じ られない、ということです。たとえば全裸で物怖じせず仁王立ちしている女性がいても、 それ以上脱がしようがないため、エロを感じる余地が少ないと考えられます。それよりも半脱ぎなどの、これから衣服を剥げるキャラクターにエロスを感じる、ということです。

ツンデレキャラはデレかけがおいしいと言われます。攻略の難しい（＝ツン）対象を征服した（＝デレ）瞬間に対して、読者はエロを感じるのでしょう。そして、征服が終わっ た対象については、エロス的興味は薄れていくと推測できます。

ツンをあまり見せずに簡単にデレてしまう＝性行為ができてしまうという状態は、初期の位置エネルギーが小さく、十分な運動エネルギーを得られない状態だと考えられます。 そのため、エロマンガにおいては、==ツン＝性行為ができない状態と、デレ＝性行為の激 しさの落差をできるだけ大きくし、その脱がす過程、剥ぐ過程の悦びを描いている==のだと推測しています。エロを描く方法としては、初期の位置エネルギーを高めるために、 ツンの大きさである処女性を高め、なかなか性行為ができない状態を作る方法 と、逆に落下点を低く、強烈なデレ＝強烈な性行為を描いて性的な存在へと「堕 とす」方法と、これらを組み合わせたパターンの、大きく 3 つに分別できると思われます。今回も、この仮説を根底において、エロマンガの分析を行っていきます。

---

# 単純集計 ①

# 身体・感情・感情行動

## 一つだけ言える真理がある。

## 「男の顔は黒に染まれ」

さて、単純集計のコーナーです。基本的にグラフをそのまま見ていただければと思 います。この辺りのグラフを見る時は、横軸にご注意ください。例えば一番上は最大 が 100% に対して、真ん中のグラフは最大が 7% です。「グラフで比べてみるとむし ろ高いように見える」ではないですが、その辺りはご注意ください。また、%は全 4828 ページのうち、何%に描かれているか、ということです。例えば<u>女性の顔は 4828 × 0.9762 = 4713 ページで描かれている</u>ことがわかります。

中身を見ていくと…‥まずは顔。女性キャラの顔がほぼ全編にわたって描かれてい るのに対して、<u>男性キャラの顔は 6 割強</u>。これを多いと考えるか少ないと考えるか は難しいですが、エロマンガでは==男の顔より女の顔が見たい==、というある意味、常識的な帰結が統計的に現れました。胸部についても同じで、男の胸は描かれ ないのですが、性器だけは別。<u>男女関わらず、性器は多くのページで描かれていました</u>。男の価値は性器にしかない、とまでは言いませんが、そういった傾向を読み取る ことは十分に可能でしょう。
感情について見てみると、<u>女性キャラの方が感情を表明している</u>傾向が見られます。女性キャラの反応を見たい、というのはエロマンガ的に当然かもしれません。穿った見方かもしれませんが、感情を表に出さないことによって「男は黙って男性器の役割 を果たせ」という隠れたメッセージを読み取れます。いわゆるエロゲ的主人公と言い ますか、==顔無し・声無し・特徴無し、でも男性器とエロテクニックだけは立派、 という性的存在としての男性が求められている==と考えることもできそうです。

行動についてはあまり大きな差は見られませんでしたが、<u>男性キャラからのキスの方が女性キャラからのキスよりも多い</u>、というのは意外な結果でした。性器でない存在としての個性を出せる数少ない機会ですので、男性キャラの個性にとってのキスは ある意味、最後の砦なのかもしれません。

![[hikaru-mizuno/Web/ビジネス/エロ統計/エロマンガ統計S/assets/img-2.jpeg]]
![[img-3.jpeg]]
![[hikaru-mizuno/Web/ビジネス/エロ統計/エロマンガ統計S/assets/img-4.jpeg]]

---

# 単純集計 ②

# 性行為・挿入行為

## 犯せないなら縛ればいいのに……

## 挿入シーンは全体の 36.8%

続いては性行為についての単純集計です。まずは拘束について。身体的な拘束や精神的・経済的な拘束についてですが、やはりというか、<u>女性が拘束の対象</u>とされる ことが多くのページで描かれていました。女性を拘束して性行為に及ぶ、というストーリーは王道とでもいいますか、ここには==『拘束しないと性行為できない対象』 として、女性の価値、女性と性行為をする価値を暗に高めている==という概念 があるのだと考えられます。==普通ではない手段を使ったから、高い価値の物が得られ るはずだ==、という形でしょうか。

愛撫行為については、女性キャラ同士のいわゆるレズ・百合系描写の割合は低めに なっています。最も多い口での愛撫も、どちらかというと多人数プレイにおける行為 のバリエーションとなっており、<u>女同士描写の 83.6% が多人数プレイ</u>におけるもの でした。男女カップリングでの行為を見ると、なかなか面白い結果が出ていて、<u>男からは手での愛撫比率が多く</u>、<u>女性からは口での愛撫が多い</u>、という結果が出ています。 フェラチオが一般化している現代で、女性からの口愛撫が多いのは当然なのかもしれ ませんが、この非対称性はなんらかの意図があると考える方が妥当でしょう。詳しく は多変量解析等で詳しく見ていきます。（なお誌面の都合でグラフ化はしていません が、女乳 → 男は 2.53% と、手愛撫よりやや低い、という結果でした。)
<u>挿入体位については、いわゆる正常位として、男性上位である体位の描写の割合が最も多く</u>なっていました。描写として、結合部・胸・顔が描きやすいということもあ り、エロマンガを書く上で多用されていると考えることができます。後背位と女性上位については、また後ほど意味を考えていきたいと思っていますが、とりあえず挿入 シーンについてひとまとめにしてしまうと、挿入は、やはりエロマンガの華。な んらかの<u>挿入シーンが描かれているページは全体の 36.8%</u> に及びました。 （なお、単純合計より少ないのは 1 ページで体位を変えている場合に複数カウントし ているからです。)

![[hikaru-mizuno/Web/ビジネス/エロ統計/エロマンガ統計S/assets/Img-5.jpeg]]
![[hikaru-mizuno/Web/ビジネス/エロ統計/エロマンガ統計S/assets/Img-6.jpeg]]
![[hikaru-mizuno/Web/ビジネス/エロ統計/エロマンガ統計S/assets/Img-7.jpeg]]

---

# 単純集計 ③

# 射精・エロ表現・台詞

## エロマンガスーパー汁エット! ハートマークリオーケストラ!

さて、続いてはいわゆる「エロさ」を表現する描写についての単純集計です。まず は射精表現から。射精している瞬間に限らず、精液が付着している描写も含めてのものですが、<u>精液表現は 16.45% のページで描写されています</u>。 1 本のマンガが平均 19.7 ページですので、 <u>1 マンガに付き、 3 ページ程度は精液が描かれている</u>、とい う計算になります。射精場所については、やはり人気トップなのか腟内射精、いわゆる中出しですね。そこに射精するのが動物的自然とは言え、人間の場合は==中出しすることに支配欲・征服欲などの意味を付加==し、複雑に混ざり合ったエロ スを形成しているからこそ、高い割合になるのだと考えられます。

愛液などの表現については、愛液表現の率と女性器の出現率（54.85\%）を比較する と見えてきますが、<u>女性器はほとんど「濡れているのが当然」</u>という勢いと考えられ ます。目には涙を浮かべ、舌を出して唾液をたらし、そして集計はしていませんが汗 をかいて……ということを考えると、==エロマンガでの女性は常に湿っている==、という ことが見えてきます。==精液描写も含め、体液で濡れているという状態はそれだ けでエロスを引き出す描写==なのだと推測することができます。

さて、次は台詞の中に現れるエロ描写ですが、単純に<u>台詞の中に記号を入れる</u>のは、 それがエロい言葉である、という表現が容易に示せるためでしょう。「気持ちいい！」 よりも「気持ちいい」というほうが感じているというのが伝わるのかもしれませんし、同じような理由で、<u>手書きの文字の方が通常の状態ではない、ということを表す</u>ことができるのでしょう。「きをすいいっ」

エロマンガにおいては、キャラクターの感じている快楽について、表情と台詞で表現しなくてはならないため、こうして記号や描き文字という表現方法を開発・発展させてきたのだと考えることもできます。また、いわゆる卑語として、性器の名称を言う／言わせる描写に関しては、 5% 前後と、記号などに比 べるとやや少なかったかと思います。

![](Img-8.png)

---

# 単純集計 ④

# 禁忌・服装・人数
## 詰君らの愛したブルマは絶滅した！ なせだ!?「エロいからさ」

続いて性行為に対する禁忌について。「こんなに小さな子と…」「まだ処女なのに…」「兄妹でこんな事…」など、一般的なモラルから外れた部分で性行為が行われているのではないか、また禁忌が強調されて描かれているのではないか、という予想に基づいて、そうした台詞が書かれているかを集計いたしました。結果は全体的に出現数がやや少ないですね。それとも禁忌が明示的に示されることは少ないのは、例えば既 に兄妹間での禁忌というテーマは既に描ききられており、読者にとって自明の理になりすぎているということかもしれません。だからわざわざ描く必要が無く、こういった数字に収まっているとも考えられます。また、例の条例の影響で、インモラルな関係を自主規制している、というのもあるかもしれません。

服装につきましては、やはり制服は強かった、という所でしょうか。<u>体操服やスクー ル水着など、学生であることを示すアイテムの人気はかなり高かった</u>です。対して、職業の服は代表的にコレ、というモノは少なかったように思われます。<u>教師のスーツ、 ナース服、メイド服などが多かった</u>ような印象です。

==制服を着た学生というキャラクターには、つまり若く、そして処女性も高いということが求められている==のでしょう。聖なる処女性を侵蝕する過程にこそエロスは宿ると考えていますが、学生制服は処女性 + 読者のリアル記憶・妄想が色々と混じり合 うことで、特有のエロスを生み出し続けています。2005 年に日本から絶滅したはず のブルマが、まだ作中で使われていることも考えると、読者や作者にとって、==ブルマなど学生時代の制服に対するフェティッシュ的愛情は積もり積もって大きくなっていると推測する==のがよいでしょう。

人数については、<u>男性は多数で、女性は2人で、というパターンがやや多く</u>なっています。これは顔を描いて個性を描かなくてはならない女性キャラは何体も出せな いが、自己主張しない男性器 = 男キャラならばいくらでも動員できるのだ、という部分から説明できるのではないでしょうか？

![[hikaru-mizuno/Web/ビジネス/エロ統計/エロマンガ統計S/assets/Img-9.jpeg]]
![[hikaru-mizuno/Web/ビジネス/エロ統計/エロマンガ統計S/assets/Img-10.jpeg]]
![[hikaru-mizuno/Web/ビジネス/エロ統計/エロマンガ統計S/assets/Img-11.jpeg]]

---

# 作品ごとの描写率相関

# 単相関・無相関の検定

## 相関ちつちゃくないよ！関係なくないよ！玩具使用は相手を縛ってから。

さて、相関係数。各作品ごとの「その描写があったページ」を合計して算出します。 (作品名をカテゴリ変数とした層別の記述統計量を計算します。) それを作品の総ペー ジ数で割り、「その作品でそのカテゴリーが登場する率」と見なします。例えば 20 ペー ジの作品の 10 ページにおっぱいが出てきたら「 50% 」 5 ページだったら「 25% 」です。 そして、関係性の高い、低いを見る表をゴニョゴニョと計算して作成します。今回は相関係数 0.3 以上の相関があるモノを見ていきます。だいたいの目安として相関係数は 0.3= 弱い相関、 0.6= 普通の相関、 0.9= 強い相関です。学術的には 0.6 くらい からが使い物になるイメージです。

こういった結果については、本来は相関表を載せるのが筋ですが、あまりに量が膨大になるため、今回は無相関の検定結果および偏相関の表のみを掲載いたします（詳 しくは次ページで)。まずは単純な相関＝単相関の相関係数（右ページ上の表）をご覧下さい。1 番上については、いわゆるアナルセックスと、肛門への射精の相関が高 いのはある意味当然です。しかし、それ以下、男性への玩具＆身体拘束がセットになっ ていることを見ると、==玩具は主導権を持った立場から使用される物==だと読み取れそうです。また、<u>男が多数になると、身体拘束・肛門への挿入などが増え</u>、==男が多数という状態は、女性を身動きできないようにしての多穴輪姦==、というのが多いこと が統計的に見ることができます。

さて下の無相関の検定ですが、これは「2 つの変数が関係なくないよ！」という、回りくどい調べ方です。\*\*が 2 つついているところは特に関係なくないところです。例えば男が多数出てくるマンガでは、身体拘束や肛門挿入、また手による愛撫が出現しそうである、ということが見えますし、女性上位の挿入が描かれる作品だと男が身体拘束され、また足愛撫（いわゆる足コキ）が描かれそうだと読み取れます。女性からキスする作品では描き文字が書かれやすそうなど、他にも いくつか面白い発見がありそうなので、ぜひ色々と探してみてください。

| **挿入・肛門** | **肛門射精** | 0.67 |
| :------------- | :----------- | :--- |
| 玩具 → 男      | 男・身体拘束 | 0.49 |
| 女口 → 男      | 口内射精     | 0.45 |
| 描き文字       | 記号         | 0.41 |
| 男多数         | 女・身体拘束 | 0.38 |
| 男多数         | 肛門射精     | 0.38 |
| 挿入・後背位   | 膣内射精     | 0.35 |
| 女口 → 男      | 女子 → 男    | 0.34 |
| 挿入・肛姦     | 男多数       | 0.34 |

![[hikaru-mizuno/Web/ビジネス/エロ統計/エロマンガ統計S/assets/Img-12.jpeg]]
※グレーで塗られているところは 1% 有意のマスです。(変数の関係がなくない部分)

---

# 作品ごとの描写率相関

# 偏相関行列

## 知ってるよ。私。挿入体位の選択にな んらかの意味があるってことぐらい。

では続いて、偏相関行列です。単相関とは違って、他の要因の影響を取り除いた係数、というものです。例えば「年齢が低さと走るのが遅いのとは相関している」という結果は、「年齢が低さと身長が低さの相関」と「走る遅さと身長の低さの相関」という別の要因が原因なのかもしれないということで、そういった別の要因を計算上で取り除いて計算されたのが偏相関と呼ばれる物です。扱いは色々と難しい代物なので すが、今回は特にその辺りを気にせずに相関表として使います。
右の表をご覧下さい……気の遠くなるように数字がたくさん並んでいますが、見るべき部分は薄いグレーまたは濃いグレーで色が付けてあります。その部分が偏相関 0.3 以上の、関係が深いと予測できる数値です。濃いグレーに関してはマイナスの相関で、「片方が出てくる時にはもう片方は出ていない」という関係になっています。

データを見ていくと、やはり<u>身体拘束の部分で、男性が多数と女性への身体拘束が、男性への玩具と男性への身体拘束が相関</u>しています。これらの表現は定番であると言 えるでしょう。

また、<u>アヘ目と舌出し、描き文字と記号が相関</u>し、エロ表現にも相性のいい組み合 わせはあるようです。描き文字でハートマークが書かれることや、アヘ目状態で表情が崩れ、舌を出すことは容易に想像できる結果です。また、射精についてはある意味当然の相関として、膣内と挿入、口内と口愛撫、肛門と肛姦が相関しているのがわか ります。

負の相関がある部分は、挿入体位についての部分です。この部分から推測できるこ とは、一つの<u>作品で次々と体位を変えている、というよりも、</u>==作品ごとに体位を選んでいる==、ということが見えてきます。つまり、キャラの関係性や性行為を行う状況、または作家性などによって、どの挿入体位を選ぶのか、その体位なら ではの必然性が隠れているのではないでしょうか。

![[hikaru-mizuno/Web/ビジネス/エロ統計/エロマンガ統計S/assets/Img-13.jpeg]]

---

# 各描写の描かれる位置

# 三分割での描写集計

## ストーリーの流れを追って…エロのビートはもう、止められないわ!

さて、ストーリ一統計の名に恥じぬよう、各区分でどんな描写が描かれているのか を分析していきましょう。まずは、各描写がどの位置で描かれるのかを集計しました。<u>「学生制服は 4828 ページ中 14.5% で書かれている」</u>ことは単純集計で書いたとお りですが、その 14.5%=700 ページが、作品を三等分した時にどの位置に多く出現 するのかを調べたのが右のグラフです。<u>学生制服の描かれたページの 42.45%=297 ページが序盤で登場し、中盤では 32.86% が登場、残りの 24.71% が終盤で描かれている</u>、ということになり、全体の中では序盤で多く登場する、ということがわかり ます。これによって各シーンの特徴的な描写を算出しようというのがこのページの魂胆です。

それではグラフを見ていきましょう。<u>学生制服やキス、各種愛撫行為が多く描かれ るのは序盤</u>、ということがわかります。続いて<u>中盤で多く描かれる傾向にあるのが各種愛撫、キス、性器描写</u>ですね。そして<u>挿入やアヘ目・描き文字などの各種エロ表現記号、そしてクライマックスたる精液が終盤で描かれる</u>ことが多くなっていることが わかります。

この結果からエロマンガストーリーの全体の流れをなんとなく確認できますね。序盤では挿入の比重は小さく、また全てのカテゴリーで全体的にパーセン テージが低いことからも、エロに関係がない描写が多いのではないか、と考えられます。そしてキスや愛撫などの軽い行為が開始され、徐々に本格化していくのが中盤。ここでは愛撫が多く描かれ、性器の露出もどんどん高まっていきます。そしてクライマックスたる終盤では、各種挿入シーン が描かれて、最終的に射精する、という展開が読み取れます。

とりあえずデータの分布の概要は掴めましたでしょうか？次のページから、折れ線グラフを用いて、物語の進行度……全体を 11 の区分に分けて、それぞれの場面で どんな描写が多く描かれているのか、細かく見ていこうと思います。

![[Img-14.jpeg]]
![[Img-15.jpeg]]
![[Img-17.jpeg]]

---

# 描写率の推移 ①

# キャラクターの感情

## こんな折れ線グラフなんかに……悔しいつ……でも、反応しちゃうっ!

さて、今度は進行度に応じた描写率を見ていきましょう。各ページをその作品の総 ページ数で割ることで算出した 11 の区分で、その作品の進行を判別するという形で す。大まかに描写がどう推移していくのかを見ています。注意していただきたいのは縦軸のパーセンテージです。その描写のピークは見えますが、上限が 100% ではな いということに注意してください。そこを間違えてしまうと、色々と悲惨です。

では、まずはキャラの感情の動きから。好意に関しては、終盤で最も高くなってい ます。イメージとしては性行為の最中に「キミが好きだ-！」と叫びながら果てて「こ れからもよろしくね」で終わる、というところでしょうか。<u>性行為をした後での告白</u>の形になっているのが見えます。また、進行度 5、中盤辺りでは、女性からの好意表明が高くなっています。この高さは<u>女性が「あなたが好きなの！」と告白し、そこから性行為が始まるというストーリー</u>に由来していると考えられます。

嫌悪感情については、男性からの嫌悪が少なかった、というのはさておき、女性キャ ラクターの嫌悪感情の動きについて。進行度 5 を境に段々と下がっていることが見えます。次ページ以降のグラフで見えてきますが、進行度 5〜6 は性行為が始まっ た辺り。<u>始まった時は拒絶が強く、激しく嫌悪を表しますが、性行為が進むにつれ て、徐々に嫌悪を示さなくなる傾向</u>が見えてきます。まさに「悔しいつ……でもっ ……(どかどか)となっていく様子が統計的に見えてきました。

また、恥ずかしい、と表明するのは、序盤が多かったです。<u>恥ずかしい行動をとることによって興奮、もしくは恥ずかしい行動をとらせることによって興奮し、その勢いで性行為に移行</u>していくというストーリーが予想できます。典型的には、「こんな恥ずかしい格好を……」という所でしょうか。性行為に入る前の進行度 3 から 4 辺 りで山を作っています。そして、<u>最後まで恥ずかしい、という表明をし続けることが ない</u>ことは、==嫌悪と同じように、恥ずかしさに慣れ、受け入れ、そして服従していく、 という流れがある==のではないでしょうか。

![[Img-18.jpeg]]
![[Img-19.jpeg]]
![[img-20.jpeg]]

---

# 描写率の推移 ②

# キス・抱擁・拘束

## 抱きしめて！エロスの果てまで！性行為は支配のための手段？

続いてはエロ描写ではないけれど、そこにつながる描写についての推移を見ていきます。まずは抱擁。今までの統計シリーズの分析から、抱擁は恋愛を示すバロメーターとして、かなり優秀なものだということが分かっています。その抱擁ですが、中々に面白い推移をしています。終盤が最も高くなるのは「これからもよろしくね」的な展開だと予想できますが、進行度 4 ～ 6 くらいの<u>中盤ほどに高くなるのは、性行為の前に抱きしめて……というラブラブ展開があるから</u>でしょう。途中で男女の主導が逆転している現象についてですが、全体的な出現率が低いため、上下動の振れ幅が大きいことには注意しつつ考えると、進行度 6 は愛情表明で女性キャラからの表明が多かった進行度。<u>女性が進行度 4 で抱きしめて 6 で告白、もしくは告白されたのを男性が抱きしめて受け入れて性行為、という 2 つの流れが推測できます</u>。男は黙って抱きしめろ、ということでしょうか。

キスについては、後半に高くなる傾向がありますが、進行度 7 くらい……<u>性行為が始まって前戯から挿入に移るあたりで低くなっています</u>。この辺りでは性器的な快楽を中心に描き、ラ<u>ストに向けてストーリー的にキスで盛り上げていく</u>、という流れが見えます。進行度 11、後日談的な、あるいはピロートーク的な部分で低くなっていることから、==エロマンガでのキスは愛情表明を兼ねた愛撫の一部である==と解釈するのがよさそうです。

身体拘束については、男性に対する拘束が低いのは既に見てきたとおりです。<u>女性に対する拘束は、序～中盤で縛り始め、性行為が本格化した終盤で解ける</u>という流れが見えてきます。<u>拘束は相手の自由を支配するための道具ですが、挿入することによって相手を支配しているから身体拘束に頼らずともいいため拘束が解けていく</u>、ということが推測できます。男性向けエロマンガの世界では、==セックスとは相手を、女性を支配する手段である==、という側面が決して弱くはないのだと考えられます。この辺りは嫌悪と同じく、悔しいピクピクな展開と近いものがあると思われます。

![](hikaru-mizuno/Web/ビジネス/エロ統計/エロマンガ統計S/assets/img-21.png)

---

# 描写率の推移 ③

# 身体描写・人数

## おっぱいは女性器の一部です。男性器は顔の一部です？

男性キャラの身体描写と女性キャラの身体描写について、それぞれの性別で一つず つのグラフにまとまっています。やや見づらい部分がありますがご勘弁を。

身体描写について、性器の描写については男女共に後半のクライマックスに向けて徐々に高まっており、特に違いがなさそうです。が、それ以外の身体描写については男性キャラと女性キャラとの間で描写のされ方に差があることが見えます。まず男性 キャラの下着・胸についてはそもそも描写率が低く、女性キャラの下着・胸に比べて読者が求めていないであろうということはすでに見たとおりです。女性キャラの胸に ついてですが、描写率は性器と同じように変遷しており、女性の胸は性器の一部 である、と考えることができそうです。

女性キャラクターの顔の描写率は高いままで推移しており、女性は顔が命です！と いう感じでしょうか。その表情の変化をずっと見ていたいという欲求が感じられます。対して、男性キャラの顔描写率はかなり興味深いモノがあります。男性器の描写率が上がっていくと顔の描写率が下がり、ラストにはまた逆転していくという流れの部分 です。これは男性の顔を描く代わりに性器を描いている、ということが考えられます。男性キャラのアイデンティティが顔から性器に移っていく、とでも言いましょうか。性器があれば男性キャラの顔なんて必要ない、と言い換えてしまうのは乱暴か もしれませんが、そういった側面がないとは言えないでしょう。下着の描写率・胸の描写率の低さを見ると、男性キャラは後半の挿入シーンでほとんど性器しか描かれて いない、性器以外に価値を置かれていない、ということがデータからわかります。

人数については、徐々に参加する人数が増えていく、というストーリーが見えます。性行為が進む度に盛り上がって「あたしも混ぜろよ！」と、次々と性行為に関わるキャ ラクターが増えていく、という形でしょうか。性行為が開始されている中盤（進行度 6 付近) で前戯に参加し、終盤（進行度 9 付近）でフィニッシュに参加する、という イメージでとらえると全体の流れがとらえられそうです。

![[img-22.jpeg]]
![[img-23.jpeg]]
![[img-24.jpeg]]

---

# 描写率の推移 ④

# 愛撫・衣装

## 中盤のクライマックスは愛撫　セーラー服は脱がしてしまいます。

続いては愛撫行為の推移です。女性キャラから女性キャラへの愛撫は数が少ないた め、今回は割愛しています。まず男性に対する愛撫を見ると、口を使った愛撫の割合 が高く、特に中盤での高さが目立ちます。前戯としてのフェラチオは定番となってい る、と言えそうです。手や足、また乳を使った愛撫についてもほぼ同じで、女性か らの能動的に動いて行う愛撫は中盤が山場になっており、終盤は挿入され る立場として描かれていると言うことが予想できます。

女性に対する愛撫としては、男性に対するものと比べると興味深い点が二点ほどあ ります。一点目は手による愛撫の推移。こちらは女性の口愛撫などとほぼ同じく、序中盤で描かれる愛撫行為ですが、女性からは口を使った愛撫が、男性からは手を使っ た愛撫が多いというアンバランスさが存在します。男性から口を使った愛撫では頭に隠れて「肝心の」性器が見えないという作画上の理由もありそうですが、立場的な上下関係もあるのかもしれません。二点目として、男性の口を使った愛撫について、他 の愛撫行為とは違い、中終盤近くに頂点が来ています。今回のデータではとれていな いですが、これはおそらく、挿入中に胸を舐める、などのスタイルでの愛撫が原因で このような形になっていると考えられます。挿入後にも能動的に愛撫ができる男性 キャラと、挿入される女性キャラでいい対比が描けていますね。

服装については、特に学生制服でわかりやすいですが、徐々に脱いでいく、という ストーリーが見えます。エロスの基本は処女性を剥ぐ過程にある、というのが このエロ統計シリーズ全体の仮説ですが、こうしてデータとして、処女性＝若さと経験の少なさを示す制服というアイテムを徐々に脱がしていく、というストーリーが見え、仮説が裏付けられていると考えられます。対して職業の服など、アイデンティ ティを示すために必要なアイテムは制服に比べると一般的ではないからか、脱げにく い構造があると考えられます。極端に言ってしまえば、その服こそがキャラのアイデ ンティティであり、脱がない、脱がさない、脱がせないのかもしれません。

![[img-25.jpeg]]

---

# 描写率の推移 ⑤

# 挿入・禁忌・体液表現

## こ、この女体は！贅沢に使われた処女の（中略）まさに至高で究極の女体だ！

続いては挿入体位についての推移を見てみましょう……推移の仕方は体位によって差はないと言えますね。後半のクライマックスに向かって一気に挿入し、そし てラストでは一気に下がるという、なんとも男性の快楽的な、熱しやすく冷めや すい上がり下がりをしています。体位を限定せずに「挿入」というカテゴリーを設定 すると、おそらくこれよりも露骨に終盤への集中が見られると考えられます。

続いては禁忌感情についてですが……なかなか乱高下しているグラフです。それだ け微妙なカテゴリーであり、まとめて推移を見るにはあまり適さないグループなのか もしれませんが…進行度ごとに見ると、進行度 3 付近で一つ目の山があります。こ こは例えば実の姉妹に触られて「兄妹でこんなことダメだ…」的な最初の抵抗の反応 だと推測できます。続いての山は性行為が始まって挿入に至る直前から直後、特に低年齢禁忌辺りが典型ですが「こんな小さな子に俺は挿入しているんだ！」という、禁忌を犯した喜びが描かれているのではないかと考えられます。また性経験がない、と いう相手については挿入する直前や挿入直後にその言及がある、ということが見えま す。仮説ですが、挿入する、ということの肉体的な気持ちよさはマンガでは表現できないため、付加価値として、価値のある部分に挿入しているのだ、 という後付けが必要なのではないかと考えられます。料理番組で、なぜこの料理 が美味しいのか、その素材がどれだけ貴重なのかを説明するのとどこか似たような、 そんな印象を受けました。

体液の描写については……やはり後半に向けて徐々に上昇していく、という予想通 りの結果が見えました。また、今までもいくつかのグラフで見えているのですが、序盤でスタート地点からほんの少しだけ下降している流れが読み取れます。これはカ ラーページを使う作品に典型ですが、まず性描写を描くことで読者を引きつけて、そ の後に回想シーン的になったり二回戦に突入したりする、という描き方が現れた結果 だと解釈できます。

![[img-26.jpeg]]
![[img-27.jpeg]]
![[img-28.jpeg]]

---

# 描写率の推移 ⑥

# 台詞・表現・射精位置

## エロさはカゲキにガンガンゆこうぜ！結果は今、一つとなり、最後の戦いが始まる！

エロマンガの表現として、絵も重要ですが台詞もエロさを演出する重要な手段。と いうことで、台詞の中に登場する、淫乱さを指摘する台詞や、性器名称についての推移から見ていきましょう。後半に高くなるのは挿入と近い部分ではありますが、挿入 に比べてピークがやや早く訪れていることが分かります。特に男性器の名称について はちょうど挿入が上がり始める進行度 6〜7 でピークを迎えます。これはアレですね、「もう我慢できないの…‥早く（男性器名称）をちょうだい！」というアレですね。で、 それに対して「淫乱な女だ…」とか言いながら挿入、という流れですねわかります。

エロ表現についても、基本的に後半の挿入シーンに向けて盛り上がってくる形に なっています。アヘ目や涙目などは、正常ではなく、快楽に悶えて表情が変わって いる事を示す表現ですが、こういったエロ記号を徐々に加えていって、快楽が増していますよ、という情報を読者にわかりやすく伝えているのでしょう。 おそらく、意識しないでエロマンガを読んでいても、「そろそろクライマックス（＝射精タイミング）だな…‥」ということは理解できる設計になっていると推測できます。

射精位置については、口内射精と体外射精を組み合わせてグラフを作成しました。 こうすることで、中盤のフェラチオ、終盤の膣内射精、というストーリーの構成が浮 き彫りになって見えます。

さて、描写率の推移を見てきたわけですが、全体的にみると序盤はエロ描写を控えめにして処女性・貴重さを描写し、告白などのイベントを経て、中盤 ではフェラチオなどの愛撫へ。そこで一発射精してから、終盤に突入する と同時に挿入し、エロ表現を次々と付加しながら射精してエンディング、少しだけ賢者モード、というエロマンガの典型的なストーリーが見えてきま す。もちろん、ここにどんな幻想を差し込むのか、という部分については作家性など が出るのでしょうが、こうした共通の基盤を持っている、ということが見えただけで も、かなり面白い結果が得られたと言えるでしょう。

![[img-29.jpeg]]
![[img-30.jpeg]]
![[img-31.jpeg]]

---

# 数量化 III 類 ①

# とりあえず全部盛り世界を分析する力を！とりあえず全部分析してみた

多変量解析・数量化 III 類のコーナーです。1 行で分かる数量化 III 類『近くにあった ら似ているデータ』以上。難しい説明は専門書に任せまして、なんとなく読み取って いただければと思います。ここから先のページで数量化 III 類を行いますが、基本的に横軸=x 軸が第 1 軸、縦軸=y 軸が第 2 軸です。ある程度わかる人は固有値や寄与率 を載せたのでご参考にどうぞ。ちなみにこのグラフは累積寄与率 10.4% となってお りますが、ハッキリ言って「学術的にはあんまり使えないなぁ」というレベルのもの です。 50% は越えたいところですが、それをするには色々と削らなくてはならない ので、そこは後ほど寄与率の高いグラフを用意するとしまして。

解析結果を散布図上に置き、内容の近そうなものについてなんとなくまとめました。 キスや制服を着たページが描かれているグループを、導入シーンのストーリーが描か れているグループと判断して、ストーリーグループとしてまとめました。またグラフ の下の方で男性から女性に向けての愛撫が、同様にグラフ上部で女性から男性に向け ての愛撫があったので、それをまとめました。さらに、グラフ右側に挿入シーンが描 かれたグループをなんとなくまとめました。先ほどの描写率推移の流れから判断する と、なんとなく左から右へ、マイナス方向からプラス方向へストーリーが流れて行っ ているのではないかと思われます。縦軸は女性と男性のどちらが能動的になっている か、という感じの軸になっていると考えられます。

いくつか面白そうな部分をピックアップします。まずスクール水着の方がよりス トーリー的で、体操着の方がより挿入シーンに近そうだ、といった部分が見えます。 これの解釈はやや難しいのですが、スク水の方がより愛撫して弄ぶ方向に、体操着の方が愛撫するよりも挿入する方向に偏っているのだと思われます。服の構造なのか、それとも触り心地なのか…そのフェティッシュ的な部分を調べるのも面白そうです。また、低年齢禁忌が下の方にきており、低年齢に対しては男性が主導的 に性行為を行っている、ということがデータ上から見えてきたりもします。

![[img-32.jpeg]]

---

# 数量化 III 類 ②

# 序中終盤における描写　挿入・射精から逆算して作成？愛と性戯のセーラー服美少女マンガ！

続いては、序盤から中盤、終盤にかけての内容の描写についての分析を行いました。序盤から線で結んだのでそちらをガイドにご覧下さい。

まず言えることとして……序盤、エロ描写少ないです。おかげで色々と外れ値っぽ くなってしまっていますね。この付近にはストーリーを表す描写カテゴリーが存在し ていると推測できます。今回はその辺りを集計していなかったので惜しまれますが、性行為に関わらないキャラクターの登場などを集計すると、おそらく序盤に現れそう な気がします。そして中盤にかけて愛撫が増えていきますね。そして終盤にかけて性器の描写があり、挿入描写がある、という流れが再確認できました。序盤について、 グラフの下半分にあるのは、おそらく、序盤に挿入シーンを描くタイプのエロマンガ があるためだと推測できます。

さて、ここまでいくつかの分析方法でエロマンガの典型的ストーリーを分析しまし たが、概略について掴んでいただけたでしょうか？挿入による射精を一つのク ライマックスとして置き、そこから逆算して愛撫シーンや日常シーンを入 れていく、というのはどのエロマンガにも共通して言えることだと思いま す。

もちろん、例外の部分も大きいですし、今回は集計していない描写……1 ページあ たりのコマ数や、コマ割の方法、またそのページで描かれた手足の状態など、まだま だ集計して分析できる項目は少なくありません。ですが、とりあえず、エロマンガの典型というものをなんとなくでも描き出せたので、もしもこの統計を参考にしてエロ マンガを描きたい、という奇特な方がいらっしゃったらとても嬉しく思います。

ではこれにてエロマンガ統計 S としての分析は終了となりますが……まだもう少 しだけ続きます。

![[img-33.jpeg]]

|         | 固有値 | 寄与率 | 累積寄与率 |
| ------- | ------ | ------ | ---------- |
| 第 1 軸 | 0.6818 | 17.72% | 17.72%     |
| 第 2 軸 | 0.4299 | 11.17% | 28.89%     |

---

# 数量化 III 類 ③

# 統計 R との同時集計

## 統計と統計が夢のコラボレーション　エロマンガは受動的快楽？

さて、今回、新しいエロマンガ雑誌ではなく、R で使った雑誌を使った理由は経費 の節や……ではなくて、こうして同時に分析することが目的でした。作品名をキーに して、数量化 III 類で全体を分析しています。

すると「主導権と受け攻め」の関係が明確に見えました。左下のグループには、女性主導＆男視点のカテゴリーと、女性からの愛撫描写が入り、右下のグループには男主導＆女視点のカテゴリーと男性からの愛撫描写が入りました。やはり主導権を持った方が攻めの役をこなし、読者は受け側の視点に移入して物語を読む傾向にある、ということが確認できました。マンガを読むという行為そのものが、自分で物語を作る能動的な快楽ではなく受動的ですので、こういった結果が出てくる のかもしれません。

また、挿入関連でひとつにグループをまとめましたが、その中でも性器の描写につ いては左右に分かれて攻められる方でより多く描かれていたり、描き文字＝台詞で能動的に快楽を表現することと、涙目＝涙が出てしまうことによって受動的に快楽 を表現することでそれぞれの方向性が分かれていることなどが見えます。

さらに、グループに囲っていない部分では、例えば女性上位はどちらかと言えば女性主導グループに近く、男受け視点で描かれそうな体位であることや、アナルセック スやアヘ目といったハードな印象のプレイについては男性主導側に近く、体位には主導権や関係性など、社会的な意味が含まれていて、ただ単に挿入すれば どんな体位でもいい、ということではないことが、統計的に見えました。

これらの事実はエロマンガを読んでいる人間にとって常識的な部分であり、「そりゃ女性上位は女が主導権を持っているよ！」と思う気持ちはよくわかりますが、こうし て数値として、グラフとして提供できたということには利点があります。決して印象論ではなく、内容について語れるということは、数字アレルギーを自称する人々が多 くいるこの世界では、かなり有利な結果を招けるはずです。

![[img-34.jpeg]]

|         | 固有値 | 寄与率 | 累積寄与率 |
| ------- | ------ | ------ | ---------- |
| 第 1 軸 | 0.2653 | 9.35%  | 9.35%      |
| 第 2 軸 | 0.2176 | 7.67%  | 17.02%     |

---

# 数量化 III 類 ④

# 愛撫・体位の主導権

## エロマンガ描写の意味を探し出せ。さぁ、統計戦略、しましょうか？

さて、ラストは前回もやって妙に受けが良かった寄与率が高くなっているグラフシ リーズです。2 軸で 50% 越えを目指すと、これくらいまでカテゴリーを搾らないと いけないのが残念ですが、その分、色々なことに使えそうなグラフ（注：夜のオカズ としての使用は想定していません）となっています。

今回は敢えてグループにまとめず、別の方向からの検討をしたいと思います。それ は、口を使った愛撫と手を使った愛撫の意味、体位の意味について。

まずは左上のグラフ。愛撫と主導権について。先ほども軽く触れましたが、口の愛撫と手の愛撫では意味合いが変わってきます。主導権をキーにしてみると、男が主導的になる時はどうやら口よりも手を使う、そして同様に女性が主導的になる時は口よ りも手を使う、という傾向がなんとなくではありますが見えてきます。つまり、口 を使った愛撫の方がより従属的・受動的であるということが言えそうです。ま た、外れ値になってしまうため、今回は入れていませんが、意味合いとしては足を使っ た愛撫はさらに主導権を発動している状態にあると言えるでしょう。実は第 1 作、エ ロマンガ統計（無印）の時から、どこに触れるか、よりもどう触れるか、の方が重要だ、 という知見を得ていて、そのためどこに触れるかについては今回は考慮していなかっ たのですが、やはりどう触れるかについては、主導権、もしくは視点、強姦／和姦傾向なども含めて、興味深い事実が見つけられそうです。

また、体位についても、後背位はやや男性主導的、女性上位は女性主導的、男性上位はニュートラルな体位である、ということが右下のグラフから読み取 れます。

何気ない行動描写の一つ一つが、こうした意味を持っているかもしれない、という面白さを感じていただければ幸いです。エロマンガを取り巻く法令的な環境は、日々激変しています。そんな中で、確かな主張をしていく材料として、この本が皆様のお役に立てればと考えています。

![[img-35.jpeg]]
