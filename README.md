# Pathfinder-orbital-ring

カードベースのトラブルシューティングシステム。
A card-based troubleshooting authoring and runtime system.

---

## 構成 / Structure

| モジュール | ファイル | 役割 |
| --- | --- | --- |
| **Zipline** | `Zipline/Zipline_v0.1.html` | カード作成エディタ（作るほう） |
| **Pathfinder** | `Pathfinder/Pathfinder_v0.1.html` | カード閲覧ランタイム（見るほう） |
| **CardSpec** | `Pathfinder/specs/CardSpec_v0.1.md` | 共通カードスキーマ定義 |
| **Zipline Spec** | `docs/zipline/v1/spec/` | Zipline 設計仕様書群 |

---

## クイックスタート / Quick Start

### カードを作る（Zipline）

1. `Zipline/Zipline_v0.1.html` をブラウザで開く
2. **＋ Q カード** / **＋ A カード** でカードを追加
3. カードをクリックして右パネルで内容・分岐を編集
4. Q カード下端の **◎** をクリック → 接続先カードをクリックして接続
5. **💾 保存** でブラウザに保存 / **📦 Export** で JSON をダウンロード

### カードを見る（Pathfinder）

| 用途 | URL |
| --- | --- |
| Zipline 保存データを読む | `Pathfinder_v0.1.html?mode=edit` → 「ブラウザから読込」 |
| JSON ファイルを直接指定 | `Pathfinder_v0.1.html?file=data.json` |
| エンドユーザー向け埋め込み | `Pathfinder_v0.1.html?file=data.json`（ロードボタン非表示） |
| デモ確認 | `Pathfinder_v0.1.html`（パラメータなし） |

---

## データフォーマット / Data Format

Zipline が出力し Pathfinder が読むデータ形式：

```json
{
  "meta": { "version": "0.1", "projectId": "...", "projectName": "..." },
  "cards": [
    { "id": "...", "type": "Q", "x": 0, "y": 0,
      "title": "...", "content": "...", "ports": 2, "labels": ["はい", "いいえ"] }
  ],
  "links": [
    { "id": "...", "from": { "cardId": "...", "portIndex": 1 }, "to": { "cardId": "..." } }
  ],
  "viewport": { "scale": 1, "x": 0, "y": 0 }
}
```

詳細は [CardSpec v0.1](Pathfinder/specs/CardSpec_v0.1.md) を参照。

---

## ドキュメント / Documentation

設計仕様書は [index.md](index.md) を参照。

---

## バージョン / Version

| コンポーネント | バージョン |
| --- | --- |
| Zipline | v0.1 |
| Pathfinder | v0.1 |
| CardSpec | v0.1 |
