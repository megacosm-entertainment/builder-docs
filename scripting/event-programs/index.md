---
layout: default
title: Event Programs
nav_order: 9
parent: Scripting
has_children: true
---

# Event Programs
{: .no_toc }

Event programs (event progs) are scripts attached to game-wide events that manage PvP tournaments, seasonal content, world bosses, invasions, and collection challenges. They let builders create fully automated, multi-stage events that start on schedule, track progress across the player base, and shut down cleanly when finished.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## What Are Event Programs?

An event program is a block of scripting code attached to an event definition. The event itself is a game-wide construct — it has a type, a scope, a schedule, stages, and a roster of participants — and the scripts drive its behavior at runtime. When a trigger fires (a tick, a reset, a reckoning, or a moon phase change), the event program executes commands as the event entity.

Every command in an event prog is prefixed with `event` — for example, `event echoat`, `event mload`, `event startevent`. This tells the engine that the event is performing the action.

Events differ from other scriptable entities in one critical way: they are **not tied to a single room, mob, or object**. An event can span the entire game world, a single area, or a defined region. Its scripts can affect any player who has joined the event, load mobs and objects anywhere within its scope, and track progress toward a shared goal.

## When to Use Event Programs

Event programs are ideal for:

- **PvP tournaments** — War events (free-for-all, genocide, jihad) that pit players against each other with kill tracking, brackets, and winners
- **World boss spawns** — Boss events that spawn a powerful mob, announce its arrival, and reward participants
- **Invasions** — Waves of hostile NPCs that appear across an area with escalating difficulty
- **Collection challenges** — Seasonal events where players gather items toward a shared or per-bracket goal
- **Seasonal content** — Holiday events that run on a schedule with custom announcements and rewards
- **World-state changes** — Events that alter the game world (weather, terrain, NPC behavior) for their duration
- **Reckoning events** — Large-scale world events that progress through phases with global effects

Event programs are preferred over area or room programs when the behavior needs to:
- Track progress across multiple players simultaneously
- Run on a fixed schedule or interval
- Manage participant rosters and brackets
- Award rewards based on contribution or ranking

## How Event Programs Work

Every event prog has three parts:

1. **The script** — A block of code stored in the area file (identified by vnum, type `EventProg`)
2. **A trigger** — The condition that causes the script to run (e.g., `tick`, `reset`, `reckoning`)
3. **An attachment** — The link between the script, the trigger, and the event definition

When the trigger fires, the engine runs the script with context about the event state — who is participating, what stage the event is in, how much progress has been made.

### Entity References in Event Programs

In an event prog, `$self` and `$this` refer to the event entity itself. Since events are not located in a specific room, `$here` is not meaningful in the same way as for room or mob programs.

| Variable | Description |
|----------|-------------|
| `$self` / `$this` | The event entity |
| `$enactor` / `$n` | Character who triggered the event (if applicable) |
| `$phrase` | The matched phrase or argument |
| `$register1`–`$register5` | Numeric registers for passing data |

## Event Types

Events are categorized by type, which determines their core behavior:

| Type | Description |
|------|-------------|
| `collection` | Players gather items toward a shared goal |
| `invasion` | Waves of hostile NPCs spawn across the event scope |
| `boss` | A powerful mob spawns and must be defeated |
| `war-ffa` | Free-for-all PvP — every player for themselves |
| `war-genocide` | Faction-based PvP — races or groups fight to eliminate each other |
| `war-jihad` | Religion-based PvP — churches wage war |
| `worldstate` | Alters game-world conditions for the event duration |
| `custom` | Fully script-driven — behavior defined entirely by event programs |

## Event Scopes

The scope determines where the event takes effect:

| Scope | Description |
|-------|-------------|
| `global` | Affects the entire game world |
| `area` | Restricted to a single area (requires anchor) |
| `region` | Spans a group of related areas (requires anchor) |
| `zones` | Covers specific zone designations (requires anchor) |
| `battlefield` | Confined to a dedicated PvP battlefield (requires anchor) |

