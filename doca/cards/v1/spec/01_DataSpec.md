# 01_DataSpec – Card Handling within Zipline (v0.1)

---

## 範囲 / Scope
この章は「Zipline がどのようにカードデータを扱うか」を定義する。  
カード自体の構造定義（フィールド名・型・共有仕様）は  
**Pathfinder CardSpec v0.1** に準拠し、ここではその利用方法と  
Zipline 内での制約・拡張・変換ルールを扱う。

This document defines **how Zipline handles and interacts with card data**.  
It references the shared *Pathfinder CardSpec v0.1* for core field structure  
and focuses on usage, constraints, and transformation logic within Zipline.

---

## 1. 参照するカードフィールド / Referenced Fields

Zipline は Pathfinder のカード構造のうち、以下の項目を直接参照する。

| Field | 説明 / Description | 取り扱い方 / Handling |
|--------|--------------------|------------------------|
| `id` | 一意識別子（自動生成） | 表示・編集不可、内部参照のみ。複製時は新規発行。 |
| `type` | Q or A | 編集パネルで切替可。A→Q変換は可、Q→A変換は警告付き。 |
| `body` | 本文（質問または回答） | 編集対象。空可（未記入ステータスへ）。 |
| `branches[]` | 分岐群（Qカード専用） | ラベル入力と接続先指定をUIで管理。Aでは無効。 |

Zipline only reads and writes the minimal fields:  
`id`, `type`, `body`, and `branches`.  
Any other metadata (tags, notes, history) is stored externally  
and not part of the editing domain.

---

## 2. 編集ルール / Editing Rules

### 2.1 生成 / Creation
- 右下「＋」で空のカードを生成。`type` 未定義、本文空、分岐なし。  
- `id` は Zipline 内部で即時発行。ユーザーには非表示。

A new blank card is created via the bottom-right "+" button.  
It starts with undefined type, empty body, and no branches.  
An `id` is immediately generated internally.

---

### 2.2 タイプ変更 / Type Switching
- **Q → A**：既存分岐を破棄。確認ダイアログを必須。本文は保持。  
- **A → Q**：本文を保持したまま分岐を空で追加可能。

Changing **Q→A** deletes branches after confirmation; body remains.  
Changing **A→Q** adds an empty branch array while keeping the text.

---

### 2.3 複製 / Duplication
- 同一本文を保持した別IDカードを生成（接続なし）。  
- 複製カードはすべて「未接続」扱い。  
- これにより、派生カードの混線を防ぐ。

Duplication creates a new card with a new ID and no links,  
preserving the body only — ensuring clean divergence.

---

### 2.4 接続 / Connections
- Qカードの各分岐（branch）に `target` を設定する形で接続を表現。  
- Aカードには出力端子が存在しない（接続禁止）。  
- 未接続分岐は空のまま保持され、検証時に警告対象となる。

Connections are represented by setting the `target` field  
in each branch of a Q card.  
A cards have no output endpoints.  
Unconnected branches remain empty and are flagged on validation.

---

### 2.5 削除 / Deletion
- カードを削除すると、他カードからの `target` 参照は無効化される。  
- 対応する分岐は「未接続」状態へ更新（赤ステータス）。  
- 削除操作は元に戻す（Undo）に対応。

When a card is deleted, all inbound references are cleared,  
and affected branches revert to "unconnected" state (red status).  
Undo restores prior state.

---

## 3. 保存構造 / Storage Structure

### 3.1 ファイル構成 / File Composition
Zipline は、カード情報とエディタ状態を分離して保存する。

| ファイル | 内容 / Description |
|-----------|--------------------|
| `cards.json` | カード本体（id, type, body, branches） |
| `editor.json` | UI状態（位置・ズーム・スナップ・開閉など） |
| `meta.json` | プロジェクト情報（タイトル、仕様バージョン、開始ノードなど） |

Cards and editor metadata are stored separately  
to keep the data layer pure and implementation-independent.

---

### 3.2 永続化タイミング / Persistence Timing
- 編集確定時（本文・分岐編集・接続変更）ごとに自動保存。  
- 手動保存で明示的にバージョンを確定（履歴比較用）。  
- Git 管理時には `cards.json` の差分が主要な追跡対象。

Auto-save triggers on confirmed edits (body, branch, or connection).  
Manual saves create explicit commit points for version tracking.

---

## 4. Pathfinder との互換方針 / Compatibility Policy

| 項目 / Item | 方針 / Policy |
|--------------|---------------|
| データ形式 / Data format | 完全互換。ZiplineはCardSpecをそのまま使用。 |
| 拡張フィールド / Extended fields | Zipline独自要素はeditor側に隔離。 |
| バージョン管理 / Versioning | `meta.json` 内に `specVersion` を保持。 |
| I/Oモード / Import & Export | Pathfinderが直接読める `.json` 構成を維持。 |

Zipline adheres strictly to the Pathfinder CardSpec.  
Any editor-specific extensions live outside the main card file.

---

## 5. 禁止事項 / Forbidden Constructs
- A → A または A → Q の接続は禁止。  
- id の手動変更禁止。  
- `branches` に無効 id（存在しない参照）を直接書き込む操作は禁止。  
- 空 type のカードを export することは禁止（検証で弾く）。

Forbidden structures include any A-to-* connections,  
manual ID modification, invalid targets,  
and exporting cards with undefined types.

---

## 6. バリデーション連携 / Validation Link
Zipline 内部では、カード編集時にリアルタイムで  
**ステータス小窓**を更新する。  
この評価ロジックは [03_Validation.md](03_Validation.md) に詳細を記載。

During editing, Zipline continuously updates the  
**status indicator** for each card, as defined in [03_Validation.md](03_Validation.md).

---

## 関連章 / Related
- [00_Overview.md](00_Overview.md) – コンセプトと思想 / Concept & Philosophy  
- [02_UI_Behavior.md](02_UI_Behavior.md) – UI挙動と操作 / User Interface Behavior  
- [03_Validation.md](03_Validation.md) – 検証ルール / Validation Rules

---
