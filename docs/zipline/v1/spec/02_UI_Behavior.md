# 02_UI_Behavior – User Interface and Operation Flow (v0.1)

---

## 1. 基本構成 / Base Layout
Zipline の編集画面は、**キャンバス＋ツールバー＋編集パネル**の三層構造で構成される。

The Zipline workspace consists of three primary layers:  
**Canvas**, **Toolbar**, and **Editor Panel**.

| 区分 / Section | 機能 / Function |
|----------------|----------------|
| **キャンバス / Canvas** | カードの配置・接続を行うメイン領域。ズーム・パン対応。 |
| **ツールバー / Toolbar** | 右下に固定された「＋」ボタンなど、作業の起点となる操作群。 |
| **編集パネル / Editor Panel** | ペンアイコンを押した時に開く。本文・分岐を編集する専用領域。 |

---

## 2. カードのライフサイクル / Card Lifecycle

### 2.1 生成 / Create
- 右下の「＋」ボタンで **空っぽカード** を生成。  
- 初期状態：`type` 未定義、本文空、分岐なし。  
- 生成時は自動で軽くハイライトされる（配置中を示す）。

A new *blank card* is created by pressing the "+" button in the lower-right corner.  
Initially: type undefined, empty body, and no branches.  
The card briefly glows to indicate creation-in-progress.

---

### 2.2 配置 / Place
- カードをドラッグしてキャンバス上に配置。  
- グリッドスナップが有効（強スナップが初期設定）。  
- 離した位置で確定。隣接カードと自動で整列ラインを引く。

Drag the card onto the canvas.  
Strong grid snapping is enabled by default.  
When released, it aligns with nearby cards for visual consistency.

---

### 2.3 編集 / Edit
- ペンアイコンを押すと編集パネルが開く。  
- `type` の設定、本文入力、分岐の追加が可能。  
- 編集完了で自動保存（Undo/Redo対応）。

Click the pencil icon to open the editor panel.  
You can set the type, edit body text, and add branches.  
Changes auto-save and support undo/redo.

---

## 3. 分岐操作 / Branch Operations

### 3.1 解錠 / Unlock
- Qカードの分岐セクションは初期状態でロックされている。  
- カバー（ミサイルスイッチ）を開くと分岐入力が解放される。  
- 開いた状態では、端子が有効化され接続操作が可能になる。

Each Q card’s branch section starts locked.  
Opening its safety cover (like a missile switch) unlocks the branch input  
and activates the output terminals for connection.

---

### 3.2 分岐追加 / Add Branch
- 「＋Branch」ボタンで分岐ラベル欄を追加（例：「はい」「いいえ」）。  
- 追加した分岐ごとに小さな端子が生成される。  
- ラベル欄はインライン編集できる。

Press “+ Branch” to add a new branch label (e.g., “Yes”, “No”).  
Each branch generates a small terminal.  
Labels can be edited inline.

---

### 3.3 接続 / Connect
- 分岐端子からドラッグして他カードへ線を引く。  
- 線を離すと `target` が設定され接続が確定。  
- 接続線は自動ルーティングで交差を避ける。  
- Aカードからの出力端子は存在しない。

Drag from a branch terminal to another card to connect.  
Releasing sets the target and finalizes the link.  
Connections auto-route to minimize crossing lines.  
A cards have no output terminals.

---

### 3.4 切断 / Disconnect
- 線をクリックして削除するか、右クリックで「接続解除」。  
- 対応する `target` が空に戻る。  
- ステータス小窓が即時「接続不良（赤）」に変化。

Click a link to delete, or right-click → “Disconnect.”  
The related target field becomes empty, and the status turns red immediately.

---

## 4. ステータス小窓 / Status Indicator
- カード右上に表示。常時1つだけの小インジケータ。  
- 色とアイコンで状態を即座に把握できる。

| 状態 / Status | 表示色 / Color | 意味 / Meaning |
|----------------|----------------|----------------|
| 🟢 正常 / Normal | Green | 本文あり・接続良好 |
| 🟡 未記入 / Empty | Yellow | 本文または分岐が空 |
| 🔴 接続不良 / Error | Red | 分岐未接続またはA出力誤り |

Indicator appears at the top-right of each card,  
showing the real-time validation result.

---

## 5. 操作体系 / Interaction Model

| 操作 / Action | 方法 / Method |
|----------------|----------------|
| パン / Pan | スペース＋ドラッグ または 中クリック |
| ズーム / Zoom | Ctrl＋スクロール or ピンチ操作 |
| 複製 / Duplicate | Ctrl＋D または右クリックメニュー |
| 削除 / Delete | Deleteキーまたは右クリックメニュー |
| 編集モード / Edit Mode | ペンアイコンを押す |
| 接続モード / Connect Mode | 分岐を解錠してドラッグ |
| 取り消し / Undo | Ctrl＋Z |
| やり直し / Redo | Ctrl＋Y |

