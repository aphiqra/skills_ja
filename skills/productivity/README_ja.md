# Productivity

コードに特化しない、一般的なワークフロー向けツールです。

## User-invoked（ユーザが起動）

下記はユーザが明示的に呼び出した場合にのみ利用できます（Claude Code: `disable-model-invocation: true`; Codex: `policy.allow_implicit_invocation: false` in `agents/openai.yaml`）。

- **[grill-me](./grill-me/SKILL.md)**: 計画や設計について徹底的なインタビューを行い、設計ツリーのすべての分岐が解決されるまで掘り下げます。
- **[handoff](./handoff/SKILL.md)**: 現在の会話を引き継ぎ用ドキュメントへ圧縮し、別のエージェントが作業を継続できるようにします。
- **[teach](./teach/SKILL.md)**: 現在のディレクトリを状態保持型の学習環境として利用しながら、複数回のセッションにわたって新しいスキルや概念を学習者へ教えます。
- **[to-questionnaire](./to-questionnaire/SKILL.md)**: 自分だけでは答えられない判断を、それに答えられる唯一の人に向けた Markdown のアンケートにします（非同期で記入してもらうか、ミーティングで一緒に埋めます）。
- **[wait-what](./wait-what/SKILL.md)**: メッセージが腑に落ちなかった瞬間に使います。エージェントが、あなたに欠けている文脈を補いながら、`GLOSSARY.md` の語彙を使って平易な言葉で説明し直します。

## Model-invoked（モデルが起動）

下記はモデルまたはユーザーから呼び出し可能です（モデルが利用できるようトリガーフレーズが豊富に用意されています）。

- **[grilling](./grilling/SKILL.md)**: 計画、意思決定、またはアイデアについて徹底的なインタビューを行い、設計ツリーのすべての分岐が解決されるまで掘り下げます。
- **[writing-for-agents](./writing-for-agents/SKILL.md)**: エージェント向けの文書を書くためのスキルです。対象は skill、AGENTS.md/CLAUDE.md、そしてエージェントがポインタをたどって読むあらゆる文書です。
