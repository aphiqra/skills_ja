Quickstart:

```bash
npx skills add mattpocock/skills --skill=implement
```

```bash
npx skills update implement
```

[Source](https://github.com/mattpocock/skills/tree/main/skills/engineering/implement)

## 機能

`implement` は、仕様書またはチケット群に記載された作業を実装します。テスト駆動開発、型チェック、および完全なテストスイートの実行を経て、レビュー工程へ引き渡し、現在のブランチへコミットします。

このスキルは **何を作るべきかを決定するものではありません**。仕様はすでに確定しており、境界（seam）も合意済みです。`implement` はその計画を再検討するのではなく、実行します。これは頭脳ではなく手です。考える作業は上流工程で完了しています。

## この機能を使うタイミング

このスキルは `/implement` と入力して呼び出します。エージェントが自動的に利用することはありません。

作業内容が仕様として文書化されるか、チケットへ分割され、それをコードへ変換する準備が整った段階で利用してください。まだ仕様が存在しない場合は、まず仕様を作成してください。そのためには [to-spec](https://aihero.dev/skills-to-spec) を利用するか、仕様をチケットへ分割するために [to-tickets](https://aihero.dev/skills-to-tickets) を利用します。完全な仕様を作成せずにテストファーストで実装したいだけであれば、直接 [tdd](https://aihero.dev/skills-tdd) を利用してください。

## 事前に合意された境界（seam）

`implement` の中心となる考え方は **境界（seam）** です。これは機能をテストするための安定したインターフェースであり、コードを書く前に決定されます。このスキルは実装中に新しい境界（seam）を考案することはありません。すでに選択済みの境界（seam）（[to-spec](https://aihero.dev/skills-to-spec) の段階で決定されたもの）を利用し、[tdd](https://aihero.dev/skills-tdd) を通じてその境界（seam）に対するテストを作成します。

事前に合意された境界（seam）を基準に作業することで、実装の健全性が保たれます。テストは長期的に維持できる対象を検証するため、その下のコードは変更できても、テスト自体は大きく変更する必要がありません。

この考え方を中心に据えつつ、ループを短く保ちます。頻繁に型チェックを実行し、作業中は個々のテストファイルを実行し、最後にテストスイート全体を実行します。その後、レビュー工程を行い、現在のブランチへコミットします。

## この機能の位置付け

`implement` は、メイン開発チェーンの終盤にある実装工程であり、レビュー直前に位置します。

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

このスキルは、作業内容が仕様化され、順序付けされた後に利用してください。その前に利用するものではありません。主要な関連スキルは [to-tickets](https://aihero.dev/skills-to-tickets) で、これは依存関係を明示したチケットを生成し、`implement` がそれらを順番に処理します。また、[tdd](https://aihero.dev/skills-tdd) は内部的に利用され、各境界（seam）に対するテストを作成します。その後、自身の [code-review](https://aihero.dev/skills-code-review) を実行してからコミットを行います。どのスキルやフローを利用すべきかわからない場合は、[ask-matt](https://aihero.dev/skills-ask-matt) が案内します。
