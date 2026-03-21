---
layout: default
title: Mobile Programs
nav_order: 1
parent: Scripting
has_children: true
---

# Mobile Programs
{: .no_toc }

Mobile programs (mob progs) are scripts attached to NPCs (mobiles) that bring them to life with custom behavior. They are the most commonly used script type in Sentience.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## What Are Mobile Programs?

A mobile program is a block of scripting code that runs when a specific event occurs — a player enters the room, says a keyword, gives an item to the NPC, or any of dozens of other triggers. The script executes commands as if the NPC were performing actions: speaking, moving, loading objects, transferring players, dealing damage, and more.

Every command in a mob prog is prefixed with `mob` — for example, `mob echo`, `mob transfer`, `mob oload`. This tells the engine that the NPC is performing the action.

## When to Use Mob Programs

Mob programs are ideal for:

- **NPC behavior** — Random idle actions, reactions to the environment, ambient dialogue
- **Quest givers** — NPCs that check player progress, hand out items, and advance quest stages
- **Shopkeeper customization** — Custom greetings, special trade logic, restricted access
- **Combat AI** — Bosses with HP-phase transitions, special attacks, self-healing, adds
- **Guards and patrols** — NPCs that check for certain items or tokens, transfer intruders, respond to speech
- **Interactive NPCs** — Conversation trees using speech triggers, bribe-based services, give-and-receive exchanges

## How Mob Programs Work

Every mob prog has three parts:

1. **The script** — A block of code stored in the area file (identified by vnum)
2. **A trigger** — The event that causes the script to run (e.g., `greet`, `speech`, `fight`)
3. **An attachment** — The link between the script, the trigger, and the specific NPC

When the trigger event occurs, the engine runs the script with context about what happened — who triggered it, what they said, what object was involved, etc. Your script accesses this context through [variables and quick codes](../variables-and-tokens.md).

## Creating a Mob Program with `mpedit`

The `mpedit` editor is used to create, edit, and manage mob programs. Here is a typical workflow:

### Step 1: Create the Script

```
mpedit create
```

This creates a new mob program and opens the editor. You will see the program's assigned vnum.

### Step 2: Name the Script

Give it a descriptive name so you can identify it later:

```
name Guard greet script
```

### Step 3: Write the Code

Enter the code editor:

```
code
```

This opens a line-based text editor. Type your script, then type `.` on a blank line to finish:

```
say Halt! State your business, $n.
mob echoaround $n The guard eyes $n suspiciously.
.
```

{: .important }
> Comments in scripts use the `**` prefix, **not** `//`. Place comments on their own line:
> ```
> ** This script fires when a player enters the room
> say Welcome, $n!
> ```

### Step 4: Compile the Script

After writing code, the script is automatically checked when you finish editing. If there are syntax errors, you will see them in the editor output.

### Step 5: Attach a Trigger to the NPC

Use `medit` (the mobile editor) to attach the script to an NPC with a trigger:

```
medit <mob vnum>
addprog <script vnum> <trigger type> <phrase>
```

For example, to make a mob greet every player who enters:

```
addprog 12345 greet 100
```

This attaches script 12345 with a `greet` trigger and phrase `100` (100% chance of firing).

### Step 6: Test

Reset the area or repop the mob, then interact with it to verify the script fires correctly. Use `stat mob <name>` to confirm the program is attached, and `mpstat <name>` to see all attached programs.

## Mob-Specific Capabilities

Mobile programs have access to capabilities that other script types do not:

| Capability | Description |
|-----------|-------------|
| `mob appear` / `mob disappear` | Toggle NPC visibility |
| `mob kill` | Initiate combat with a target |
| `mob assist` | Join another character in combat |
| `mob hunt` | Track and pursue a target across rooms |
| `mob take` | Take an object directly from a character |
| `mob teleport` | Teleport all characters in the room |
| `mob chargemoney` | Deduct coins from a player's inventory |
| `mob remember` / `mob forget` | Store and recall a target character |
| `mob flee` | Flee from combat |

Mob programs also have the widest trigger selection of any entity type — 153 triggers covering movement, combat, speech, commerce, quests, and more.

## Script Comments

Use `**` at the start of a line to write a comment. Comments are ignored when the script runs:

```
** Check if the player has the quest token
if tokenvalue $n 50100 0 >= 1
  say Welcome back, hero!
else
  say I don't know you. Begone!
endif
```

## Quick Reference

| Topic | Link |
|-------|------|
| All mob triggers (153) | [Triggers](mprog-triggers.md) |
| All mob commands (167) | [Commands](mprog-commands.md) |
| Practical examples | [Examples](mprog-examples.md) |
| Shared commands reference | [Shared Commands](../shared-commands.md) |
| Variables and quick codes | [Variables and Tokens](../variables-and-tokens.md) |
| If-checks and conditions | [If-Checks Reference](../ifchecks-reference.md) |
| Scripting basics | [Scripting Basics](../scripting-basics.md) |