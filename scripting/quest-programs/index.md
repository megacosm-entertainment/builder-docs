---
layout: default
title: Quest Programs
nav_order: 8
parent: Scripting
has_children: true
---

# Quest Programs
{: .no_toc }

Quest programs (quest progs) are scripts attached directly to quest definitions. They let you control the entire quest lifecycle — from acceptance through completion or failure — with programmatic logic.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## What Are Quest Programs?

A quest program is a block of scripting code attached to a quest definition that runs when specific quest events occur. Unlike mobile or object programs that react to world events (speech, combat, movement), quest programs react to **quest-specific events** — a player accepting the quest, completing a stage, resolving an objective target, or failing the quest entirely.

Every command in a quest prog is prefixed with `quest` — for example, `quest echoat`, `quest varset`, `quest mload`. This tells the engine that the quest entity is performing the action.

Quest programs have access to 13 triggers that are **exclusive** to quests — they do not exist on any other entity type. These triggers cover the full quest lifecycle: acceptance, focus, stage progression, objective resolution, completion, and failure.

## When to Use Quest Programs

Quest programs are ideal for:

- **Quest lifecycle scripting** — React to acceptance, stage transitions, objective completion, and quest turn-in
- **Dynamic quest generation** — Resolve targets and destinations at runtime (e.g., pick a random mob to slay, a random room to visit)
- **Quest rewards** — Scale rewards based on player level, time elapsed, or custom variables
- **Multi-stage quest logic** — Advance stages programmatically, skip stages conditionally, or branch quest progression
- **Quest state tracking** — Store and retrieve custom variables on the quest entity for complex quest logic
- **Notification and flavor** — Send contextual messages to the player as quest milestones are reached

## Relationship to Area Programs

Quest programs share the same command table as area programs (`area_cmd_table`). This means the 44 commands available to quest progs are **identical** to those available to area progs. If you already know area scripting, you know quest scripting — the only difference is the triggers and the entity context.

| Aspect | Area Programs | Quest Programs |
|--------|--------------|---------------|
| Command table | `area_cmd_table` (44 commands) | `area_cmd_table` (44 commands) |
| Command prefix | `area` | `quest` |
| Triggers | `random`, `reset` | 13 quest-exclusive triggers |
| Entity context | `$self` = the area | `$self` = the quest |
| Editor | `apedit` | `qedit` / `qpedit` |

## How Quest Programs Work

Every quest prog has three parts:

1. **The script** — A block of code stored in the area file (identified by vnum)
2. **A trigger** — The quest event that causes the script to run (e.g., `quest_accepted`, `stage_completed`)
3. **An attachment** — The link between the script, the trigger, and the quest definition

When the trigger event occurs, the engine runs the script with context about the quest state — which player is involved, which stage or objective triggered it, and so on.

## Creating a Quest Program with `qedit` / `qpedit`

The `qedit` editor manages quest definitions, and `qpedit` manages the scripts attached to them.

### Step 1: Create the Script

```
qpedit create
```

This creates a new quest program and opens the editor. You will see the program's assigned vnum.

### Step 2: Name the Script

Give it a descriptive name for identification:

```
name Reward scaling on completion
```

### Step 3: Write the Code

Enter the code editor:

```
code
```

Type your script, then type `.` on a blank line to finish:

```
** Scale gold reward based on player level
quest echoat $n {YQuest complete! You have earned your reward.{x
.
```

{: .important }
> Comments in scripts use the `**` prefix, **not** `//`. Place comments on their own line:
> ```
> ** This fires when the quest is accepted
> quest echoat $n {CYou have accepted the quest!{x
> ```

### Step 4: Compile the Script

The script is automatically checked when you finish editing. Syntax errors will be reported in the editor output.

### Step 5: Attach a Trigger to the Quest

Use `qedit` to attach the script to a quest with a trigger:

```
qedit <quest vnum>
addprog <script vnum> <trigger type> <phrase>
```

For example, to fire a script when a player accepts the quest:

```
addprog 50001 quest_accepted 100
```

This attaches script 50001 with a `quest_accepted` trigger and phrase `100` (100% chance of firing).

### Step 6: Test

Accept the quest in-game and verify the script fires. Use `stat quest <vnum>` to confirm the program is attached.

## Quest-Specific Capabilities

Quest programs excel at managing the quest lifecycle through their exclusive trigger set:

| Capability | Triggers | Description |
|-----------|----------|-------------|
| Acceptance handling | `quest_accepted`, `quest_focused` | React when a player takes on or focuses a quest |
| Stage management | `stage_commenced`, `stage_completed`, `stage_failed` | Control multi-stage quest progression |
| Objective tracking | `objective_completed`, `objective_failed` | Respond to individual objective resolution |
| Target resolution | `stage_target_resolved`, `objective_target_resolved` | Dynamically assign targets at runtime |
| Destination resolution | `stage_dest_resolved`, `objective_dest_resolved` | Dynamically assign locations at runtime |
| Completion handling | `quest_completed`, `quest_failed` | Award rewards, clean up state, trigger follow-up quests |

## Script Comments

Use `**` at the start of a line to write a comment. Comments are ignored when the script runs:

```
** Check player level for reward scaling
if level $n >= 50
  quest echoat $n {YYou receive a bonus reward for your experience!{x
endif
```

## Quick Reference

| Topic | Link |
|-------|------|
| All quest triggers (13) | [Triggers](qprog-triggers.md) |
| All quest commands (44) | [Commands](qprog-commands.md) |
| Practical examples | [Examples](qprog-examples.md) |
| Area program commands (shared table) | [Area Commands](../area-programs/aprog-commands.md) |
| Variables and quick codes | [Variables and Tokens](../variables-and-tokens.md) |
| If-checks and conditions | [If-Checks Reference](../ifchecks-reference.md) |
| Scripting basics | [Scripting Basics](../scripting-basics.md) |
