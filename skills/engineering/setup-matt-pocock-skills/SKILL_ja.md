---
name: setup-matt-pocock-skills
description: エンジニアリングスキルのためにこのリポジトリを設定します — 課題トラッカー、トリアージラベル語彙、ドメインドキュメント構成をセットアップします。最初の使用前に1回実行してください。
disable-model-invocation: true
---

# Setup Matt Pocock's Skills

エンジニアリングスキルが前提とするリポジトリ単位の設定を足場として構築します：

- **Issue tracker** — 課題がどこに存在するか（デフォルトはGitHub；ローカルMarkdownもそのまま利用可能）
- **Triage labels** — 5つの標準トリアージロールで使用されるラベル文字列
- **Domain docs** — `GLOSSARY.md` や ADR の配置場所と、それらを参照する際のルール

これは決定的スクリプトではなく、プロンプト駆動のスキルです。探索し、発見した内容を提示し、ユーザーに確認してから書き込みます。

## Process

### 1. Explore

現在のリポジトリの状態を理解するために確認します。既存のものをすべて読み、前提で決めつけないでください：

- `git remote -v` と `.git/config` — これはGitHubリポジトリか？どのリポジトリか？
- ルートの `AGENTS.md` と `CLAUDE.md` — どちらか存在するか？すでに `## Agent skills` セクションはあるか？
- ルートの `GLOSSARY.md` と `GLOSSARY-MAP.md`、および旧形式の `CONTEXT.md` / `CONTEXT-MAP.md`。旧形式のファイルがある場合は、`git mv` で `GLOSSARY` の名前に変更し、`docs/agents/domain.md` 内の名前も書き換えることを提案します。
- `docs/adr/` と `src/*/docs/adr/` ディレクトリ
- `docs/agents/` — このスキルの過去の出力がすでに存在するか？
- `.scratch/` — ローカルMarkdown課題トラッカー規約がすでに使われている兆候
- `triage` スキルがインストールされているか？（同ディレクトリに `triage` フォルダがある、または利用可能スキルに存在する）これによりセクションBを実行するかが決まる
- モノレポの兆候 — `pnpm-workspace.yaml`、`package.json` の `workspaces` フィールド、または `packages/*` に `src/` が存在するか。本当に大規模なマルチパッケージ構成でのみ存在し、ない場合は単一コンテキスト（ほとんどのリポジトリ）とみなす

### 2. Present findings and ask

既存の内容と不足しているものを要約します。その後、セクションを順番に進めます — 1セクションにつき1回答、その後次へ進みます。

各セクションは推奨される回答から始め、ユーザーが一言で承認できるようにします。選択肢が本当に分岐する場合のみ1行の説明を加え、それ以外は省略します。探索結果ですでに解決している場合はセクション自体をスキップします（`triage` がインストールされていない場合のSection B、モノレポでない場合のSection C）。

**Section A — Issue tracker.**

> 説明："issue tracker" とは、このリポジトリにおいて課題が管理される場所のことです。`to-tickets`、`triage`、`to-spec` などのスキルはここから読み取り・書き込みを行います — `gh issue create` を呼ぶのか、`.scratch/` 配下にMarkdownファイルを書くのか、あるいは別のワークフローを使うのかを知る必要があります。このリポジトリで実際に作業を管理している場所を選択してください。

デフォルト前提：これらのスキルはGitHub向けに設計されています。`git remote` がGitHubを指している場合はそれを提案します。GitLab（`gitlab.com` または自己ホスト）を指している場合はGitLabを提案します。それ以外の場合（またはユーザーが希望する場合）は次を提示します：

