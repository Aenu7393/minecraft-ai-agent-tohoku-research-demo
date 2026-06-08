# minecraft-ai-agent-tohoku-research-demo
# Minecraft AI Agent Research Demo

## Overview

This repository is a public demo version of my research project on an autonomous AI agent in Minecraft.

The goal of this project is to build an agent that can understand the current game situation, decompose a high-level goal into executable subgoals, execute actions, store experiences, and recover from failures.

This project focuses not only on using an LLM for planning, but also on designing a memory-based agent architecture inspired by human memory systems.

## Research Motivation

Large Language Models can generate high-level plans, but they often struggle to ground those plans in the actual environment.

For example, even if an LLM can generate a plan such as:

```text
collect wood -> craft planks -> craft sticks -> craft pickaxe
```

the agent still needs to understand:

* what items it currently has
* what blocks are nearby
* whether the current subgoal is executable
* why an action failed
* how to recover from the failure
* which past experiences can be reused

To address this problem, this project aims to combine LLM-based planning with memory, state representation, and failure recovery mechanisms.

## Key Idea

This project is inspired by the structure and functions of human memory.

Human memory is not a single storage system. It consists of multiple types of memory, such as short-term memory, working memory, long-term memory, episodic memory, semantic memory, and procedural memory.

In this project, I apply this idea to an autonomous Minecraft agent.

The agent uses different types of memory for different purposes:

* **Working Memory**
  Stores the current goal, current subgoal, current state, and recent execution context.

* **Episodic Memory**
  Stores past experiences such as successful actions, failed actions, and the context in which they occurred.

* **Semantic Memory**
  Stores general knowledge such as crafting recipes, item relationships, and task dependencies.

* **Procedural Memory**
  Stores executable skills and action procedures, such as collecting wood, crafting items, mining blocks, and placing blocks.

* **Repair Memory**
  Stores past failure cases and recovery strategies so that the agent can reuse them when similar failures occur.

By separating memory into multiple roles, the agent can make decisions more structurally instead of relying only on direct LLM output.

## Current Approach

In the earlier version of this project, the agent used Mineflayer to obtain structured state information from Minecraft.

For example, the agent could obtain information such as:

* inventory
* position
* nearby blocks
* health
* current task
* execution result

This structured state was used as a state vector for planning, execution, and failure recovery.

However, Mineflayer-based state representation has limitations. It can represent symbolic information, but it does not fully capture the visual situation of the game world.

For example, it may be difficult to understand the surrounding environment, spatial layout, terrain, obstacles, and object relationships only from symbolic state values.

Therefore, I am currently extending the system to use Minecraft screen images as part of the state representation.

## Visual State Representation

The current development direction is to obtain visual information from Minecraft screenshots and convert it into a state vector.

This visual state vector can be used for:

* understanding the current environment more accurately
* supporting lightweight decision making
* detecting environmental changes
* improving subgoal execution
* helping the agent reflect on failures
* modifying subgoals when the current plan does not work

Instead of relying only on text-based or symbolic state information, the agent aims to use visual information to better understand the game situation.

## Non-Associative Learning

I am also interested in applying concepts from non-associative learning to the agent.

In particular, I am considering the use of **habituation**.

In human learning, habituation refers to the process of gradually reducing attention or response to repeated stimuli that are not important.

I want to apply this idea to visual state understanding.

For example, if the agent repeatedly observes the same unimportant visual information, it should not focus on it every time. Instead, the agent should gradually reduce attention to familiar and unimportant information, and focus more on novel, uncertain, or task-relevant parts of the environment.

This idea may help the agent:

* reduce unnecessary observation cost
* focus on important visual changes
* improve failure reflection
* decide where to observe more carefully
* update subgoals based on meaningful environmental changes

## Architecture

```text
User Goal
   ↓
LLM Planner
   ↓
Subgoal Queue
   ↓
Meta Controller
   ↓
Executor
   ↓
Minecraft Agent
```

The agent also uses memory and state representation during planning, execution, and failure recovery.

```text
State Observer
   ├── Symbolic State from Mineflayer
   └── Visual State from Minecraft Screenshots

Memory System
   ├── Working Memory
   ├── Episodic Memory
   ├── Semantic Memory
   ├── Procedural Memory
   └── Repair Memory
```

