## 機能

`codebase-design` は、モジュールを設計するときに使う言葉を固定します: **module**、**interface**、**depth**、**seam**、**adapter**、**leverage**、**locality** です。それぞれを厳密に定義し、曖昧な代用語（「component」「service」「API」「boundary」）を禁止し、そこから導かれる少数の原則を示します。

これはリファレンスであって、プロセスではありません。ループを実行することも、成果物を作ることも、立ち止まってあなたに質問することもありません。設計に関わる他のすべてのスキルがこの語彙を使います。単独で使うと、言葉を示してそこで止まります。呼び出す前にこのことを知っておいてください。プロセスも停止条件もないスキルに [session](https://www.aihero.dev/ai-coding-dictionary/session) を向けて「進めて」と言うと、エージェントはプロセスを自分で作り出してしまいます。下の質問で、それがどういうことかを説明しています。

## この機能を使うタイミング

`/codebase-design` と入力するか、設計のタスクに合うときにエージェントが自動的に使います。

どのコードを設計し直すかはすでに分かっていて、その形について考える必要があるときに使います: seam をどこに置くか、interface をどこまで小さくできるか、切り出しによって呼び出し側から複雑さが取り除かれるか、といったことです。設計用語の意味をめぐる議論に決着をつけるのにも使えます。

これに近いスキルがいくつかあります。抱えている問題で選んでください:

| 問題 | スキル |
|---|---|
| 1 つのモジュールの形: その interface、seam、depth | `codebase-design` |
| *ドメインの言葉*: 「account」が 3 つの意味を持つ、2 人が「cancellation」で別のことを指している | [domain-modeling](https://aihero.dev/skills-domain-modeling) |
| *どの*モジュールを設計し直すべきか、まだ分からない | [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture)（候補を見つける調査） |
| 設計に名前を付けるだけでなく、議論してほしい | [grilling](https://aihero.dev/skills-grilling) |
| 作るべき具体的な振る舞いがあり、リファクタリングに耐えるテストが欲しい | [tdd](https://aihero.dev/skills-tdd) |

## 語彙

用語集こそがこのスキルです。スキルはすべての用語を互いに関連づけて定義し、それぞれが置き換える言葉も示します。

| 用語 | 意味 | 使わない言葉 |
|---|---|---|
| **Module** | interface と実装を持つものすべて。意図的に規模を問いません: 関数、クラス、パッケージ、複数の層にまたがるスライスなど。 | unit, component, service |
| **Interface** | 呼び出し側が正しく使うために知らなければならないことのすべて: 型シグネチャに加えて、不変条件、順序の制約、エラーの種類、必要な設定、性能特性。 | API, signature |
| **Depth** | interface におけるレバレッジ: 呼び出し側やテストが、学ぶ必要のある interface の単位あたりにどれだけの振る舞いを動かせるか。**Deep**: 小さな interface の背後に多くの振る舞いがある。**Shallow**: interface が実装とほぼ同じくらい複雑。 | なし |
| **Seam** | Michael Feathers の用語: その場所を編集せずに振る舞いを変えられる場所。interface の*位置*であり、それをどこに置くかは、その背後に何を置くかとは別の独立した判断です。 | boundary |
| **Adapter** | seam において interface を満たす具体的なもの。物の種類ではなく役割を指します: インメモリのフェイクも Postgres のリポジトリも、どちらも adapter です。 | なし |
| **Leverage** | depth から呼び出し側が得るもの: 学んだ interface の単位あたりの、より多くの能力。 | なし |
| **Locality** | depth から保守する人が得るもの: 変更、バグ、検証が 1 か所にとどまるので、1 つの修正ですべての呼び出し側に効きます。 | なし |

このスキルは、depth を実装の行数と interface の行数の比として定義*しません*。それは Ousterhout 自身の定義です。その指標は実装を水増しすることを評価してしまうので、このスキルは代わりにレバレッジとしての depth を使います。

## 4 つの原則

- **depth は実装ではなく interface の性質である。** deep なモジュールは、内部では小さな交換可能な部品から作れます。呼び出し側にはそれが見えないだけです。モジュールは、自分のテストが使う内部の seam を持ちつつ、interface には外部向けの seam を 1 つだけ持つことができます。
- **削除テスト。** そのモジュールを削除したと想像してください。複雑さが消えるなら、そのモジュールは素通りするだけのものでした。N 個の呼び出し側に複雑さが再び現れるなら、そのモジュールは本物の複雑さを隠していました。
- **interface がテストの対象面である。** 呼び出し側とテストは同じ seam を通ります。interface を*越えて*テストしたくなるなら、そのモジュールの形が間違っています。
- **adapter が 1 つなら仮説上の seam。2 つなら本物の seam。** 何かがその seam をまたいで変化するまでは、seam を切らないでください。adapter が 1 つだけの seam は、ただの間接参照です。

さらに踏み込んだ補助ファイルが 2 つあり、スキルはそれを最初からではなく必要に応じて読みます。[DEEPENING.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/codebase-design/DEEPENING.md) は、候補の依存関係を 4 つのカテゴリ（in-process、local-substitutable、remote-but-owned、true-external）に分類します。カテゴリによって、深くしたモジュールを seam をまたいでどうテストするかが決まるからです。[DESIGN-IT-TWICE.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/codebase-design/DESIGN-IT-TWICE.md) は、並列の [sub-agents](https://www.aihero.dev/ai-coding-dictionary/subagent) を起動して、同じモジュールに対して根本的に異なる interface を 3 つ以上作らせ、それらを depth、locality、seam の配置で比較します。

## よくある質問

**TypeScript で deep なモジュールを実際にどう作ればいいですか？**

これはこのスキルについて最も多い質問ですが、スキルはそれに答えません。deep なモジュールが*何であるか*は定義しますが、迷い込んだ import が interface を越えて内部に届くのをどう防ぐかについては何も言いません。[Issue #458](https://github.com/mattpocock/skills/issues/458) はこう率直に述べています: 「interface に満足していて、それが詳細を隠しているとしよう。でもそれをどう強制するのか？lint や明確なガードレールがなければ、人間も LLM も、時間とともにそれを散らかし始めると思う。」そのスレッドの回答では 3 つの選択肢が示されました: クラスか IIFE で包み、クラスが巨大になることを受け入れる。monorepo のパッケージにして、monorepo のツールを受け入れる。または [dependency-cruiser](https://github.com/sverweij/dependency-cruiser) のような linter を使って、interface を迂回する import を禁止する。これらの仕組みの中では Effect が最良で、dependency-cruiser が次点です。リポジトリの `in-progress/` バケットには `src/packages/<name>/index.ts` という規約を敷く `setup-ts-deep-modules` スキルがありますが、これはベータ版のスキルで docs ページがなく、lint ルールも付属していません。

**セッションをこれに向けたら、頼んでもいないものの設計し直しに 10 万 [tokens](https://www.aihero.dev/ai-coding-dictionary/token) を使われました。**

これは既知の問題で、[issue #449](https://github.com/mattpocock/skills/issues/449) として登録されています。このスキルはモデル起動型で、自分を語彙だと説明していますが、エージェントがこれを実行可能なプロセスとして扱うのを強制的に止めるものは何も書かれていません。「/codebase-design で再開して、未決定の事項を進めて」と言われたエージェントは、スキルの中で最もプロセスに近い部分、つまり `DESIGN-IT-TWICE.md` の並列 sub-agent を選びました。前のセッションがすでに調べ終えていたコードを再調査し、長時間作業してからようやく何かを尋ねました。このスキルはリファレンスなので、ドライバー役のスキルが持つガードレール（チェックポイント、1 回に 1 つの質問、自動で先に進まないこと）を何も持っていません。回避策は、ドライバー役のスキル（`/grill-with-docs`、`/improve-codebase-architecture`、`/tdd`）を呼び出し、`codebase-design` をその語彙として使うことです。この issue は未解決です。

**`design-an-interface` はどこへ行きましたか？また、`/interface-design` というスキルはありますか？**

このスキルが `design-an-interface` を置き換え、その内容を引き継ぎました。失われたものはありません。その「design it twice」という手法（Ousterhout による、並列の sub-agent が根本的に異なる設計を生み出すもの）は、ここに `DESIGN-IT-TWICE.md` として含まれています。それとは別に、deep module と薄い interface という考え方のための専用の `/interface-design` スキルを求める声が何度かありました。このスキルがすでにその考え方を扱っているので、別のスキルの予定はありません。どちらかの名前を探してここに来たなら、このページで合っています。

**これはフォルダ、barrel ファイル、機能スライスといったファイル構造の規約ではないのですか？**

違います。これについては何度も異論が出ていますが、スキルは変わっていません。[Issue #95](https://github.com/mattpocock/skills/issues/95) は、deep module の具体的な実装として、形式化されたフラクタルツリー型のファイル構造を提案しました。その回答は、両者は独立しているというものでした: 「deep module は interface の設計と、厳密な interface を通じたアクセスに関するものであって、ファイルシステムがどうなっているかは関係ない。このやり方でも shallow なモジュールを持つことは十分ありうると思う。」#458 でも同じ話が出ました: 「モジュールという概念をファイルシステムに結びつけすぎているのではないか。ファイルシステムはモジュールの形を知るための有用なヒントにはなるが、deep module を作るのにファイルシステムを使う必要はない。」用語集は、**module** を意図的に規模を問わないものとして定義しています。

**`tdd` は実際にこの語彙を使っていますか？**

今は使っていますが、長い間使っていませんでした。v1.0 では、この共通のスキルに任せるために `tdd` からインラインの deep module の説明を取り除きましたが、その代わりになるポインタを誰も追加しなかったので、`tdd` は「seam」を独自に定義し、何も参照していませんでした。今は `tdd` にポインタがあり、未解決の問いがテストではなく interface の形であるときに、エージェントはそれをたどります。`tdd` は引き続き、*テストする*境界としての「seam」を担当し、このスキルはその背後にあるモジュールの形を担当します。

**design-it-twice のパターンは Claude Code 以外でも機能しますか？**

すんなりとは機能しません。`DESIGN-IT-TWICE.md` には「spawn 3+ sub-agents in parallel using the Agent tool」と書かれていますが、「Agent」は Claude Code の [tool](https://www.aihero.dev/ai-coding-dictionary/tool) の名前です。リポジトリは Codex を含む他の [harnesses](https://www.aihero.dev/ai-coding-dictionary/harness) 向けのメタデータも同梱していますが、それらにはその名前のツールがないかもしれません。そのため、並列設計のフェーズは、スキルのメタデータから想像されるほど移植性が高くありません。[Issue #564](https://github.com/mattpocock/skills/issues/564) がこれを追跡しており、未解決です。

**connascence、module secrets、[progressive disclosure](https://www.aihero.dev/ai-coding-dictionary/progressive-disclosure) のような独自の概念を用語集に追加できますか？**

そうした提案は出ています。[Issue #180](https://github.com/mattpocock/skills/issues/180) は、seam をまたいで*何が*漏れているかに名前を付ける層として、Parnas の module secrets と Page-Jones の connascence を追加するもので、動作する差分も添えられています。[issue #303](https://github.com/mattpocock/skills/issues/303) は実装の内部での progressive disclosure を提案しており、公開 interface で deep なモジュールが内部にも構造を持つようにするものです。どちらも未解決で、マージされていません。配布中の用語集は意図的に小さくしてあり、その理由はスキル自身が述べています: 一貫した言葉こそが要点であり、誰も一貫して使わない用語は、用語がないより悪いのです。

## 正常に機能している場合

- 設計の会話から「component」「service」「boundary」という言葉が出なくなり、「module」「interface」「seam」が出てくるようになる。
- 提案された切り出しを指して、それが削除テストに合格するかどうかを、言葉を濁さずに言える。
- 提案された seam に、最初の adapter だけでなく 2 つ目の adapter が名指しで添えられている。
- interface についての議論が、型シグネチャだけでなく、不変条件、順序、エラーの種類にも及んでいる。
- 呼び出してもセッションが始まらない。`/codebase-design` だけでエージェントがファイルを読み始め、リファクタリングを提案し始めたなら、リファレンスをドライバーと取り違えています。

## この機能の位置付け

`codebase-design` は**いつでも使えるスタンドアロン**であり、どのチェーンのステップでもなく、エンジニアリングスキルの下にある語彙レイヤーです。最も近い隣人は [domain-modeling](https://aihero.dev/skills-domain-modeling) で、モジュールの形ではなく*問題領域*の言葉を扱う、並行したリファレンスです。deep なモジュールにうまく名前を付けるには両方が必要なので、たいていは一緒に使われます。もう 1 つは [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) です。コードベースを調べて深くする候補を探し、そのすべてをこの用語集の言葉で書くので、それがモジュールを見つけ、あなたはこのスキルの言葉でそれを設計します。どのスキルやフローが合うか分からないときは、[ask-matt](https://aihero.dev/skills-ask-matt) が案内します。
