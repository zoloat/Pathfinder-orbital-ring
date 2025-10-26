# 05_GitStructure – Git Management and Commit Policy (v0.1)

---

## 1. 目的 / Purpose
この章は、Zipline プロジェクトを Git リポジトリ上で安全かつ効率的に  
管理するための構成・ルール・運用指針を定義する。

This document defines the structure, rules, and operational policy  
for managing the Zipline project safely and efficiently in Git.

---

## 2. 推奨フォルダ構成 / Recommended Folder Structure

```
zipline/
├── cards.json        ← カード本体（論理構造）
├── editor.json       ← UI状態（座標・ズームなど）
├── meta.json         ← プロジェクト情報（タイトル・仕様ver）
├── README.md         ← 概要
├── validate.json     ← 検証レポート（自動生成・通常は除外）
└── export.zip        ← 出力アーカイブ（リリース時のみ）
```

---

## 3. コミット単位 / Commit Units

| 操作 / Operation | コミット対象 / Commit Target | 備考 / Notes |
|------------------|-------------------------------|---------------|
| カードの追加・編集 | `cards.json` | コンテンツ更新 |
| カードの位置変更 | `editor.json` | レイアウト変化 |
| 設定・タイトル変更 | `meta.json` | 管理情報更新 |
| 検証実行 | `validate.json`（必要時のみ） | 通常は .gitignore |
| 出力アーカイブ生成 | `export.zip` | タグ付けリリース時のみ |

Each file has a clear ownership of meaning,  
so commits can be granular and descriptive.

---

## 4. 推奨コミットメッセージ形式 / Recommended Commit Messages

| Prefix | 意味 / Meaning | 例 / Example |
|---------|----------------|---------------|
| `add:` | 新規カード・ノードの追加 | `add: Q102 new question about safety` |
| `fix:` | 本文修正・誤接続修正 | `fix: corrected branch target of Q056` |
| `move:` | 位置調整・スナップ変更 | `move: repositioned A205 near Q102` |
| `meta:` | プロジェクト設定変更 | `meta: updated spec version to v0.1` |
| `export:` | 出力生成・検証済み版 | `export: release build v0.1` |

---

## 5. .gitignore 推奨設定 / Recommended `.gitignore`

```
# Temporary & auto-generated files
validate.json
export.zip

# Optional (per-user layout)
# editor.json
```

These exclusions keep the repository clean while allowing flexible layouts.

---

## 6. バージョンタグ運用 / Tagging Policy
- 仕様が確定したタイミングで `v0.x` タグを付与。  
- リリース版は `export.zip` をコミットし、タグ名と一致させる。  
- ドラフト段階では `.zip` を含めずテキストのみ追跡。

Version tags (e.g., `v0.1`, `v0.2`) are created when specs stabilize.  
Exported archives are included only in tagged releases.

---

## 7. コラボレーション指針 / Collaboration Policy
- **cards.json** は一人がロックして編集する運用が安全。  
- **editor.json** は個人単位（チーム共有しない）を推奨。  
- PR レビューでは「論理構造変更」と「見た目変更」を別に扱う。  
- 分岐接続の変更には常に `validate` 実行を伴う。

Best practices for collaboration:  
lock logical edits to one user, keep layout local,  
and always validate before merging connection changes.

---

## 8. リポジトリ層級 / Repository Layers

| 階層 / Layer | 目的 / Purpose | 管理内容 / Contents |
|---------------|----------------|----------------------|
| **Pathfinder-orbital-ring** | 全体統括 | 各モジュール（zipline, pathfinder, docs） |
| **zipline/** | 作成・管理UI | ソース・仕様・docs |
| **zipline/docs/** | 設計資料 | Markdown仕様書群 |

Each layer has independent versioning but unified release numbering.

---

## 9. 変更追跡ポリシー / Change Tracking Policy
- `cards.json` の差分が最も重要。  
  差分レビューでは「削除・追加・再接続」を明確に把握できるよう diff を活用。  
- 編集ツール（Zipline UI）自体の更新は別リポジトリで管理する。  
- GitHub Actions 等で export 検証を自動化する構想あり。

The key diff to track is in `cards.json`.  
Automated validation pipelines may be added in future versions.

---

## 10. 今後の拡張 / Future Enhancements
- `branch-level` 履歴追跡（カード単位の git blame 対応）。  
- `meta.lock` 機構による編集中警告。  
- release build の自動署名（checksum付き）検討。

Planned: branch-level history tracking, edit-lock mechanism,  
and release signing with checksums.

---

## 関連章 / Related
- [04_EditorMeta.md](04_EditorMeta.md) – エディタメタ構造 / Editor Metadata  
- [06_FuturePlans.md](06_FuturePlans.md) – 将来拡張計画 / Future Plans

---
