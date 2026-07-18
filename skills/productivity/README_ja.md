# Productivity
  
コード固有ではない、一般的な workflow tools です。

## User-invoked

あなたが入力したときだけ到達できます（Claude Code: disable-model-invocation: true; Codex: agents/openai.yaml の policy.allow_implicit_invocation: false）。

- **[grill-me](./grill-me/SKILL.md)** — plan や design について、decision tree のすべての branch が解決されるまで徹底的に interview を受けます。
- **[handoff](./handoff/SKILL.md)** — 現在の conversation を handoff document に圧縮し、別の agent が作業を続けられるようにします。
- **[teach](./teach/SKILL.md)** — 現在の directory を stateful teaching workspace として使い、複数 sessions にわたって新しい skill や concept をユーザーに教えます。
- **[writing-great-skills](./writing-great-skills/SKILL.md)** — skills をうまく書き、編集するための reference です：skill を predictable にする vocabulary と principles です。

## Model-invoked
  
Model または user が到達可能です（model がそれらを使えるように rich trigger phrasing があります）。
- **[grilling](./grilling/SKILL.md)** — plan、decision、または idea について、decision tree のすべての branch が解決されるまでユーザーを徹底的に interview します。
