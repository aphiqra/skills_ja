## 機能

`implement` は、すでに決まった作業を構築します。[ticket](https://www.aihero.dev/ai-coding-dictionary/ticket)、[spec](https://www.aihero.dev/ai-coding-dictionary/spec)、または会話で合意したばかりの計画を指定すると、コードを書き、seam で [tdd](https://aihero.dev/skills-tdd) を駆動し、進めながら型チェックを行い、最後に [code-review](https://aihero.dev/skills-code-review) を実行して、現在のブランチにコミットします。

計画を蒸し返すことは決してありません。インタビューも、確認のためのラウンドも、別のアプローチの提案もありません。上流で決まったことがそのまま入力になり、スキルの仕事はそれをコミットに変えることだけです。それが、新しい [agent](https://www.aihero.dev/ai-coding-dictionary/agent) に「これを作って」と入力することとの違いです。そうすると、エージェントは作りながら作業を設計し直してしまうことがよくあります。

## この機能を使うタイミング

これは自分で `/implement` と入力して呼び出します。エージェントが自分から使うことはありません。`disable-model-invocation: true` 付きで配布されているので、他のスキルから呼び出すこともできません。[ask-matt](https://aihero.dev/skills-ask-matt) や [to-tickets](https://aihero.dev/skills-to-tickets) が「その後チケットごとに `/implement`」と言うとき、それはあなたへの指示であって、エージェントが促されずに行うことではありません。

このスキルが適切かどうかは、作業が今どこにあるかで決まります:

| 作業の状態 | 使うもの |
| --- | --- |
| トラッカー上のチケット | `/implement #42`。1 [session](https://www.aihero.dev/ai-coding-dictionary/session) につき 1 チケットで、チケットの間でコンテキストを [clearing](https://www.aihero.dev/ai-coding-dictionary/clearing) する |
| まだ分割していない spec があり、ビルドが複数セッションにまたがる | まず [to-tickets](https://aihero.dev/skills-to-tickets)、その後チケットごとに `/implement` |
| spec があり、ビルドは小さい | spec に対して直接 `/implement` |
| 直前の会話の中にしかなく、まだ小さい | 同じウィンドウで、その場で `/implement` |
| まだどこにも書かれていない | [grill-with-docs](https://aihero.dev/skills-grill-with-docs)。コードベースがなければ [grill-me](https://aihero.dev/skills-grill-me) |
| spec はなく、テストファーストで作りたい具体的な振る舞いが 1 つある | 直接 [tdd](https://aihero.dev/skills-tdd) |
| すでに作ってあり、確認してほしい | 直接 [code-review](https://aihero.dev/skills-code-review) |

同じセッションでのケースをわざわざ挙げているのは、スキル自身の最初の行がそれを扱っていないからです。`SKILL.md` には「the spec or tickets」と書かれており、[model](https://www.aihero.dev/ai-coding-dictionary/model) は存在しないファイルを探しに行きがちです。計画がスレッドの中にしかない場合は、呼び出すときにそう伝えてください。

## 前提条件

`implement` は今いるブランチにコミットします。ブランチを作ることも、確認することもしません。始める前に、作業を載せたいブランチにいることを確認してください。

チケットが [to-tickets](https://aihero.dev/skills-to-tickets) から来たものなら、その置き場所であるトラッカーは [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) が設定しています。`code-review` は、締めくくりのときに元の spec を見つけるために同じ設定を読みます。

## 1 回の実行で行うこと

1 回の実行は、次の 5 つのステップを順に行います:

1. チケットか spec を読み、seam を割り出す。
2. 事前に合意した seam で [tdd](https://aihero.dev/skills-tdd) を駆動し、レッド・グリーンを 1 スライスずつ進める。
3. こまめに型チェックを行い、進めながら単一のテストファイルを実行する。
4. 最後に 1 回、テストスイート全体を実行する。
5. [code-review](https://aihero.dev/skills-code-review) を実行し、現在のブランチにコミットする。

1 回の実行で扱うのは 1 チケットです。[to-tickets](https://aihero.dev/skills-to-tickets) が作るチケットは、新しい [context window](https://www.aihero.dev/ai-coding-dictionary/context-window) 1 つに収まる大きさのトレーサーバレット型の縦のスライスなので、想定しているリズムは次のとおりです: コンテキストをクリアし、1 チケットを実装し、コミットし、またクリアする。各チケットは自己完結しているので、前のチケットのコンテキストは捨ててかまいません。

## 事前に合意した seam

このスキルの中心となる考え方は **seam** です。内部に手を入れずに振る舞いを観察できる公開の境界のことです。テストは seam に置かれます。コードが存在する前に seam が合意されていれば、テストは長持ちし、その下の実装を書き直してもテストを変える必要はありません。

「事前に合意した」という部分が重要ですが、そこはこのスキルの最も弱い点でもあります。`implement` の中には seam を合意する仕組みがありません。尋ねるのは `tdd` で、`tdd` は確認されていない seam にテストを書くことを拒否します。そのため実際には、合意は上流の spec で行われるか、実行の最初のやり取りで行われます。どこでも行われなければ、実行は「ただコードを書く」ものになり、何も警告されません。spec の中で seam に名前を付けておくことが、それを防ぎます。

## よくある質問

**完了したのに、チケットがまだ開いていて、受け入れ条件もチェックされていません。**

それで正しく、想定どおりです。`implement` には完了処理のステップがありません。コミットで終わり、作業項目には一切触れません。これは GitHub Issues でもローカルの Markdown トラッカーでも同じなので、トラッカー連携の問題ではありません。`code-review` が出した指摘に対応することもなく、元の issue の `- [ ]` のチェックボックスに印を付けることもありません。チケットを閉じ、条件を照らし合わせるのは自分で行ってください。これが最も効いてくるのは依存関係の連鎖があるときです。`to-tickets` はフロンティアを、ブロッカーがすべて閉じられたチケットとして定義しているからです。何も閉じられなければ、目に見えてブロック解除されるものも何もありません。

**すべてのチケットを一度に指定したり、複数を並列に実行したりできますか？**

`/implement` ではできません: 1 回の呼び出しで 1 チケットです。spec 全体を 1 回で実行するには [implement-spec](https://aihero.dev/skills-implement-spec) を使ってください。着手可能なフロンティア上の各チケットを、それぞれ専用の worktree にいる [subagent](https://www.aihero.dev/ai-coding-dictionary/subagent) に渡し、結果を 1 つの統合ブランチにマージします。1 つのチェックアウトで複数の `/implement` セッションを並べて実行するのは、サポート外というより悪い結果になります。ある現場の報告では、3 つの issue にまたがるたった一日の午後のうちに、あるセッションの `git commit --amend` が別のセッションのコミットに適用され、`refs/stash` から stash が消え、コミットが間違ったブランチに載りました。セッションは 1 つの作業ディレクトリ、1 つのインデックス、1 つの HEAD を共有しています。ユーザーは git worktree で回避していますが、`refs/stash` は worktree 間でも共有されるので、worktree だけでは stash の問題は解決しません。

**コミットする代わりにプルリクエストを開けますか？**

組み込みではできません。現在のブランチに直接コミットします。動作を確認する前にコードが入ってしまうので、これは先走りすぎだと感じる人もいます。設定フラグも PR モードもありません。呼び出しのときに上書きする（「ブランチにコミットして PR を開いて」）か、スキルのローカルのコピーを編集して対応されています。エージェントが PR を書くときは、[pr](https://aihero.dev/skills-pr) が本文の形を整えます。

**`code-review` が変更を見られないと言います。**

`code-review` は `git diff <fixed-point>...HEAD` をレビューし、これにはステージ済みや作業ツリーの変更が含まれません。`implement` はコミットの前にそれを実行するので、途中のコミットがすでにない限り、その差分にはレビューするものが何もありません。複数の人がこれを報告していますが、どちらのスキル側でも修正されていません。先にコミットし、それからブランチを切った地点に対してレビューしてください。

それとは別に、実行の中でのレビュー自体を望まない人もいます。書いたばかりのコードをレビューするエージェントは、自分の解決策に偏るからです。新しいセッションで固定点に対して [code-review](https://aihero.dev/skills-code-review) を実行するのは、妥当な代替手段です。そのスキルが 2 つの軸を別々の sub-agent で実行するのも、同じ偏りのためです。

**1 つのチケットで 15 万トークンを使いました。使い方が間違っていますか？**

おそらく違います。チケットが大きすぎる可能性が高いです。1 回の実行では、コードベースの調査、seam ごとのレッド・グリーンのループ、スイート全体、レビューを行うので、小さくないチケットが 10 万 [tokens](https://www.aihero.dev/ai-coding-dictionary/token) を超えるのは、何かが壊れたしるしではなく普通のことです。対処は上流にあります。[to-tickets](https://aihero.dev/skills-to-tickets) でチケットの大きさを適正にし、それぞれが新しいウィンドウ 1 つに収まるようにしてください。1 つのチケットが何度も超えてしまうなら、[effort](https://www.aihero.dev/ai-coding-dictionary/effort) のレベルを上げるのではなく、チケットを分割してください。

**新しいセッションで `/implement #2` としたら、まったく関係のないものに取り組み始めました。**

エージェントが `#2` を、設定されたトラッカーではなく、コンテキストにある別の番号付きリスト（todo ファイルやチェックリストなど）に対して解決してしまったのです。今の `implement` は、渡された参照を issue tracker から取得し、作業を始める前にそのタイトルを伝え、参照が曖昧なときは尋ねるようになっています。そのタイトルが意図したチケットと一致するか確認してください。issue の URL や `owner/repo#2` を渡せば、曖昧さは完全になくなります。

## 正常に機能している場合

- セッションが、何を作るかをあなたに尋ねるのではなく、チケットか spec を読んで、何を作るかを言い直すところから始まる。
- 差分にテストが現れるだけでなく、トレースに実際の `tdd` の Skill tool の呼び出しが見える。
- 実行中に型チェックと単一のテストファイルが繰り返し実行され、終わり近くでスイート全体が 1 回実行される。
- あなたが続けるよう促さなくても、実行が現在のブランチへのコミットまで到達する。
- 差分が 1 チケット分の変更になっている: 複数のチケットをまとめたものではなく、すべての層を貫く縦のスライスである。

## この機能の位置付け

`implement` はメインチェーンのビルドのステップです:

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review → retro
```

隣人は、このスキルが消費するチケットを作り、その順序を決めるブロッキングエッジを宣言する [to-tickets](https://aihero.dev/skills-to-tickets)、各 seam で内部的に駆動する [tdd](https://aihero.dev/skills-tdd)、コミット前に実行する [code-review](https://aihero.dev/skills-code-review) です。計画系のスキルの下流にあり、それらを信頼しています。渡されたものの形を検証し直すことはないので、構造の悪いマップや、横方向に層を切ったチケットは、書かれたとおりに作られます。

その信頼があるからこそ、[wayfinder](https://aihero.dev/skills-wayfinder) はマップを `implement` に直接つなげるのではなく、[to-spec](https://aihero.dev/skills-to-spec) でチェーンに合流します。マップから直接 `implement` に進むのは、取り組みが小さかったと分かった場合だけにしてください。

どのフローにいるのか分からないときは、[ask-matt](https://aihero.dev/skills-ask-matt) がスキル全体を案内するルーターです。
