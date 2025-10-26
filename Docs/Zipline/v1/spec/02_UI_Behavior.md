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

## 関連章 / Related
- [01_DataSpec.md](01_DataSpec.md) – データ取り扱い / Data handling rules  
- [03_Validation.md](03_Validation.md) – 検証と状態判定 / Validation logic  
- [04_EditorMeta.md](04_EditorMeta.md) – エディタ設定 / Editor metadata

---