- **GitHub** — 課題はGitHub Issuesで管理（`gh` CLIを使用）
- **GitLab** — 課題はGitLab Issuesで管理（[`glab`](https://gitlab.com/gitlab-org/cli) CLIを使用）
- **Local markdown** — 課題はこのリポジトリの `.scratch/<feature>/` 配下にファイルとして保存（個人開発やリモートがない場合に適する）
- **Other**（Jira、Linearなど） — ワークフローを1段落で説明してもらい、スキル側で自由記述として記録

選択内容は `docs/agents/issue-tracker.md` に記録します。GitHub と GitLab のテンプレートには "PRをリクエスト面として扱う" フラグがありますが、デフォルトは **off** です — そのままにし、この段階では説明しません。外部PRをトリアージ対象にしたい場合は後でユーザーが変更できます。

GitHub または GitLab のトラッカーには、到達可能なリポジトリが必要です。ユーザーがどちらかを選び、そのホストを指す `git remote` がない場合は、先に進む前にユーザーからリポジトリの URL を聞いて接続します（必要なら `git init`、続けて `git remote add origin <url>`）。このディレクトリ内で `gh repo view` / `glab repo view` がプロジェクトを表示できれば完了です。表示できない場合は中断し、何が足りないかをユーザーに伝えてください。

**Section B — Triage label vocabulary.** `triage` スキルがインストールされていない場合、このセクションは完全にスキップします（探索で確認済み） — 未インストールのスキルにはラベルは不要です。

インストールされている場合は、質問は1つだけ行います：

> デフォルトのトリアージラベルをそのまま使用しますか？（推奨：**yes**）

デフォルトは5つの標準ロールで、各ラベルはその名前と同じ文字列になります：`needs-triage`、`needs-info`、`ready-for-agent`、`ready-for-human`、`wontfix`。**yes** の場合はそのまま記録します。ユーザーが no と答えた場合のみ — 通常は既存のトラッカーで別のラベル名を使用している場合（例：`needs-triage` の代わりに `bug:triage`） — 上書き設定を収集し、`triage` が重複を作らず既存ラベルを使用するようにします。

**Section C — Domain docs.** デフォルトは **単一コンテキスト** — ルートに `GLOSSARY.md` と `docs/adr/` を配置します。これはほとんどすべてのリポジトリに適しているため、確認せずにそのまま適用します。

**マルチコンテキスト**（ルートの `GLOSSARY-MAP.md` が各コンテキストの `GLOSSARY.md` を指す構成）は、探索でモノレポの兆候が見つかった場合にのみ提案します。その場合、どのレイアウトにするかを確認します。

### 3. Confirm and edit

以下のドラフトをユーザーに提示します：

- 編集対象となる `CLAUDE.md` または `AGENTS.md` に追加する `## Agent skills` ブロック（どのファイルを編集するかは手順4のルールに従う）
- `docs/agents/issue-tracker.md`、`docs/agents/domain.md`、および `docs/agents/triage-labels.md` の内容（後者は `triage` がインストールされている場合のみ）

書き込み前に、ユーザーが内容を編集できるようにします。

### 4. Write

**編集するファイルを選択：**

- `CLAUDE.md` が存在する場合はそれを編集する
- 存在しない場合、`AGENTS.md` が存在すればそれを編集する
- どちらも存在しない場合は、どちらを作成するかをユーザーに確認する（勝手に選ばない）

`CLAUDE.md` が存在する状態で `AGENTS.md` を新規作成してはいけません（逆も同様） — 必ず既存のファイルを編集すること。

選択されたファイルにすでに `## Agent skills` ブロックが存在する場合は、新しく追加するのではなく、その場で内容を更新します。周囲のユーザー編集部分は上書きしないでください。

対象ブロック：

```markdown
## Agent skills

### Issue tracker

[課題がどこで管理されているかの1行要約]。詳しくは `docs/agents/issue-tracker.md` を参照。

### Triage labels

[ラベル語彙の1行要約]。詳しくは `docs/agents/triage-labels.md` を参照。

### Domain docs

[構成の1行要約 — "single-context" または "multi-context"]。詳しくは `docs/agents/domain.md` を参照。
```

`triage` がインストールされており、Section B が実行されている場合のみ、`### Triage labels` のサブブロックを含め、`docs/agents/triage-labels.md` を作成します。インストールされていない場合は、その両方を省略します。

Section B を GitHub または GitLab で実行した場合は、設定したラベルのうちトラッカーにないものをそれぞれ作成します（`gh label create` / `glab label create`）。

その後、このスキルフォルダ内のテンプレートをベースに docs ファイルを作成します：

- [issue-tracker-github.md](./issue-tracker-github.md) — GitHub の課題トラッカー
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md) — GitLab の課題トラッカー
- [issue-tracker-local.md](./issue-tracker-local.md) — ローカルMarkdown課題トラッカー
- [triage-labels.md](./triage-labels.md) — ラベルマッピング（`triage` がインストールされている場合のみ）
- [domain.md](./domain.md) — ドメインドキュメントの参照ルールと構成

「Other」の課題トラッカーを使用する場合は、ユーザーの説明に基づいて `docs/agents/issue-tracker.md` をゼロから作成します。

### 5. Done

セットアップが完了したことをユーザーに伝え、どのエンジニアリングスキルがこれらのファイルを参照するかを説明します。また、後から `docs/agents/*.md` を直接編集できることも伝えます — 課題トラッカーの変更や、セットアップを最初からやり直したい場合のみ、このスキルを再実行する必要があります。

## 言語ポリシー（Language policy）

対象リポジトリの `AGENTS.md` または `CLAUDE.md` を作成・更新する際は、以下のルールを記載した `## Language Policy` セクション を追加します。

- ユーザー向けのすべてのコミュニケーションは日本語で行います。
- 仕様書、PRD、issue、チケット、ADR の要約、実装計画、レビューコメント、引き継ぎメモなど、
すべての成果物は原則として日本語で記述します。
- コード、コマンド、ファイルパス、パッケージ名、API 名、識別子、ラベル、既存の技術用語は、英語のほうが明確な場合は英語のまま使用して構いません。
- ユーザーが明示的に別の言語を指定した場合は、その指示に従います。
- 
