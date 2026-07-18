# Matt Pocock Skills

Claude Code によって読み込まれるエージェントスキル（スラッシュコマンドと振る舞い）のコレクションです。skills はバケットごとに整理され、`/setup-matt-pocock-skills` によって生成されるリポジトリごとの設定として利用されます。

## Language

**Issue tracker**:
リポジトリの issue を管理するツール — GitHub Issues、Linear、ローカルの `.scratch/` マークダウン規約、またはそれに類するもの。`to-tickets`、`to-spec`、`triage`、`qa` などのスキルは、これを読み書きします。
_Avoid_: backlog manager, backlog backend, issue host

**Issue**:
**Issue tracker** 内でトラッキングされる単一の作業単位 — バグ、タスク、仕様、または `to-tickets` によって生成された作業単位。
_Avoid_: ticket（外部システムが ticket と呼んでいる場合の引用、または **Decision ticket**（下記参照）の場合を除く）

**Decision ticket**:
`wayfinder` の単位 — `wayfinder:map` の子 **Issue** であり、*決定*が必要な「問い」を保持するもの（実装する作業単位ではない）。この **decision** という修飾によって、実装チケットと区別されます。`wayfinder` はまずこの用語を導入し、その後は「ticket」を使用します。

**Triage role**:
トリアージ時に **Issue** に適用される標準的なステートマシンラベル（例：`needs-triage`、`ready-for-afk`）。各ロールは、`docs/agents/triage-labels.md` を通じて **Issue tracker** 内の実際のラベル文字列にマッピングされます。

## Relationships

- **Issue tracker** は複数の **Issues** を保持する
- **Issue** は常に1つの **Triage role** を持つ
- **Decision ticket** は **Issue** である（`wayfinder:map` の子要素）

## Flagged ambiguities

- "backlog" は以前、issue を管理する*ツール*とその中にある*作業の集合*の両方を指す言葉として使われていました — 解決策：ツールは **Issue tracker** とし、"backlog" はドメイン用語としては使用しません。
- "backlog backend" / "backlog manager" — 解決策：**Issue tracker** に統一しました。