## Example Task

Input goal:

```text
Craft a stone pickaxe
```

Example subgoal sequence:

```text
find_tree
chop_tree
craft_planks
craft_crafting_table
craft_sticks
craft_wooden_pickaxe
mine_cobblestone
craft_stone_pickaxe
```

If one of these subgoals fails, the agent checks the current state and past failure cases, then tries to generate or reuse a repair strategy.

## Features

* LLM-based goal decomposition
* Subgoal-based task execution
* State observation using Mineflayer
* Visual state representation using Minecraft screenshots
* Memory-inspired agent architecture
* Episodic memory for past execution history
* Procedural memory for reusable skills
* Repair memory for failure recovery
* Meta-controller for deciding the next action mode
* Future extension based on habituation in non-associative learning

## My Role

I designed and implemented the overall architecture of the agent system.

My main responsibilities include:

* designing the memory-based agent architecture
* implementing the planner and executor
* implementing state observation
* designing subgoal-based task execution
* implementing failure recovery mechanisms
* experimenting with visual state representation
* exploring how human memory and learning mechanisms can be applied to autonomous agents

## Tech Stack

* JavaScript / Node.js
* Mineflayer
* Minecraft
* LLM API
* JSON-based memory system
* Visual state representation




## 概要

このリポジトリは、Minecraft内で動作する自律AIエージェントに関する研究プロジェクトの公開用デモ版です。

本プロジェクトの目的は、エージェントが現在のゲーム状況を理解し、高レベルな目標を実行可能なサブゴールへ分解し、行動を実行し、経験を記憶し、失敗から回復できる仕組みを構築することです。

本研究では、LLMを単に行動計画の生成に使うだけでなく、人間の記憶システムに着想を得た、記憶ベースのエージェントアーキテクチャの設計に重点を置いています。

## 研究の背景・目的

大規模言語モデルは高レベルな計画を生成することができますが、その計画を実際の環境状況に結びつけることは得意ではありません。

例えば、LLMが次のような計画を生成できたとしても、

```text
木を集める -> 板材を作る -> 棒を作る -> ツルハシを作る
```

実際にエージェントが行動するためには、以下のような情報を理解する必要があります。

* 現在どのアイテムを持っているか
* 周囲にどのブロックがあるか
* 現在のサブゴールは実行可能か
* なぜ行動が失敗したのか
* 失敗からどのように回復するか
* 過去の経験を再利用できるか

この問題に対処するため、本プロジェクトではLLMによる計画生成に加えて、記憶、状態表現、失敗回復の仕組みを組み合わせたエージェントの構築を目指しています。

## 主要なアイデア

本プロジェクトは、人間の記憶の種類や機能に着想を得ています。

人間の記憶は単一の記憶領域ではなく、短期記憶、作業記憶、長期記憶、エピソード記憶、意味記憶、手続き記憶など、複数の役割を持つ記憶から構成されています。

本プロジェクトでは、この考え方をMinecraft内の自律エージェントに応用しています。

エージェントは、目的に応じて異なる記憶を使い分けます。

* **作業記憶**
  現在の目標、現在のサブゴール、現在の状態、直近の実行状況を保持します。

* **エピソード記憶**
  過去の成功した行動、失敗した行動、そのときの状況などの経験を保存します。

* **意味記憶**
  クラフトレシピ、アイテム同士の関係、タスクの依存関係などの一般的な知識を保存します。

* **手続き記憶**
  木を集める、アイテムをクラフトする、ブロックを採掘する、ブロックを設置するなど、実行可能なスキルや行動手順を保存します。

* **修復記憶**
  過去の失敗事例とその回復方法を保存し、似た失敗が発生したときに再利用します。

このように記憶を役割ごとに分けることで、エージェントはLLMの出力に直接依存するだけでなく、より構造的に意思決定を行えるようになります。

## 現在のアプローチ

これまでのバージョンでは、Mineflayerを用いてMinecraftから構造化された状態情報を取得していました。

例えば、エージェントは以下のような情報を取得できます。

* インベントリ
* 位置情報
* 周囲のブロック
* 体力
* 現在のタスク
* 実行結果

このような構造化された状態情報を、計画、実行、失敗回復のための状態ベクトルとして利用していました。

