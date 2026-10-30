## 機能

`to-spec` は、直前までの会話を **[spec](https://www.aihero.dev/ai-coding-dictionary/spec)** に変換し、issue tracker に 1 つの issue として公開します。

インタビューはしません。これを使う時点で、決めることはもう決まっています。そのため、わかっていること（スレッド、コードベース、`GLOSSARY.md`、ADR）をまとめるだけで、新しい質問のラウンドは始めません。spec はすでに下した決定を記録するもので、新しい決定を下す場所ではありません。

## この機能を使うタイミング

これは `/to-spec` と入力して呼び出します。[agent](https://www.aihero.dev/ai-coding-dictionary/agent) が自分から使うことはありません。

ビルドが 1 つのエージェントの [session](https://www.aihero.dev/ai-coding-dictionary/session) には大きすぎて、複数に分けなければならないときに使います。きっかけはそれだけです:

| 今いる場所 | 実行するもの |
| --- | --- |
| まだ何も決めていない | まず [grill-with-docs](https://aihero.dev/skills-grill-with-docs) |
| 決まっていて、作業が 1 つの [context window](https://www.aihero.dev/ai-coding-dictionary/context-window) に収まる | [implement](https://aihero.dev/skills-implement): spec は省きます |
| 決まっていて、作業が複数のセッションにまたがる | `/to-spec`、その後 [to-tickets](https://aihero.dev/skills-to-tickets) |
| [wayfinder](https://aihero.dev/skills-wayfinder) のマップが晴れた | `/to-spec #<map_issue>` |

## 前提条件

`to-spec` は spec を issue として公開するので、先に [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) がこのリポジトリのトラッカーとトリアージラベルの語彙を設定している必要があります。トラッカーはどちらの種類でも動きます: GitHub のような本物のトラッカーでも、追加のセットアップなしで動く `.scratch/` 配下のローカルの Markdown ファイルでもかまいません。

## spec は決定の記録である

spec が存在するのは、コンテキストウィンドウには終わりがあるからです。[grilling](https://www.aihero.dev/ai-coding-dictionary/grilling) の間に、多くのことが決まりました: 解決策の形、議論を重ねた選択、意図的に退けたもの。そのすべてが、これからクリアしようとしている 1 つの会話の中にあります。spec はそれを残します。

そのため spec は何かを検証したり決めたりはしません。決めたことを、プロジェクト独自の語彙で記録し、新しいセッションがあなたの説明し直しなしで作業を引き継げるようにします。あなたが言ってもいないことが spec に書かれていたら、それは欠陥です。

## 文章より先に seam

何かを書く前に、`to-spec` は機能がテストされる **seams** を大まかに描き、あなたと確認します。新しい seam より既存の seam を優先し、できるだけ高い位置の seam を選びます。1 つの変更にとって理想的な seam の数は 1 つです。

合意された seam は、後で他のスキルが使います。[tdd](https://aihero.dev/skills-tdd) は事前に合意された seam でしか作業しません。[code-review](https://aihero.dev/skills-code-review) は spec に照らして差分をレビューするので、誰も合意していない seam はレビューの指摘として現れます。どちらのつながりも、この文書を通じています。だからこそ、seam についての話し合いは実装に持ち越さず、ここで真剣に扱うべきなのです。

## よくある質問

**`/to-prd` はどこへ行きましたか？**
このスキルのことで、v1.1 で名前が変わりました。今は全体を通じて「spec」という 1 つの用語を使い、古い `to-prd` という名前はもう使えないので、新しい名前で再インストールしてください。古い語彙は *spec* と *tickets* という組に置き換えられました。spec は目的地と、それを固める決定です。[tickets](https://www.aihero.dev/ai-coding-dictionary/ticket) はそこに至るステップです。方向を変えるなら、未完了のチケットを削除し、spec は残してください。

**なぜ spec に `ready-for-agent` ラベルが付くのですか？エージェントにこれを元に実装してほしくはありません。**
このラベルは「これ以上のトリアージは不要」という意味です: 文書がエージェントの作業の元にできるほど完成している、ということです。入力であることを示すもので、作業指示ではありません。しかし `ready-for-agent` を監視している [AFK](https://www.aihero.dev/ai-coding-dictionary/afk) のエージェントには、その違いが分かりません。チケットのスライスを拾う代わりに、spec 全体を 1 回で作ろうとします。これはこのスキルで最も多く報告されている問題です。これが変わるまでは、AFK エージェントのプロンプトで親の spec を明示的に除外するか、`/to-tickets` を実行した後でラベルを外してください。

**グリルから直接 `/to-tickets` に進んで、spec を省いてはいけないのですか？**
そうすべきことはよくあります。spec がそのステップに見合うのは、複数セッションの作業のときだけです。その価値は、チケットは使い捨てで spec はそうではない点にあります。各チケットは新しいコンテキストウィンドウ 1 つ分の大きさで、その後削除されるか閉じられますが、spec はその背後にある理由を記録する唯一の場所として残ります。1 セッションの変更では、それは何の役にも立たず、[model](https://www.aihero.dev/ai-coding-dictionary/model) がずれる可能性のある余分なまとめのステップのコストを払うだけです。グリルから `/implement` に進んでください。

**wayfinder のマップが完成したところです。何を渡せばいいですか？**
個々の意思決定チケットではなく、マップの本体の issue を渡してください: `/to-spec #<map_issue>`。[wayfinder](https://aihero.dev/skills-wayfinder) が生むのは、成果物ではなく、マップに散らばった決定です。`to-spec` はそれらを、構築の元にできる 1 つの文書にまとめます。マップを直接 `/implement` につなげると、このステップが失われます。

**spec は私がレビューするためのものですか、それともエージェントのためだけのものですか？**
主にエージェントのためのもので、そういう読み心地です: 完全で、密度が高く、参照に満ちています。seam と対象外のセクションは読んでください。この 2 か所では、間違った決定は今見つけるのが最も安く、後で見つけるのが最も高くつきます。全部読むのはつらいという不満は実際にありますが、要約モードはありません。ただ、spec に驚かされるなら、spec が長すぎるのではなく、グリルが浅すぎたのです。

**チケットの作業が始まったら spec は凍結しておくべきですか、それともエージェントに書き直させるべきですか？**
同期を保つ仕組みは何もありません。実際には、それはその時点で分かっていたことのスナップショットで、実装から何かを学んだ最初の時点で古くなります。作業が出荷された後は使い捨てとして扱ってください。長く残すためのファイルは `GLOSSARY.md` と ADR です。実装中に長く残すべきことを学んだら、spec を編集するのではなく、そちらに書いてください。

**私の作業は機能ではなく、リファクタリングやモジュールの境界です。テンプレートは合いますか？**
あまり合いません。これは既知の制限です。テンプレートはユーザーストーリーに大きく頼っていて、それはアーキテクチャの作業には合いません。本当は interface と不変条件についての決定なのに、その周りに誰も求めていないストーリーを書く羽目になります。代わりに、実装上の決定とテスト上の決定のセクションを使ってください。長く残るアーキテクチャ上の決定は、spec ではなく [grill-with-docs](https://aihero.dev/skills-grill-with-docs) を通じて ADR として記録してください。

**関連する作業をトラッカーで確認したり、従っている ADR を引用したりしてくれますか？**
どちらもしません。触れる領域の ADR を読んで従いますが、リンクはしません。また、書く前に重複する issue をトラッカーで探さないので、spec が誰かがすでに登録した作業と重複することがあり、それを警告するものもありません。その領域が混み合っているなら、先に自分でトラッカーを検索してください。

**`/to-tickets` が spec を読めませんでした: 途中で切れ続けました。**
トラッカーの issue は、非常に大きな spec を全文返さないことがあり、代わりに使えるローカルのコピーもありません。これを直すには、`/to-spec` と `/to-tickets` の間で [clear](https://www.aihero.dev/ai-coding-dictionary/clearing) や [compact](https://www.aihero.dev/ai-coding-dictionary/compaction) をしないでください。同じウィンドウで実行すれば、`/to-tickets` が spec を取得し直す必要は一切ありません。

## 正常に機能している場合

- 新しい質問のラウンドを始めるのではなく、書き始める。
- 書く前に seam を見せ、できるだけ少ない数を提案する。
- ありきたりなプロダクト管理の定型文ではなく、プロジェクトの名詞を使う。
- 書かれているすべての決定について、自分が下したことを覚えている。セクションを埋めるためにでっち上げたものがない。
- 対象外のセクションに本物の項目が並んでいる。退けたものこそ、たいていページで最も役に立つ行です。

## この機能の位置付け

`to-spec` はメインのビルドチェーンのステップですが、その複数セッションの分岐にだけ現れます:

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review → retro
```

上流では、[grill-with-docs](https://aihero.dev/skills-grill-with-docs) が、このスキルが記録するだけの決定を下し、完成した [wayfinder](https://aihero.dev/skills-wayfinder) のマップはここでチェーンに合流します。下流では、[to-tickets](https://aihero.dev/skills-to-tickets) が spec をトレーサーバレット型のチケットに切り分け、[implement](https://aihero.dev/skills-implement) がそれを構築します。どのスキルやフローが合うか分からないときは、[ask-matt](https://aihero.dev/skills-ask-matt) が案内します。
