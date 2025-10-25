# 06_FuturePlans – Expansion and Parking Ideas (v0.1)

---

## 1. 概要 / Overview
この章では、Zipline モジュールの将来的な拡張案や、  
現段階では仕様化していない検討中の要素を整理する。  
いずれも v0.1 では未実装だが、次期バージョン以降の候補として  
ドキュメント化しておく。

This document lists potential future enhancements for the Zipline module.  
These items are **not yet implemented in v0.1**,  
but are recorded as candidates for later releases.

---

## 2. 協働編集 / Collaborative Editing

| 項目 / Item | 説明 / Description |
|--------------|--------------------|
| **目的 / Goal** | 複数の設計者が同時にカードを編集可能にする。 |
| **アプローチ / Approach** | 差分同期（position delta, text patch）方式を採用。 |
| **課題 / Challenges** | 同時書き換え時の競合解決、Undo/Redoの整合性。 |
| **参考 / Notes** | WebSocket同期やCRDT技術の利用を検討。 |

Enable multiple designers to edit cards simultaneously,  
using delta-based or CRDT synchronization to handle conflicts gracefully.

---

## 3. メモ・コメント機能 / Notes and Comments

| 項目 / Item | 説明 / Description |
|--------------|--------------------|
| **目的 / Goal** | 各カードに設計者メモやレビューコメントを添付可能にする。 |
| **保存先 / Storage** | `notes.json` または外部DB。cards.json には混在させない。 |
| **UI案 / UI Concept** | 吹き出し形式のコメントバッジ、サイドパネルで一覧表示。 |

Allow per-card notes or comments without polluting main data files.  
UI may use floating badges and a collapsible notes panel.

---

## 4. 条件付き分岐・論理式 / Conditional Branching

| 項目 / Item | 説明 / Description |
|--------------|--------------------|
| **目的 / Goal** | 分岐に論理条件（if式、変数参照）を付与できるようにする。 |
| **仕様候補 / Proposal** | `condition` フィールドを branch に追加し、Pathfinder 側で評価。 |
| **UI** | 分岐ラベルの右に条件アイコンを表示、クリックで式入力ダイアログ。 |

Add an optional `condition` field to each branch,  
allowing logic-based transitions evaluated by Pathfinder at runtime.

---

## 5. 国際化 / Internationalization (i18n)

| 項目 / Item | 説明 / Description |
|--------------|--------------------|
| **目的 / Goal** | カード本文・分岐ラベルを多言語で管理する。 |
| **方針 / Policy** | 翻訳文字列を外部ファイル（locale）で管理、id参照方式。 |
| **影響範囲 / Impact** | エクスポート・検証ロジック・プレビューUI全体に及ぶ。 |

Introduce multilingual support by mapping text IDs to localized strings,  
maintained in separate locale files.

---

## 6. プレビュー統合 / Integrated Preview Mode

| 項目 / Item | 説明 / Description |
|--------------|--------------------|
| **目的 / Goal** | Zipline 内で Pathfinder 風の進行シミュレーションを可能にする。 |
| **動作 / Behavior** | Q/Aカードを順に辿るミニランタイムを内蔵。 |
| **利点 / Benefit** | エクスポート前にフローの確認・検証ができる。 |

Allow running Pathfinder-like simulation directly inside Zipline,  
to verify flow before export.

---

## 7. UIカスタマイズ / UI Customization

| 項目 / Item | 説明 / Description |
|--------------|--------------------|
| **テーマ / Theme** | カラースキームの自由設定（CSS変数対応）。 |
| **レイアウト / Layout** | パネル位置・ショートカットの再マップ。 |
| **スナップ / Snap Behavior** | 自動配置アルゴリズムやグリッド形状を切替可能に。 |

Allow theme and layout customization through configurable CSS variables  
and remappable shortcuts.

---

## 8. Git統合拡張 / Git Integration Enhancements

| 項目 / Item | 説明 / Description |
|--------------|--------------------|
| **履歴ブラウザ / History Browser** | 変更履歴をUIで可視化（カード単位のdiff）。 |
| **自動コミット / Auto Commit** | 定期的にGitへスナップショット保存。 |
| **レビュー支援 / Review Support** | 変更点にコメントを付けるレビュー画面。 |

Provide built-in history and diff browsing tools for card-level Git tracking.

---

## 9. 外部連携 / External Integrations

| 対象 / Target | 連携内容 / Integration |
|----------------|--------------------------|
| **Pathfinder** | 直接データ送信・即プレビュー連携。 |
| **APIエクスポート** | REST/GraphQL 経由で他ツールに提供。 |
| **バリデーションAPI** | CIツール（例：GitHub Actions）から検証を実行可能に。 |

Integrate Zipline with Pathfinder runtime and external APIs  
for validation and CI/CD workflows.

---

## 10. 研究要素 / Experimental Ideas
- **AI補助編集**：自然言語で「次の質問を提案」など生成補助。  
- **自動整列学習**：配置のクセを学習して自動で美しく並べる。  
- **分岐統計可視化**：全体の分岐数・到達率をヒートマップ表示。

Experimental directions:  
AI-assisted editing (“suggest next question”),  
layout auto-learning,  
and branch statistics visualization.

---

## 11. 実装優先度 / Priority (tentative)

| 優先度 / Priority | 項目 / Item |
|--------------------|-------------|
| ★★★ | 協働編集 / Collaborative editing |
| ★★☆ | メモ・コメント機能 / Notes & Comments |
| ★★☆ | プレビュー統合 / Integrated preview |
| ★☆☆ | 国際化 / i18n |
| ★☆☆ | 条件付き分岐 / Conditional branching |

---

## 12. 関連章 / Related
- [05_GitStructure.md](05_GitStructure.md) – Git運用 / Git Operation  
- [00_Overview.md](00_Overview.md) – コンセプト / Concept Overview  

---

### 注記 / Notes
この章に記載された機能はすべて構想段階であり、  
正式な実装計画や期日は未定。  
アイデアの蓄積・レビュー用ドキュメントとして扱う。

All items here are **conceptual placeholders**.  
They serve as a reference for brainstorming and future design discussions.

---