---

## 6. 表示制御 / Display Controls
- **スナップ強度**：強（初期）／中／オフを設定可能。  
- **ミニマップ**：全体俯瞰用の小ウィンドウを右下に表示。  
- **ズーム範囲**：25%〜200%。  
- **テーマ**：ライト／ダーク両対応。  
- **カード整列**：選択複数を「整列」ボタンで縦・横に自動整理。

Snap strength, minimap visibility, zoom range (25–200%),  
and theme toggles (light/dark) are available.  
Multiple cards can be aligned vertically or horizontally.

---

## 7. 検証モード / Validation Mode
- 上部メニュー「検証」ボタンで全カードをスキャン。  
- 結果をサイドペインに一覧表示。クリックで該当カードへジャンプ。  
- 検証ロジックは [03_Validation.md](03_Validation.md) に定義。

Use the top “Validate” button to scan all cards.  
Results are listed in a side panel; clicking an entry focuses the card.  
Logic details are in [03_Validation.md](03_Validation.md).

---

## 8. 操作フィードバック / Feedback Design
- 操作のたびに軽いハイライト（光る／揺れるなど）。  
- 編集確定時は下部トーストで「保存しました」。  
- 複製時は一瞬グレーアウトして「コピー作成中」を示す。

Each operation provides subtle visual feedback:  
glows, motion cues, or a “Saved” toast message at the bottom.  
Duplication briefly greys out the card to show action in progress.

---

## 9. 型変換確認ダイアログ / Type Conversion Confirmation Dialog

### 9.1 Q → A 変換時

Q カードを A カードに変換すると、すべての分岐（branches）が削除される。
この操作は不可逆なため、確認ダイアログを必ず表示する。

| 要素 | 内容 |
|------|------|
| **タイトル** | 「分岐を削除しますか？」 |
| **本文** | 「Q → A に変更すると、このカードの分岐がすべて削除されます。本文は保持されます。」 |
| **ボタン（確定）** | 「変更する」（破壊的アクション、赤系ボタン） |
| **ボタン（キャンセル）** | 「キャンセル」（グレー、デフォルトフォーカス） |
| **キャンセル時の挙動** | type は変更されず、ダイアログを閉じる。カードは Q のまま保持。 |
| **確定時の挙動** | branches を空配列に設定 → type を `"A"` に変更 → 自動保存 → Undo 対象に登録 |

When converting a Q card to A, a confirmation dialog must be shown before deleting branches.
Cancelling leaves the card unchanged. Confirming deletes all branches and sets type to `"A"`.

### 9.2 A → Q 変換時

A カードを Q に変換する場合は分岐が追加されるだけで**破壊操作はない**ため、確認ダイアログは不要。
type を `"Q"` に変更し、空の branches 配列を初期化するだけでよい。

---

## 10. Undo / Redo 仕様 / Undo & Redo Specification

### 10.1 対象操作（Undo スタックに積む操作）

| 操作 | Undo 後の状態 |
|------|--------------|
| カード作成 | カードを削除 |
| カード削除 | カードを復元（接続も含む） |
| カード移動 | 移動前の座標に戻す |
| 本文編集 | 編集前のテキストに戻す |
| type 変更（Q→A） | A→Q に戻し、削除された branches を復元 |
| type 変更（A→Q） | Q→A に戻し、空の branches を削除 |
| 分岐追加 | 分岐を削除 |
| 分岐削除 | 分岐を復元（target も含む） |
| 接続設定 | target を `null` に戻す |
| 接続解除 | target を元の値に戻す |
| 複製 | 複製されたカードを削除 |

### 10.2 対象外操作（Undo 不可）

- キャンバスのパン・ズーム（UI状態のため）
- テーマ変更
- スナップ設定変更
- プロジェクト名の変更（meta.json の操作）

### 10.3 スタック仕様

| 項目 | 値 |
|------|----|
| スタック深さ上限 | 100 操作 |
| スコープ | グローバル（カード横断） |
| 自動保存との関係 | 自動保存は Undo スタックをリセットしない |
| セッション持続性 | セッション内のみ。再起動時にスタックはクリアされる |

Undo/Redo operates globally (not per-card) with a stack depth of 100.
Auto-save does not clear the stack. The stack resets on session start.

---

## 関連章 / Related
- [01_DataSpec.md](01_DataSpec.md) – データ取り扱い / Data handling rules
- [03_Validation.md](03_Validation.md) – 検証と状態判定 / Validation logic
- [04_EditorMeta.md](04_EditorMeta.md) – エディタ設定 / Editor metadata

---
