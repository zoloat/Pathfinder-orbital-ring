# 00_Overview – Zipline Concept & Philosophy (v0.1)

---

## 概要 / Overview
Zipline は、**Pathfinder-orbital-ring** プロジェクト内における  
「質問カード（Q/A構造）を設計・管理するためのエディタモジュール」です。  
利用者側の UI「Pathfinder」が実行時に参照するカードデータを、  
設計者が視覚的かつ安全に構築できることを目的としています。

Zipline is the **authoring and management module** within the *Pathfinder-orbital-ring* project.  
It enables creators to visually and safely design the Q/A card structures  
that the Pathfinder user interface will later consume and execute.

---

## 開発目的 / Design Goals

| 日本語 | English |
|--------|----------|
| **1. 視覚的設計**：思考の流れをカードと線で表現し、構造を直感的に理解できるようにする。 | **1. Visual design:** Represent logical flow using cards and links, allowing intuitive comprehension. |
| **2. 安全操作**：誤接続や未記入を自動検出し、常に健全なデータを保つ。 | **2. Safe operations:** Detect missing connections or empty content to ensure structural integrity. |
| **3. 冗長排除**：カードデータは本文と分岐のみを保持し、余計な情報を持たない。 | **3. Minimal data:** Keep only essential fields (body & branches) to prevent redundancy. |
| **4. 柔軟な再利用**：カードを複製・再配置しても ID 衝突や参照破壊が起きない。 | **4. Flexible reuse:** Duplication and repositioning are conflict-free and self-contained. |
| **5. 共通仕様**：Pathfinder 側と同一形式のデータを共有できる。 | **5. Shared spec:** Maintain compatibility with Pathfinder’s runtime card format. |

---

## コアコンセプト / Core Concepts

### 🧩 カードとノード / Cards & Nodes
Zipline の世界は **Q（Question）カード**と **A（Answer）カード**で構成されます。  
Q は分岐ノード、A は終端ノードとして振る舞い、  
カード同士を線で接続することで「思考の地図」が形成されます。

The Zipline world consists of **Q (Question)** and **A (Answer)** cards.  
Q acts as a branching node, and A as a terminal node.  
By connecting cards via lines, the creator builds a visual “map of reasoning.”

---

### ⚙️ 編集の流れ / Editing Flow
Zipline の基本操作は「生成 → 配置 → 編集 → 接続」。  
カードは最初、空っぽの状態で生まれ、配置後に内容を与えます。  
分岐を開く際はカバーを解除して編集する「安全弁」設計を採用しています。

The core workflow is “Create → Place → Edit → Connect.”  
Cards are born empty, positioned first, and filled afterward.  
Branch editing uses a covered toggle mechanism — a safety lock metaphorically akin to a missile switch.

---

### 🧠 設計思想 / Design Philosophy
Zipline は「**書く前に考える**」を支援するためのツールです。  
操作はシンプルでも、結果は論理的な構造として再現可能であるべきと考えています。  
また、開発思想として「データを汚さず、UIで演出する」を徹底します。

Zipline aims to support **thinking before writing.**  
Though interactions remain simple, every action must map to a logical structure.  
Its guiding principle: *keep data pure; let UI provide the drama.*

---

## Pathfinder との関係 / Relation to Pathfinder

| 日本語 | English |
|--------|----------|
| Zipline は「作る側」、Pathfinder は「使う側」。 | Zipline represents the *creator side*, Pathfinder the *user side*. |
| Zipline が生成するカードデータを、Pathfinder が実行時に読み取る。 | Pathfinder reads and executes the card data produced by Zipline. |
| 両者は共通のデータ仕様を共有し、双方向変換が可能。 | Both share a common schema, enabling two-way conversion. |
| 今後は Zipline から直接 Pathfinder セッションをプレビューできる構想。 | A future goal is to preview Pathfinder sessions directly within Zipline. |

---

## 今後の展望 / Outlook
将来的には、複数人が同時にカード設計を行える「協働編集」や、  
カードごとのコメント・メモ機能、国際化対応などを予定しています。

Future milestones include real-time collaborative editing,  
per-card annotations, and full internationalization.

---

## 関連章 / Related Chapters
- [01_DataSpec.md](01_DataSpec.md) – カードデータ取り扱い / Card data handling  
- [02_UI_Behavior.md](02_UI_Behavior.md) – UI挙動と操作設計 / UI behavior and operation flow

---
