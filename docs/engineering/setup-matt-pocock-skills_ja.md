Quickstart:

```bash
npx skills add mattpocock/skills --skill=setup-matt-pocock-skills
```

```bash
npx skills update setup-matt-pocock-skills
```

[Source](https://github.com/mattpocock/skills/tree/main/skills/engineering/setup-matt-pocock-skills)

## 機能

`setup-matt-pocock-skills` は、このエンジニアリング向けスキル群が特定のリポジトリ内でどのように動作すべきかを学習します。課題管理はどこで行われるのか、トリアージ用ラベルは何という名前なのか、ドメインドキュメントはどこに配置されているのかを把握し、それらの回答を他のスキルが参照する **設定情報（config）** として記録します。

このスキルは設定を書き込むのであって、動作をハードコードするわけではありません。エンジニアリングチェーンでは `docs/agents/` 配下に 3 つのファイルが存在することを前提としています。このスキルは、それらを生成する一度限りの初期設定処理です。設定内容は実際のリポジトリ（`git remote`、既存ラベル、既存の `CONTEXT.md` など）から検出され、推測ではなくユーザーへの確認を経て決定されます。これはプロンプト主導型です。探索し、発見内容を提示し、確認を受け、その後に書き込みを行います。決め打ちのスキャフォールド生成ではありません。

## この機能を使うタイミング

このスキルは `/setup-matt-pocock-skills` と入力して呼び出します。エージェントが自動的に利用することはありません。

**他のエンジニアリングスキルを初めて使用する前に、各リポジトリごとに一度だけ** 実行してください。[triage](https://aihero.dev/skills-triage)、[to-spec](https://aihero.dev/skills-to-spec)、または [to-tickets](https://aihero.dev/skills-to-tickets) が、課題の場所を推測したり存在しないラベルを適用しようとしたりする場合、このリポジトリではまだ設定が完了していません。再実行が必要なのは、課題（issue）管理システムを変更する場合や最初からやり直したい場合のみです。日常的な調整は `docs/agents/*.md` を編集するだけで行えます。

## 3つの決定事項

確認は推奨回答付きで行われ、多くの場合は一言で承認できます。また、既に推測できる内容は確認を省略するため、ほとんどの実行は数回の簡単な確認で終わります。

- **課題（issue）管理システム（Issue tracker）** — 作業を管理している場所を指定します。これにより `triage`、`to-spec`、`to-tickets` は `gh` や `glab` を利用すべきか、`.scratch/` 配下に Markdown を書くべきか、あるいはユーザー定義のワークフローに従うべきかを判断できます。GitHub、GitLab、ローカル Markdown、またはその他の選択肢があります。（`git remote` に一致するものが提案されます。）
- **トリアージラベル（Triage labels）** — `triage` スキルがインストールされている場合のみ確認されます。質問内容は、デフォルトラベル（`needs-triage`、`needs-info`、`ready-for-agent`、`ready-for-human`、`wontfix`）をそのまま使うかどうかです。既に別名のラベルを利用している場合のみ「いいえ」と答えてください。そうすることで `triage` が重複ラベルを作成せず、実際に存在するラベルを使用できます。
- **ドメインドキュメント（Domain docs）** — デフォルトでは単一コンテキスト構成（ルートに 1 つの `CONTEXT.md` と `docs/adr/`）を前提とします。これはほとんどのリポジトリに適しています。モノレポを示す兆候が見つかった場合のみ、マルチコンテキスト構成が提案されます。

出力として `docs/agents/` 配下にファイル群が作成されます。具体的には `issue-tracker.md`、`domain.md`、そして `triage` がインストールされている場合は `triage-labels.md` です。また、リポジトリで使用されている `CLAUDE.md` または `AGENTS.md` のいずれかに、それらを参照する `## Agent skills` セクションが追加されます。これらのファイルは、他のツール群が依存する共通基盤となります。

## 正常に機能している場合

- `issue-tracker.md` と `domain.md` が `docs/agents/` 配下に作成されること（さらに `triage` がインストールされている場合は `triage-labels.md` も作成されること）。また、`CLAUDE.md` または `AGENTS.md` に `## Agent skills` セクションが追加されること。
- 提案される課題（issue）管理システムが実際の `git remote` と一致しており、ラベル名がリポジトリ内に既に存在する文字列と一致していること。
- その後 `triage` や `to-tickets` が、質問や推測を行わずに正しい場所と正しいラベルを利用できること。

## この機能の位置付け

`setup-matt-pocock-skills` は **一度だけ実行する初期設定** です。エンジニアリングスキル群全体の基盤であり、繰り返し実行する工程ではありません。

近い関連スキルとして、この設定内容を利用する [triage](https://aihero.dev/skills-triage) があります。これはここで設定されたラベル体系を適用します。また、[to-spec](https://aihero.dev/skills-to-spec) および [to-tickets](https://aihero.dev/skills-to-tickets) は、ここで設定された課題（issue）管理システムへ情報を書き込みます。まずこのスキルを実行してください。後続のすべてのスキルは、それが完了していることを前提としています。どのスキルやフローを利用すべきかわからない場合は、[ask-matt](https://aihero.dev/skills-ask-matt) が案内します。
