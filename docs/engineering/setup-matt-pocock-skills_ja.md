## 機能

`setup-matt-pocock-skills` は、1 つのリポジトリについて 3 つの問いに答えます: issue をどこで管理するか、トリアージラベルを何と呼ぶか、ドメインの文書をどこに置くか、です。その答えを `docs/agents/` 配下の Markdown ファイルとして記録します。

リポジトリごとに異なるのは、そのファイルだけです。スキル自体はどこでも同じです。実行時に `docs/agents/issue-tracker.md` を読み、そこに書かれたとおりに動きます。だからこのセットは GitHub に縛られておらず、別のトラッカーに向けるためにスキルのファイルを編集する必要もないのです。「link the skills to a custom issue tracker」と言って呼び出せば、プログラムから接続できるものなら何でも、スキルを変更せずに使えます。

これはプロンプトで動くスキルであって、決定的なスクリプトではありません。`git remote`、`CLAUDE.md`、`GLOSSARY.md` を読み、見つけたものを提案し、何かを書く前にあなたの確認を待ちます。

## この機能を使うタイミング

これは `/setup-matt-pocock-skills` と入力して呼び出します。[agent](https://www.aihero.dev/ai-coding-dictionary/agent) が自分から使うことはありません。メタデータで意図的に呼び出し不可に設定されているので、他のスキルがあなたの代わりに起動することもできません。

リポジトリごとに 1 回、他のエンジニアリングスキルを初めて使う前に実行します。[triage](https://aihero.dev/skills-triage)、[to-spec](https://aihero.dev/skills-to-spec)、[to-tickets](https://aihero.dev/skills-to-tickets)、[wayfinder](https://aihero.dev/skills-wayfinder) が issue の置き場所を推測し始めたり、トラッカーにないラベルを付けたりするなら、このリポジトリはまだセットアップされていません。プロジェクトの途中のリポジトリでも実行できます。スキルはすでにあるものを読むので、それまでの作業が失われることはありません。

## 前提条件

実行したリポジトリに書き込みます:

| 書き込むもの | 場所 |
| --- | --- |
| `issue-tracker.md` | `docs/agents/` |
| `domain.md` | `docs/agents/` |
| `triage-labels.md` | `docs/agents/`。`triage` スキルがインストールされている場合のみ |
| `## Agent skills` のブロック | `CLAUDE.md` と `AGENTS.md` のうち、すでに存在するほう |

これらはすべて Markdown としてコミットします。ユーザー単位やグローバルのモードはありません。設定はリポジトリの中にあるので、リポジトリごとに独自のコピーを持ちます。

## 3 つの決定

各セクションは推奨の答えから始まり、調査ですでに答えが分かった質問は飛ばします。ほとんどの実行では、確認は 2 回で済みます。

| 決定 | 提案するもの | 尋ねるとき |
| --- | --- | --- |
| **Issue tracker** | `git remote` に一致するもの | 常に。これが唯一の本当の選択だからです |
| **Triage labels** | 5 つの正規の名前（`needs-triage`、`needs-info`、`ready-for-agent`、`ready-for-human`、`wontfix`）をそのまま使う | `triage` スキルがインストールされている場合のみ |
| **Domain docs** | 単一コンテキスト: ルートに `GLOSSARY.md` 1 つと `docs/adr/` | モノレポの兆候を見つけた場合のみ。その場合はマルチコンテキストの `GLOSSARY-MAP.md` を提案します |

トラッカーの選択肢:

| 選択肢 | issue の置き場所 | 必要なもの |
| --- | --- | --- |
| **GitHub** | リポジトリの GitHub Issues | `gh` CLI |
| **GitLab** | リポジトリの GitLab Issues | `glab` CLI |
| **Local markdown** | このリポジトリの `.scratch/<feature>/` 配下のファイル | 何も要りません。リモートさえ不要です |
| **Other** | あなたが指定する場所 | ワークフローを説明する 1 段落 |

最初の 3 つはスキルにテンプレートとして同梱されており、そのまま使えます。Local markdown は代替手段ではなく、正式な選択肢です。このスキルはリモートのない個人プロジェクトにも対応しています。注意点が 1 つあります: GitHub を使っているなら local markdown は使わないでください。両者は二者択一なので、どちらか 1 つを選んでください。

「Other」も正式な選択肢です。Jira、Linear、Azure DevOps、Beads はすべてこれで動いています。あなたがワークフローを説明すると、スキルはその文章を `docs/agents/issue-tracker.md` に記録し、下流のスキルはその文章に従います。ユーザーはすでにこれを作っています: [MCP](https://www.aihero.dev/ai-coding-dictionary/mcp) 経由の Jira 版、`gh` と同じ形の Gitea CLI、手作りのローカルのダッシュボードなどです。

## よくある質問

**GitHub を使わなければいけませんか？**

いいえ。GitHub、GitLab、`.scratch/` 配下の local markdown はすべて既製のテンプレートとして同梱されており、それ以外のものは「other」の道で使えます。これは最も繰り返される質問で、だいたい次のような言葉で聞かれます: *「GitHub に固く縛られている」*、*「GitLab や Jira は使えるか」*、*「Azure DevOps はどうか」*。答えはいつも同じです: トラッカーを選ぶのはスキルではなくセットアップです。

**スキルを更新した後、再実行する必要はありますか？**

v1.1 の後の端的な答えは「はい」でした。スキル自身の最後のメッセージはもっと穏やかで、再実行が必要なのはトラッカーを切り替えるときかやり直すときだけだと伝えます。どちらにも一理あります。元になるテンプレートはバージョン間で変わるので、古いリリースの `docs/agents/issue-tracker.md` は、今それを読むスキルに対して古くなっていることがあります。下流のスキルが文書の説明と違う動きをしたら、セットアップを再実行してください。手間はかかりません。

**`CLAUDE.md` に書かれましたが、私は Codex を使っています。**

これは既知の不足で、まだ未解決です。ファイルを選ぶルールは「`CLAUDE.md` があればそれを編集し、なければ `AGENTS.md`」です。どのファイルが存在するかを確認しているだけで、どの [harness](https://www.aihero.dev/ai-coding-dictionary/harness) が動いているかは見ていません。Claude Code から残った `CLAUDE.md` があるリポジトリでは、スキルは Codex が決して読まないファイルに `## Agent skills` のブロックを書いてしまいます。ユーザーの回避策は 2 つあります: ブロックを手で `AGENTS.md` に移すか、`AGENTS.md` を正とし、`CLAUDE.md` をそれを指す 1 行のポインタにするかです。どちらのファイルもない場合、スキルはどちらかを選ぶ代わりに、どちらを作るかをあなたに尋ねます。スキルが決めてくれると思っていた人は、これに戸惑っています。

**トリアージラベルを作ってくれませんでした。**

作りません。`docs/agents/triage-labels.md` は*対応表*です: トラッカーのどの文字列が 5 つの正規のロールに対応するかを `/triage` に伝えるものです。`gh label create` は実行しません。新しい GitHub リポジトリにはまだラベルがなく、ユーザーはこれを何度もバグとして報告しています。その結果、次の 2 つのことが起きます:

- トラッカーがすでに正規の名前を使っているなら、対応表は恒等表になり、設定するものは何もありません。それが想定される一般的なケースであって、手順の抜けではありません。
- このスキルは [wayfinder](https://aihero.dev/skills-wayfinder) の `wayfinder:map` や `wayfinder:<type>` のラベルも作りません。そして `gh issue create --label <missing>` はラベルを作らずに失敗します。GitHub のリポジトリで wayfinder を初めて実行する前に、手で作っておいてください。

**他のスキルの動作（[grilling](https://www.aihero.dev/ai-coding-dictionary/grilling) のペース、質問の形式、口調）もここで設定できますか？**

いいえ。設定するのは 3 つだけです: トラッカー、ラベル、文書の配置です。ユーザーごとの好みを設定する場所にしてほしいという要望がありました。答えは、スキルは自分の考え方を貫き、ユーザーごとの設定は受け付けない、というものです。好みは、すべてのスキルがすでに読んでいる `CLAUDE.md` に、普通の指示として書いてください。

**すべてのリポジトリにコミットする代わりに、設定を `~/.claude` に置けますか？**

今はできません。多くのリポジトリでスキルを使っているユーザーからこれを求める要望が出ていて未解決ですが、ユーザー単位のモードはありません。どのリポジトリも自分の `docs/agents/` を持ちます。

**他のスキルを設定するスキルがあるのは、おかしくありませんか？**

昔からある不満の 1 つは、まさにそう言っています: *「他のスキルをセットアップするためのスキルがあるのは、どうもしっくりこない。それは LLM が自分のスキルを設定しているということだから。」*このトレードオフは本物です。セットアップのステップがなければ、issue に触れるすべてのスキルが、トラッカーの指示のコピーを自分で持つ必要があります。出力は読んで編集できる Markdown なので、リスクは限られています。書かれたファイルはすべて読めますし、手で変更できます。日々の変更は、もう一度実行するのではなく、そうやって行ってください。

## 正常に機能している場合

- `docs/agents/issue-tracker.md` と `docs/agents/domain.md` があり、`triage` がインストールされていれば `triage-labels.md` もある。
- ハーネスが読む指示ファイルに `## Agent skills` のセクションが現れ、それぞれのファイルを指す 1 行の要約がある。
- 提案されたトラッカーが使っているリモートと一致し、ラベルの文字列がトラッカーに存在するラベルと一致している。
- その後、`/to-tickets` が issue の置き場所を尋ねずに公開し、`/triage` がラベルをでっち上げずに付ける。
- スキルのファイル自体は何も変わっていない。セットアップが `SKILL.md` を編集したなら、何かがおかしくなっています。

## この機能の位置付け

`setup-matt-pocock-skills` はエンジニアリングのフローのための**1 回だけ実行するセットアップ**であり、チェーンのステップではなく、他のすべてが前提とする前提条件です。隣人はその読み手です: ここに書かれたラベルの語彙を適用する [triage](https://aihero.dev/skills-triage)、ここで指定されたトラッカーに公開する [to-spec](https://aihero.dev/skills-to-spec) と [to-tickets](https://aihero.dev/skills-to-tickets)、そして同じトラッカーファイルの「Wayfinding operations」のセクションを読んで、マップと子の [tickets](https://www.aihero.dev/ai-coding-dictionary/ticket) をどう保存するかを知る [wayfinder](https://aihero.dev/skills-wayfinder) です。セットアップが記録したドメイン文書の配置は、後で [domain-modeling](https://aihero.dev/skills-domain-modeling) が埋めていきます。domain-modeling は用語や決定が決まったときにだけ `GLOSSARY.md` と ADR を作るので、セットアップの後にドメイン文書がないリポジトリは普通です。次にどのスキルを使うかについては、[ask-matt](https://aihero.dev/skills-ask-matt) がスキル全体を案内します。
