# タイムラインガイド

これは、タイムラインを書きたい人のためのガイドである 主にcactbotため。

![スクリーンショットをインポートする](images/timelineguide_timeline.png)

cactbotは使用 [raidbossモジュール](https://github.com/quisquous/cactbot#raidboss-module) トリガし、タイムラインのため。 これらを組み合わせて、アクション 基づくトリガー、またはタイムライン自体に基づくトリガーを作成できます。

## 目次

* [歴史](#history)
* [タイムラインファイルの構文](#timeline-file-syntax)
  * [コメント](#comments)
  * [エントリー](#entries)
  * [コマンド](#commands)
  * [例](#examples)
  * [テスト](#testing)
  * [シャスタコタガイド](#shasta-kota-guide)
* [Cactbotスタイルガイド](#cactbot-style-guide)
* [タイムライントリガー](#timeline-triggers)
* [タイムラインインジェクション](#timeline-injection)
* [タイムライン翻訳](#timeline-translation)
* [タイムライン作成の例](#example-timeline-creation)
  * [戦いを数回実行します](#run-the-fight-a-few-times)
  * [ソフトウェアの前提条件](#software-prerequisites)
  * [タイムラインスケルトン](#timeline-skeleton)
  * [初期タイムラインファイルの生成](#generating-an-initial-timeline-file)
  * [ループの構築](#building-loops)
  * [フェーズの追加](#adding-phases)
  * [次の段階](#next-phase)
  * [最終段階](#final-phase)
  * [ボイラープレート接着剤](#boilerplate-glue)
  * [ループをループにする](#making-loops-loop)
  * [すべてを一緒に入れて](#putting-it-all-together)
  * [タイムラインのテスト](#testing-timelines)
  * [他のタイムラインに対してテストする](#test-against-other-timelines)

## 歴史

戻る2016年、死と税金ウェブサイト上シャスタコタは、この作ら [ガイド](https://dtguilds.enjin.com/forum/m/37032836/viewthread/26353492-act-timeline-plugin) anoyettaのに使用する [プラグインACTタイムライン](https://github.com/anoyetta/ACT.Hojoring)。 そのプラグインは現在Hojoringの一部です。

古いもあります [kaizoban](https://github.com/090/act_timeline/releases) 一部の人々は以前からのanoyettaの作品ということで使用していることをプラグインのバージョンが。

cactbotタイムラインファイルは元々これらとの下位互換性を意図していたため、 であるため、cactbot固有の拡張子は後でトリガーファイルから挿入されます。

## タイムラインファイルの構文

タイムラインファイルの各行は、独自のタイムラインエントリと見なされます。 注文は一切ありません。 タイムラインファイルが順序付けられているという事実は、読者の便宜のためです。

### コメント

どの行でも、ハッシュ文字 `＃` はコメントを示します。 それ以降の現在の行はすべて無視されます。

### エントリー

タイムラインエントリの文法例を次に示します。 すべてのタイムラインエントリは、能力時間と能力名で始まります。

`番号「文字列」（期間番号）`

`番号 "文字列"同期/正規表現/（ウィンドウ番号、番号）（ジャンプ番号）（期間番号）`

ここでの括弧はオプションを示し、文字通りの括弧ではありません。

数値は、整数（ `34`など）または浮動小数点数（ `84.381`など）にすることができます。

文字列は文字列です。例： `"Liftoff"` または `"Double Attack"`

正規表現は、 [Javascriptの正規表現](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)。

能時間と能力名は常に最初に来る必要があるが、 `ウィンドウ`、 `ジャンプ`、 `期間`、及び `シンク` 互いに対して任意の順序である必要はありません。 様式的には、通常、同期が最初に置かれます。

`期間番号` は、アクションを表示する秒単位の時間です。 通常、タイムラインエントリはすぐに消えますが、 が、5x Bahamut'sClawが連続するなどのアクションが進行中の場合もあります。 `期間` を使用して、その期間のアクションを表示できます。 これを行うために同期は必要ありません。

`ウィンドウ番号、番号` は、同期を検討する時間枠です。 場合、デフォルトで、 `ウィンドウ` 指定されていない、cactbotは考慮 指定したのと同じ `ウィンドウ2.5,2.5`。  言い換えれば、能力時間の2.5秒前と2.5秒後の です。 一例として、ラインの `/：3118.9「Lancingのボルト」同期/：雷電：3876`、 正規表現の場合 `/：雷電：3876：/` 3116.4と3121.4の間のどこにでも発生した それは再同期されます3118.9までのタイムライン再生。 多くの場合、タイムラインは独自の機能のために非常に大きなウィンドウを使用します 、戦闘の途中で開始された場合でもタイムラインが適切な場所に同期するようにします。

`ジャンプ番号` は、同期が検出された場合に特定の時間 にジャンプするようにタイムライン再生に指示します。 時間0にジャンプすると、タイムラインは再生を停止します。 これは通常、フェーズプッシュとループに使用されます。 ジャンプする時間のタイムラインエントリは必要ありませんが、 は非常に一般的です。

### コマンド

能力のすべてのインスタンスを非表示にするには、使用することができます `hideall` コマンドを。 ほとんどのタイムラインは、 `行目hideall "--sync--"` 始まり、タイムラインを軌道に乗せるために使用されているが、プレーヤーには表示されない同期を非表示にします。

タイムラインエントリに基づいてアラートを生成するための他のコマンドがいくつかあります。 これらは引き続きサポートされていますが、文書化されていません。 代わりに、cactbotのタイムラインに基づくアラートは、 [タイムライントリガー](#timeline-triggers)使用する必要があります。

### 例

```bash
677.0 "Heavensfallトリオ"
1044 "激怒" ＃???
35.2 "フレアブレス×3"期間4
1608.1 "石化"の同期/：メリュジーヌ：7B1：/ウィンドウ1610,5
1141.4 "レッグショット"同期/：Mustadio：3738：/期間20
だけコメント私は＃
hideall "--sync--"

28.0 "Damning Edict？"同期/：カオス：3150：/ウィンドウ30,10ジャンプ2028.0
524.9「アラガンフィールド」同期/：アバター：7C4：/期間31ジャンプ444.9
1032.0「管制塔」期間13.5同期/：ハッシュマル、ブリンガーオブオーダー開始Hashmal /ウィンドウ20,20で管制塔を使用＃キャストの開始> 塔の落下
```

### テスト

cactbotでは、 `npm run test` を実行すると、ノードで **test / check_timelines.js** が実行され、構文が検証されます。

### シャスタコタガイド

また、読む価値シャスタコタの元である [ガイド](https://dtguilds.enjin.com/forum/m/37032836/viewthread/26353492-act-timeline-plugin) まだ良好です。

## Cactbotスタイルガイド

これらは、cactbotがタイムラインで従おうとするガイドラインです。

* 可能なすべての同期を追加する
* 常にエンゲージを追加してください！ エントリですが、/ countdownがない場合は同期を追加します
* 最初のボスアクションが自動攻撃の場合は、同期を追加してタイムラインをできるだけ早く開始します。 （ボスの自動攻撃が文字通り「攻撃」ではない場合があることに注意してください！）
* タイムラインを生成するために使用されるコマンドラインを上部のコメントに含めます
* rpテキストよりも同期のアクションを優先しますが、それが唯一のオプションである場合、rpテキストは同期します
* フェーズをrpテキストと同期する場合は、アクションの大きなウィンドウ同期を追加します
* 同期正規表現は短くする必要があります
* 可能な限り元の名前を使用する
* ループは、以前の能力に大きなウィンドウを持たせる代わりに、 `ジャンプ` を使用する必要があります
* 空白とコメントを自由に使用して、タイムラインを読みやすくします
* タイムラインファイル自体にトリガー、tts、アラートを配置しないでください
* アラートには [タイムライントリガー](#timeline-triggers) を使用します
* ループ用に少なくとも30秒の先読みウィンドウを追加します
* 互いに7秒以内にあるすべての能力からの同期をコメントアウトします （これにより、将来のメンテナのために能力IDが保持されます）。

### トリガーファイル名

ファイル名の一般的な目標は、一貫している 社会がこれらの戦いを呼んで。 トライアルはボスの名前で呼び出され、 レイドは省略されて番号が付けられ、 ダンジョンはゾーンによって呼び出されます。

ファイル名の場合は、アンダースコアを使用して単語を区切ります。 `nm` （ノーマルモード）、 `hm` （ハードモード）、 `ex` （エクストリームモード）などのトライアルの場合、 はハイフンで区切ります。 名前にハードが含まれるダンジョンは、「ハード」を完全な単語として綴ることができます。 以下のような記事 `` 落下させることができます。 襲撃は、層を通って番号が付けられている 、例えば `、T1` 介して `T13` 及び `A1S` を介し `a12s`。 野蛮な戦いがなければならない `、S` サフィックス 通常の戦いは、「N」接尾辞を有します。 （ただし、これはコイルレイドには適用されません。）

例：

* グランドコスモス： `grand_cosmos`
* タイタンエクストリーム： `タイタン-例`
* Ruby Weapon Extreme： `ruby_weapon-ex`
* グレートグバルライブラリ（ハード）： `great_gubal_library_hard`
* Sigmascape V2.0（サベージ）： `o6s`
* アレクサンダー-父の腕： `a3n`
* バハムートの最終コイル： `t13`

## タイムライントリガー

cactbotのトリガーファイルは、タイムライントリガーの追加をサポートしています。 人々は、彼らがデフォルトで有効になって欲しいトリガーものについての選好ている可能性があるため とcactbotタイムライン構文拡張は、他のタイムラインのプラグインと互換性がないため、 cactbotは、タイムラインファイルからそのタイムライントリガのすべてを追加します。

これは、追加することによって行われる `timelineTriggers` トリガファイルにセクションを。

例：

* [オーボンヌ修道院](https://github.com/quisquous/cactbot/blob/main/ui/raidboss/data/04-sb/alliance/orbonne_monastery.js)
* [T9](https://github.com/quisquous/cactbot/blob/main/ui/raidboss/data/02-arr/raid/t9.js)
* [O12ノーマル](https://github.com/quisquous/cactbot/blob/main/ui/raidboss/data/04-sb/raid/o12n.js)

これらのトリガーは持って [と同じ構文](https://github.com/quisquous/cactbot/blob/main/ui/raidboss/data/README.txt) ノーマルトリガなどを。 何かを返したい場合でも、関数を使用できます。 [条件](https://github.com/quisquous/cactbot/blob/5a7011c662d65f44c12c2fbff255484f2d31b8ef/ui/raidboss/data/02-arr/raid/t9.js#L10) を使用して、特定のジョブまたはロールに対してのみトリガーすることができます。

ただし、いくつかの違いがあります。

`正規表現` は翻訳されるべきではなく、タイムラインファイルに正確に含まれているものに基づいている必要があります。 タイムライン行の引用符で囲まれた能力名と一致します。 関数に `` パラメーターと一致すると、これが返されます。

これらは、 `beforeSeconds` パラメーター をサポートします。これにより、トリガーを表示できるようになるまでの時間を指定できます。

## タイムラインインジェクション

タイムラインファイル自体は、すべての人に役立つように作成されています。 ただし、タイムラインの一部を非表示にしたり、タイムラインに他のものを追加したりする場合があります。

これは、cactbotがデフォルトでどこでも行うことではないため、よりユーザー構成機能です。

トリガーファイルは、タイムラインに追加するものの配列である `タイムライン` エントリをサポートします。 それらが文字列の場合、直接追加されます。 それらが関数の場合、関数を呼び出して戻り値を追加します。 （渡された `データ` パラメーターには、プレーヤーの役割とジョブのみが含まれ、他のものは含まれません。）

印象的なダミーに/ countdownまたは/ bow-ingを実行することから開始できるSummerfordFarmsのテストタイムラインには、この例があります。  参照： [test.js](https://github.com/quisquous/cactbot/blob/79239abda888dd7a277da0501a7d4ac60d8cf963/ui/raidboss/data/triggers/test.js#L10)。

また、あなたのタイムラインエントリを追加することができます **/ cactbot /ユーザーraidboss.js** パーソナライズされたタイムラインのエントリとトリガのファイルを。  参照： [user / raidboss.js](https://github.com/quisquous/cactbot-user/blob/641488590e3ea499cc3b54cc9f2f2f856dee4ad8/raidboss.js#L28)

## タイムライン翻訳

複数の言語をサポートするために、cactbotトリガファイルはサポートして `timelineReplace` セクションを。 あなたには例を見ることができます [o12s.js](https://github.com/quisquous/cactbot/blob/ecbb723f097328c7bd0476352e5135bd5f776248/ui/raidboss/data/triggers/o12s.js#L608)。 このセクションには、同期、テキスト、および効果で置き換える正規表現が多数含まれています。 これには2つの目的があります。

最初の目的は、トリガーの正規表現変換を自動生成するツールです。

2番目の目的は、実行時のタイムラインです。 cactbotを使用する `replaceSync` の内部に何かを自動交換するセクション `同期/テキスト`タイムラインのライン、上/ と `のreplaceText` 能力テキスト内の自動交換何かにセクション。

これらは、行全体ではなく、行内の正規表現の正確なテキストにのみ一致します。 交換が熱心すぎないように注意する必要があります。

## タイムライン作成の例

これは、cactbotのツールを使用してCapeWestwindのタイムラインファイルを作成する例です。 これは非常に簡単で、テストに必要なのは1人だけなので、最初の良い例です。

### 戦いを数回実行します

タイムラインを作成する最初のステップは、いくつかのACTログを生成することです。

cactbotを使用すると、fflogsをクリアしてタイムラインを作成することもできますが、これにより多くのログ行が削除されます。 特に、rpテキスト行、ゾーンの封印/開封のテキスト、および新しい戦闘員を取得することはできません。

戦闘を実行すると、ネットワークログファイルが [生成されます](LogGuide.md#network-log-lines)。

これらのリンクをたどり、 **Raw**クリックしてから、右クリックして **Save As** をクリックして、ディスクに保存します。

優れたログを取得するための適切なガイドラインは次のとおりです。

1. 怒りを見るのに十分長く走る
1. すべてのメカニズムを見るのに十分な人数がいる（たとえば、t11テザーは2人がないと表示されない）
1. フェーズごとに、メカニックループを確認するのに十分な時間実行します
1. あなたがそれをテストできるように数回実行します

### ソフトウェアの前提条件

* [Python 3](https://www.python.org/downloads/release/python-373/)
* cactbotの [ソースコード](https://github.com/quisquous/cactbot/archive/main.zip)のコピー

可能であれば、Python 3をシステム全体にインストールする必要があり を指定すると、PythonがWindows PATHに配置され、コマンドラインから簡単に実行できるようになります。

### タイムラインスケルトン

cactbotに新しいタイムラインを追加するために必要なものが3つあります。

（1）空のタイムラインファイルを作成します。

新しいファイルを追加しますと呼ばれる **UI / raidboss /データ/タイムライン/ cape_westwind.txt**。 空白のままにすることができます。

（2）新しいトリガーファイルが存在しない場合は追加します。

作成 **UI / raidboss /データ/ 02-ARR /トライアル/ cape_westwind.js**。 これには、任意の名前を付けることができます。 タイムラインファイルはトリガーファイルを介してのみロードできるため、 ため、トリガーファイルは常に必要です。

初期トリガーファイルは次のようになります。

```javascript
[{
  zoneId：ZoneId.CapeWestwind、
  timelineFile： 'cape_westwind.txt'、
  トリガー：[
  ]、
}];
```

（3）マニフェストファイルを更新します。

Update **ui/raidboss/data/raidboss_manifest.txt** with both the name of the new triggers file and the new timeline file.

（4）レイドボスをリロードする

あなたはこれらのファイルのいずれかを変更した場合は、お使いのcactbot raidbossリロード 変更をピックアップしてプラグインを。

### 初期タイムラインファイルの生成

ネットワークログファイルを取得したら、開始と終了を見つける必要があります。

[ACT](LogGuide.md#viewing-logs-after-a-fight) のログを表示し、開始と終了を見つけます。

![遭遇ログのスクリーンショット](images/timelineguide_encounterlogs.png)

たとえば、この戦いでは、これらは関連するログ行と時間です。

```log
[18：42：23.614] 15：105E5703：Potato Chippy：2E：Tomahawk：4000EE16：Rhitahtyn sas Arvina：710003：9450000：1C：2E8000：0：0：0：0：0：0：0：0：0： 0：0：0：140279：140279：8010：8010：1000：1000：-707.8608：-822.4221：67.74045：3858：74095：4560：0：1000：1000：-693.7162：-816.4633：65.55687：
[18： 49：22.934] 19：Rhitahtyn SasArvinaがPotatoChippyに敗れた。
```

（既知のバグ：他の人のタイムゾーンからのネットワークログでは、動作ログの行から時間を変換する必要がある場合があります。  パッチは大歓迎です。）

次に、次のコマンドを実行して、それらの時間からタイムラインを作成できます。

```bash
python util/make_timeline.py -f CapeWestwind.log -s 18:42:23.614 -e 18:49:22.934

0 "Start"
2.0 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
10.6 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
19.0 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
24.4 "Gate Of Tartarus" sync /:Rhitahtyn sas Arvina:473:/
29.8 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
38.4 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
46.8 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
52.2 "Gate Of Tartarus" sync /:Rhitahtyn sas Arvina:473:/
57.7 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
66.2 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
74.7 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
80.2 "Gate Of Tartarus" sync /:Rhitahtyn sas Arvina:473:/
85.6 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
94.0 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
102.5 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
106.1 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
110.4 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/
114.9 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
119.2 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
123.5 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
127.8 "Drill Shot" sync /:Rhitahtyn sas Arvina:475:/
132.1 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
136.4 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
140.7 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
145.2 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/
149.8 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
154.3 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
158.8 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
163.3 "Drill Shot" sync /:Rhitahtyn sas Arvina:475:/
167.8 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
172.3 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
175.8 "Gate Of Tartarus" sync /:Rhitahtyn sas Arvina:473:/
179.3 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
184.5 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
189.0 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/
193.7 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
195.9 "Fast Blade" sync /:7th Cohort Optio:2CD:/
196.1 "Fast Blade" sync /:7th Cohort Optio:2CD:/
198.2 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
202.7 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
202.9 "Fast Blade" sync /:7th Cohort Optio:2CD:/
203.3 "Fast Blade" sync /:7th Cohort Optio:2CD:/
207.2 "Drill Shot" sync /:Rhitahtyn sas Arvina:475:/
207.6 "Rampart" sync /:7th Cohort Optio:0A:/
210.0 "Fast Blade" sync /:7th Cohort Optio:2CD:/
211.7 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
212.6 "Shield Bash" sync /:7th Cohort Optio:2CE:/
214.3 "Fast Blade" sync /:7th Cohort Optio:2CD:/
214.9 "Fight Or Flight" sync /:7th Cohort Optio:14:/
216.2 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
217.3 "Rampart" sync /:7th Cohort Optio:0A:/
218.0 "Celeris" sync /:7th Cohort Optio:194:/
220.8 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
222.0 "Fight Or Flight" sync /:7th Cohort Optio:14:/
225.2 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/
226.3 "Fast Blade" sync /:7th Cohort Optio:2CD:/
229.9 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
234.4 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
239.0 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
243.4 "Drill Shot" sync /:Rhitahtyn sas Arvina:475:/
259.1 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/
263.6 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
267.9 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
269.1 "Magitek Missiles" sync /:Rhitahtyn sas Arvina:478:/
274.2 "Drill Shot" sync /:Rhitahtyn sas Arvina:475:/
278.5 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
282.8 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
299.3 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/
303.8 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
308.1 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
309.3 "Magitek Missiles" sync /:Rhitahtyn sas Arvina:478:/
314.4 "Drill Shot" sync /:Rhitahtyn sas Arvina:475:/
318.7 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
323.0 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
339.5 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/
344.0 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
348.3 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
349.5 "Magitek Missiles" sync /:Rhitahtyn sas Arvina:478:/
354.6 "Drill Shot" sync /:Rhitahtyn sas Arvina:475:/
358.9 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
363.2 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
378.7 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/
383.2 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
387.5 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
388.7 "Magitek Missiles" sync /:Rhitahtyn sas Arvina:478:/
393.8 "Drill Shot" sync /:Rhitahtyn sas Arvina:475:/
398.1 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
402.4 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
```

（ `-lf` パラメーターを使用して、戦闘ログに遭遇をリストすることもできることに注意してください。

```bash
パイソンmake_timeline.py -f CapeWestwind.log -lf
1。 02：03：44.018 02：16：53.632ケープウエストウインド
2。 18：32：52.981 18：36：14.086ケープウエストウインド
3。 18：42：23.614 18：49：22.934ケープウェストウィンド
。 18：57：09.114 19：10：13.200ケープウェストウィンド
。 19：29：42.265 19：36：22.437ケープウェストウィンド
。 19：40：20.606 19：46：44.342ケープウェストウィンド
```

ここから、使用するエンカウンターの番号 を `-lf 3`としてコマンドを再実行できます。）

これはまだ実際に実行可能なタイムラインではありませんが、それは始まりです。 これを貼り **UI / raidboss /データ/タイムライン/ cape_westwind.txt**。

Windows cmd.exeまたはMINGW32を端末として使用している場合は、 左上隅のアイコンをクリックし、 選択して **編集**選択し、次に **マーク**選択してこれをコピーできます。 マウスで必要なものを強調表示してから、Enterキーを に押すと、それがコピーされ、別の場所に を貼り付けることができます。

![スクリーンショットをマークする](images/timelineguide_copy.png)

このログからノートへの最初のものがたくさんありますということです 追加されますから、ジャンクの。 ほとんどの場合、メインボスに対して信頼できる タイミングを持つために追加を当てにすることはできないため、通常は 削除することをお勧めします。

make_timeline.pyスクリプトには、これを行うための2つのオプションがあります。 1つは「戦闘員を無視する」で、もう1つは「IDを無視する」です。 `-ic "7Th Cohort Optio"` または `-ii 0A 2CD 2CE 194 14` は、これらの能力をすべて削除します。 IDを使用します。

：クリーンアップバージョン持って無視これで再度コマンドを実行してください `のpythonのutil / make_timeline.py -f CapeWestwind.log -s 18：42：23.614 -e 18：49：22.934 -II 0A 2CD 2CE 194 14`

この時点で、追加する他の行を調べて見つけることも価値があるかもしれません。 通常、これらはrpテキストの [追加戦闘員](LogGuide.md#03-addcombatant) 行 または [ゲームログ行](LogGuide.md#00-logline) です。 あなたは時間を見て、彼らが自分でどこに行くのかを理解することができます。 （パッチは、これらのいずれかを **make_timeline.py** 自動的に追加することを歓迎します。）

ここでの関連行は次のとおりです。

```log
[18：45：27.041] 03：新しい戦闘員の第7コホートオプティオを追加しました。  ジョブ：0レベル：49最大HP：24057最大MP：8010位置：（-665.5159、-804.6631,62.33055）。
[18：45：27.041] 03：新しい戦闘員の第7コホートオプティオを追加しました。  ジョブ：0レベル：49最大HP：24057最大MP：8010位置：（-665.5013、-807.1013,62.45256）。
[18：42：24.000] 00：0044：Rhitahtyn sas Arvina：ヴァン・バエルサー卿に反対することはありません！
[18：44：08.000] 00：0044：Rhitahtyn sas Arvina：私の盾は難攻不落です！ 彼らに打ち勝った無数の挑戦者に加わってください！
[18：46：27.000] 00：0044：Rhitahtyn sas Arvina：あなたの敗北は、ヴァン・ベールサー卿の高貴な征服を実現に一歩近づけるでしょう！
[18：48：27.000] 00：0044：Rhitahtyn sas Arvina：うーん... それは私の人生を犠牲にしましたが...私はあなたを打ち倒します！
```

開始時間から時間を差し引いて、それらがどこにあるかを把握することができます。 たとえば、adds popはt = 183.5（18：45：27.041-18：42：23.614）にあります。

### ループの構築

次のステップは、フェーズの周りにいくつかのループを構築することです。 観察から、フェーズプッシュがいくつかあるように見えます。

これが初期フェーズの様子です。わかりやすくするために、余分な改行が ています。

```bash
2.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
10.6 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
19.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
24.4 " Gate Of Tartarus "sync /：Rhitahtyn sas Arvina：473：/

29.8" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
38.4" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
46.8" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
52.2" Gate Of Tartarus "sync /：Rhitahtyn sas Arvina：473：/

57.7" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
66.2" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
74.7" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
80.2" Gate Of Tartarus "sync /：Rhitahtyn sas Arvina：473：/
```

およそ27.8秒または27.9秒のループがあることはかなり明らかです。 27.8だとしましょう

完璧なループを作るための最良のツールがある **のutil / timeline_adjust.py**。 このスクリプトは、タイムラインファイルをウォークスルーし、正または負の任意の量で調整された同じ タイムラインファイルを出力します。

If you are using VSCode, you can also use the [adjust time feature](https://github.com/MaikoTan/cactbot-highlight#adjust-time) from the [cactbot-highlight](https://marketplace.visualstudio.com/items?itemName=MaikoTan.cactbot-highlight) extension, which offer a simple way to adjust time in one-click.

(Note: they both will not adjust jumps.)

Here's an abbreviated version of the output from running this command:

```bash
python util / timeline_adjust.py --file = ui / raidboss / data / timelines / cape_westwind.txt --adjust = 27.8

29.8 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
38.4 "Shield Skewer" sync / ：Rhitahtyn sas Arvina：471：/
46.8 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
52.2 "Gate Of Tartarus" sync /：Rhitahtyn sas Arvina：473：/

57.6 "Shield Skewer" sync /： Rhitahtyn sas Arvina：471：/
66.2 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
74.6 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
80.0 "Gate Of Tartarus" sync /：Rhitahtyn sas Arvina：473：/

85.5 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
94.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
102.5 "Shield Skewer" sync /：Rhitahtyn sas Arvina ：471：/
108.0 "Gate Of Tartarus" sync /：Rhitahtyn sas Arvina：473：/
```

Comparing to the original, it looks like this loops fairly perfectly. The first loop is perfect and the second loop is off by a little, as this adjusted loop has 57.6, 74.6, 80.0 but the original is 57.7, 74.7, 80.2.  Close enough.

In cactbot, there's a configurable window of time for how far ahead to show in the timeline.  By default it is 30 seconds, so you should at least make a loop that goes 30 seconds ahead.

Here's what a completed version of the first phase loop looks like.

Note that we've used the times from **timeline_adjust.py** rather than the original times. (You could also use `cactbot-highlight` if you prefer that.) This is so that when we jump from 52.2 to 24.4 that all of the relative times stay the same.  In both cases when `Gate Of Tartarus` occurs, there's a `Shield Skewer` in 5.4 seconds after it.

We'll add the jumps in later.

```bash
2.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
10.6 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
19.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
24.4 " Gate Of Tartarus "sync /：Rhitahtyn sas Arvina：473：/

29.8" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
38.4" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
46.8" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
52.2" Gate Of Tartarus "sync /：Rhitahtyn sas Arvina：473：/

57.6" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
66.2" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
74.6" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
80.0" Gate Of Tartarus "sync /：Rhitahtyn sas Arvina：471：/
```

### フェーズの追加

Now on to the second phase. From observation, it's clear that at 80% the boss does some rp text and then starts doing some different abilities.

Unfortunately for us, it looks like the boss starts phase 2 by doing another `Shield Skewer` which it does a lot of in phase 1, so it won't be easy to sync to that.

**make_timeline.py** has an option to move the first usage of an ability to a particular time. As cactbot usually has a window of 30 seconds ahead, feel free to generously move phases ahead in time.

Let's move phase 2 to start its first ability at time=200. Since `Shrapnel Shell` starts 4.3 seconds after that, let's adjust the first usage of `Shrapnel Shell` (ability id 474) to time=204.3.

Here's the new command line we've built up to: `python util/make_timeline.py -f CapeWestwind.log -s 18:42:23.614 -e 18:49:22.934 -ii 0A 2CD 2CE 194 14 -p 474:204.3`

This gets us the following output for phase 2, with manually added blank lines to break out the loops.

```bash
＃
で手動で追加199.0 "--sync--" sync / 00：0044：[^：] *：私の盾は難攻不落です/
200.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/

＃の出力make_timeline
204.3 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474：/
208.8 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472：/
213.1 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/

217.4「シールドスキューワー」同期/：Rhitahtyn sas Arvina：471：/
221.7「ドリルショット」同期/：Rhitahtyn sas Arvina：475：/
226.0「WindsOfTartarus」同期/：Rhitahtyn sas Arvina：472：/
230.3 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/

234.6 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
239.1 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474：/
243.7 "Winds Of Tartarus "sync /：Rhitahtyn sas Arvina：472：/
248.2" Firebomb "sync /：Rhitahtyn sas Arvina：476：/

252.7" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
257.2" Drill Shot "sync /：Rhitahtyn sas Arvina：475：/
261.7 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472：/
266.2 "F irebomb "sync /：Rhitahtyn sas Arvina：476：/
```

It looks like there's a clear loop here, where every iteration of the loop has 2x `Firebomb` and 2x `Shield Skewer`. The loop time is 34.6.  Time to break out **timeline_adjust.py** again.

Running `python util/timeline_adjust.py --file=ui/raidboss/data/timelines/cape_westwind.txt --adjust=27.8`, the relevant output is:

```bash
234.6 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
238.9 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474：/
243.4 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472：/
247.7 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/

252.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
256.3 "Drill Shot" sync /：Rhitahtyn sas Arvina：475：/
260.6 "Winds Of Tartarus "sync /：Rhitahtyn sas Arvina：472：/
264.9" Firebomb "sync /：Rhitahtyn sas Arvina：476：/
```

This has diverged a fair bit from the original times, with the last Firebomb being 264.9 vs 266.2. If you want to be more precise, this is where you would compare against some other runs.

However, this is good enough for Cape Westwind, so we will replace the second loop with this output from **timeline_adjust.py**.

The current state of our timeline is now:

```bash
0 "Start"
2.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
10.6 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
19.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471 ：/
24.4 "Gate Of Tartarus" sync /：Rhitahtyn sas Arvina：473：/

29.8 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
38.4 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471： /
46.8 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
52.2 "Gate Of Tartarus" sync /：Rhitahtyn sas Arvina：473：/

57.6 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
66.2 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
74.6 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
80.0 "Gate Of Tartarus" sync /：Rhitahtyn sas Arvina：473：/

＃80％
199.0 "--sync--" sync / 00：0044：[^：] *：シールドは難攻不落/
200.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
204.3 "Shrapnel Shell "sync /：Rhitahtyn sas Arvina：474：/
208.8" Winds Of Tartarus "sync /：Rhitahtyn sas Arvina：472：/
213.1" Firebomb "sync /：Rhitahtyn sas Arvina：476：/

217.4 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
221.7 "Drill Shot" sync /：Rhitahtyn sas Arvina：475：/
226.0 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472：/
230.3 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/

234.6 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
238.9 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474 ：/
243.4 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472：/
247.7 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/

252.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
256.3「ドリルショット」同期/：Rhitahtyn sas Arvina：475：/
260.6「WindsOfTartarus」同期/：Rhitahtyn sas Arvina：472：/
264.9「Firebomb」同期/：Rhitahtyn sas Arvina：476：/

＃ 60％
```

### 次の段階

From observation, I know that the next phase starts at 60% and there's two adds.

From reading the timeline, there's a random "Gate of Tartarus" around the time the adds show up.

This is the original timeline, before any phases were adjusted:

```bash
175.8 "門タルタロスの"同期/：Rhitahtyn SAS Arvina：473：/
183.5 "追加"
```

Unfortunately, the boss uses `Gate of Tartarus` in phase 1, so we can't add it using `-p` like we did for phase 2. (Patches welcome to add more options to make this possible?)

Instead, we can just use **timeline_adjust.py** to just shift the timeline forward automatically. If we adjust the original timeline by 400-175.8=224.2 then we can start phase 3 at t=400.

Here's the adjusted output, with the adds manually added back in:

```bash
400.0 "Gate Of Tartarus" sync /：Rhitahtyn sas Arvina：473：/
403.5 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/

407.7 "Adds"
408.7 "Shield Skewer" sync /：Rhitahtyn sas Arvina： 471：/
413.2 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474：/
417.9 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472：/
422.4 "Firebomb" sync /：Rhitahtyn sas Arvina：476： /

426.9 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
431.4 "Drill Shot" sync /：Rhitahtyn sas Arvina：475：/
435.9 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472：/
440.4 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/

445.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
449.4 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474：/
454.1 " Winds Of Tartarus "sync /：Rhitahtyn sas Arvina：472：/
458.6" Firebomb "sync /：Rhitahtyn sas Arvina：476：/

463.2" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
467.6" Drill Shot "sync /：Rhitahtyn sas Arvina：475：/

＃40％フェーズプッシュ??
483.3 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474：/
487.8 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/
492.1 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472：/

493.3 " Magitekミサイル」同期/：Rhitahtyn SAS Arvina：478：/
407.7 "追加"
```

Without recorded video, it's not 100% clear from the logs whether the `Shrapnel Shell` is part of phase 3 or phase 4. I know from observation that `Magitek Missiles` is the last phase, so because the Shrapnel Shell breaks the pattern let's assume it starts phase 4. We'll test this later.

It looks a bit like there's another loop just like phase 2.

One consideration is to see if it's exactly the same as phase 2. You can use **timeline_adjust.py** with an adjustment of 208.7 to move phase 2's `Shield Skewer` on top of phase 3. However, you can see from that output that it's not quite the same. Therefore, we'll need to build phase 3 separately.

It's pretty clear that this loop is also a 36.2 second loop. Using **timeline_adjust.py** with a 36.2 adjustment gets this output:

```bash
445.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
449.5 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474：/
454.2 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472：/
458.7 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/

463.2 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
467.7 "Drill Shot" sync /：Rhitahtyn sas Arvina：475：/
472.2 "Winds Of Tartarus "sync /：Rhitahtyn sas Arvina：472：/
476.7" Firebomb "sync /：Rhitahtyn sas Arvina：476：/
```

This is really close to the original times, so let's consider that our loop.

### 最終段階

Finally, we need the phase that starts at 40%. We assumed that the `Shrapnel Shell` started this phase. However, this isn't a unique skill. `Magitek Missiles` is the first unique skill in this phase and that starts 10 seconds after "Shrapnel Shell". Let's start phase 4 at 600 seconds, so we'll adjust the first use of `Magitek Missile` (ability id 478) to be t=610.

Here's the final command line, including this second phase: `python util/make_timeline.py -f CapeWestwind.log -s 18:42:23.614 -e 18:49:22.934 -ii 0A 2CD 2CE 194 14 -p 474:204.3 478:610`

```bash
＃
で手動で追加595.0 "--sync--" sync / 00：0044：[^：] *：敗北すると/ウィンドウ600,0
600.0 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474：/
604.5 "Firebomb"同期/：Rhitahtyn SAS Arvina：476：/
608.8 "タルタロスの風"同期/：Rhitahtyn SAS Arvina：472：/

make_timelineから＃部分出力
Rhitahtyn SAS Arvina：610.0 "Magitekミサイル"同期/ ：478：/
615.1「ドリルショット」同期/：Rhitahtyn sas Arvina：475：/
619.4「Firebomb」同期/：Rhitahtyn sas Arvina：476：/
623.7「WindsOfTartarus」同期/：Rhitahtyn sas Arvina：472 ：/
640.2 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474：/
644.7 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/
649.0 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472：/

650.2 "Magitek Missiles" sync /：Rhitahtyn sas Arvina：478：/
655.3 "Drill Shot" sync /：Rhitahtyn sas Arvina：475：/
659.6 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/
663.9 " Winds Of Tartarus "sync /：Rhitahtyn sas Arvina：472：/
680.4" Shrapnel Shell "sync /：Rhitahtyn sas Arv ina：474：/
684.9 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/
689.2 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472：/

690.4 "Magitek Missiles" sync /：Rhitahtyn sas Arvina： 478：/
695.5 "Drill Shot" sync /：Rhitahtyn sas Arvina：475：/
699.8 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/
704.1 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472： /
719.6 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474：/
724.1 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/
728.4 "Winds Of Tartarus" sync /：Rhitahtyn sas Arvina：472：/

729.6 "Magitek Missiles" sync /：Rhitahtyn sas Arvina：478：/
734.7 "Drill Shot" sync /：Rhitahtyn sas Arvina：475：/
739.0 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/
743.3 "Windsタルタロスの "同期/：Rhitahtyn sas Arvina：472：/
```

This sure looks like a 40.2 second loop. Using **timeline_adjust.py**, with a 40.2 second adjustment, we get the following output:

```bash
650.2 "Magitek Missiles" sync /：Rhitahtyn sas Arvina：478：/
655.3 "Drill Shot" sync /：Rhitahtyn sas Arvina：475：/
659.6 "Firebomb" sync /：Rhitahtyn sas Arvina：476：/
663.9 "Winds Of Tartarus "sync /：Rhitahtyn sas Arvina：472：/
680.4" Shrapnel Shell "sync /：Rhitahtyn sas Arvina：474：/
684.9" Firebomb "sync /：Rhitahtyn sas Arvina：476：/
689.2" Winds Of Tartarus "同期/：Rhitahtyn sas Arvina：472：/
```

This is a perfect match for the original times, so there's our loop.

### ボイラープレート接着剤

In general, most timelines should include some boilerplate at the top like this:

```bash
＃ケープウェストウィンド
＃-ii 0A 2CD 2CE 194 14 -p 474：204 478：610

hideall "--Reset--"
hideall "--sync--"

0.0 "--Reset--" sync / 21 ：........:40000010:/ window 10000 jump 0

0 "Start"
0.0 "--sync--" sync /：Engage！/ window 0,1
2.0 "Shield Skewer" sync /：Rhitahtyn sasアルビナ：471：/
```

It's good practice to include the command line you used to generate this so that other people can come back and see what you skipped.

`hideall` hides all instances of a line, so that players don't see `--sync--` show up visually, but the timeline itself will still sync to those lines. There can be anything in the text, it is just called `--sync--` for convenience.

It's good practice to have a Reset line to stop the timeline when there's a wipe. On fights where the entire zone resets (e.g. all of omegascape, a4s, a8s, a12s, t9, t13), `sync / 21:........:40000010:/` is a good sync to use. On fights with zones that seal and unseal, (e.g. a1s,  t1-8) you should use the zone sealing message itself to reset.

Finally, to start a fight, you should always include an Engage! sync for countdowns. If the first boss ability is slow to happen, you should also include the first auto so that the timeline starts.

For instances, on o11s, the first two lines are:

```bash
0.0 "--sync--" sync /：Engage！/ window 0,1
2.5 "--sync--" sync /：Omega：368：/ window 3,0
```

### ループをループにする

Here's the phase 1 loop, again. We're going to edit this so that whenever we get to 52.2 seconds it will jump back to 24.4 seconds seamlessly.

```bash
2.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
10.6 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
19.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
24.4 " Gate Of Tartarus "sync /：Rhitahtyn sas Arvina：473：/

29.8" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
38.4" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
46.8" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
52.2" Gate Of Tartarus "sync /：Rhitahtyn sas Arvina：473：/

57.6" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
66.2" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
74.6" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
80.0" Gate Of Tartarus "sync /：Rhitahtyn sas Arvina：473：/
```

On the 52.2 line, add the following `window 10,100 jump 24.4`. This means, if you see this ability 10 seconds in the past or 100 seconds in the future, jump to t=24.4. By default, all syncs are `window 5,5` unless otherwise specified, meaning they sync against the ability any time within the last 5 seconds or 5 seconds in the future.

The abilities from 57.6 to 80.0 will never be synced against because as soon as the ability at 52.2 is seen, we will want to jump back.  So, we will remove those syncs.

Finally, because sometimes log lines get dropped or ACT starts mid-combat, it can be good to put large syncs on infrequent or important abilities so that the timeline can sync there.

This leaves us with this final version of the initial loop.

```bash
2.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
10.6 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
19.0 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
24.4 " Gate Of Tartarus "sync /：Rhitahtyn sas Arvina：473：/ window 30,10

29.8" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
38.4" Shield Skewer "sync /：Rhitahtyn sas Arvina：471：/
46.8 "Shield Skewer" sync /：Rhitahtyn sas Arvina：471：/
52.2 "Gate Of Tartarus" sync /：Rhitahtyn sas Arvina：473：/ window 10,100 jump 24.4

＃fake loop
57.6 "Shield Skewer"
66.2 "シールド串」
74.6 『盾串』
タルタロス80.0 『門』
```

This is done on all the following loops as well.

### すべてを一緒に入れて

Putting all the loops that we have created together leaves us with the following timeline.

Because there are so many `Shield Skewer` abilities, any loops are on less frequent abilities just to be more careful.

```bash
# Cape Westwind
# -ii 0A 2CD 2CE 194 14 -p 474:204 478:610

hideall "--Reset--"
hideall "--sync--"

0.0 "--Reset--" sync / 21:........:40000010:/ window 10000 jump 0

### Phase 1: skewers and stuns
0 "Start"
0.0 "--sync--" sync /:Engage!/ window 0,1
2.0 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
10.6 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
19.0 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
24.4 "Gate Of Tartarus" sync /:Rhitahtyn sas Arvina:473:/ window 30,10

29.8 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
38.4 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
46.8 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
52.2 "Gate Of Tartarus" sync /:Rhitahtyn sas Arvina:473:/ window 10,100 jump 24.4

57.6 "Shield Skewer"
66.2 "Shield Skewer"
74.6 "Shield Skewer"
80.0 "Gate Of Tartarus"


### Phase 2 (80%): firebombs
199.0 "--sync--" sync /00:0044:[^:]*:My shields are impregnable/ window 200,0
200.0 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
204.3 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/ window 205,10
208.8 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
213.1 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/

217.4 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
221.7 "Drill Shot" sync /:Rhitahtyn sas Arvina:475:/
226.0 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
230.3 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/

234.6 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
238.9 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/ window 20,100 jump 204.3
243.4 "Winds Of Tartarus"
247.7 "Firebomb"

252.0 "Shield Skewer"
256.3 "Drill Shot"
260.6 "Winds Of Tartarus"
264.9 "Firebomb"


### Phase 3 (60%): Adds
400.0 "Gate Of Tartarus" sync /:Rhitahtyn sas Arvina:473:/ window 200,20
403.5 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/

407.7 "Adds"
408.7 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
413.2 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/ window 20,20
417.9 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
422.4 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/

426.9 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
431.4 "Drill Shot" sync /:Rhitahtyn sas Arvina:475:/
435.9 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
440.4 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/

445.0 "Shield Skewer" sync /:Rhitahtyn sas Arvina:471:/
449.5 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/ window 20,100 jump 413.2
454.2 "Winds Of Tartarus"
458.7 "Firebomb"

463.2 "Shield Skewer"
467.7 "Drill Shot"
472.2 "Winds Of Tartarus"
476.7 "Firebomb"


### Phase 4 (40%): magitek missiles
595.0 "--sync--" sync /00:0044:[^:]*:Your defeat will bring/ window 600,0

600.0 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/
604.5 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
608.8 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/

610.0 "Magitek Missiles" sync /:Rhitahtyn sas Arvina:478:/ window 610,30
615.1 "Drill Shot" sync /:Rhitahtyn sas Arvina:475:/
619.4 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
623.7 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/
640.2 "Shrapnel Shell" sync /:Rhitahtyn sas Arvina:474:/
644.7 "Firebomb" sync /:Rhitahtyn sas Arvina:476:/
649.0 "Winds Of Tartarus" sync /:Rhitahtyn sas Arvina:472:/

650.2 "Magitek Missiles" sync /:Rhitahtyn sas Arvina:478:/ window 20,100 jump 610.0
655.3 "Drill Shot"
659.6 "Firebomb"
663.9 "Winds Of Tartarus"
680.4 "Shrapnel Shell"
684.9 "Firebomb"
689.2 "Winds Of Tartarus"
```

### タイムラインのテスト

cactbot has a testing tool called **util/test_timeline.py** that can test a network log file or an fflogs fight against an existing timeline.

The test tool will tell you when a sync in your timeline is not matched against the fight, and if a future sync is hit, it will let you know that some were skipped over. It will not do the reverse, and tell you about abilities in the fight that have no timeline entries.

It will also tell you about how far off the sync was, which can help with adjusting timelines to be more accurate.

Here's an example. The `-t` parameter here refers to the file name of the timeline you want to test against in the **ui/raidboss/data/timelines** folder, minus the .txt extension. (As with `make_timeline`, you can use the `-lf` parameter to list encounters.)

```bash
$ python util / test_timeline.py -f CapeWestwind.log -s 18：42：23.614 -e18：49：22。
934 -t cape_westwind
0.000: Matched entry: 2.0 Shield Skewer (+2.000s)
10.556: Matched entry: 10.6 Shield Skewer (+0.044s)
18.985: Matched entry: 19.0 Shield Skewer (+0.015s)
24.411: Matched entry: 24.4 Gate Of Tartarus (-0.011s)
29.810: Matched entry: 29.8 Shield Skewer (-0.010s)
38.372: Matched entry: 38.4 Shield Skewer (+0.028s)
46.835: Matched entry: 46.8 Shield Skewer (-0.035s)
52.239: Matched entry: 52.2 Gate Of Tartarus (-0.039s)
    Jumping to 24.400
29.897: Matched entry: 29.8 Shield Skewer (-0.097s)
38.286: Matched entry: 38.4 Shield Skewer (+0.114s)
46.851: Matched entry: 46.8 Shield Skewer (-0.051s)
52.291: Matched entry: 52.2 Gate Of Tartarus (-0.091s)
    Jumping to 24.400
29.831: Matched entry: 29.8 Shield Skewer (-0.031s)
38.204: Matched entry: 38.4 Shield Skewer (+0.196s)
46.888: Matched entry: 46.8 Shield Skewer (-0.088s)
48.695: Matched entry: 199.0 --sync-- (+150.305s)
    Missed sync: Gate Of Tartarus at 52.2 (last seen at 24.411)
200.693: Matched entry: 200.0 Shield Skewer (-0.693s)
204.273: Matched entry: 204.3 Shrapnel Shell (+0.027s)
208.809: Matched entry: 208.8 Winds Of Tartarus (-0.009s)
213.111: Matched entry: 213.1 Firebomb (-0.011s)
217.417: Matched entry: 217.4 Shield Skewer (-0.017s)
221.711: Matched entry: 221.7 Drill Shot (-0.011s)
226.017: Matched entry: 226.0 Winds Of Tartarus (-0.017s)
230.313: Matched entry: 230.3 Firebomb (-0.013s)
234.642: Matched entry: 234.6 Shield Skewer (-0.042s)
239.144: Matched entry: 238.9 Shrapnel Shell (-0.244s)
    Jumping to 204.300
208.885: Matched entry: 208.8 Winds Of Tartarus (-0.085s)
213.304: Matched entry: 213.1 Firebomb (-0.204s)
217.623: Matched entry: 217.4 Shield Skewer (-0.223s)
221.857: Matched entry: 221.7 Drill Shot (-0.157s)
226.191: Matched entry: 226.0 Winds Of Tartarus (-0.191s)
230.480: Matched entry: 230.3 Firebomb (-0.180s)
233.846: Matched entry: 400.0 Gate Of Tartarus (+166.154s)
    Missed sync: Shield Skewer at 234.6 (last seen at 217.623)
    Missed sync: Shrapnel Shell at 238.9 (last seen at 204.273)
403.546: Matched entry: 403.5 Shield Skewer (-0.046s)
408.741: Matched entry: 408.7 Shield Skewer (-0.041s)
413.195: Matched entry: 413.2 Shrapnel Shell (+0.005s)
417.897: Matched entry: 417.9 Winds Of Tartarus (+0.003s)
422.385: Matched entry: 422.4 Firebomb (+0.015s)
426.890: Matched entry: 426.9 Shield Skewer (+0.010s)
431.389: Matched entry: 431.4 Drill Shot (+0.011s)
435.882: Matched entry: 435.9 Winds Of Tartarus (+0.018s)
440.393: Matched entry: 440.4 Firebomb (+0.007s)
444.969: Matched entry: 445.0 Shield Skewer (+0.031s)
449.380: Matched entry: 449.5 Shrapnel Shell (+0.120s)
    Jumping to 413.200
417.884: Matched entry: 417.9 Winds Of Tartarus (+0.016s)
422.366: Matched entry: 422.4 Firebomb (+0.034s)
426.975: Matched entry: 426.9 Shield Skewer (-0.075s)
431.292: Matched entry: 431.4 Drill Shot (+0.108s)
431.400: Matched entry: 595.0 --sync-- (+163.600s)
    Missed sync: Winds Of Tartarus at 435.9 (last seen at 417.884)
    Missed sync: Firebomb at 440.4 (last seen at 422.366)
    Missed sync: Shield Skewer at 445.0 (last seen at 426.97499999999997)
    Missed sync: Shrapnel Shell at 449.5 (last seen at 413.195)
620.783: Matched entry: 610.0 Magitek Missiles (-10.783s)
    Missed sync: Shrapnel Shell at 600.0 (last seen at 610.739)
    Missed sync: Firebomb at 604.5 (last seen at 615.247)
    Missed sync: Winds Of Tartarus at 608.8 (last seen at 619.564)
615.098: Matched entry: 615.1 Drill Shot (+0.002s)
619.409: Matched entry: 619.4 Firebomb (-0.009s)
623.711: Matched entry: 623.7 Winds Of Tartarus (-0.011s)
640.166: Matched entry: 640.2 Shrapnel Shell (+0.034s)
644.715: Matched entry: 644.7 Firebomb (-0.015s)
649.012: Matched entry: 649.0 Winds Of Tartarus (-0.012s)
650.179: Matched entry: 650.2 Magitek Missiles (+0.021s)
    Jumping to 610.000
615.137: Matched entry: 615.1 Drill Shot (-0.037s)
619.413: Matched entry: 619.4 Firebomb (-0.013s)
623.709: Matched entry: 623.7 Winds Of Tartarus (-0.009s)
640.170: Matched entry: 640.2 Shrapnel Shell (+0.030s)
644.712: Matched entry: 644.7 Firebomb (-0.012s)
649.013: Matched entry: 649.0 Winds Of Tartarus (-0.013s)
650.177: Matched entry: 650.2 Magitek Missiles (+0.023s)
    Jumping to 610.000
615.135: Matched entry: 615.1 Drill Shot (-0.035s)
619.418: Matched entry: 619.4 Firebomb (-0.018s)
623.709: Matched entry: 623.7 Winds Of Tartarus (-0.009s)
639.166: Matched entry: 640.2 Shrapnel Shell (+1.034s)
644.713: Matched entry: 644.7 Firebomb (-0.013s)
649.014: Matched entry: 649.0 Winds Of Tartarus (-0.014s)
650.175: Matched entry: 650.2 Magitek Missiles (+0.025s)
    Jumping to 610.000
615.131: Matched entry: 615.1 Drill Shot (-0.031s)
619.413: Matched entry: 619.4 Firebomb (-0.013s)
623.709: Matched entry: 623.7 Winds Of Tartarus (-0.009s)
```

As this timeline was generated against this network log, it's not surprising that most of these timings are very accurate.

The `Missed sync` lines are the ones to look at more closely.

These three examples aren't worrisome because they are just the end of the loop and we've skipped forward in time to the next phase. (Patches welcome to figure out how to not warn on this sort of jump.)

```text
48.695：一致したエントリ：199.0 --sync-（+ 150.305s）
    同期の失敗：52.2のタルタロスの門（最後に見たのは24.411）

233.846：一致したエントリ：400.0タルタロスの門（+ 166.154s）
    同期の失敗： 234.6のシールドスキュワー（最後に見られたのは217.623）
    同期の失敗：238.9の榴散弾（最後に見られたのは204.273）

431.400：一致したエントリ：595.0 --sync-（+ 163.600s）
    同期の失敗：Winds Of Tartarus at 435.9 （最後に見られたのは417.884）
    同期が取れなかった：Firebombが440.4（最後に見られたのは422.366）
    見落とされた同期：Shield Skewerが445.0（最後に見られたのは426.97499999999997）
    見当たらない同期：Shrapnel Shellが449.5（最後に見られたのは413.195）
```

However, this looks like a problem:

```text
620.783：一致するエントリ：610.0 Magitekミサイル（-10.783s）
    不在同期：シュラプネルシェル600.0で（最後610.739で見られる）
    不在同期：604.5でFirebomb（615.247で見られる最後）
    不在同期：608.8でタルタロスの風（最後に見られたのは619.564）
```

It seems pretty likely that we've put the rp text sync in the wrong place. Because we added such a large window on `Magitek Missile` the timeline resynced (thank goodness), but some of the abilities before that were wrong.

The original timeline is:

```bash
595.0 "--sync--" sync / 000044：[^：] *：敗北すると/ウィンドウ600,0
600.0 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474：/
```

However, Shrapnel Shell is off by 10.7 seconds, as the tester output mentioned `(last seen at 610.739)`. The fix is to move the rp text sync back in time by that amount. The new time will be 595 - 10.7 = 584.3.

```bash
584.3 "--sync--" sync / 000044：[^：] *：敗北すると/ウィンドウ600,0
600.0 "Shrapnel Shell" sync /：Rhitahtyn sas Arvina：474：/
```

Rerunning the tester (most output omitted)

```bash
$ python util / test_timeline.py -f CapeWestwind.log -s 18：42：23.614 -e18：49：22。
934 -t cape_westwind

431.400：一致したエントリ：584.3--sync-（+ 152.900s）
    同期の失敗：435.9のタルタロスの風（最後に417.884で見られた）
    同期の失敗：440.4での火爆弾（422.366で最後に見られた）
    同期の失敗：シールドスキューワー445.0（最後に見られたのは426.97499999999997）
    同期の失敗：シュラプネルシェルの449.5（最後に見られたのは413.195）
600.039：一致したエントリ：600.0シュラプネルシェル（-0.039s）
604.508：一致したエントリ：604.5 Firebomb（ -0.008s）
608.817：一致したエントリ：608.8タルタロスの風（-0.017s）
610.019：一致したエントリ：610.0 Magitekミサイル（-0.019s）
```

This is what we want to see. As before, these missed syncs are just from the phase push, but `Shrapnel Shell` is now in the right spot.

### 他のタイムラインに対してテストする

It's important to test against multiple fight instances to make sure that the timeline is good. Here's an example of running against the **CapeWestwind2.log** file.

If you run `python util/test_timeline.py -f CapeWestwind2.log -s 13:21:00.688 -e 13:29:36.976 -t cape_westwind` yourself, you can spot at least two problems.

One minor problem is that this boss is inconsistent:

```text
447.329：一致したエントリ：445.0シールドスキュワー（-2.329s）
443.789：一致したエントリ：445.0シールドスキュワー（+ 1.211s）
444.792：一致したエントリ：445.0シールドスキューワー（+ 0.208s）
447.361：一致したエントリ：445.0シールドスキューワー（-2.361s）
```

This `Shield Skewer` in phase 3 comes at wildly different times. However, the abilities before and after seem to be just fine. Often if there are inconsistencies like this, the best thing to do is make sure there are larger windows around surrounding abilities to make sure that even if one ability is inconsistent the entire timeline doesn't get derailed.

However, one major problem is that there's a missing `Shield Skewer`:

```text
403.454：一致したエントリ：403.5シールドスキュワー（+ 0.046s）
407.876：一致したエントリ：413.2榴散弾（+ 5.324s）
    同期が失われました：シールドスキュワー408.7（最後に見たのは403.454）
417.748：一致したエントリ：417.9 Winds Ofタルタロス（+ 0.152s）
```

One timeline has two `Shield Skewer`s and one only has one. And the `Shrapnel Shell` is horribly mistimed here.

What to do in this case is subjective. Here are some options:

* より多くのデータを取得し、最も一般的なケースのタイムラインを作成します
* タイムラインにコメントを残す
* これが重要な能力（タンクバスターなど）である場合は、疑問符を付けて、保証されていないことをプレーヤーに知らせます。