Scopes other than `global` require an **anchor area** — the area that the event is centered on. The `scopefloating` option allows the anchor to be dynamically assigned at start time.

## Event Flags

| Flag | Description |
|------|-------------|
| `winner_only` | Only the top contributor receives rewards |
| `all_parts` | All participants receive rewards regardless of ranking |
| `top3` | Top three contributors receive rewards |
| `noannounce` | Suppress automatic start/end announcements |
| `joinlate` | Allow players to join after the event has started |
| `exclusive_player` | A player can only participate in one event of this type at a time |
| `unique_global` | Only one instance of this event can be active at a time |
| `scaling` | Event difficulty scales with participant count |
| `repeatable` | Event can run again after completing (respects cooldown) |
| `passive` | Event runs in the background without requiring player sign-up |
| `autoadd` | Players are automatically added when they enter the event scope |

## Creating an Event with `evtedit`

The `evtedit` editor creates and configures event definitions. Here is a typical workflow:

### Step 1: Create the Event

```
evtedit create
```

This creates a new event and opens the editor. You will see tabs for Identity, Schedule, Stages, Messages, Meta, and Scripting.

### Step 2: Set Basic Properties

```
name Harvest Moon Collection
description Players gather moonberries during the harvest moon.
type collection
scope area
```

### Step 3: Configure Schedule and Timing

```
schedule recurring
interval 720
duration 60
cooldown 120
variance 30
```

- **interval** — Minutes between event starts
- **duration** — How long the event runs (minutes)
- **cooldown** — Minimum minutes before the event can run again
- **variance** — Random variance added to the interval

### Step 4: Set Player Requirements

```
minlevel 10
maxlevel 0
minplayers 3
maxplayers 50
```

Setting `maxlevel` to 0 means no upper limit. Setting `minplayers` to 3 means the event won't start unless at least 3 eligible players are online.

### Step 5: Configure Messages

```
announce {WThe Harvest Moon rises! Moonberries are appearing across the Silverwood.{x
endmsg {WThe Harvest Moon sets. The harvest is complete!{x
joinmsg {CYou join the Harvest Moon collection event!{x
```

### Step 6: Set Goal and Stages

```
goal 100
stages 3
```

The goal is the target progress value. Stages define phase transitions — each stage can trigger a `phaseevent` or `stageevent` in your scripts.

### Step 7: Configure Rewards

```
rewardsuccess 5000 gold
rewardfail 500 gold
```

### Step 8: Set Flags

```
flags joinlate repeatable scaling
```

### Step 9: Attach Event Programs

```
addeprog <script vnum> <trigger> <phrase>
```

For example:

```
addeprog 500 tick 100
addeprog 501 reset 100
```

### Step 10: Save

```
save
```

## Writing Event Program Scripts

Event program scripts are created with the `epedit` editor (Event Program Editor):

```
epedit create
```

Write the script using the `code` command. All commands use the `event` prefix:

```
** Spawn collection items on each tick
event oload 5001
event gecho {YA shimmer of moonlight reveals new moonberries in the Silverwood!{x
```

Use `epstat` to inspect attached programs on an event, and `epdump <vnum>` to view a script's source code. Use `eplist` to list all event programs in an area.

## Script Comments

Use `**` at the start of a line to write a comment. Comments are ignored when the script runs:

```
** Check if we've reached the goal
if eventprogress $self >= 100
  event gecho {GThe Harvest Moon event is complete! Rewards are being distributed!{x
  event stopevent
endif
```

## Quick Reference

| Topic | Link |
|-------|------|
| All event triggers (6) | [Triggers](eprog-triggers.md) |
| All event commands (44) | [Commands](eprog-commands.md) |
| Practical examples | [Examples](eprog-examples.md) |
| Shared commands reference | [Shared Commands](../shared-commands.md) |
| Variables and quick codes | [Variables and Tokens](../variables-and-tokens.md) |
| If-checks and conditions | [If-Checks Reference](../ifchecks-reference.md) |
| Scripting basics | [Scripting Basics](../scripting-basics.md) |
