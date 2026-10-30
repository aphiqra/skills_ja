## 機能

`code-review` は、`HEAD` とあなたが指定した固定点（コミット、ブランチ、タグ、`main`、`HEAD~5` など）との差分を、2 つの軸でレビューします。**Standards** は、そのコードがこのリポジトリのコードの書き方に従っているかを問います。**Spec** は、そのコードが元の issue や [spec](https://www.aihero.dev/ai-coding-dictionary/spec) の求めたことを実現しているかを問います。それぞれの軸は専用の [sub-agent](https://www.aihero.dev/ai-coding-dictionary/subagent) で実行されるので、どちらも相手の推論を見ません。

このスキルは 2 つの軸を統合したり、並べ替えたりすることは決してしません。レポートの最後には*軸ごと*の最悪の問題が示され、軸をまたいで 1 つの勝者を選ぶことはしません。変更は一方の軸を通過し、もう一方で不合格になることがあります。すべての規約に従っていても間違ったものを実装したコードは、Standards を通過し Spec で不合格になります。[ticket](https://www.aihero.dev/ai-coding-dictionary/ticket) の求めたことをそのまま実現していてもリポジトリの規約を破っているコードは、その逆になります。判定を混ぜてしまうと、通過した軸が不合格の軸を隠してしまいます。

## この機能を使うタイミング

`/code-review` と入力するか、ブランチ、PR、作業中の変更、「X 以降」のレビューを頼んだときに、エージェントが自動的に使います。

| あなたの状況 | 使うもの |
| --- | --- |
| 差分があり、それが正しく作られているか、*そして*正しいものかを知りたい | `code-review` |
| 差分の中のバグ（null の経路、競合状態、off-by-one）を探してほしい | これではなく Claude Code の組み込みレビュー（下記の名前の衝突を参照） |
| まだ何も書いておらず、テストファーストで書きたい | [tdd](https://aihero.dev/skills-tdd) |
| spec 全体をレビュー込みで構築する必要がある | [implement](https://aihero.dev/skills-implement)。これ自体がこのスキルを呼び出します |
| 1 つの差分ではなく、コードベース全体が劣化している | [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) |
| 何かが壊れていて、原因が分からない | [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs) |

固定点はあなたが指定する必要があります。指定しない場合、スキルは推測せずに尋ねてきます。何かを起動する前に、参照が解決できることと差分が空でないことを確認するので、ブランチ名を打ち間違えても、2 つの sub-agent の中ではなく、あなたの目の前で失敗します。

## 前提条件

Standards の軸には何も必要ありません。リポジトリが文書化しているもの（`CODING_STANDARDS.md`、`CONTRIBUTING.md` など）を読み、何も文書化されていなければ組み込みのベースラインを使います。

Spec の軸には、spec が存在し、見つけられることが必要です。次の順に探します:

1. コミットメッセージ内の issue 参照（`#123`、`Closes #45`、GitLab の `!67`）。tracker doc を通じて取得します。
2. 引数として渡されたパス。
3. `docs/`、`specs/`、`.scratch/` 配下にある、ブランチ名や機能名に一致する spec ファイル。
4. あなたに尋ねる。

ステップ 1 は tracker doc に依存しており、これは [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) が書き出します。それがなくても、パスを渡せばこの軸は機能します。spec がまったくない場合は Spec の sub-agent を省略し、要件をでっち上げる代わりに、レポートに「no spec available」と記載します。

## 2 つの軸

| | Standards | Spec |
| --- | --- | --- |
| 問い | 正しく作られているか？ | 正しいものか？ |
| 読むもの | リポジトリが文書化した規約と、smell baseline | 元の issue または spec |
| 報告するもの | 文書化された規約への違反（明確な違反になりうる）と、smell（常に判断を要するもの） | 欠けている、または一部だけの要件、スコープの膨張、誤って実装された要件 |
| 各指摘が引用するもの | 規約ファイルとそのルール、または smell の名前と該当ハンク | spec の該当行 |

この設計は、あなたの規約を知らない汎用レビュースキルを避けるためのものです。そういうスキルは、コードベース内で意図的にしていることを指摘し、コードベースが依存している不変条件を見逃します。そのため、Standards の軸ではリポジトリ自身の文書が [primary source](https://www.aihero.dev/ai-coding-dictionary/primary-source) となり、**常にリポジトリが優先**されます。

**smell baseline** はリポジトリの規約の下に位置します。Fowler の _Refactoring_ 第 3 章にある 12 のコードスメルです: Mysterious Name、Duplicated Code、Feature Envy、Data Clumps、Primitive Obsession、Repeated Switches、Shotgun Surgery、Divergent Change、Speculative Generality、Message Chains、Middle Man、Refused Bequest。それぞれはラベル付きのヒューリスティック（「possible Feature Envy」）であり、明確な違反になることはありません。それぞれに smell の内容と直し方が書かれているので、指摘は不満だけでなく修正方法とセットで届きます。どちらの軸も、linter がすでに強制しているものは省略します。

## よくある質問

**Claude Code 自身の `/code-review` と衝突します。どうすればいいですか？**

これはこのスキルで最も多く報告されている問題で、まだ修正されていません。Claude Code には独自の `/code-review` が付属しており、そちらは別のことをします: 差分の中のバグを探すもので、このスキルは spec への準拠とリポジトリの規約を確認するものです。このライブラリをインストールするとどちらか一方が勝ち、どちらが勝つかはインストール方法によって決まります:

- **Plugin marketplace。** すべてのスキルに `mattpocock-skills:` という接頭辞が付き、組み込みのほうは接頭辞なしの名前では呼び出しにくくなります。
- **通常の skills インストール。** ローカルのファイルが勝ち、このスキルが組み込みのほうを覆い隠します。

1 つの答えは、Claude Code の組み込みスキルを完全に削除することです。それで多くの [context](https://www.aihero.dev/ai-coding-dictionary/context) を節約でき、衝突も気にならなくなります。覆い隠されること自体はおそらく Claude Code の [harness](https://www.aihero.dev/ai-coding-dictionary/harness) のバグ（スキルの作者はどんな名前でも自由に付けられるべき）なので、もう 1 つの答えはローカルのコピーの名前を変えることです。ただし `npx skills update` は、frontmatter の編集やディレクトリ名の変更を元に戻してしまいます。ユーザーが報告している長続きする回避策は、スキルを新しい名前でフォークし、管理対象のセットから `code-review` を外すことです。手作業で再同期できるよう、フォーク元のコミットを控えておいてください。

**sub-agent が `/code-review` を何度も呼び出し、さらにエージェントを起動し続けます。**

これは既知の未解決のバグです。複数の人が、複数のハーネスで再現しています。Standards と Spec のプロンプトは委譲を禁止していないので、sub-agent がまたこのスキルを見つけ、再び分散してしまいます。ある報告では 50 を超えるエージェントに達しました。フォークで適用されている修正は、両方の sub-agent への指示の末尾に 1 行追加することです: 「Do not invoke `/code-review` or spawn additional agents: perform this review directly.」ハーネスのレベルで対処し、すべてのスキルがその防御を受け継ぐようにしたい人もいます。どちらも、配布中のスキルにはまだ入っていません。無人で実行する場合は、エージェントの数に注意してください。

**コードを書いたのと同じ [session](https://www.aihero.dev/ai-coding-dictionary/session) で実行すべきですか？**

新しいセッションを推奨します。ある読者の言葉を借りれば、「同じコンテキストが自分自身をレビューするのはレビューではなく、スラッシュコマンド付きの確証バイアスだ」ということです。コードを書いたセッションでレビューするエージェントは、そのコードを形作ったすべての前提をコンテキストに抱えています。独立したレビュアーならそのコンテキストは持っていません。[implement](https://aihero.dev/skills-implement) から組み込みのレビュー手順を外してほしいという要望があるのも、このためです。その手順は、差分を書いたばかりのセッションの中でレビューを実行するからです。独立したやり方は、クリーンなセッションから自分で `/code-review` を呼び出すことです。

**チケットごとに実行すべきですか、それとも最後に 1 回ですか？**

どちらでも機能し、スキルがあなたの代わりに決めることはありません。チケットごとなら各差分が小さく保たれ、Spec の軸が照らし合わせる spec が 1 つに明確に定まります。`implement` が使うのはこの方式です。ブランチの最後にまとめれば、チケットごとのレビューではそれぞれ見逃してしまうチケット間の相互作用を捉えられます。迷うなら、チケットごとにレビューし、最後にブランチの分岐点に対してもう 1 回実行してください。

**指摘を信用できますか？**

確認しないままでは信用できません。sub-agent の出力は仮説であって、証拠ではありません。あるチームは、文章ベースのレビューでは見逃していた互換性を壊す変更を十数件見つけたと報告しています。スキルは 2 つのレポートをそのまま、または軽く整えて組み合わせるだけです。各主張をファイルに照らして再検証することはしないので、指摘が間違った場所を引用したり、影響を誇張したりすることがあります。行動する前に、各指摘の引用を読んでください。スキルはすべての指摘に引用（規約のルール、smell と該当ハンク、または spec の行）を付けることを求めており、それによって指摘を確認できるようになっています。

**実行するたびに新しい問題が見つかるのはなぜですか？**

修正するたびにレビュー対象の新しいコードが増えますし、Standards の軸のうち判断を要する部分は実行ごとに結果が変わるからです。ある読者はそのループをこう表現しました: 「/code-review と /improve-code-architecture は毎回必ず新しいものを見つける。修正を実装して、これらのスキルを再実行して、また同じことの繰り返し。」収束する保証はありません。1 回の実行結果は手がかりの一覧として扱ってください。引用されたルールの裏付けがあるものに対応したら、そこで止めます。きれいな結果が返ってくるまでループで実行してはいけません。そうなることは決してないからです。

**コミットしていない作業もレビューしますか？**

いいえ。`<fixed-point>...HEAD` の差分を取ります。3 ドット形式はマージベースから測るので、ステージ済みや作業ツリーの変更は含まれません。`implement` が途中でコミットしていなければ、次のコミットに入る予定の作業はレビューから見えません。先にコミットし、レビューしてから、amend するか fixup を追加してください。

## 正常に機能している場合

- 無効な参照や空の差分では、sub-agent を起動する前に開始を拒否する。
- レポートが 1 つに統合された一覧ではなく、`## Standards` と `## Spec` の下に別々のブロックとして届く。
- Standards の各指摘が、リポジトリのファイル内のルールか 12 の smell のどれかを名指しし、該当ハンクを引用している。Spec の各指摘は spec の行を引用している。
- 最後の要約が軸ごとの最悪の問題を示し、全体の勝者を選ぶことはしない。
- spec がない場合、Spec のブロックはコードから推測した要件を並べる代わりに、そのことを伝える。

## この機能の位置付け

`code-review` は、ビルドチェーンの終わり近くにあるレビューのステップです: `grill-with-docs → to-spec → to-tickets → implement → code-review → retro`。指定した任意のブランチや PR に対して、単独でも使えます。

- [implement](https://aihero.dev/skills-implement) は最も近い隣人です。ビルドを進め、コミット前の締めくくりのレビューとしてこのスキルを呼び出します。[implement-spec](https://aihero.dev/skills-implement-spec) も、統合ブランチ全体に対して 1 回、同じことをします。
- [retro](https://aihero.dev/skills-retro) はこのスキルの後に来て、このスキルを調整します。レビューがある種のミスを見逃していることがセッションで分かると、`retro` はチェックや `CODING_STANDARDS.md` のルールを提案し、Standards の軸がそれを読むようになります。
- [pr](https://aihero.dev/skills-pr) は、レビュー済みの作業を出すときにプルリクエストの本文を書きます。
- [to-spec](https://aihero.dev/skills-to-spec) と [to-tickets](https://aihero.dev/skills-to-tickets) は Spec の軸が照らし合わせる文書を作るので、spec が曖昧だとその軸も曖昧になります。
- [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) はコードベース全体を対象にする対応物です。このスキルは 1 つの差分しか見ないからです。

状況にどのスキルが合うか分からないときは、[ask-matt](https://aihero.dev/skills-ask-matt) がスキル全体を案内します。
