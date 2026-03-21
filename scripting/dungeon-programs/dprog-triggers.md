---
layout: default
title: Triggers
nav_order: 1
parent: Dungeon Programs
grand_parent: Scripting
---

# Dungeon Program Triggers
{: .no_toc }

This page documents all 9 triggers available for dungeon programs (dungeon progs). Triggers define *when* a script fires — the event that causes your code to run.

For general trigger concepts and phrase matching, see [Scripting Basics](../scripting-basics.md). For the commands you can use inside a triggered script, see [Commands](dprog-commands.md).

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## How Triggers Work

When you attach a script to a dungeon blueprint, you specify:

1. **Trigger type** — The event name (e.g., `dungeon_commenced`, `random`, `completed`)
2. **Phrase** — A percent chance (0–100) that controls whether the script fires

All 9 dungeon triggers use **percent-based phrases**. The engine generates a random number from 1 to 100; if the number is less than or equal to the phrase value, the script fires. A phrase of `100` always fires; `50` fires roughly half the time; `0` never fires.

### Attaching a Trigger

Use the dungeon editor (`dedit`) to attach a script to a dungeon blueprint:

```
dedit <dungeon vnum>
addprog <script vnum> <trigger_name> <percent>
```

**Example:**

```
dedit 100
addprog 101 dungeon_commenced 100
addprog 102 random 15
```

## Entity Bindings

When a dungeon trigger fires, your script can reference:

| Variable | Type | Description |
|----------|------|-------------|
| `$self` / `$(dungeon)` | Dungeon | The dungeon blueprint (self-reference) |
| `$n` / `$(enactor)` | Mobile | The character who triggered the event (when applicable) |
| `$(phrase)` | String | The matched trigger phrase |

{: .note }
> Not all triggers provide an enactor (`$n`). Triggers like `random`, `tick`, `reset`, and `repop` fire on a timer with no specific character involved.

---

## Trigger Quick Reference

