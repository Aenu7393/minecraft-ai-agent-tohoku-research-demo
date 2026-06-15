# minecraft-ai-agent-tohoku-research-demo
[日本語版はこちら](./README_ja.md)

## 1. Overview

This repository is a public demo version of a research project on an autonomous AI agent that operates inside Minecraft.

The goal of this research is to build a system in which an agent can understand the current game situation, decompose high-level goals into executable subgoals, execute actions, store experiences in memory, and recover from failures.

In particular, this project focuses not only on using LLMs for planning, but also on visual state understanding with VLMs, a memory-based agent architecture inspired by human memory systems, and a mechanism for cognitive monitoring, correction, interruption, and replanning during low-level action execution.

## 2. Research Background

Large Language Models can generate high-level task plans. For example, given the goal "make a stone pickaxe," an LLM can generate a subgoal sequence such as "collect wood," "craft planks," "craft sticks," "make a wooden pickaxe," and "mine cobblestone."

However, simply generating a subgoal sequence is not enough to complete tasks in an actual Minecraft environment. The agent must consider its current inventory, nearby blocks, visual situation, enemies or obstacles, subgoal executability, failure reasons, and past experiences.

Therefore, this research explores an agent architecture that combines LLM-based planning with state representation, visual understanding, memory, failure recovery, and cognitive intervention in low-level actions.

## 3. Research Positioning

In recent years, hierarchical agents that combine LLM-based high-level planning with low-level action control based on reinforcement learning or imitation learning have been studied in open-world environments such as Minecraft.

In many existing approaches, an LLM decomposes a task into subgoals or skill sequences, while a low-level policy executes concrete actions such as moving, mining, and crafting.

However, after the high-level module determines a subgoal, the problem of continuously monitoring whether the low-level policy is moving toward the goal during execution, and correcting, interrupting, or replanning when necessary, has not yet been sufficiently explored.

This research focuses on this point. Instead of treating low-level actions as a simple execution module, this project aims to build an architecture in which LLMs and VLMs cognitively intervene in ongoing actions through observation, reasoning, prediction, and memory retrieval.

This idea is inspired by the relationship between unconscious motor control and conscious attention or reasoning in humans. Humans often walk, look around, and approach objects unconsciously, but when they get stuck, lose sight of the target, or notice danger, conscious reasoning intervenes and modifies the behavior.

## 4. Main Ideas

The core ideas of this project are the following three points:

1. High-level goal decomposition using LLMs
2. Situation understanding using VLMs, state observation, and memory
3. Cognitive intervention during low-level action execution

The agent decomposes a user-given goal into a sequence of subgoals and executes each subgoal. However, the agent does not simply execute the subgoals in order. During execution, it observes the current state and judges whether the ongoing behavior is aligned with the goal.

If the agent loses sight of the target, stops making progress, encounters danger, finds a similar past failure, or determines that the current subgoal is difficult to execute, an LLM/VLM-based cognitive module corrects, interrupts, or replans the behavior.

## 5. Cognitive Intervention Architecture

This research explores an architecture in which LLMs and VLMs can intervene during low-level policy or skill execution.

```text
User Goal
   ↓
LLM Planner
   ↓
Subgoal Queue
   ↓
Meta Controller
   ↓
Executor / Low-Level Policy
   ↓
Minecraft Agent
```

During execution, the Cognitive Monitor observes the current behavior based on state information obtained from the Minecraft Agent.

```text
Minecraft Agent
   ↓
State Observer
   ├── Symbolic state obtained from Mineflayer
   └── Visual state obtained from Minecraft screenshots
   ↓
Cognitive Monitor
   ├── VLM: Understanding the current visual situation
   ├── LLM: Judging consistency with the goal
   ├── World Model: Predicting action outcomes
   └── Memory System: Retrieving similar past experiences
   ↓
Intervention Controller
   ├── continue: Continue the current action
   ├── correct: Correct view direction, movement, or exploration strategy
   ├── interrupt: Interrupt the current action
   └── replan: Modify or replan the subgoal
   ↓
Executor / Low-Level Policy
```

In this architecture, the low-level policy is responsible for concrete operations such as movement, camera control, mining, attacking, using items, and crafting. On the other hand, LLMs and VLMs do not directly decide every low-level operation. Instead, they monitor the execution state and provide guidance signals or correction policies when necessary.

## 6. Memory System

This project designs the agent's memory system by referring to the types and functions of human memory.

The agent aims not only to temporarily store the current state and recent actions, but also to organize important experiences and save them as reusable long-term knowledge.

```text
Memory System
├── Working Memory
│   └── Current goal, subgoal, state, and recent execution status
│
├── Intermediate Memory
│   └── Temporary storage for recent experiences before transferring important information to long-term memory
│
└── Long-Term Memory
    ├── Declarative Memory
    │   ├── Episodic Memory
    │   │   └── Past experiences, state information, failures, and reflections
    │   └── Semantic Memory
    │       └── Crafting recipes, item relationships, and task dependencies
    │
    └── Non-Declarative Memory
        ├── Associative Learning
        ├── Non-Associative Learning
        ├── Priming
        ├── Procedural Memory
        └── Emotional Conditioning
```

## 7. Transition from Intermediate Memory to Long-Term Memory

In this project, not all experiences are directly stored in long-term memory. Instead, experiences are first stored in intermediate memory, and only important information is transferred to long-term memory.

Examples of candidates for long-term memory include:

