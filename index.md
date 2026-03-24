# Zipline Documentation Index (v0.1)

## 概要 / Overview
このフォルダには、Zipline モジュール（Pathfinder-orbital-ring 内）の  
設計ドキュメントが章ごとにまとめられています。  
各ファイルは独立して閲覧できますが、上から順に読むことで  
思想 → 構造 → 挙動 → 運用 → 拡張 の流れが理解できる構成になっています。

This directory contains the Zipline module’s design documentation,  
a subsystem of the *Pathfinder-orbital-ring* project.  
Each file can be read independently, but the sequence provides  
a natural flow: philosophy → structure → behavior → operation → future plans.

---

## 章構成 / Chapter Structure

| No | ファイル名 | 内容概要 / Description |
|----|-------------|----------------|
| 00 | [00_Overview.md](00_Overview.md) | コンセプトと思想 / Concept and philosophy |
| 01 | [01_DataSpec.md](01_DataSpec.md) | カードデータ取り扱い仕様 / Card handling within Zipline |
| 02 | [02_UI_Behavior.md](02_UI_Behavior.md) | UI挙動と編集フロー / User interface behavior and flow |
| 03 | [03_Validation.md](03_Validation.md) | 検証とステータス評価 / Validation and status logic |
| 04 | [04_EditorMeta.md](04_EditorMeta.md) | エディタメタ構造 / Editor meta-data structure |
| 05 | [05_GitStructure.md](05_GitStructure.md) | Git運用ルール / Git structure and commit policy |
| 06 | [06_FuturePlans.md](06_FuturePlans.md) | 将来拡張・駐車場 / Future extensions and parking ideas |

---

## Pathfinder 仕様 / Pathfinder Specification

| ファイル | 内容 |
| --- | --- |
| [Pathfinder_v0.1.md](Pathfinder/specs/Pathfinder_v0.1.md) | 起動オプション・レイアウト・ナビゲーション仕様 |
| [CardSpec_v0.1.md](Pathfinder/specs/CardSpec_v0.1.md) | カード共通スキーマ（`id`, `type`, `body`, `branches`） |

---

## 関連リンク / Related
- **Parent Repository:** Pathfinder-orbital-ring
- **Zipline Spec:** [docs/zipline/v1/spec/](docs/zipline/v1/spec/)
- **Card Spec:** [Pathfinder/specs/CardSpec_v0.1.md](Pathfinder/specs/CardSpec_v0.1.md)
- **Project Wiki:** (準備中 / Coming soon)

---

## 更新履歴 / Revision Log

| Date | Version | Description |
| --- | --- | --- |
| 2025-10 | v0.1 | 初版作成 / Initial draft |
| 2026-03 | v0.1 | Zipline v0.1・Pathfinder v0.1 実装完了 |

---
