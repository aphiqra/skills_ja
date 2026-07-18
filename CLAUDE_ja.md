`skills/` 配下では、skills はバケットフォルダーごとに整理されています：

- `engineering/` — 日常的なコード作業
- `productivity/` — 日常の非コード系ワークフローツール
- `misc/` — 保持しているがほとんど使用せず、公開対象ではない
- `personal/` — 自分の環境に紐づくため公開対象ではない
- `in-progress/` — まだリリース準備ができていないドラフト
- `deprecated/` — もはや使用されていない

`engineering/` または `productivity/`（**promoted** バケット）に含まれるすべての skill は、トップレベルの `README.md` に参照を持ち、かつ `.claude-plugin/plugin.json` の `skills` 配列にエントリがある必要があります（Claude Code プラグインには、この promoted セットのみが含まれます）。`misc/`、`personal/`、`in-progress/`、および `deprecated/` に含まれる skills は、いずれにも含めてはなりません。

リポジトリ自体は単一プラグインの Claude Code marketplace でもあり、`.claude-plugin/marketplace.json` には `mattpocock-skills` プラグインが1つだけ記載されています。リリースバージョンを更新する際は、`.claude-plugin/plugin.json` の `version` を `package.json` のものと常に同期してください — Claude はこの plugin の `version` を使って、インストール済みユーザーにアップデートを表示するかどうかを判断します。いずれかのマニフェストを変更した後は、`claude plugin validate . --strict` を実行してください。Claude プラグインとして提供し、（現時点では）Codex プラグインではない理由は [.agents/adr/0002-ship-as-a-claude-code-plugin.md](./.agents/adr/0002-ship-as-a-claude-code-plugin.md) に記載されています。

トップレベルの `README.md` にある各 skill エントリは、skill 名からその `SKILL.md` へリンクされている必要があります。

各バケットフォルダには `README.md` があり、そのバケット内のすべての skill を1行の説明付きで列挙し、skill 名からその `SKILL.md` へリンクします。promoted バケットの `README.md` およびトップレベルの `README.md` では、エントリは **User-invoked** と **Model-invoked** にグループ化されます。一方、非 promoted バケットの `README.md`（`misc/`、`personal/`）はフラットなリスト形式を使用します。

`engineering/` および `productivity/` の skill には、`docs/<bucket>/<skill-name>.md` に人間向けのドキュメントページもあります（docs ツリーは `skills/` 配下のこれら2つのバケットフォルダを反映しています）。公開 URL はバケットに関係なく `https://aihero.dev/skills-<skill-name>` です — docs のパスはリポジトリ構成上のものに過ぎません。`engineering/` または `productivity/` の skill を追加・リネーム・動作変更した場合は、[.agents/writing-docs.md](./.agents/writing-docs.md) に従ってドキュメントページを作成または再同期してください。非 promoted バケット（`misc/`、`personal/`、`in-progress/`、`deprecated/`）の skill には**ドキュメントページは作成しません**。

すべての `SKILL.md` は、user-invoked（`agents/openai.yaml` に `disable-model-invocation: true` と `policy.allow_implicit_invocation: false` を設定し、人間からのみ到達可能）または model-invoked（モデルまたはユーザーから到達可能）のいずれかです。詳細は [.agents/invocation.md](./.agents/invocation.md) を参照してください。

[`ask-matt`](./skills/engineering/ask-matt/SKILL.md) は、ユーザーから到達可能なすべての skill とその関係をマッピングするルーターです。ドキュメントページの再同期と同じトリガーが適用されます。ユーザーから到達可能な skill を追加・リネーム・削除・フローへの適合方法を変更した場合は、`ask-matt` の `SKILL.md` を読み直して更新し、マップの正確性を保ってください — 新しい skill が言及されていない、またはすでに存在しないのにルーティングされ続けている場合、そのルーターは正しくありません。

すべての skill をローカルのハーネス用 skill ディレクトリ（`~/.claude/skills`, `~/.agents/skills`）に（再）リンクするには、`scripts/link-skills.sh` を実行します。各エントリはこのリポジトリへのシンボリックリンクであるため、`git pull` によりインストール済みの skill は常に最新の状態に保たれます。skill の追加・削除・リネームを行った後は、このスクリプトを再実行してください。


# Language Policy

ユーザー向けのコミュニケーションおよび成果物は、すべて日本語で行うこと。

Skill の指示には原文どおり従う。ただし、ユーザーとやり取りする際は、常に日本語でコミュニケーションすること。これには、質問、説明、要約、計画、レビューコメント、PRD、仕様書、Issue、ADR の要約、引き継ぎメモが含まれる。

コード、コマンド、ファイルパス、パッケージ名、API 名、識別子、ラベル、および一般的に確立された技術用語については、英語のままのほうが明確な場合、またはリポジトリ内で既に英語で使われている場合は、英語のままにしてよい。

Skill がプロジェクトドキュメント、チケット、PRD、仕様書、ADR、要約を作成または更新する場合、それらの成果物の既定言語は日本語とする。ただし、ユーザーが明示的に別の言語を指定した場合は、その指定に従う。
``

## 言語ポリシー（Language Policy）

ユーザーに向けたすべてのコミュニケーションおよび成果物は、日本語で提供します。

スキルの指示内容には従いますが、ユーザーとやり取りする際は常に日本語で対応します。
これには、質問・説明・要約・計画・レビューコメント・PRD・仕様書・issue・ADR の要約・引き継ぎメモなどが含まれます。

コード、コマンド、ファイルパス、パッケージ名、API 名、識別子、ラベル、既存の技術用語については、英語のほうが明確な場合、またはリポジトリがすでに英語で統一している場合は、英語のまま使用します。

スキルがプロジェクト文書、チケット、PRD、仕様書、ADR、要約などを作成または更新する場合、ユーザーが明示的に別の言語を指定しない限り、これらの成果物のデフォルト言語は日本語とします。