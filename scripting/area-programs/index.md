---
layout: default
title: Area Programs
nav_order: 5
parent: Scripting
has_children: true
---

# Area Programs
{: .no_toc }

Area programs (area progs) are scripts attached to an entire zone rather than to a specific mob, object, or room. They manage zone-wide behavior — periodic announcements, reset logic, global state tracking, world events, and wilderness map manipulation.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## What Are Area Programs?

An area program is a block of scripting code that runs in response to a zone-level event — the area resets, a random tick fires, a reckoning event begins, or the in-game moon phase changes. Unlike mob or room programs, area programs have no single "location" — they operate on the zone as a whole.

Every command in an area prog is prefixed with `area` — for example, `area echo`, `area mload`, `area varset`. This tells the engine the command originates from the area entity.

{: .note }
> **Quest programs share the same command table.** Any command available to area programs is also available to quest programs (with the `quest` prefix). If you are writing quest scripts, this command reference applies to you as well.

## When to Use Area Programs

Area programs are ideal for:

- **Zone-wide announcements** — Periodic messages broadcast to everyone in the area (weather changes, ambient events, NPC crier messages)
- **Reset-triggered repopulation** — Custom logic that runs when the area resets, spawning mobs or objects based on conditions rather than static reset lists
- **Global state management** — Tracking zone-wide variables that persist across resets (invasion progress, seasonal states, player-driven world changes)
- **World events and reckonings** — Starting, advancing, and ending zone-level events that affect multiple rooms
- **Wilderness map control** — Dynamically modifying wilderness tiles, overlays, anchors, and virtual links based on game conditions
- **Dungeon orchestration** — Controlling dungeon phases, instance completion, and floor transitions from the zone level

## How Area Programs Work

Every area prog has three parts:

1. **The script** — A block of code stored in the area file (identified by widevnum)
2. **A trigger** — The event that causes the script to run (e.g., `random`, `reset`, `tick`)
3. **An attachment** — The link between the script, the trigger, and the area

When the trigger event occurs, the engine runs the script in the context of the area. Because area programs have no inherent room or character context, many commands require explicit targets — you must specify room vnums, character names, or use variables set by other scripts.

## Creating an Area Program with `apedit`

The `apedit` editor creates, edits, and manages area programs. Here is a typical workflow:

### Step 1: Create the Script

```
apedit create
```

This creates a new area program in the current area and opens the editor. You will see the program's assigned widevnum.

To create a program with a specific vnum:

```
apedit create <vnum>
```

### Step 2: Name the Script

Give it a descriptive name:

```
name Area reset announcement
```

### Step 3: Write the Code

Enter the code editor:

```
code
```

Type your script, then type `.` on a blank line to finish:

```
** Announce the area reset to all players in the zone
area echo {CThe ancient forest stirs as creatures return to their lairs.{x
area varset reset_count $@(reset_count + 1)
.
```

{: .important }
> Comments in scripts use the `**` prefix, **not** `//`. Place comments on their own line.

### Step 4: Attach to the Area

Use `aedit` (the area editor) to attach the script to the area with a trigger:

```
aedit
addprog <script vnum> <trigger type> <phrase>
```

For example, to fire on every area reset:

```
addprog 12345 reset 100
```

This attaches script 12345 with a `reset` trigger and phrase `100` (100% chance of firing).

### Step 5: Test

Force an area reset or wait for the next tick, then verify the script fires. Use `astat` to confirm programs are attached to the area.

## Area Program Capabilities

Area programs have a focused but powerful command set (44 commands):

| Category | Capabilities |
|----------|-------------|
| **Output** | `echoat`, `questechoat`, `wiznet` |
| **Entity creation** | `mload`, `oload`, `resetroom` |
| **Room modification** | `alterroom` |
| **Player management** | `mute`, `unmute`, `mail`, `sendfloor` |
| **Variables** | `varset`, `varseton`, `varclear`, `varclearon`, `varcopy`, `varsave`, `varsaveon` |
| **Scripting** | `call`, `xcall` |
| **Events** | `event`, `startevent`, `stopevent`, `phaseevent`, `stageevent` |
| **Reckonings** | `reckoning`, `startreckoning`, `stopreckoning` |
| **Dungeons** | `dungeoncommence`, `dungeoncomplete`, `dungeonfailure`, `instancecomplete`, `instancefailure`, `specialkey`, `unlockdungeon` |
| **Wilderness** | `wildernessmap`, `wildsoverlay`, `wildsanchor`, `wildstile`, `wildsvlink` |
| **Other** | `churchannouncetheft`, `treasuremap`, `unlockarea`, `quest`, `questechoat` |

Area programs have only 6 triggers — far fewer than mob programs (153), but each serves a clear purpose in zone-level scripting.

## Script Comments

Use `**` at the start of a line to write a comment. Comments are ignored when the script runs:

```
** Track how many times this area has been reset
** and announce to players every 10th reset
area varset reset_count $@(reset_count + 1)
if var reset_count == 10
  area echo {YThe land renews itself with ancient power!{x
  area varset reset_count 0
endif
```

## Quick Reference

| Topic | Link |
|-------|------|
| All area triggers (6) | [Triggers](aprog-triggers.md) |
| All area commands (44) | [Commands](aprog-commands.md) |
| Practical examples | [Examples](aprog-examples.md) |
| Shared commands reference | [Shared Commands](../shared-commands.md) |
| Variables and quick codes | [Variables and Tokens](../variables-and-tokens.md) |
| If-checks and conditions | [If-Checks Reference](../ifchecks-reference.md) |
| Scripting basics | [Scripting Basics](../scripting-basics.md) |
