---
layout: default
title: Room Programs
nav_order: 3
parent: Scripting
has_children: true
---

# Room Programs
{: .no_toc }

Room programs (room progs) are scripts attached to rooms that make environments reactive and dynamic. They bring areas to life with traps, puzzles, atmospheric effects, teleporters, and ambushes — all without requiring an NPC to be present.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## What Are Room Programs?

A room program is a block of scripting code attached to a room that runs when a specific event occurs — a player walks in, speaks a keyword, opens a door, drops an object, or any of dozens of other triggers. The script executes commands as if the room itself were performing actions: echoing messages, transferring players, loading mobiles, dealing damage, and more.

Every command in a room prog is prefixed with `room` — for example, `room echo`, `room transfer`, `room mload`. This tells the engine that the room is performing the action.

## When to Use Room Programs

Room programs are ideal for:

- **Environmental effects** — Weather narration, ambient sounds, hazardous terrain that damages on entry
- **Puzzles** — Speech-activated mechanisms, sequence-based door unlocks, hidden passage reveals
- **Traps** — Rooms that damage or debuff players on entry, pressure plates, collapsing floors
- **Teleporters** — Portals, magical circles, and one-way transfers triggered by entry or speech
- **Ambushes** — Rooms that spawn hostile mobs when a player enters and force combat
- **Area transitions** — Atmospheric messages when moving between zones, locked gates that require keys
- **Door and exit control** — Programmatic opening, closing, and locking of exits based on events
- **Dungeon mechanics** — Checkpoints, floor transitions, instance progression triggers

Room programs are preferred over mob programs when the behavior belongs to the environment itself rather than to a specific NPC. A collapsing bridge, a magical ward, or a room that echoes with whispers should all use room progs.

## How Room Programs Work

Every room prog has three parts:

1. **The script** — A block of code stored in the area file (identified by vnum)
2. **A trigger** — The event that causes the script to run (e.g., `greet`, `speech`, `entry`)
3. **An attachment** — The link between the script, the trigger, and the specific room

When the trigger event occurs, the engine runs the script with context about what happened — who triggered it, what direction they came from, what they said, etc. Your script accesses this context through [variables and quick codes](../variables-and-tokens.md).

### Entity References in Room Programs

In a room prog, `$self` and `$this` refer to the room itself. Since the room is also the location, `$here` is equivalent to `$self`. Other key variables:

| Variable | Description |
|----------|-------------|
| `$self` / `$this` | The scripted room |
| `$here` | Current room (same as `$self` in room context) |
| `$enactor` / `$n` | Character who triggered the event |
| `$victim` / `$t` | Victim or target of the action |
| `$obj1` / `$p` | Primary object involved |
| `$obj2` | Secondary object |
| `$random` | Random character in the room |
| `$phrase` | The matched phrase or argument |

## Creating a Room Program with `rpedit`

The `rpedit` editor is used to create, edit, and manage room programs. Here is a typical workflow:

### Step 1: Create the Script

```
rpedit create
```

This creates a new room program and opens the editor. You will see the program's assigned vnum.

### Step 2: Name the Script

Give it a descriptive name so you can identify it later:

```
name Trap room — fire damage on entry
```

### Step 3: Write the Code

Enter the code editor:

```
code
```

This opens a line-based text editor. Type your script, then type `.` on a blank line to finish:

```
** Fire trap — damage anyone who enters
room echo {RFlames erupt from the floor!{x
room damage $n 50 fire
.
```

{: .important }
> Comments in scripts use the `**` prefix, **not** `//`. Place comments on their own line:
> ```
> ** This script fires when a player enters the room
> room echo The walls hum with energy.
> ```

### Step 4: Attach a Trigger to the Room

Use `redit` (the room editor) to attach the script to a room with a trigger:

```
redit <room vnum>
addprog <script vnum> <trigger type> <phrase>
```

For example, to make a room react when any player enters:

```
addprog 12345 greet 100
```

This attaches script 12345 with a `greet` trigger and phrase `100` (100% chance of firing).

### Step 5: Test

Reset the area, then walk into the room to verify the script fires. Use `stat room` to confirm the program is attached, and `rpstat` to see all attached programs and their triggers.

## Room vs. Mob Programs

| Feature | Room Programs | Mob Programs |
|---------|--------------|--------------|
| Entity | Attached to a room | Attached to an NPC |
| Prefix | `room` | `mob` |
| Triggers | 71 | 153 |
| Commands | 157 | 167 |
| Best for | Environment, traps, puzzles, transitions | NPC behavior, quests, dialogue |
| Persistence | Always present (room exists permanently) | Only when NPC is loaded |
| `$self` | The room | The NPC |
| Unique triggers | `knock`, `knocking`, `reset`, `recall`, `prerecall`, `clone_extract` | `bribe`, `give`, `hit`, `animate`, `mount`, etc. |

## Script Comments

Use `**` at the start of a line to write a comment. Comments are ignored when the script runs:

```
** Check if the player has completed the puzzle
if varexists $n puzzle_solved
  room echo The passage remains open.
else
  room echo {YThe stone wall slides shut behind you!{x
  room alterexit south door closed
endif
```

## Quick Reference

| Topic | Link |
|-------|------|
| All room triggers (71) | [Triggers](rprog-triggers.md) |
| All room commands (157) | [Commands](rprog-commands.md) |
| Practical examples | [Examples](rprog-examples.md) |
| Shared commands reference | [Shared Commands](../shared-commands.md) |
| Variables and quick codes | [Variables and Tokens](../variables-and-tokens.md) |
| If-checks and conditions | [If-Checks Reference](../ifchecks-reference.md) |
| Scripting basics | [Scripting Basics](../scripting-basics.md) |