* Repeated failures
* Effective repair strategies
* Knowledge reusable across different tasks
* Visual information important for task completion
* Situations strongly associated with danger or success
* Relationships between specific actions and results

```text
Current state / experience
   ↓
Working Memory
   ↓
Intermediate Memory
   ↓
Evaluate importance, novelty, reusability, and failure frequency
   ↓
Long-Term Memory
```

## 8. State Representation

In earlier versions, the agent obtained structured state information from Minecraft using Mineflayer.

For example, inventory, position, nearby blocks, health, current task, and execution results were used as state representations for planning, execution, and failure recovery.

However, state representations obtained through Mineflayer are mainly symbolic and cannot fully represent the visual situation of the entire game screen.

Therefore, the current direction is to extend the state representation by using Minecraft screenshots as part of the agent's visual state.

## 9. Image-Based State Representation

Image-based state representation is expected to be used for the following purposes:

* Understanding the current environment in more detail
* Visually detecting targets and obstacles
* Detecting environmental changes
* Improving subgoal execution
* Supporting reflection after failures
* Modifying subgoals when the current plan does not work well

By incorporating visual information instead of relying only on text-based or symbolic state information, this project aims to help the agent understand in-game situations more accurately.

## 10. Application of Non-Associative Learning

This research explores the application of habituation, a type of non-associative learning, to image-based state understanding.

Habituation is a phenomenon in which responses or attention to repeated unimportant stimuli gradually decrease.

In a Minecraft agent, this idea can be applied as a mechanism that reduces attention to repeatedly observed low-importance visual information and increases attention to novel, uncertain, or task-relevant regions.

This is expected to reduce unnecessary observation costs while allowing the agent to focus on information needed for failure reflection and subgoal correction.

## 11. Task Example

Input goal:

```text
Make a stone pickaxe
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

If a failure occurs during one of these subgoals, the agent checks the current state, past failure cases, and visual state representation, then generates or reuses a repair strategy.

For example, if the agent loses sight of a tree while trying to chop it, the VLM checks the visual situation, the LLM judges consistency with the goal, and the Memory System retrieves similar past failures. Based on the result, the Intervention Controller may correct the camera direction, change the exploration strategy, interrupt the current subgoal, or replan another subgoal.

## 12. Main Features

* LLM-based goal decomposition into subgoals
* Subgoal-level task execution
* Symbolic state observation using Mineflayer
* Visual state representation using Minecraft screenshots
* Cognitive monitoring during execution using LLMs/VLMs
* Correction, interruption, and replanning of low-level actions
* Action outcome prediction using a World Model
* Memory architecture using working memory, intermediate memory, and long-term memory
* Episodic memory for storing experiences, failures, and reflections
* Semantic memory for crafting knowledge and task dependencies
* Procedural memory for reusable skills
* Visual attention adjustment based on habituation in non-associative learning
* Meta Controller for deciding the next action mode
* Subgoal correction and reuse of repair strategies after failures

## 13. Current Implementation Status

The current implementation includes two main approaches for building Minecraft agents.

### 13.1 Mineflayer-Based Agent

The first approach is an agent based on Mineflayer.

In this architecture, the agent obtains the Minecraft state through Mineflayer, and the LLM Planner decomposes the user's high-level goal into executable subgoals.

The Executor executes the generated subgoals in order and stores the success or failure of each subgoal, the current state, and failure reasons as episodic memory.

For example, for the goal "make a stone pickaxe," the LLM Planner generates the following subgoal sequence and executes each action using Mineflayer.

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

In this implementation, inventory, position, nearby blocks, health, current subgoal, and execution results are handled as symbolic state representations.

When a failure occurs during execution, the agent refers to the current state and past failure cases, then generates a subgoal modification or repair strategy.

### 13.2 STEVE-1-Based Agent

The second approach uses STEVE-1, an existing research model, as the low-level action policy.

STEVE-1 is a model that generates low-level actions in Minecraft based on text instructions. In this project, STEVE-1 is used as the low-level processing module, and an LLM Planner is connected above it.

In this architecture, the LLM Planner decomposes the user's high-level goal into a sequence of subgoals. Each subgoal is then given to STEVE-1 as a text instruction, and STEVE-1 executes low-level actions in Minecraft based on the instruction.

```text
User Goal
   ↓
LLM Planner
   ↓
Subgoal Queue
   ↓
Text Instruction
   ↓
STEVE-1
   ↓
Low-level actions in Minecraft
```

For example, for the goal "make a stone pickaxe," the LLM Planner generates subgoals such as "find a tree," "chop a tree," "craft planks," "craft sticks," and "mine cobblestone." Each subgoal is then passed to STEVE-1, enabling execution down to the low-level action level.

This implementation allows the project to examine not only API-based action execution with Mineflayer, but also an agent architecture that uses a visual-language-conditioned low-level action policy.

## 14. Future Directions

Future work includes the following directions:

* Improving visual state understanding using VLMs
* Designing guidance signals for low-level policies
* Integrating reinforcement learning policies with cognitive intervention modules
* Introducing a World Model for executability prediction
* Improving failure recovery using memory retrieval
* Implementing criteria for transferring information from intermediate memory to long-term memory
* Experimenting with visual attention control based on habituation
* Applying the architecture to longer-horizon Minecraft tasks

## 15. Technologies Used

* JavaScript / Node.js
* Mineflayer
* Minecraft
* LLM API
* VLM
* STEVE-1
* JSON-based memory system
* Image-based state representation
* Reinforcement learning / imitation learning-based low-level policies