しかし、Mineflayerによる状態表現には限界があります。
記号的な情報は取得できますが、ゲーム画面全体の視覚的な状況を十分に表現することは難しいです。

例えば、周囲の環境、空間的な配置、地形、障害物、オブジェクト同士の関係などは、記号的な状態値だけでは十分に理解できない場合があります。

そのため現在は、Minecraftの画面画像を状態表現の一部として利用する方向に拡張しています。

## 画像ベースの状態表現

現在取り組んでいる方向性は、Minecraftのスクリーンショットから視覚情報を取得し、それを状態ベクトルに変換することです。

この画像ベースの状態ベクトルは、以下のような用途に利用することを想定しています。

* 現在の環境をより詳細に理解する
* 軽い判断を支援する
* 環境の変化を検出する
* サブゴールの実行を改善する
* 失敗時の反省を支援する
* 現在の計画がうまくいかない場合にサブゴールを修正する

テキストベースや記号ベースの状態情報だけに依存するのではなく、視覚情報を取り入れることで、エージェントがゲーム内の状況をより正確に理解できるようにすることを目指しています。

## 非連合学習の応用

本研究では、非連合学習の考え方をエージェントに応用することにも関心があります。

特に、**馴化** の仕組みを取り入れることを検討しています。

人間の学習において、馴化とは、重要でない刺激が繰り返し与えられたときに、その刺激への反応や注意が徐々に小さくなる現象です。

この考え方を、画像ベースの状態理解に応用したいと考えています。

例えば、エージェントが同じような重要でない視覚情報を繰り返し観測した場合、それに毎回強く注目する必要はありません。
その代わりに、見慣れた重要度の低い情報への注意を下げ、新規性が高い部分、不確実な部分、タスクに関係する部分へより注目できるようにします。

この仕組みにより、以下のような効果が期待できます。

* 不要な観測コストを減らす
* 重要な視覚的変化に注目する
* 失敗時の反省を改善する
* どこをより詳しく観察すべきか判断する
* 意味のある環境変化に基づいてサブゴールを更新する

## アーキテクチャ

```text
ユーザーの目標
   ↓
LLM Planner
   ↓
Subgoal Queue
   ↓
Meta Controller
   ↓
Executor
   ↓
Minecraft Agent
```

エージェントは、計画、実行、失敗回復の各段階で、記憶と状態表現を利用します。

```text
State Observer
   ├── Mineflayerから取得した記号的状態
   └── Minecraftスクリーンショットから取得した視覚的状態

Memory System
   ├── Working Memory
   ├── Episodic Memory
   ├── Semantic Memory
   ├── Procedural Memory
   └── Repair Memory
```

## タスク例

入力目標：

```text
石のツルハシを作る
```

サブゴール列の例：

```text
find_tree
chop_tree
craft_planks
craft_crafting_table
craft_sticks
craft_wooden_pickaxe
mine_cobblestone
craft_stone_pickaxe
```

もしこれらのサブゴールの途中で失敗した場合、エージェントは現在の状態と過去の失敗事例を確認し、修復戦略を生成または再利用します。

## 主な機能

* LLMによる目標のサブゴール分解
* サブゴール単位のタスク実行
* Mineflayerを用いた状態観測
* Minecraftスクリーンショットを用いた視覚的状態表現
* 人間の記憶に着想を得たエージェントアーキテクチャ
* 過去の実行履歴を保存するエピソード記憶
* 再利用可能なスキルを扱う手続き記憶
* 失敗回復のための修復記憶
* 次の行動モードを決定するメタコントローラ
* 非連合学習における馴化を用いた将来的な拡張

## 自分の担当

本プロジェクトでは、エージェントシステム全体のアーキテクチャ設計と実装を行っています。

主に以下を担当しています。

* 記憶ベースのエージェントアーキテクチャの設計
* PlannerとExecutorの実装
* 状態観測機能の実装
* サブゴール単位のタスク実行の設計
* 失敗回復機構の実装
* 画像ベースの状態表現の実験
* 人間の記憶や学習の仕組みを自律エージェントへ応用する方法の検討

## 使用技術

* JavaScript / Node.js
* Mineflayer
* Minecraft
* LLM API
* JSONベースの記憶システム
* 画像ベースの状態表現

