# 03_Validation – Card Status and Verification Logic (v0.1)

---

## 1. 概要 / Overview
Zipline は編集時にリアルタイムでカードの状態を判定し、  
右上の「ステータス小窓」に表示する。  
また、任意のタイミングで全カードを対象とした検証（Validate）を行い、  
結果を一覧として提示できる。

Zipline performs real-time status evaluation for each card,  
displayed via the *status indicator* at the top-right corner.  
It also supports global validation across all cards,  
showing results in a summary panel.

---

## 2. ステータス評価 / Status Evaluation

### 2.1 評価タイミング / Timing
- 編集確定時（本文入力、分岐追加・削除、接続変更など）  
- 自動保存直後  
- 手動で「検証」ボタンを押した時（全カード再評価）

Evaluations occur after any confirmed edit,  
immediately after auto-save,  
or manually through the “Validate” button.

---

### 2.2 判定基準 / Criteria

| 状態 / Status | 条件（日本語） / Condition (English) |
|----------------|---------------------------------------|
| 🟢 **正常 / Normal** | A: 本文あり・出力なし / Q: 本文あり・分岐全接続済み  |
| 🟡 **未記入 / Empty** | 本文が空、または Q で分岐が 0 個（生成直後を含む） |
| 🔴 **接続不良 / Error** | Q: 分岐があるが未接続が存在する / A: 出力端子を持っている |
| ⚪ **未定義 / Undefined** | type が未設定、または新規生成直後の空っぽカード |

Each card’s visual indicator updates accordingly:  
Green (valid), Yellow (incomplete), Red (broken), White (undefined).

---

### 2.3 表示ルール / Display Behavior
- ステータス小窓は常時表示。  
- マウスホバーで詳細（例：「分岐2件が未接続」）をツールチップ表示。  
- クリックでサイドペインに詳細を展開（ログ形式）。

The indicator is always visible.  
Hovering shows tooltips (e.g., “2 unconnected branches”).  
Clicking opens a detailed log in the side panel.

---

## 3. グローバル検証 / Global Validation

### 3.1 検証項目 / Validation Items

| 検証項目 / Item | 検出条件 / Detection Rule |
|------------------|---------------------------|
| **未接続分岐** / Unconnected branch | Qカードの分岐で `target` が空 |
| **到達不能ノード** / Unreachable node | 開始カードから到達しないカード |
| **誤接続** / Invalid connection | Aカードから出力接続がある |
| **壊れた参照** / Broken reference | 分岐の `target` に存在しない id |
| **未定義カード** / Undefined card | type 未設定または body/type 両方空 |

---

### 3.2 出力形式 / Output Presentation
- 検証結果は右サイドペインにリスト表示。  
- 各項目をクリックすると対象カードを中央へフォーカス。  
- 「修正提案」ボタンを用意（例：「空分岐を削除」「未定義カードを削除」）。

Results appear in a right-hand panel.  
Clicking an entry focuses the card in the canvas.  
A “suggest fix” button offers quick actions (e.g., delete empty branch).

---

### 3.3 検証レポート / Validation Report
- 検証結果は `validate.json` として出力可能（自動生成）。  
- Git 管理下ではコミット不要（キャッシュ扱い）。  
- エクスポート時には `export.zip` に含めることができる。

Validation results can be exported as `validate.json`.  
It’s treated as a temporary cache and excluded from Git commits.  
However, it may be bundled in exported archives.

---

## 4. ステータス小窓と検証の関係 / Relation between Status and Validation
- ステータス小窓：カード単体のリアルタイム評価（軽量）  
- 検証：全体を対象にした完全スキャン（重い）

| 項目 / Item | ステータス小窓 / Status Indicator | 検証 / Global Validation |
|--------------|----------------------------------|---------------------------|
| 対象範囲 | 単一カード | 全カード |
| タイミング | 編集時リアルタイム | 任意トリガ |
| 表示位置 | カード右上 | サイドペイン |
| 出力ファイル | なし | validate.json |
| 主目的 | 操作中のフィードバック | 出力前の品質確認 |

---

## 5. 例示 / Example Scenarios

| 状況 / Scenario | ステータス | 検証結果例 / Validation Output Example |
|------------------|------------|---------------------------------------|
| 生成直後の空カード | ⚪ 未定義 | `"id": "c01", "error": "type undefined"` |
| 本文入力済み・分岐なし(Q) | 🟡 未記入 | `"warning": "No branches defined"` |
| 分岐2件中1件未接続 | 🔴 接続不良 | `"error": "1 branch unconnected"` |
| すべて接続済み・本文あり | 🟢 正常 | `"ok": true"` |

---

## 6. 今後の拡張 / Future Enhancements
- エラー項目に「自動修正」提案を付与（例：孤立カードの削除）。  
- 複数エラーがある場合は優先度順に並べる。  
- Pathfinder 側ログフォーマットとの互換を検討（共通バリデーション化）。

Planned extensions include automated fix suggestions,  
error prioritization, and format alignment with Pathfinder’s validation logs.

---

## 関連章 / Related
- [02_UI_Behavior.md](02_UI_Behavior.md) – UI挙動と編集操作 / UI Behavior  
- [04_EditorMeta.md](04_EditorMeta.md) – エディタメタ情報 / Editor Meta Data  
- [05_GitStructure.md](05_GitStructure.md) – Git運用構造 / Git Commit Policy

---
