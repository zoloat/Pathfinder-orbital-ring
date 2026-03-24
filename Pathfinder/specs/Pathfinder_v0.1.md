# Pathfinder v0.1 – Runtime Viewer Specification

---

## 1. 概要 / Overview

Pathfinder は Zipline が生成したカードデータを読み込み、
エンドユーザーが Q/A フローを対話的に辿るための**閲覧ランタイム**です。

Pathfinder is a read-only runtime viewer that loads card data produced by Zipline
and lets end-users interactively navigate Q/A flows.

---

## 2. 起動オプション / Startup Options

URL パラメータで動作モードを切り替えます。

| パラメータ | 値 | 動作 |
| --- | --- | --- |
| なし | — | デモデータで起動。読込ボタン非表示。 |
| `?file=path` | JSON ファイルパス / URL | 指定 JSON を fetch して自動読込。読込ボタン非表示。 |
| `?mode=edit` | `edit` | 編集補助モード。読込ボタン表示。ブラウザ保存データ優先。 |

### 使用例 / Examples

```
# エンドユーザー向け（ロードボタンなし）
Pathfinder_v0.1.html?file=data.json

# 同じオリジンのサブフォルダ内ファイル
Pathfinder_v0.1.html?file=cases/server_trouble.json

# Zipline での作業確認（ブラウザ保存データを読む）
Pathfinder_v0.1.html?mode=edit
```

### `?file=` の注意 / Notes on `?file=`

- 同一オリジン（同ドメイン・同フォルダ）の JSON のみ fetch 可能。
- ローカルファイル（`file://` プロトコル）では CORS 制限により使用不可。
  ローカルで試す場合は `?mode=edit` → 「JSON を開く」で読み込む。
- fetch 失敗時はステータスバーにエラーを表示し、デモデータにフォールバックしない。

---

## 3. 表示モード / Display Modes

### 3.1 読み取り専用モード（デフォルト）

- 読込ボタン（「デモを見る」「ブラウザから読込」「JSON を開く」）は非表示
- リセットボタンのみ表示
- `?file=` 指定時は自動でデータを読み込み、即座にナビゲーション開始

### 3.2 編集補助モード（`?mode=edit`）

- 読込ボタンをすべて表示
- 起動時に `localStorage['zipline:auto']` を確認し、あれば自動読込
- Zipline と組み合わせて「作る → 確認」のサイクルに使用

---

## 4. レイアウト / Layout

| 領域 | 説明 |
| --- | --- |
| **トップバー** | ブランド名・読込ボタン（モード依存）・リセット |
| **左パネル（ナビ）** | 現在カードの内容・分岐ボタン・経路履歴・操作ボタン |
| **キャンバス** | Konva.js による全カードの俯瞰ビュー |

モバイル（≤640px）ではキャンバスを非表示にし、左パネルを全幅表示。

---

## 5. ナビゲーション / Navigation

- 現在のカードは青いグロー（ハイライト）で表示
- 次の選択肢カードは薄い青でハイライト
- 訪問済みカードは 65% 不透明度
- 未到達カードは 22% 不透明度
- 左パネルの分岐ボタンをクリック、またはキャンバス上の次カードをクリックして進む
- 「◀ 戻る」で直前のカードに戻る（履歴スタック方式）
- 「↺ 最初から」でフローをリセット

---

## 6. カメラ / Camera

| 操作 | 方法 |
| --- | --- |
| パン | Space + ドラッグ、または背景ドラッグ |
| ズーム | Ctrl + ホイール |
| カード中心移動 | カード選択時に自動アニメーション |
| 全体表示 | データ読込時に自動 fit-all |

ズーム範囲：10% ～ 400%

---

## 7. データ互換性 / Data Compatibility

読み込む JSON は [CardSpec v0.1](CardSpec_v0.1.md) に準拠した形式。
Zipline v0.1 の Export 出力をそのまま読み込み可能。

---

## 関連 / Related

- [CardSpec v0.1](CardSpec_v0.1.md) – カード共通スキーマ
- [Zipline Spec](../../docs/zipline/v1/spec/00_Overview.md) – エディタ仕様
