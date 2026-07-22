---
name: obsidian-vault
description: Obsidian ボールト内のノートを検索、作成、管理する。wikilink とインデックスノートを用いて、ノートの検索、作成、整理を行いたい場合に利用する。
---

# Obsidian Vault

## Vault（保管庫）の場所

/mnt/d/Obsidian Vault/AI Research/  
主にルートレベルでフラットに管理されています。

## 命名規則
- **インデックスノート**: 関連トピックを集約します（例: `Ralph Wiggum Index.md`, `Skills Index.md`, `RAG Index.md`）
- すべてのノート名には **Title Case** を使用してください
- 整理のためにフォルダーは使用しないでください。代わりにリンクとインデックスノートを使用してください

## リンク
- Obsidian の `[[wikilinks]]` 構文を使用してください: `[[Note Title]]`
- ノートの末尾に依存先や関連ノートへのリンクを記載してください
- インデックスノートは `[[wikilinks]]` の一覧のみで構成してください

## ワークフロー

### ノートを検索する

```bash
# ファイル名で検索
find "/mnt/d/Obsidian Vault/AI Research/" -name "*.md" | grep -i "keyword"

# 内容で検索
grep -rl "keyword" "/mnt/d/Obsidian Vault/AI Research/" --include="*.md"  
```

または、ボールトのパスに対して Grep/Glob ツールを直接使用してください。

### 新しいノートを作成する

1. ファイル名には **Title Case** を使用してください
2. ボールトのルールに従い、学習単位として内容を書いてください
3. 末尾に関連ノートへの `[[wikilinks]]` を追加してください
4. 番号付きシーケンスの一部である場合は、階層的な番号付け規則を使用してください

### 関連ノートを探す

ボールト全体から `[[Note Title]]` を検索し、バックリンクを見つけます:

```bash
grep -rl "\\[\\[Note Title\\]\\]" "/mnt/d/Obsidian Vault/AI Research/"
```

### インデックスノートを探す

```bash
find "/mnt/d/Obsidian Vault/AI Research/" -name "*Index*"
```
