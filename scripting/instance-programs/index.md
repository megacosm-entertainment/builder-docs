---
layout: default
title: Instance Programs
nav_order: 6
parent: Scripting
has_children: true
---

# Instance Programs
{: .no_toc }

Instance programs (instance progs) are scripts attached to dungeon instances — the private, runtime copies of dungeon blueprints that each player group receives. They manage the lifecycle, state, and dynamic content of instanced dungeons: tracking completion, spawning scaled encounters, managing floor transitions, and coordinating instance-wide variables.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## What Are Instance Programs?

An instance program is a block of scripting code attached to a dungeon instance. When a dungeon is spawned for a player group, the engine creates an **instance** — a private copy of the dungeon blueprint with its own rooms, mobs, objects, and state. Instance programs run on that private copy, isolated from every other group's version of the same dungeon.

Every command in an instance prog is prefixed with `instance` — for example, `instance echoat`, `instance mload`, `instance loadinstanced`. This tells the engine that the instance entity is performing the action.

Instances differ from dungeons (blueprints) in a critical way: a **dungeon** is a template definition, while an **instance** is a live, running copy. Multiple instances can exist simultaneously from the same dungeon blueprint, each with independent state. Instance programs run on the live copy; dungeon programs run on the template.

## When to Use Instance Programs

Instance programs are ideal for:

- **Instanced dungeons** — Private dungeon runs where each party gets their own isolated copy of rooms, mobs, and loot
- **Completion tracking** — Monitoring boss kills, objective progress, and marking the instance complete or failed
- **Scaled content** — Adjusting mob difficulty, spawn counts, or rewards based on party size or level
- **Floor transitions** — Managing multi-floor dungeons where players advance through levels
- **Instance-wide state** — Storing variables that persist across the entire instance (kill counts, puzzle state, phase tracking)
- **Dynamic spawning** — Loading mobs and objects that are scoped to the instance and cleaned up automatically when it ends
- **Private events** — Boss encounters, puzzles, or challenges that run independently for each group

Use instance programs instead of room or mob programs when the behavior needs to:
- Span across multiple rooms within the instance
- Track state that belongs to the instance as a whole, not a single room or mob
- Coordinate events across all players in the instance
- Spawn entities that are automatically scoped to the instance lifecycle

## How Instance Programs Work

Every instance prog has three parts:

1. **The script** — A block of code stored in the area file (identified by vnum, type `InstanceProg`)
2. **A trigger** — The condition that causes the script to run (e.g., `completed`, `entry`, `tick`)
3. **An attachment** — The link between the script, the trigger, and the instance blueprint

When a trigger fires, the engine runs the script with context about the instance — which players are inside, what floor they are on, and what state the instance is in.

### Entity References in Instance Programs

In an instance prog, `$self` and `$this` refer to the instance entity.

| Variable | Alias | Description |
|----------|-------|-------------|
| `$self` / `$this` | `$i` | The instance entity |
| `$enactor` | `$n` | Character who triggered the event (if applicable) |
| `$phrase` | — | The matched phrase or argument |
| `$trigger` | — | The trigger phrase pattern that matched |
| `$register1`–`$register5` | — | Numeric registers for passing data |

### Instance Structure

Each instance maintains:

- **Blueprint reference** — The dungeon blueprint it was created from
- **Rooms, mobs, objects** — Private copies scoped to this instance
- **Player list** — Characters currently inside the instance
- **Boss list** — Designated boss mobs for completion tracking
- **Entrance / exit rooms** — Defined by the blueprint
- **Recall room** — Where players respawn within the instance
- **Floor number** — For multi-level dungeons
- **Flags** — Completion state (`completed`, `failed`)
- **Variables** — Script variables stored on the instance entity
- **Age / idle timer** — Tracks instance lifetime and inactivity

## Relationship to Dungeons and Blueprints

The dungeon system has three layers:

