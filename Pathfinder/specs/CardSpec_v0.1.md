# CardSpec v0.1 – Pathfinder Card Data Specification

> **Status**: v0.1 Draft
> **Scope**: Defines the canonical data structure for Q/A cards shared between Zipline (editor) and Pathfinder (runtime).
> This is the single source of truth referenced by `01_DataSpec.md` and all Pathfinder runtime documents.

---

## 1. 概要 / Overview

カードは Pathfinder システムの基本単位である。
Zipline で作成・編集され、Pathfinder が実行時に読み込む。
両システムは**このスペックで定義された共通スキーマ**のみを交換する。

A card is the fundamental unit of the Pathfinder system.
Cards are authored in Zipline and consumed by Pathfinder at runtime.
Both systems exchange data using **only the schema defined here**.

---

## 2. カード型 / Card Types

| type | 役割 / Role |
|------|------------|
| `"Q"` | Question カード。本文＋分岐（branches）を持つ。ユーザーに選択肢を提示する。 |
| `"A"` | Answer カード。本文のみ。フローの終端または中継点。出力端子を持たない。 |
| `null` / `undefined` | 未設定（Zipline での作成直後）。エクスポート不可。 |

---

## 3. フィールド定義 / Field Definitions

### 3.1 共通フィールド（Q・A 両方）

| フィールド | 型 | 必須 | 説明 |
|-----------|-----|------|------|
| `id` | `string` | ✅ | 一意識別子。システムが自動生成。手動変更不可。形式: UUIDv4 推奨。 |
| `type` | `"Q" \| "A" \| null` | ✅ | カード種別。`null` は未設定（エクスポート不可）。 |
| `body` | `string` | ✅ | カードの本文テキスト。空文字列は許容するが検証時に警告。 |
| `branches` | `Branch[]` | Q のみ | Q カードの分岐リスト。A カードでは**省略または空配列**。 |

### 3.2 Branch オブジェクト（Q カード専用）

| フィールド | 型 | 必須 | 説明 |
|-----------|-----|------|------|
| `label` | `string` | ✅ | 分岐に表示するラベル（例: 「はい」「いいえ」）。空文字列は検証エラー。 |
| `target` | `string \| null` | ✅ | 接続先カードの `id`。未接続の場合は `null`。 |

---

## 4. JSON スキーマ例 / JSON Schema Example

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Card",
  "type": "object",
  "required": ["id", "type", "body"],
  "properties": {
    "id":   { "type": "string", "format": "uuid" },
    "type": { "type": ["string", "null"], "enum": ["Q", "A", null] },
    "body": { "type": "string" },
    "branches": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["label", "target"],
        "properties": {
          "label":  { "type": "string", "minLength": 1 },
          "target": { "type": ["string", "null"] }
        }
      }
    }
  }
}
```

---

## 5. cards.json 構造 / cards.json Structure

`cards.json` はカードの配列を格納する。順序はレンダリングに影響しない（位置は `editor.json` が管理）。

```json
{
  "version": "0.1",
  "cards": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "type": "Q",
      "body": "サーバーは起動していますか？",
      "branches": [
        { "label": "はい", "target": "550e8400-e29b-41d4-a716-446655440001" },
        { "label": "いいえ", "target": "550e8400-e29b-41d4-a716-446655440002" }
      ]
    },
    {
      "id": "550e8400-e29b-41d4-a716-446655440001",
      "type": "A",
      "body": "次のステップに進んでください。"
    },
    {
      "id": "550e8400-e29b-41d4-a716-446655440002",
      "type": "A",
      "body": "サーバーを起動してから再試行してください。"
    }
  ]
}
```

---

## 6. 制約と禁止事項 / Constraints

| 制約 | 説明 |
|------|------|
| A カードから出力不可 | `branches` フィールドを A カードに持たせてはならない |
| A→A / A→Q 接続禁止 | `target` に A カードや Q カードを A カードから指定不可 |
| ID 手動変更禁止 | `id` はシステム生成のみ。ユーザーが編集してはならない |
| 存在しない `target` 禁止 | `target` が指す `id` は同一 `cards.json` 内に存在しなければならない |
| type=null のままエクスポート禁止 | `type` が `null` のカードはエクスポート前に検証エラーとなる |

---

## 7. バージョニング / Versioning

- 本スペックのバージョンは `cards.json` の `version` フィールドに記録する
- Zipline と Pathfinder は `version` フィールドを読み込み、非対応バージョンを警告または拒否する
- フィールド追加は minor bump（`0.1` → `0.2`）、破壊的変更は major bump（`0.x` → `1.0`）

---

## 関連ドキュメント / Related

- [01_DataSpec.md](../../docs/zipline/v1/spec/01_DataSpec.md) – Zipline側のデータ操作ルール
- [03_Validation.md](../../docs/zipline/v1/spec/03_Validation.md) – 検証ロジック
- Pathfinder Runtime Spec（未作成・今後追加予定）
