---
layout: default
title: Triggers
nav_order: 1
parent: Instance Programs
grand_parent: Scripting
---

# Instance Program Triggers
{: .no_toc }

Instance programs support **7 triggers** that fire based on periodic ticks, instance lifecycle events, player entry, and repop/reset cycles. Because instances manage the lifecycle of a private dungeon copy, their trigger set focuses on timing, state transitions, and player movement.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## How Triggers Work

Each trigger is attached to an instance program with a **phrase** that controls when it fires. For instance triggers, the phrase is always a **percent chance** — a number from 1 to 100 indicating the probability of the trigger firing each time the underlying event occurs.

| Phrase Value | Meaning |
|-------------|---------|
| `100` | Always fires (100% chance) |
| `50` | Fires half the time |
| `1` | Fires 1% of the time (rare) |

### Variable Bindings

When a trigger fires, the script receives context through variables:

| Variable | Alias | Description |
|----------|-------|-------------|
| `$self` / `$this` | `$i` | The instance entity |
| `$enactor` | `$n` | Character who triggered the event (if applicable) |
| `$phrase` | — | Matched phrase/argument |
| `$trigger` | — | Trigger phrase pattern that matched |
| `$register1`–`$register5` | — | Numeric registers for passing data |

### Attaching Triggers

Triggers are attached to instance blueprints through the `ipedit` editor:

```
ipedit <blueprint>
addiprog <script vnum> <trigger> <phrase>
```

For example, to attach a script that fires every tick:

```
addiprog 500 tick 100
```

To remove an attached program:

```
deliprog <script vnum> <trigger> <phrase>
```

---

## Lifecycle Triggers

### completed

Fires when the instance is marked as successfully completed — either by a script calling `instance instancecomplete` or by the engine detecting all objectives are met.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the instance entity
- **Fires:** Once, when `INSTANCE_COMPLETED` flag is set
- **Also available on:** Dungeon

Use this trigger for post-completion logic: awarding rewards, opening exit portals, spawning loot chests, or sending congratulatory messages.

```
** Instance completed — reward players and open the exit
instance echoat $n {G*** DUNGEON COMPLETE ***{x
instance echoat $n {YA shimmering portal appears, leading back to the surface.{x

** Spawn a reward chest at the boss room
instance oload 8500

** Set a variable so we don't double-reward on subsequent ticks
instance varset rewards_given number 1
```

{: .note }
> The `completed` trigger only fires once per completion. If you need ongoing post-completion behavior, set a variable in the completed handler and check it in the `tick` trigger.

### failed

Fires when the instance is marked as failed — either by a script calling `instance instancefailure` or by the engine determining the instance cannot be completed (e.g., all players dead, timer expired).

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the instance entity
- **Fires:** Once, when `INSTANCE_FAILED` flag is set
- **Also available on:** Dungeon

Use this trigger for failure cleanup: messaging players, resetting state, or offering a retry option.

```
** Instance failed — notify players
instance echoat $n {R*** DUNGEON FAILED ***{x
instance echoat $n {wThe dungeon crumbles around you. Better luck next time.{x

** Log the failure
instance wiznet Instance failed: blueprint $blueprintname($self)
```

{: .warning }
> Once an instance is marked as failed, it cannot be un-failed. The `failed` trigger is a terminal state handler — use it only for cleanup and messaging.

---

## Entry Trigger

### entry

Fires when a character enters a room within the instance. This includes walking, teleporting, and being transferred.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the instance entity, `$enactor` = the entering character
- **Fires:** Each time any character enters any room in the instance
- **Also available on:** Mob, Obj, Room, Token, Dungeon

Use this trigger to track player movement through the instance, trigger encounters on room entry, or update progress when players reach specific rooms.

```
** Track unique rooms visited
if ispc $n
  ** Count player entries for difficulty scaling
  if varexists $self entry_count
    instance varset entry_count number $regmath($getvar($self entry_count) + 1)
  else
    instance varset entry_count number 1
  endif
endif
```

{: .note }
> The `entry` trigger fires for every character movement in the instance, including NPCs. Use `if ispc $n` to filter for player entries only. Be mindful of performance — keep entry trigger scripts lightweight.

---

## Periodic Triggers

### tick

