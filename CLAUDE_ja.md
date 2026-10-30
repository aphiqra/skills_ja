`skills/` 配下では、skills はバケットフォルダーごとに整理されています：

- `engineering/`: 日常的なコード作業
- `productivity/`: 日常の非コード系ワークフローツール
- `misc/`: 保持しているがほとんど使用せず、公開対象ではない
- `in-progress/`: ベータ版。意図的に公開しフィードバックを求めているが、プラグインには含めない
- `deprecated/`: もはや使用されていない

`engineering/` または `productivity/`（**promoted** バケット）に含まれるすべての skill は、トップレベルの `README.md` に参照を持ち、かつ `.claude-plugin/plugin.json` の `skills` 配列にエントリがある必要があります（Claude Code プラグインには、この promoted セットのみが含まれます）。`misc/`、`in-progress/`、および `deprecated/` に含まれる skills は、いずれにも含めてはなりません。

インストールコマンドは [.agents/install-block.md](./.agents/install-block.md) からそのままコピーします。`.claude-plugin/marketplace.json` はこのリポジトリ自身の marketplace です。Claude Code 以外のすべてのエージェントは、自動アップデートを受けるためにここからインストールし、Claude Code は `claude-plugins-official` を使用します。いずれかのマニフェストを変更した後は、`claude plugin validate . --strict` を実行してください。その理由は [ADR 0002](./.agents/adr/0002-ship-as-a-claude-code-plugin.md) に記録されています。

トップレベルの `README.md` にある各 skill エントリは、skill 名からその `SKILL.md` へリンクされている必要があります。

各バケットフォルダには `README.md` があり、そのバケット内のすべての skill を1行の説明付きで列挙し、skill 名からその `SKILL.md` へリンクします。promoted バケットの `README.md` およびトップレベルの `README.md` では、エントリは **User-invoked** と **Model-invoked** にグループ化されます。一方、非 promoted バケットの `README.md`（`misc/`、`in-progress/`）はフラットなリスト形式を使用します。

`engineering/` および `productivity/` の skill には、`docs/<bucket>/<skill-name>.md` に人間向けのドキュメントページもあります（docs ツリーは `skills/` 配下のこれら2つのバケットフォルダを反映しています）。公開 URL はバケットに関係なく `https://aihero.dev/skills-<skill-name>` です。docs のパスはリポジトリ構成上のものに過ぎません。`engineering/` または `productivity/` の skill を追加・リネーム・動作変更した場合は、[.agents/writing-docs.md](./.agents/writing-docs.md) に従ってドキュメントページを作成または再同期してください。完成したページには4つのセクションがあります: **What it does**、**When to reach for it**、**Common questions**、**It's working if** です。`writing-docs.md` には、テンプレート、セクションの順序、質問を探す場所が書かれています。非 promoted バケット（`misc/`、`in-progress/`、`deprecated/`）の skill には**ドキュメントページは作成しません**。唯一の例外は、promoted な skill をまるごと削除した場合です。そのページは archived と明記して残します（`writing-docs.md` を参照）。

すべての `SKILL.md` は、user-invoked（`disable-model-invocation: true` に加えて `agents/openai.yaml` に `policy.allow_implicit_invocation: false` を設定し、人間からのみ到達可能）または model-invoked（モデルまたはユーザーから到達可能）のいずれかです。詳細は [.agents/invocation.md](./.agents/invocation.md) を参照してください。

[`ask-matt`](./skills/engineering/ask-matt/SKILL.md) は、ユーザーから到達可能なすべての skill とその関係をマッピングするルーターです。ドキュメントページの再同期と同じトリガーが適用されます。ユーザーから到達可能な skill を追加・リネーム・削除・フローへの適合方法を変更した場合は、`ask-matt` の `SKILL.md` を読み直して更新し、マップの正確性を保ってください。新しい skill が言及されていない、またはすでに存在しないのにルーティングされ続けている場合、そのルーターは正しくありません。

`deprecated/` と `misc/` 以外のすべての skill をローカルのハーネス用 skill ディレクトリ（`~/.claude/skills`, `~/.agents/skills`）に（再）リンクするには、`scripts/link-skills.sh` を実行します。各エントリはこのリポジトリへのシンボリックリンクであるため、`git pull` によりインストール済みの skill は常に最新の状態に保たれます。skill の追加・削除・リネームを行った後は、このスクリプトを再実行してください。

このリポジトリの文章（`SKILL.md` ファイル、docs、`README.md`、`CHANGELOG.md`、ADR、changeset、コードコメント）では、どこにも em-dash（—）を使いません。文が em-dash を使いたくなったら、その文に本当に合うもの（読点、コロン、句点、括弧、接続詞）を使って書き直してください。機械的な文字の置き換えは決してしないでください。

サブエージェントを起動するすべての skill は、新しいコンテキストウィンドウで起動するよう指示します。一部のハーネス（たとえば Codex）は、デフォルトで親の会話をフォークします。フォークは親のコンテキストを引き継ぐため、サブエージェントが本来逃れるはずだったものをそのまま抱えてしまいます。

## Agent skills

### Triage labels

正規の名前のままで、変更はありません。`docs/agents/triage-labels.md` を参照してください。課題は [`SCOPE.md`](./SCOPE.md) に照らして判断されます。

## 言語ポリシー（Language Policy）

ユーザーに向けたすべてのコミュニケーションおよび成果物は、日本語で提供します。

スキルの指示内容には従いますが、ユーザーとやり取りする際は常に日本語で対応します。これには、質問・説明・要約・計画・レビューコメント・PRD・仕様書・issue・ADR の要約・引き継ぎメモなどが含まれます。

コード、コマンド、ファイルパス、パッケージ名、API 名、識別子、ラベル、既存の技術用語については、英語のほうが明確な場合、またはリポジトリがすでに英語で統一している場合は、英語のまま使用します。

スキルがプロジェクト文書、チケット、PRD、仕様書、ADR、要約などを作成または更新する場合、ユーザーが明示的に別の言語を指定しない限り、これらの成果物のデフォルト言語は日本語とします。
