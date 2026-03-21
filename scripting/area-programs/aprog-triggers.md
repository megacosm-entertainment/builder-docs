---
layout: default
title: Triggers
nav_order: 1
parent: Area Programs
grand_parent: Scripting
---

# Area Program Triggers
{: .no_toc }

This page documents all 6 triggers available for area programs. Area programs have a deliberately limited trigger set — each one serves a distinct purpose in zone-level scripting.

For general trigger concepts and phrase matching, see [Scripting Basics](../scripting-basics.md). For the commands you can use inside a triggered script, see [Commands](aprog-commands.md).

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## How Triggers Work

When you attach a script to an area, you specify:

1. **Trigger type** — The event name (e.g., `reset`, `random`, `tick`)
2. **Phrase** — A percent chance (1–100) that the script fires when the event occurs

All area triggers use percent-chance phrases:

| Phrase Value | Meaning |
|-------------|---------|
| `100` | Fires every time the event occurs |
| `50` | 50% chance of firing each time |
| `1` | 1% chance — very rare |
| `*` | Always fires (equivalent to 100) |

## Entity Bindings

Area programs have limited entity context compared to mob or room programs. Because they fire at the zone level, there is no inherent "actor" or "room":

| Variable | Type | Description |
|----------|------|-------------|
| `$i` / `$(self)` | Area | The area entity itself |

Other variables (`$n`, `$t`, `$r`, etc.) are **not automatically set** in area programs. You must target entities explicitly by name or vnum when using commands like `area echoat` or `area mload`.

---

## Trigger Quick Reference