Fires every game tick while the instance is active. This is the primary heartbeat for instance logic — use it for progress tracking, timed events, phase management, and periodic spawns.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the instance entity
- **Fires:** Every game tick (approximately 30 seconds)
- **Also available on:** Mob, Obj, Room, Token, Area, Dungeon, Event

```
** Main instance tick handler — manage phases
if varexists $self phase
else
  instance varset phase number 1
endif

** Check time limit
if var $self age >= 120
  instance echoat $n {RTime is running out! The dungeon begins to collapse!{x
  if var $self age >= 150
    instance instancefailure $self
  endif
endif

** Periodic atmosphere
if rand 15
  instance echoat $n {wThe walls shudder. Something stirs deeper in the dungeon...{x
endif
```

{: .note }
> The `tick` trigger is the workhorse of instance programs. Most instance logic — phase progression, timers, periodic spawns, and status updates — runs on tick.

### random

Fires on each random tick while the instance is active. Random ticks occur at irregular intervals, making this trigger useful for unpredictable effects.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the instance entity
- **Fires:** Each random tick (irregular interval)
- **Also available on:** Mob, Obj, Room, Token, Area, Dungeon, Event

```
** Random environmental effects
if rand 30
  instance echoat $n {DA low rumble echoes through the corridors...{x
endif
```

{: .warning }
> The `random` trigger is deprecated. Use `tick` with `rand` if-checks for randomized behavior in new scripts.

---

## Reset Triggers

### repop

Fires when the instance undergoes a repop cycle. Repop restores spawned entities and refreshes the instance according to its blueprint. This typically happens when the instance is first created and on periodic refresh cycles.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the instance entity
- **Fires:** On instance repopulation cycle
- **Also available on:** Mob, Obj, Token, Dungeon

Use this trigger for initial instance setup, spawning custom content that goes beyond the blueprint's standard spawn tables, or adjusting the instance after a repop.

```
** Instance repop — set up custom spawns and scale to party size
** This fires when the instance is first created

** Spawn the main boss
instance loadinstanced mobile 9001 $entrance($self)
instance makeinstanced $lastloaded

** Set initial state
instance varset bosses_remaining number 3
instance varset difficulty number 1
```

{: .note }
> The `repop` trigger is the best place for one-time initialization logic. It fires when the instance is first populated. Combine with a variable check to distinguish first-time setup from subsequent repops.

### reset

Fires when the area associated with the instance resets. Area resets happen on a regular cycle (typically every 10–15 minutes).

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$self` = the instance entity
- **Fires:** On area reset cycle
- **Also available on:** Room, Token, Area, Dungeon, Event

Use this trigger for periodic maintenance — restocking consumables, refreshing traps, or cleaning up dead mobs.

```
** Reset handler — restock health potions at checkpoints
instance oload 3001
instance oload 3001
instance oload 3002
```

---

## Trigger Summary Table

| Trigger | Category | Fires When | Typical Use |
|---------|----------|------------|-------------|
| `completed` | Lifecycle | Instance marked complete | Rewards, exit portals, congratulations |
| `failed` | Lifecycle | Instance marked failed | Failure messaging, cleanup |
| `entry` | Movement | Character enters a room | Progress tracking, encounter triggers |
| `tick` | Periodic | Every game tick (~30s) | Phase management, timers, spawns |
| `random` | Periodic | Random tick (irregular) | Deprecated — use `tick` with `rand` |
| `repop` | Reset | Instance repopulation | Initial setup, custom spawns |
| `reset` | Reset | Area reset cycle | Restocking, maintenance |

---

## Trigger Combinations

Most instances use multiple triggers together:

### Standard Dungeon Instance (Tick + Repop + Completed + Failed)

```
addiprog 500 tick 100          ** Main instance logic
addiprog 501 repop 100         ** Initial setup
addiprog 502 completed 100     ** Reward distribution
addiprog 503 failed 100        ** Failure cleanup
```

### Entry-Tracked Instance (Entry + Tick + Completed)

```
addiprog 600 entry 100         ** Track player movement
addiprog 601 tick 100          ** Check objectives
addiprog 602 completed 100     ** Final rewards
```

### Timed Challenge (Tick + Repop + Failed)

```
addiprog 700 repop 100         ** Initialize timer
addiprog 701 tick 100          ** Countdown and phase logic
addiprog 702 failed 100        ** Time-out failure handler
```
