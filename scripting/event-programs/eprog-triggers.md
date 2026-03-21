---
layout: default
title: Triggers
nav_order: 1
parent: Event Programs
grand_parent: Scripting
---

# Event Program Triggers
{: .no_toc }

Event programs support **6 triggers** that fire based on periodic ticks, area resets, moon phases, and reckoning events. Because events operate at a game-wide or area-wide scope rather than reacting to individual player actions, their trigger set is intentionally small and focused on timing and world-state changes.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## How Triggers Work

Each trigger is attached to an event program with a **phrase** that controls when it fires. For event triggers, the phrase is always a **percent chance** — a number from 1 to 100 indicating how likely the trigger is to fire each time the underlying event occurs.

| Phrase Value | Meaning |
|-------------|---------|
| `100` | Always fires (100% chance) |
| `50` | Fires half the time |
| `1` | Fires 1% of the time (rare) |

### Variable Bindings

When a trigger fires, the script receives context through variables:

| Variable | Alias | Description |
|----------|-------|-------------|
| `$self` / `$this` | `$i` | The event entity |
| `$enactor` | `$n` | Character who triggered the event (if applicable) |
| `$phrase` | — | Matched phrase/argument |
| `$trigger` | — | Trigger phrase pattern that matched |
| `$register1`–`$register5` | — | Numeric registers for passing data |

### Attaching Triggers

Triggers are attached to events through the `evtedit` editor:

```
evtedit <event vnum>
addeprog <script vnum> <trigger> <phrase>
```

For example, to attach a script that fires every tick:

```
addeprog 500 tick 100
```

To remove an attached program:

```
deleprog <script vnum> <trigger> <phrase>
```

---

## Periodic Triggers

### tick

Fires every game tick while the event is active. This is the primary heartbeat for event logic — use it to check progress, spawn content, update participants, and advance stages.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the event entity
- **Fires:** Every game tick (approximately 30 seconds)

```
** Track event progress each tick
if eventprogress $self >= 100
  event gecho {GThe collection goal has been reached!{x
  event phaseevent
else
  ** Spawn items periodically
  if rand 30
    event oload 5001
    event gecho {YNew moonberries have appeared!{x
  endif
endif
```

{: .note }
> The `tick` trigger is the workhorse of event programs. Most event logic — progress tracking, stage transitions, periodic spawns, and status announcements — runs on tick.

### random

Fires on each random tick while the event is active. Random ticks occur at irregular intervals, making this trigger useful for unpredictable effects.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the event entity
- **Fires:** Each random tick (irregular interval)
- **Notes:** Deprecated — prefer `tick` for new scripts

```
** Random atmospheric effect during event
event gecho {DThe sky darkens momentarily as storm clouds gather...{x
```

{: .warning }
> The `random` trigger is deprecated. Use `tick` with `rand` if-checks for randomized behavior in new scripts.

### reset

Fires when the area associated with the event resets. Area resets happen on a regular cycle (typically every 10–15 minutes). Use this for spawning event-related content that should refresh with the area.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the event entity
- **Fires:** On area reset cycle

```
** Restock event vendors and spawn guards on area reset
event mload 8001
event mload 8002
event mload 8003
event gecho {CThe arena guards take their positions.{x
```

{: .note }
> For area-scoped events, `reset` fires when the anchor area resets. For global events, `reset` fires on each area reset within the game world, so use it carefully to avoid excessive spawning.

### moon

Fires when a moon phase changes. Sentience tracks a lunar cycle that affects gameplay, and this trigger lets events react to moon transitions.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the event entity
- **Fires:** On each moon phase transition

```
** Lunar event — spawn special mobs during full moon
event mload 9500
event gecho {WThe full moon bathes the world in pale light. Lycanthropes stir...{x
```

{: .note }
> The `moon` trigger is ideal for events that are thematically tied to lunar cycles, such as werewolf hunts, harvest festivals, or tide-based events.

---

## Reckoning Triggers

Reckoning is Sentience's world-event system — large-scale happenings that progress through phases and affect the entire game. Events can participate in reckonings by reacting to their start and progression.

### prereckoning

Fires when a reckoning event is about to begin. Use this to prepare the event — announce warnings, spawn preliminary content, or set up the stage before the reckoning fully activates.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the event entity
- **Fires:** Just before a reckoning starts

```
** Warn players that a reckoning is approaching
event gecho {RThe ground trembles. Something terrible is coming...{x
event varset reckoning_phase number 0
```

### reckoning

Fires when a reckoning event is actively in progress. Use this for ongoing reckoning effects — spawning waves, dealing damage, advancing phases, and tracking the reckoning's progression.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the event entity
- **Fires:** During active reckoning progression

```
** Reckoning wave — spawn invaders
event mload 9700
event mload 9701
event mload 9702
event gecho {RAnother wave of invaders pours through the breach!{x
```

---

## Trigger Summary Table

| Trigger | Category | Fires When | Typical Use |
|---------|----------|------------|-------------|
| `tick` | Periodic | Every game tick (~30s) | Progress tracking, spawns, stage logic |
| `random` | Periodic | Random tick (irregular) | Deprecated — use `tick` with `rand` |
| `reset` | Periodic | Area reset cycle | Restocking NPCs, refreshing objects |
| `moon` | World state | Moon phase changes | Lunar-themed events |
| `prereckoning` | Reckoning | Before reckoning starts | Warnings, preparation |
| `reckoning` | Reckoning | During active reckoning | Waves, progression, effects |

---

## Trigger Combinations

Most events use multiple triggers together:

### Basic Event (Tick + Reset)

```
addeprog 500 tick 100       ** Main event logic
addeprog 501 reset 100      ** Respawn event content
```

### Reckoning Event (PreReckoning + Reckoning + Tick)

```
addeprog 600 prereckoning 100   ** Preparation phase
addeprog 601 reckoning 100      ** Active reckoning logic
addeprog 602 tick 50            ** Periodic effects during event
```

### Seasonal Lunar Event (Moon + Tick)

```
addeprog 700 moon 100       ** React to moon phase change
addeprog 701 tick 100       ** Ongoing event management
```