| Trigger | When It Fires | Phrase |
|---------|--------------|-------|
| [`moon`](#moon) | When the in-game moon phase changes | percent chance |
| [`prereckoning`](#prereckoning) | Just before a reckoning event begins | percent chance |
| [`random`](#random) | On each random tick (periodic game pulse) | percent chance |
| [`reckoning`](#reckoning) | When a reckoning event is actively occurring | percent chance |
| [`reset`](#reset) | When the area resets (repopulation cycle) | percent chance |
| [`tick`](#tick) | On each game tick (regular interval) | percent chance |

---

## Trigger Details

### moon

```
addprog <vnum> moon <percent>
```

Fires when the in-game moon phase changes. The phrase is a percent chance of the script firing each time the moon transitions to a new phase.

**When it fires:** Each time the game's lunar cycle advances to the next phase (new moon, waxing crescent, first quarter, waxing gibbous, full moon, waning gibbous, third quarter, waning crescent).

**Use cases:**
- Spawning werewolves or other moon-sensitive creatures during full moon
- Changing area descriptions or ambient messages based on lunar phase
- Triggering lycanthropy-related events
- Activating moonlight-dependent puzzles or portals

**Example:**

```
** Spawn special mobs during full moon
if moon == full
  area echo {WThe full moon bathes the forest in silver light...{x
  area mload 5032 3001
endif
```

**Attachment:**

```
aedit
addprog 12345 moon 100
```

{: .note }
> The `moon` trigger is also available on tokens and events but **not** on mobs, objects, rooms, instances, or dungeons. It is unique to entity types that manage global or persistent state.

---

### prereckoning

```
addprog <vnum> prereckoning <percent>
```

Fires just before a reckoning event begins in the area. A reckoning is a major world event — an invasion, a cataclysm, or a server-wide challenge. The `prereckoning` trigger lets you prepare the zone before the event starts.

**When it fires:** After `startreckoning` is called but before the reckoning event fully activates. This is the setup phase.

**Use cases:**
- Broadcasting warning messages to players in the zone
- Setting up defensive positions or spawning preparatory NPCs
- Initializing zone variables that track event progress
- Modifying room descriptions to foreshadow the coming event

**Example:**

```
** Warn players that a reckoning is about to begin
area echo {RThe sky darkens ominously. Something terrible approaches...{x
area varset reckoning_stage preparing
area mload 6000 3050
```

**Attachment:**

```
aedit
addprog 12346 prereckoning 100
```

---

### random

```
addprog <vnum> random <percent>
```

Fires on each random tick with the specified percent chance. Random ticks occur periodically but at slightly irregular intervals, making events feel less mechanical.

**When it fires:** Each random tick pulse. The phrase is evaluated as a percent chance — `25` means a 25% chance of firing each random tick.

{: .warning }
> The `random` trigger is marked as deprecated in the source code. For new scripts, prefer the `tick` trigger which fires on regular game ticks. Existing `random` triggers continue to work.

**Use cases:**
- Ambient area messages (weather, sounds, environmental flavor)
- Periodic NPC spawning with controlled frequency
- Random events that should feel organic rather than clockwork
- Variable tracking that accumulates over time

**Example:**

```
** Random ambient weather messages
area echo {cA cool breeze drifts through the valley, carrying the scent of pine.{x
```

**Attachment:**

```
aedit
addprog 12347 random 15
```

This attaches with a 15% chance of firing each random tick — roughly once every 6–7 ticks on average.

---

### reckoning

```
addprog <vnum> reckoning <percent>
```

Fires while a reckoning event is actively occurring in the area. This trigger fires repeatedly (each tick) during the reckoning, allowing scripts to drive the event's progression.

**When it fires:** Each tick while the area's reckoning event is active. The phrase is a percent chance per tick.

**Use cases:**
- Spawning waves of invading mobs during the event
- Escalating difficulty as the reckoning progresses
- Tracking player kills or objectives during the event
- Sending periodic status updates to players in the zone

**Example:**

```
** Spawn a wave of invaders during the reckoning
area varset wave_count $@(wave_count + 1)
if var wave_count <= 5
  area echo {RAnother wave of demons pours through the rift!{x
  area mload 6010 3001
  area mload 6010 3002
  area mload 6010 3003
elseif var wave_count == 10
  area echo {YThe demon lord himself steps through the portal!{x
  area mload 6099 3001
endif
```

**Attachment:**

```
aedit
addprog 12348 reckoning 100
```

---

### reset

```
addprog <vnum> reset <percent>
```

Fires when the area undergoes its periodic reset (repopulation). Area resets restore mobs, objects, and room states according to the zone's reset list. The `reset` trigger lets you run custom logic alongside or instead of the standard reset.

**When it fires:** Each time the area's reset timer expires and the zone repopulates. The phrase is a percent chance of the script firing on each reset.

**Use cases:**
- Custom mob placement that varies based on zone variables
- Conditional repopulation (only spawn certain mobs if a quest flag is set)
- Resetting zone-wide state variables
- Announcing area repopulation to players
- Tracking how many times the area has been reset

**Example:**

```
** Custom reset logic — spawn a rare merchant 20% of the time
area varset reset_count $@(reset_count + 1)
if rand 20
  area mload 5050 3010
  area echo {GA traveling merchant has arrived at the crossroads!{x
  area varset merchant_spawned 1
else
  area varset merchant_spawned 0
endif
```

**Attachment:**

```
aedit
addprog 12349 reset 100
```

{: .note }
> The `reset` trigger is also available on rooms, tokens, instances, dungeons, and events, but **not** on mobs or objects. Room reset triggers fire for individual rooms; area reset triggers fire once for the entire zone.

---

### tick

```
addprog <vnum> tick <percent>
```

Fires on each regular game tick. Unlike `random`, the `tick` trigger fires at consistent intervals, making it suitable for predictable periodic behavior.

**When it fires:** Each game tick (a fixed-interval server pulse). The phrase is a percent chance per tick.

**Use cases:**
- Regular status checks and state machine progression
- Timed countdowns (decrement a variable each tick)
- Periodic zone-wide effects (weather cycling, day/night transitions)
- Monitoring player activity or zone conditions

**Example:**

```
** Countdown timer for a zone event
if var countdown > 0
  area varset countdown $@(countdown - 1)
  if var countdown == 5
    area echo {YThe ritual will complete in 5 ticks!{x
  elseif var countdown == 0
    area echo {RThe ritual is complete! The portal opens!{x
    area mload 7000 3001
  endif
endif
```

**Attachment:**

```
aedit
addprog 12350 tick 100
```

---

## Trigger Comparison

| Trigger | Timing | Typical Use |
|---------|--------|-------------|
| `moon` | Lunar phase change | Lycanthropy events, moon-gated content |
| `prereckoning` | Before reckoning starts | Event preparation, warnings |
| `random` | Irregular intervals *(deprecated)* | Ambient flavor, organic-feeling events |
| `reckoning` | During active reckoning | Event progression, wave spawning |
| `reset` | Area repopulation | Custom spawning, state reset |
| `tick` | Regular intervals | Timers, countdowns, periodic checks |

{: .important }
> Area programs have only 6 triggers — this is by design. Areas are high-level containers, and most interactive behavior belongs in mob, room, or token programs. Use area programs for zone-wide coordination, not for responding to individual player actions.
