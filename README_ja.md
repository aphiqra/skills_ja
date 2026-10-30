<p>
  <a href="https://www.aihero.dev/s/skills-newsletter">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skills-repo-dark_2x.png">
      <source media="(prefers-color-scheme: light)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png">
      <img alt="Skills" src="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png" width="369">
    </picture>
  </a>
</p>

# 実践派エンジニアのための Skills

[![skills.sh](https://skills.sh/b/mattpocock/skills)](https://skills.sh/mattpocock/skills)

私が実際のエンジニアリングを行うために毎日使っているエージェントスキルです。vibe coding ではありません。

本物のアプリケーション開発は大変です。GSD、BMAD、Spec-Kit のようなアプローチは、プロセスを引き受けることで助けようとします。しかしそうする中で、あなたのコントロールを奪い、プロセス内のバグを解決しにくくしてしまいます。

これらのスキルは、小さく、適応しやすく、組み合わせ可能になるように設計されています。どのモデルでも使えます。何十年にもわたるエンジニアリング経験に基づいています。自由にいじってみてください。自分のものにしてください。楽しんでください。

これらのスキルの変更や、私が作成する新しいスキルを追いかけたい場合は、約 60,000 人の他の開発者と一緒に私のニュースレターに参加できます：

[ニュースレターに登録する](https://www.aihero.dev/s/skills-newsletter)

## インストール（30 秒セットアップ）

plugin は自動で更新されます。[skills.sh](https://skills.sh/mattpocock/skills) は編集可能なファイルをプロジェクトにコピーし、更新は手作業で行います。エージェントごとにどちらか 1 つを選んでください。両方をインストールすると、すべてのスキルが 2 つずつ入ってしまいます。

### 1. スキルを入手する

<details>
<summary><strong>Claude Code</strong></summary>

```bash
claude plugin install mattpocock-skills@claude-plugins-official
```

デフォルトで自動更新されます。

</details>

<details>
<summary><strong>Codex</strong></summary>

```bash
codex plugin marketplace add mattpocock/skills
codex plugin add mattpocock-skills@mattpocock
```

起動時に自動更新されます。

</details>

<details>
<summary><strong>GitHub Copilot</strong>（CLI と VS Code）</summary>

```bash
copilot plugin marketplace add mattpocock/skills
copilot plugin install mattpocock-skills@mattpocock
```

その後、一度だけ `~/.copilot/settings.json` に次を追加します：

```json
{
  "extraKnownMarketplaces": {
    "mattpocock": { "source": { "source": "github", "repo": "mattpocock/skills" }, "autoUpdate": true }
  }
}
```

VS Code では **Chat: Install Plugin From Source** を実行し、`https://github.com/mattpocock/skills` を入力します。毎日更新されます。

</details>

<details>
<summary><strong>Gemini CLI</strong>（手動更新）</summary>

```bash
gemini skills install https://github.com/mattpocock/skills.git --path skills/engineering
gemini skills install https://github.com/mattpocock/skills.git --path skills/productivity
```

更新するには両方のコマンドを再実行します。

</details>

<details>
<summary><strong>その他のエージェント、または編集可能なファイル</strong>（手動更新）</summary>

```bash
npx skills@latest add mattpocock/skills -a <agent>  # cursor, opencode, devin, windsurf, amp, pi; omit -a to choose
```

インストーラーがどのスキルを入れるか尋ねてきたら、`setup-matt-pocock-skills` を含めてください。更新するには `npx skills@latest update` を実行し、新しいスキルを取り込むには `add` を再実行します。

</details>

### 2. `/setup-matt-pocock-skills` を実行する

エージェント内で、repo ごとに一度実行します。これは以下を行います：

- 使用したい issue tracker（GitHub、GitLab、ローカルファイル、またはあなたが説明するその他のもの）を尋ねます
- チケットをトリアージするときに適用するラベルを尋ねます（`/triage` はラベルを使用します）
- 作成する docs をどこに保存したいかを尋ねます

### 3. これで準備完了です。

## これらの Skills が存在する理由

私はこれらのスキルを、Claude Code、Codex、その他のコーディングエージェントでよく見かける失敗パターンを直す方法として作りました。

### #1: エージェントが私の望むことをしてくれなかった

> 「自分が何を望んでいるのかを正確に知っている人はいない」
>
> David Thomas & Andrew Hunt, [The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**問題**。ソフトウェア開発で最も一般的な失敗パターンは、認識のズレです。あなたは開発者が自分の望みを理解していると思っています。そして、彼らが作ったものを見て、まったく理解していなかったのだと気づきます。

AI の時代でもこれはまったく同じです。あなたとエージェントの間にはコミュニケーションギャップがあります。これを直す方法は **grilling session** です。構築しているものについて、エージェントに詳細な質問をさせることです。

**解決策** は以下を使うことです：

- [`/grill-me`](./skills/productivity/grill-me/SKILL.md): コード以外の用途向け
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md): [`/grill-me`](./skills/productivity/grill-me/SKILL.md) と同じですが、さらに便利なものが追加されています（下記参照）

これらは私の最も人気のあるスキルです。始める前にエージェントと認識を合わせ、これから行う変更について深く考えるのに役立ちます。変更を加えたいときは、_毎回_ 使ってください。

### #2: エージェントがあまりにも冗長すぎる

> ユビキタス言語があれば、開発者同士の会話もコード上の表現も、すべて同じドメインモデルから導かれます。
>
> Eric Evans, [Domain-Driven-Design](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)

**問題**: プロジェクトの開始時には、開発者と、そのソフトウェアを作ってもらう人たち（domain experts）は、たいてい別々の言葉を話しています。

私はエージェントにも同じ緊張を感じました。エージェントは通常、プロジェクトに放り込まれ、進めながら専門用語を理解するよう求められます。そのため、1 語で済むところに 20 語を使います。

この **解決策** は共有言語です。これは、プロジェクトで使われる専門用語をエージェントが解読するのを助けるドキュメントです。

<details>
<summary>
例
</summary>

これは私の `course-video-manager` repo からの [glossary](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md) の例です（固定したこのコミットの時点では、スキルが命名規則を変更する前だったため、まだ `CONTEXT.md` という名前です）。どちらが読みやすいでしょうか？

- **BEFORE**: 「course の section 内にある lesson が「real」になる（つまり file system 上の場所を与えられる）ときに問題があります」
- **AFTER**: 「materialization cascade に問題があります」

この簡潔さは、セッションを重ねるたびに効いてきます。

</details>

これは [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) に組み込まれています。grilling session ですが、AI と共有言語を構築し、説明しにくい決定を ADR に文書化するのにも役立ちます。

これがどれほど強力かを説明するのは難しいです。この repo で最もクールなテクニックかもしれません。試して、実感してみてください。

> [!TIP]
> 共有言語には、冗長さを減らす以外にも多くの利点があります：
>
> - 共有言語を使うため、**変数、関数、ファイルに一貫した名前が付けられます**
> - その結果、エージェントにとって **codebase をナビゲートしやすく** なります
> - エージェントはより簡潔な言語にアクセスできるため、**思考に使うトークンが少なくなります**

### #3: コードが動かない

> 「常に小さく、意図的なステップを踏みなさい。フィードバックの速度があなたの速度制限です。大きすぎるタスクに取り組んではいけません。」
>
> David Thomas & Andrew Hunt, [The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**問題**: あなたとエージェントが、何を作るかについて認識を合わせられたとします。それでもエージェントが _まだ_ ひどいものを出してきたら、どうなるでしょう？

フィードバックループを見るべき時です。生成したコードが実際にどう動くかについてのフィードバックがなければ、エージェントは目隠しで飛んでいるようなものです。

**解決策**: 通常の一連のフィードバックループが必要です：static types、browser access、automated tests です。

automated tests では、red-green-refactor loop が重要です。これは、エージェントがまず失敗するテストを書き、その後そのテストを通す、という流れです。これによりエージェントに一貫したレベルのフィードバックが与えられ、はるかに良いコードにつながります。

どのプロジェクトにも差し込める **[`/tdd`](./skills/engineering/tdd/SKILL.md) skill** を作りました。これは red-green-refactor を促し、良いテストと悪いテストを分けるものについて十分なガイダンスをエージェントに与えます。

デバッグ向けには、デバッグのベストプラクティスを、フェーズごとに関門を設けた規律あるループにまとめた **[`/diagnosing-bugs`](./skills/engineering/diagnosing-bugs/SKILL.md)** skill も作りました。

### #4: Ball Of Mud を作ってしまった

> 「システムの設計に _毎日_ 投資しなさい。」
>
> Kent Beck, [Extreme Programming Explained](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)

> 「最良のモジュールは深いものです。シンプルなインターフェースを通して多くの機能にアクセスできるようにします。」
>
> John Ousterhout, [A Philosophy Of Software Design](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X)

**問題**: エージェントで作られたアプリの多くは複雑で、変更しにくくなります。エージェントはコーディングを劇的に速くできるため、ソフトウェアのエントロピーも加速させます。コードベースは前例のない速度で複雑化します。

この **解決策** は、AI-powered development に対する根本的に新しいアプローチです：コードの設計を大切にすることです。

これは、これらのスキルのすべての層に組み込まれています：

- [`/to-spec`](./skills/engineering/to-spec/SKILL.md) は spec を作成する前に、どの modules に触れるのかについてあなたに質問します

そして重要なのは、[`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md) が、コードベースを deepening opportunities の観点で調査し、候補を提示してくれることです。数日に一度、コードベースで実行することをおすすめします。これは調査であって救済ではありません: 本当に古いコードベースでも本物の候補は見つけてくれますが、泥をほどいてくれるわけではありません。

### まとめ

ソフトウェアエンジニアリングの基礎は、これまで以上に重要です。これらのスキルは、その基礎を反復可能な実践に凝縮し、あなたのキャリアで最高のアプリを出荷する助けにするための、私なりのベストエフォートです。楽しんでください。

## Reference

これらは 1 つの軸で分かれます: 誰が呼び出せるかです。**User-invoked** skills は、あなたが入力したときだけ到達できます（例：`/grill-me`）。その役割はオーケストレーションです。**Model-invoked** skills は、あなたが呼び出すことも、タスクに合うときにエージェントが自動的に使うこともできます。再利用可能な規律を持っています。user-invoked skill は model-invoked skills を呼び出すことがありますが、別の user-invoked skill を呼び出すことはありません。

### Engineering

コード作業で私が毎日使っているスキルです。

**User-invoked**

- **[ask-matt](./skills/engineering/ask-matt/SKILL.md)**: あなたの状況にどの skill や flow が合うかを尋ねます。この repo の user-invoked skills を巡回する router です。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)**: プロジェクトの domain model も構築する grilling session です。用語を磨き、`GLOSSARY.md` と ADRs をインラインで更新します。
- **[triage](./skills/engineering/triage/SKILL.md)**: issues を triage roles の state machine を通して移動します。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)**: codebase を deepening opportunities の観点でスキャンし、それらを visual HTML report として提示し、あなたが選んだものについて grilling します。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)**: engineering skills 用にこの repo を設定します（issue tracker、triage labels、domain doc layout）。他の engineering skills を使う前に、repo ごとに一度実行します。
- **[to-spec](./skills/engineering/to-spec/SKILL.md)**: 現在の会話を spec に変換し、issue tracker に公開します。インタビューはなく、すでに話し合った内容を合成するだけです。
- **[to-tickets](./skills/engineering/to-tickets/SKILL.md)**: plan、spec、または conversation を tracer-bullet tickets のセットに分解します。各チケットは blocking edges を宣言し、それはローカルファイルのテキストとして、または実際の tracker 上の native blocking links として書かれます。
- **[implement](./skills/engineering/implement/SKILL.md)**: spec または tickets に記述された作業を構築し、事前に合意した seams で `/tdd` を進め、commit 前に `/code-review` で締めます。
- **[implement-spec](./skills/engineering/implement-spec/SKILL.md)**: spec 全体を 1 つの integration branch 上で実装します。tickets を task graph として扱い、着手可能な frontier 全体で implementer subagents を走らせて並行度を最大化し、最後に `/code-review` で締めます。
- **[wayfinder](./skills/engineering/wayfinder/SKILL.md)**: 1 回の agent session には収まらない大きな作業単位を、issue tracker 上の decision tickets の共有マップとして計画します。destination への道筋が明確になるまで、1 つずつ解決します。
- **[retro](./skills/engineering/retro/SKILL.md)**: セッションの後で、coding agent の環境（navigation、automated checks、coding standards、steering files、tooling）の改善点を、深刻なものから順に提案します。

**Model-invoked**

- **[prototype](./skills/engineering/prototype/SKILL.md)**: design question に答えるための throwaway prototype を構築します。state/logic questions 向けの共有可能な単一の HTML ファイル、または 1 つの route から切り替えられる、複数の大きく異なる UI variations です。
- **[diagnosing-bugs](./skills/engineering/diagnosing-bugs/SKILL.md)**: hard bugs と performance regressions のための disciplined diagnosis loop です：このバグで red になる feedback loop を作る → minimise → hypothesise → instrument → fix → regression-test。
- **[research](./skills/engineering/research/SKILL.md)**: high-trust primary sources に対して question を調査し、findings を repo 内の cited Markdown file として記録します。background agent として実行します。
- **[tdd](./skills/engineering/tdd/SKILL.md)**: red-green-refactor loop による Test-driven development。features や bugs を 1 つずつ vertical slice で構築・修正します。
- **[domain-modeling](./skills/engineering/domain-modeling/SKILL.md)**: プロジェクトの domain model を能動的に構築・改善します。glossary に照らして terms を検証し、edge-case scenarios で stress-test し、`GLOSSARY.md` と ADRs をインラインで更新します。
- **[codebase-design](./skills/engineering/codebase-design/SKILL.md)**: deep modules を設計するための共有された discipline と vocabulary です：小さな interface の背後に多くの behaviour を置き、clean seam に配置し、その interface 経由で testable にします。
- **[code-review](./skills/engineering/code-review/SKILL.md)**: 固定点以降の diff を 2 軸でレビューします：**Standards**（repo の coding standards と Fowler smell baseline に従っているか）と **Spec**（元の issue/spec を忠実に実装しているか）。parallel sub-agents として実行するため、互いを汚染しません。
- **[pr](./skills/engineering/pr/SKILL.md)**: pull request の本文があるべき形です：変更を明確にする最小限のビジュアルとしての summary、動作することを示す before/after の証拠、そして merge の危険度の判断（one-way door か two-way door か、および blast radius）。
- **[wizard](./skills/engineering/wizard/SKILL.md)**: 人間にしかできない手順を案内する、対話型の bash wizard を生成します：インフラの provisioning、credentials や CI secrets の設定、慣れない third-party dashboard の操作、一度きりの migration や cutover の実行などです。

### Productivity

コード固有ではない、一般的な workflow tools です。

**User-invoked**

- **[grill-me](./skills/productivity/grill-me/SKILL.md)**: plan や design について、design tree のすべての分岐が解決されるまで徹底的にインタビューを受けます。
- **[handoff](./skills/productivity/handoff/SKILL.md)**: 現在の会話を handoff document に圧縮し、別の agent が作業を続けられるようにします。
- **[teach](./skills/productivity/teach/SKILL.md)**: 現在の directory を stateful teaching workspace として使い、複数セッションにわたって新しい skill や concept をユーザーに教えます。
- **[to-questionnaire](./skills/productivity/to-questionnaire/SKILL.md)**: 自分だけでは答えられない判断を、それに答えられる唯一の人に向けた Markdown の questionnaire にします。非同期で記入してもらうか、ミーティングで一緒に埋めます。主題についてではなく、送ること（誰に向けたものか、何を返してほしいか）についてあなたを grilling します。
- **[wait-what](./skills/productivity/wait-what/SKILL.md)**: メッセージが腑に落ちなかった瞬間に使います。agent が、あなたに欠けている文脈を補いながら、`GLOSSARY.md` の語彙を使って平易な言葉で説明し直します。

**Model-invoked**

- **[grilling](./skills/productivity/grilling/SKILL.md)**: plan、decision、または idea について、design tree のすべての分岐が解決されるまでユーザーを徹底的にインタビューします。`grill-me`、`grill-with-docs`、`triage`、`wayfinder`、`improve-codebase-architecture` の背後にある再利用可能な interview primitive です。
- **[writing-for-agents](./skills/productivity/writing-for-agents/SKILL.md)**: agent 向けの文書を書くためのスキルです：skills、AGENTS.md/CLAUDE.md、そして agent がポインタをたどって読むあらゆる文書が対象です。