| Trigger | When It Fires | Phrase |
|---------|--------------|-------|
| [`completed`](#completed) | Dungeon is marked as successfully completed | percent (0–100) |
| [`dungeon_commenced`](#dungeon_commenced) | Dungeon is officially started | percent (0–100) |
| [`dungeon_schematic`](#dungeon_schematic) | During dungeon creation when scripted levels are enabled | percent (0–100) |
| [`entry`](#entry) | A character enters the dungeon | percent (0–100) |
| [`failed`](#failed) | Dungeon is marked as failed | percent (0–100) |
| [`random`](#random) | Random tick during the dungeon update cycle | percent (0–100) |
| [`repop`](#repop) | Dungeon repopulation event | percent (0–100) |
| [`reset`](#reset) | Dungeon age exceeds the repop timer | percent (0–100) |
| [`tick`](#tick) | Per-pulse game tick | percent (0–100) |

---

## Lifecycle Triggers

These triggers correspond to key events in the dungeon's lifecycle — creation, commencement, completion, and failure.

### dungeon_schematic

Fires during dungeon creation when the `DUNGEON_SCRIPTED_LEVELS` flag is set on the dungeon blueprint. This trigger allows scripts to dynamically define the dungeon's level layout instead of using static floor definitions.

- **Phrase:** percent chance (0–100)
- **Bindings:** `$self` = the dungeon being created
- **Fires from:** `create_dungeon()` during instantiation

**When to use:** When you want procedurally generated floor layouts. Set the `DUNGEON_SCRIPTED_LEVELS` flag in the dungeon editor, then attach a script to this trigger that defines which floors to include and how they connect.

```
** Dynamically generate a 3-floor dungeon layout
** Fires on dungeon_schematic
dungeon varset $self num_floors 3
dungeon echoat $self {DThe dungeon structure materializes...{x
```

{: .warning }
> This trigger fires during dungeon creation, before any players have entered. Do not reference `$n` or attempt player-targeted commands.

### dungeon_commenced

Fires when a script calls `dungeon dungeoncommence` on the dungeon instance. This is a one-time event — once the `DUNGEON_COMMENCED` flag is set, this trigger will not fire again for the same instance.

- **Phrase:** percent chance (0–100)
- **Bindings:** `$self` = the dungeon
- **Fires after:** the `DUNGEON_COMMENCED` flag is set

**When to use:** To initialize combat encounters, activate boss timers, display introductory messages, or begin tracking dungeon progression.

```
** Dungeon officially begins
** Fires on dungeon_commenced
dungeon echoat $self {YThe dungeon doors slam shut behind you!{x
dungeon varset $self phase 1
dungeon startreckoning 30
```

### completed

Fires when a script calls `dungeon dungeoncomplete` on the dungeon instance. This is a one-time event — once the `DUNGEON_COMPLETED` flag is set, this trigger will not fire again.

- **Phrase:** percent chance (0–100)
- **Bindings:** `$self` = the dungeon
- **Fires before:** the `DUNGEON_COMPLETED` flag is set

**When to use:** To handle dungeon completion rewards, unlock subsequent dungeons for players, display victory messages, or trigger cleanup logic.

```
** Dungeon successfully completed
** Fires on completed trigger
dungeon echoat $self {GVictory! The dungeon has been conquered!{x
```

### failed

Fires when a script calls `dungeon dungeonfailure` on the dungeon instance. This is a one-time event — once the `DUNGEON_FAILED` flag is set, this trigger will not fire again.

- **Phrase:** percent chance (0–100)
- **Bindings:** `$self` = the dungeon
- **Fires after:** the `DUNGEON_FAILED` flag is set

**When to use:** To handle failure conditions — resetting the dungeon, displaying defeat messages, or applying penalties.

```
** Dungeon failed
** Fires on failed trigger
dungeon echoat $self {RThe dungeon collapses around you! All is lost!{x
```

---

## Entry Trigger

### entry

Fires when a character enters the dungeon or transfers into a room within the dungeon (via portal, movement, or script transfer).

- **Phrase:** percent chance (0–100)
- **Bindings:** `$self` = the dungeon, `$n` = the entering character
- **Fires from:** character movement into dungeon rooms

**When to use:** To greet players entering the dungeon, check prerequisites, apply dungeon-specific buffs or debuffs, or auto-commence the dungeon when the first player arrives.

```
** Welcome message on entry
** Fires on entry trigger
dungeon echoat $n {cYou feel a chill as you step into the depths...{x
if dungeonflag $self commenced
else
  dungeon dungeoncommence $self
endif
```

{: .note }
> The `entry` trigger fires for every character who enters, including NPCs that are transferred into the dungeon. Use `if isnpc $n` to filter if needed.

---

## Periodic Triggers

These triggers fire on repeating game cycles, useful for ambient effects, timed events, and progressive difficulty.

### random

Fires during the dungeon update cycle on each game pulse. Use the phrase to control how frequently the script actually executes.

- **Phrase:** percent chance (0–100)
- **Bindings:** `$self` = the dungeon
- **No enactor** — this is a timer-based trigger

**When to use:** For ambient dungeon effects, periodic difficulty scaling, spawning random encounters, or timed warnings.

```
** Random ambient effects
** Fires on random trigger (15% chance per tick)
dungeon echoat $self {dStrange whispers echo through the corridors...{x
```

### tick

Fires on each game pulse, similar to `random` but using the random trigger slot. Use this for per-pulse logic that needs to run at a controlled frequency.

- **Phrase:** percent chance (0–100)
- **Bindings:** `$self` = the dungeon
- **No enactor** — this is a timer-based trigger

**When to use:** For time-based mechanics — countdown timers, periodic damage, or progressive environmental changes.

```
** Countdown timer
** Fires on tick trigger (100% each pulse)
if varexists $self timer
  dungeon varset $self timer -= 1
  if var $self timer <= 0
    dungeon dungeonfailure $self
  endif
endif
```

---

## Reset Triggers

These triggers handle dungeon repopulation and reset cycles.

### repop

Fires during dungeon repopulation — both after initial dungeon creation and during periodic repop cycles. Use this to spawn mobs, place objects, or set initial room conditions.

- **Phrase:** percent chance (0–100)
- **Bindings:** `$self` = the dungeon
- **No enactor** — this is a system event

**When to use:** To customize what spawns in the dungeon, override default reset behavior, or conditionally populate rooms based on dungeon state.

```
** Custom repop logic
** Fires on repop trigger
if var $self boss_defeated == 0
  dungeon loadinstanced mobile 50100 $self.entry_room
endif
```

### reset

Fires when the dungeon's age exceeds its configured repop timer. This is the dungeon-wide reset event, similar to area resets.

- **Phrase:** percent chance (0–100)
- **Bindings:** `$self` = the dungeon
- **No enactor** — this is a system event

**When to use:** To perform cleanup on dungeon reset, respawn bosses, restore room descriptions, or reset progression variables.

```
** Full dungeon reset
** Fires on reset trigger
dungeon varset $self boss_defeated 0
dungeon varset $self phase 0
dungeon echoat $self {dThe dungeon stirs... darkness reclaims its halls.{x
```