| Layer | Entity | Scripts | Purpose |
|-------|--------|---------|---------|
| **Blueprint** | Dungeon definition | Dungeon programs | Template — defines layout, mob placement, loot tables |
| **Instance** | Runtime copy | Instance programs | Live copy — one per party, isolated state |
| **Content** | Rooms, mobs, objects | Mob/obj/room programs | Individual entities within the instance |

When a dungeon is spawned (via `spawndungeon` or the dungeon queue system), the engine:

1. Creates an **instance** from the dungeon blueprint
2. Copies rooms, exits, and spawn tables into the instance
3. Populates the instance with mobs and objects
4. Attaches instance programs from the blueprint's prog list
5. Fires the `repop` trigger on the new instance

Instance programs then take over, managing the lifecycle from that point forward.

## Creating Instance Programs with `ipedit`

The `ipedit` editor manages instance program attachments on dungeon blueprints. Instance scripts are written as standard prog scripts and attached to the blueprint — they run on each instance created from that blueprint.

### Step 1: Write the Script

Create a script with type `InstanceProg` in your area's script list:

```
sedit create
type instanceprog
```

Write the script body using instance commands:

```
** Track boss kills and mark instance complete
if varexists $self bosses_killed
  if var $self bosses_killed >= 3
    instance instancecomplete $self
  endif
endif
```

### Step 2: Attach to the Blueprint

Open the dungeon blueprint editor and attach the script with a trigger:

```
ipedit <blueprint>
addiprog <script vnum> <trigger> <phrase>
```

For example, to run a script every tick:

```
addiprog 500 tick 100
```

### Step 3: Test

Spawn an instance of the dungeon and verify the scripts fire:

```
spawndungeon <dungeon vnum>
```

### Removing Programs

```
ipedit <blueprint>
deliprog <script vnum> <trigger> <phrase>
```

## Instance Capabilities at a Glance

| Capability | Commands | Notes |
|-----------|----------|-------|
| Communication | `echoat`, `questechoat`, `mail`, `wiznet` | Message players in the instance |
| Entity loading | `mload`, `oload`, `loadinstanced`, `makeinstanced` | Spawn instance-scoped content |
| Instance lifecycle | `instancecomplete`, `instancefailure` | Mark completion or failure |
| Dungeon control | `dungeoncommence`, `dungeoncomplete`, `dungeonfailure` | Control the parent dungeon |
| Floor management | `sendfloor` | Move players between floors |
| Room management | `alterroom`, `resetroom` | Modify instance rooms |
| Variables | `varset`, `varseton`, `varclear`, `varclearon`, `varcopy`, `varsave`, `varsaveon` | Instance-wide state |
| Quest integration | `quest`, `specialkey` | Dungeon quest objectives and keys |
| Script control | `call`, `xcall` | Subroutines and cross-entity calls |
| String modification | `stringmob`, `stringobj` | Customize instance mob/obj names |
| Wilderness | `wildernessmap`, `wildsanchor`, `wildsoverlay`, `wildstile`, `wildsvlink` | Wilderness integration |
| Events | `startevent`, `stopevent`, `phaseevent`, `stageevent` | Event system integration |
| Reckoning | `reckoning`, `startreckoning`, `stopreckoning` | World event system |
| Unlocking | `unlockarea`, `unlockdungeon` | Unlock gated content |
| Player | `mute`, `unmute` | Manage player communication |

## Quick Reference

| Topic | Page |
|-------|------|
| All 7 triggers | [Triggers](iprog-triggers.md) |
| All 48 commands | [Commands](iprog-commands.md) |
| Working examples | [Examples](iprog-examples.md) |
| Shared command details | [Shared Commands](../shared-commands.md) |
| Dungeon programs (blueprints) | [Dungeon Programs](../dungeon-programs/) |
| Variables and tokens | [Variables and Tokens](../variables-and-tokens.md) |
| If-check reference | [If-Checks](../ifchecks-reference.md) |
