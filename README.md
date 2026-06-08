# minecraft-ai-agent-tohoku-research-demo
[日本語版はこちら](./README_ja.md)

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

