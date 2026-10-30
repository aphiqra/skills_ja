---
name: domain-modeling
description: プロジェクトのドメインモデルを構築し、洗練させます。コードベースの用語について議論するとき、GLOSSARY.md を作成・編集するとき、または ADR を記録・編集するときに使用します。
---

# Domain Modeling

設計を進める中で、プロジェクトのドメインモデルを積極的に構築・洗練します。これは**能動的な作業**です — 用語に異議を唱え、境界ケースを考案し、用語集や意思決定を、それが明確になった瞬間に書き留めます。（`GLOSSARY.md` を読んで語彙を理解するだけではこのスキルではありません — それはどのスキルでも行う基本的な行為です。このスキルは、モデルを変更する場合に使用します。）

## File structure

ほとんどのリポジトリでは単一のコンテキストを持ちます：

```
/
├── GLOSSARY.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

ルートに `GLOSSARY-MAP.md` が存在する場合、そのリポジトリは複数のコンテキストを持ちます。マップはそれぞれの場所を指し示します：

```
/
├── GLOSSARY-MAP.md
├── docs/
│   └── adr/                          ← system-wide decisions
├── src/
│   ├── ordering/
│   │   ├── GLOSSARY.md
│   │   └── docs/adr/                 ← context-specific decisions
│   └── billing/
│       ├── GLOSSARY.md
│       └── docs/adr/
```

ファイルは必要になったときに初めて作成するようにします。`GLOSSARY.md` が存在しない場合は、最初の用語が解決されたタイミングで作成します。`docs/adr/` が存在しない場合は、最初の ADR が必要になったときに作成します。

## During the session

### Challenge against the glossary

ユーザーが `GLOSSARY.md` の既存の用語体系と矛盾する用語を使用した場合は、すぐに指摘します。"あなたの用語集では 'cancellation' はXと定義されていますが、今はYの意味で使っているように見えます — どちらですか？"

### Sharpen fuzzy language

ユーザーが曖昧または多義的な用語を使った場合は、正確で標準的な用語を提案します。"あなたは 'account' と言っていますが、それは Customer のことですか、それとも User ですか？これらは別の概念です。"

### Discuss concrete scenarios

ドメイン間の関係が議論されている場合、具体的なシナリオで検証します。境界条件を突くシナリオを創作し、概念間の境界を明確にさせます。

### Cross-reference with code

ユーザーが振る舞いを説明した場合、コードと一致しているか確認します。矛盾があれば指摘します："コードでは Order 全体をキャンセルしていますが、一部キャンセルが可能だと説明していますね — どちらが正しいですか？"

### Update GLOSSARY.md inline

用語が確定したら、その場で `GLOSSARY.md` を更新します。まとめて処理せず、その都度記録します。[GLOSSARY-FORMAT.md](./GLOSSARY-FORMAT.md) の形式に従います。

`GLOSSARY.md` には実装詳細を一切含めてはいけません。仕様書やメモとして使ってはいけません。これは用語集専用です。

### Offer ADRs sparingly

以下の3条件を**すべて満たす場合のみ** ADR の作成を提案します：

1. **あとから変更しにくい** — 後から変えるコストが大きい
2. **文脈なしでは意図が分かりにくい** — 将来の読者が "なぜこの方法なのか？" と疑問に思う
3. **実際にトレードオフがあった** — 複数の選択肢から理由があって選んだ

3つのうち1つでも欠ける場合は、ADRは作成しません。[ADR-FORMAT.md](./ADR-FORMAT.md) の形式を使用します。
