# 04_EditorMeta – Editor Metadata Structure (v0.1)

---

## 1. 概要 / Overview
この章では、Zipline エディタが保持する「表示・操作状態（Editor Meta）」の構造と役割を定義する。  
カード本体（cards.json）は内容を、エディタメタ（editor.json）は**視覚的な状態と作業環境**を表す。  
この二つを分離することで、カードデータを汚さずにUI設定を柔軟に変えられる。

This document defines the structure and role of **editor metadata** used in Zipline.  
While `cards.json` stores logical content, `editor.json` captures the **visual and operational state**.  
This separation keeps the data pure while allowing flexible UI configurations.

---

## 2. 保存対象 / Stored Elements

| 要素 / Element | 内容 / Description | 備考 / Notes |
|----------------|--------------------|---------------|
| **canvas** | キャンバス全体のズーム倍率・スクロール位置 | 全体ビューの状態 |
| **cards** | 各カードの位置座標・開閉状態・選択状態 | 見た目の配置情報 |
| **grid** | スナップ強度・グリッド間隔・可視状態 | 視覚補助設定 |
| **theme** | 現在のテーマ（light/dark/system） | 表示スタイル |
| **panel** | 編集パネルや検証パネルの開閉状態 | UI構成要素の開閉 |
| **minimap** | ミニマップの表示・位置・ズーム率 | 全体ナビゲーション用 |
| **autosave** | 自動保存の間隔・ON/OFF | 設定値 |
| **timestamp** | 最終更新時刻 | 差分検出・比較用 |

---

## 3. カード座標情報 / Card Layout Metadata
Zipline ではカードの内容と位置を分離して保存する。  
各カードの `id` をキーとして、座標や状態を記録する。

In Zipline, the layout is stored separately from card content.  
Each card’s metadata is keyed by its unique `id`.

| 項目 / Field | 内容 / Description |
|---------------|--------------------|
| `x`, `y` | キャンバス上の位置（ピクセル単位） |
| `isOpen` | 編集パネルを展開中かどうか |
| `isSelected` | 選択中状態（複数選択時に利用） |
| `snapLevel` | 個別にスナップを上書きする場合（任意） |

> 例：`"cards": { "q001": { "x":420, "y":260, "isOpen":false } }`

---

## 4. グリッドとスナップ / Grid & Snap Settings

| 設定項目 / Setting | 意味 / Meaning | 初期値 / Default |
|---------------------|----------------|------------------|
| `snapStrength` | スナップの強さ（strong / weak / off） | `strong` |
| `gridVisible` | グリッド表示のON/OFF | `true` |
| `gridSize` | 格子間隔（px） | `40` |
| `autoAlign` | 自動整列のON/OFF | `true` |

These settings determine the grid’s visibility and snapping strength.  
They can be adjusted in preferences and saved per project.

---

## 5. キャンバスビュー設定 / Canvas View Settings

| 項目 / Field | 内容 / Description | 例 / Example |
|---------------|--------------------|---------------|
| `zoom` | ズーム倍率 | `1.0`（=100%） |
| `scrollX`, `scrollY` | 現在のスクロール位置 | `120, 80` |
| `showMinimap` | ミニマップの表示 | `true` |
| `theme` | カラーテーマ | `"dark"` |

Canvas view metadata helps restore the exact working state when reopening a project.

---

## 6. 自動保存設定 / Auto-Save Settings

| 項目 / Field | 説明 / Description |
|---------------|--------------------|
| `enabled` | 自動保存を有効化 |
| `interval` | 保存間隔（秒） |
| `mode` | `onEdit`（編集ごと） / `periodic`（定期） |
| `lastSave` | 最終保存の時刻 |

Auto-save ensures minimal data loss and consistent save intervals.  
It can operate either *on edit* or *periodically*.

---

## 7. メタデータの運用 / Operation Policy
- **Git管理対象**：editor.json は原則追跡対象（個人用なら除外可）。  
- **他端末での再現**：同じ meta ファイルを開くと、UI状態が復元される。  
- **マージ戦略**：複数人が編集する場合、cards.json と同時変更しない方が安全。  
- **エクスポート時**：export.zip では任意同梱（オプション扱い）。

Git tracking of `editor.json` is optional.  
It allows state restoration across devices, but merging conflicts  
can occur if multiple users modify layout simultaneously.  
It’s optional in exported archives.

---

## 8. 今後の拡張 / Future Enhancements
- 各ユーザー別のローカル設定ファイル（user.json）を分離予定。  
- UIテーマをカスタムCSSで上書きできる拡張性。  
- collaborative editing 時に同期できる軽量差分（position delta 方式）の採用を検討。

Planned extensions include per-user local configs,  
custom CSS theme overrides,  
and lightweight deltas for future collaborative editing.

---

## 関連章 / Related
- [02_UI_Behavior.md](02_UI_Behavior.md) – UI動作 / UI Behavior  
- [05_GitStructure.md](05_GitStructure.md) – Git構成と管理 / Git Structure  
- [06_FuturePlans.md](06_FuturePlans.md) – 拡張計画 / Future Extensions

---
