Quickstart:

```bash
npx skills add mattpocock/skills --skill=code-review
```

```bash
npx skills update code-review
```

[Source](https://github.com/mattpocock/skills/tree/main/skills/engineering/code-review)

## 機能

`code-review` は、`HEAD` とユーザーが指定した基準点（コミット、ブランチ、タグ、またはマージベース）との差分を、**Standards**（コードがこのリポジトリで文書化されている規約に従っているか）と **Spec**（元となる issue や仕様で求められた内容を実装しているか）という 2 つの独立した観点でレビューします。各観点は並列のサブエージェントとして実行され、結果は並べて報告されます。この 2 つの結果を統合したり優先順位付けしたりすることはありません。変更が一方の観点では合格し、もう一方では不合格になることがあり、単一の総合判定では片方の問題が隠れてしまうため、分離しておくこと自体が重要だからです。

## この機能を使うタイミング

`/code-review` と入力するか、ブランチ、PR、作業途中の変更、あるいは「X 以降の変更」をレビューするよう依頼した場合はエージェントが自動的に利用します。

既知の正常な基準点と比較できる差分があり、「*正しく作られているか？*」と「*正しいものを作っているか？*」という 2 つの質問に独立して答えてほしい場合に利用してください。これは開発ループの最後に実行されます。テスト駆動で実際にコードを書く場合は [tdd](https://aihero.dev/skills-tdd) を使用し、仕様全体をコードへ実装する場合は [implement](https://aihero.dev/skills-implement) を使用してください。`implement` はコミット前に独自の `/code-review` を実行します。

## 前提条件

**Spec** の観点では、元となる仕様を見つけられる場所が必要です。たとえばコミットメッセージ内の issue 参照、ユーザーが指定したパス、あるいは `docs/` や `specs/` 配下の仕様書です。その課題（issue）管理システムとの連携は [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) によって提供されます。仕様が見つからない場合、Spec の観点は単にスキップされ、その旨が報告されます。**Standards** の観点では事前設定は不要です。リポジトリに規約が文書化されていない場合でも、組み込みの Fowler コードスメルの基準が常に適用されます。

## 2 つの観点を統合しない

このスキルの中心的な考え方は **2 つの観点** にあります。**Standards** は、差分がこのリポジトリのコーディングスタイルに従っているかを確認します。対象は `CODING_STANDARDS.md` や `CONTRIBUTING.md`、さらに約 12 種類の Fowler コードスメル（Mysterious Name、Duplicated Code、Feature Envy、Data Clumps など）の固定基準です。この基準を安全に適用するためのルールは 2 つあります。リポジトリで文書化された規約が常に優先されること、そして各コードスメルはあくまで判断材料であり、絶対的な違反ではないことです。**Spec** はこれとは直交する質問を行います。つまり、コードが issue や仕様で求められた内容を実際に満たしているか、要求の漏れや不要な機能追加（スコープクリープ）がないかを確認します。

これらは互いのコンテキストを汚染しないように並列サブエージェントとして実行されます。最終レポートでは、それぞれが独立した `## Standards` および `## Spec` セクションとして提示され、観点ごとの要約も含まれます。意図的に、両観点を横断した単一の総合評価は存在しません。

## 正常に機能している場合

- サブエージェントを実行する前に、まず基準点を特定して確認し（`git rev-parse`）、不正な参照や空の差分であれば、サブエージェント内部で失敗するのではなく、その時点で即座に失敗すること。
- Standards と Spec の指摘事項が 2 つの独立したブロックとして表示され、それぞれが参照元（リポジトリ規約や基準コードスメル、または引用された仕様の記述）を示していること。
- 仕様が見つからない場合、Spec の観点は要件を捏造せず、「仕様が利用できない」と報告すること。

## この機能の位置付け

`code-review` は、メインの開発チェーンの末尾に位置するレビュー工程です。

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

最も近い関連スキルは [implement](https://aihero.dev/skills-implement) で、これは開発を推進し、コミット前のレビュー工程としてこのスキルを呼び出します。上流では、このスキルが参照する仕様は [to-spec](https://aihero.dev/skills-to-spec) および [to-tickets](https://aihero.dev/skills-to-tickets) によって作成されます。どのスキルやフローを利用すべきかわからない場合は、[ask-matt](https://aihero.dev/skills-ask-matt) が案内します。
