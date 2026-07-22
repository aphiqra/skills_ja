# Engineering

コード作業で日常的に使用するスキル群です。

## User-invoked（ユーザが起動）

下記はユーザが明示的に呼び出した場合にのみ利用できます（Claude Code: `disable-model-invocation: true`; Codex: `policy.allow_implicit_invocation: false` in `agents/openai.yaml`）。

- **[ask-matt](./ask-matt/SKILL.md)** — 現在の状況に適したスキルまたはフローを尋ねます。このリポジトリ内のユーザー起動型スキルを案内するルーターです。
- **[grill-with-docs](./grill-with-docs/SKILL.md)** — プロジェクトのドメインモデルも構築するグリルセッションです。用語を洗練し、`CONTEXT.md` と ADR をその場で更新します。
- **[triage](./triage/SKILL.md)** — issue をトリアージロールの状態機械に沿って進めます。
- **[improve-codebase-architecture](./improve-codebase-architecture/SKILL.md)** — コードベースをスキャンして深いモジュール化の機会を探し、視覚的な HTML レポートとして提示した後、選択した候補についてグリル形式で掘り下げます。
- **[setup-matt-pocock-skills](./setup-matt-pocock-skills/SKILL.md)** — このリポジトリをエンジニアリングスキル向けに設定します（課題管理システム、トリアージラベル、ドメインドキュメント構成）。リポジトリごとに 1 回実行します。
- **[to-spec](./to-spec/SKILL.md)** — 現在の会話を仕様書へ変換し、課題管理システムへ公開します。
- **[to-tickets](./to-tickets/SKILL.md)** — 計画、仕様、または会話をトレーサーバレット形式のチケット群へ分割します。各チケットは依存関係を宣言し、ローカルファイル上のテキストとして、または実際のトラッカー上のネイティブな依存リンクとして表現されます。
- **[implement](./implement/SKILL.md)** — 仕様書またはチケット群に記載された作業を実装し、事前に合意された境界（seam）で `/tdd` を実行した後、コミット前に `/code-review` で締めくくります。
- **[wayfinder](./wayfinder/SKILL.md)** — 1 回のエージェントセッションでは扱いきれない大規模な作業を、課題（issue）管理システム上の意思決定チケットによる共有マップとして計画し、目的地への道筋が明確になるまで 1 つずつ解決します。

## Model-invoked（モデルが起動）

下記はモデルまたはユーザーから呼び出し可能です（モデルが利用できるようトリガーフレーズが豊富に用意されています）。

- **[prototype](./prototype/SKILL.md)** — 設計上の疑問に答えるための使い捨てプロトタイプを作成します。状態遷移やロジック向けの実行可能なターミナルアプリ、または切り替え可能な複数の UI バリエーションを生成します。

- **[diagnosing-bugs](./diagnosing-bugs/SKILL.md)** — 難しいバグやパフォーマンス退行に対する規律ある診断ループです。再現 → 最小化 → 仮説立案 → 計測 → 修正 → リグレッションテストの順に進みます。
- **[research](./research/SKILL.md)** — 信頼性の高い一次情報源に基づいて調査を行い、その結果を引用付き Markdown ファイルとしてリポジトリへ記録します。バックグラウンドエージェントとして実行されます。
- **[tdd](./tdd/SKILL.md)** — レッド・グリーン・リファクタリングループによるテスト駆動開発です。機能追加やバグ修正を、1 回に 1 つの垂直スライスで進めます。
- **[domain-modeling](./domain-modeling/SKILL.md)** — プロジェクトのドメインモデルを積極的に構築・洗練します。用語へ異議を唱え、シナリオで検証し、`CONTEXT.md` と ADR をその場で更新します。
- **[codebase-design](./codebase-design/SKILL.md)** — 深いモジュール設計のための共通知識と語彙体系です。小さなインターフェース、明確な境界（seam）、インターフェース経由でテスト可能な設計を重視します。
- **[code-review](./code-review/SKILL.md)** — 基準点以降の差分を 2 軸でレビューします。**Standards**（リポジトリのコーディング規約と Fowler のコードスメル基準に従っているか）と **Spec**（元の issue や PRD を忠実に実装しているか）を、並列サブエージェントとして実行します。
- **[resolving-merge-conflicts](./resolving-merge-conflicts/SKILL.md)** — 進行中の git マージまたはリベースの競合をハンク単位で処理し、各側の一次情報源から追跡した意図に基づいて解決した後、処理を完了します。`--abort` は決して使用しません。
