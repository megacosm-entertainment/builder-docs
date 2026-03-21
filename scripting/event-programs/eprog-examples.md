---
layout: default
title: Examples
nav_order: 3
parent: Event Programs
grand_parent: Scripting
---

# Event Program Examples
{: .no_toc }

Practical examples of event programs covering common use cases. Each example includes the event definition setup, complete script code, trigger attachments, and an explanation of how the pieces fit together.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

{: .important }
> All event program commands use the `event` prefix. Comments use `**` on their own line.

---

## Example 1: PvP Arena Event — Free-for-All War

A competitive PvP event where players enter a battlefield arena and fight to accumulate kills. The event runs in three stages: a signup phase, the active battle, and a reward phase. Kill progress is tracked, and the top contributor wins.

### Event Definition (evtedit)

```
evtedit create
name Blood Arena — Free-for-All
description A brutal free-for-all PvP tournament in the Grand Arena.
type war-ffa
scope battlefield
scopeanchor <arena area>
duration 30
minlevel 20
maxlevel 0
minplayers 4
maxplayers 32
goal 50
stages 3
flags joinlate winner_only
announce {RThe Blood Arena is open! Type 'event join' to enter the free-for-all!{x
endmsg {RThe Blood Arena has closed! The champion has been crowned!{x
joinmsg {RYou step into the Blood Arena. Fight or die!{x
```

### Script 1: Event Startup (tick trigger — stage management)

```
** Blood Arena — main tick handler
** Trigger: tick 100

** Stage 0: Signup / warmup phase
if varexists $self stage
else
  ** First tick — initialize
  event varset stage number 0
  event varset stage_ticks number 0
  event gecho {R[Blood Arena] {WSignups are open! The battle begins in 2 minutes.{x
endif

** Increment tick counter
event varset stage_ticks number $regmath($getvar($self stage_ticks) + 1)

** Stage transitions based on tick count
if var $self stage == 0
  ** Signup phase — 4 ticks (~2 minutes)
  if var $self stage_ticks >= 4
    event varset stage number 1
    event varset stage_ticks number 0
    event phaseevent
    event gecho {R[Blood Arena] {YFIGHT! The battle has begun!{x
  else
    ** Countdown reminders
    if var $self stage_ticks == 2
      event gecho {R[Blood Arena] {W1 minute until the battle begins!{x
    endif
  endif
endif

if var $self stage == 1
  ** Active combat phase — 40 ticks (~20 minutes)
  ** Check if kill goal reached
  if eventprogress $self >= 50
    event varset stage number 2
    event varset stage_ticks number 0
    event phaseevent
    event gecho {R[Blood Arena] {GThe kill goal has been reached! Victory!{x
  endif

  ** Time limit
  if var $self stage_ticks >= 40
    event varset stage number 2
    event varset stage_ticks number 0
    event phaseevent
    event gecho {R[Blood Arena] {YTime's up! Tallying results...{x
  endif

  ** Periodic status updates
  if rand 20
    event gecho {R[Blood Arena] {wThe battle rages on...{x
  endif
endif

if var $self stage == 2
  ** Reward phase — runs for 2 ticks then stops
  if var $self stage_ticks >= 2
    event gecho {R[Blood Arena] {WThe arena closes. Return to your lives, warriors.{x
    event stopevent
  endif
endif
```

### Script 2: Area Reset — Refresh Arena

```
** Blood Arena — reset handler
** Trigger: reset 100

** Respawn arena healing fountains and weapon racks
event oload 8100 3500
event oload 8101 3501
event oload 8102 3502
event oload 8103 3503
```

### Trigger Attachments

```
evtedit <event vnum>
addeprog <script 1 vnum> tick 100
addeprog <script 2 vnum> reset 100
```

### How It Works

1. The event is started (manually by staff or on schedule).
2. The `tick` trigger fires every ~30 seconds, driving a state machine through the `stage` variable.
3. **Stage 0 (Signup):** Lasts ~2 minutes. Players join with `event join`. Countdown messages are sent.
4. **Stage 1 (Combat):** Lasts up to ~20 minutes. Kill progress is tracked automatically by the `war-ffa` event type. If 50 kills are reached early, the event advances.
5. **Stage 2 (Reward):** Runs for 2 ticks to allow reward processing, then calls `stopevent`.
6. The `reset` trigger replenishes arena supplies each area reset cycle.

---

## Example 2: Timed World Event — Elemental Invasion

A global invasion event where elemental creatures spawn across the world in waves. The event runs for a fixed duration with escalating difficulty across three stages.

### Event Definition (evtedit)

```
evtedit create
name Elemental Convergence
description Portals to the elemental planes tear open across the world.
type invasion
scope global
schedule recurring
interval 1440
duration 45
cooldown 720
variance 120
minlevel 10
maxlevel 0
minplayers 5
maxplayers 0
goal 200
stages 3
flags joinlate autoadd scaling repeatable
announce {MElemental portals are tearing open across the world! The Convergence has begun!{x
endmsg {MThe elemental portals seal shut. The Convergence has ended.{x
joinmsg {MYou feel the pull of elemental forces. You are part of the Convergence.{x
```

### Script 1: Invasion Waves (tick trigger)

```
** Elemental Convergence — main tick handler
** Trigger: tick 100

** Initialize on first tick
if varexists $self wave
else
  event varset wave number 1
  event varset tick_count number 0
endif

event varset tick_count number $regmath($getvar($self tick_count) + 1)

** Determine current wave intensity based on progress
if eventprogress $self >= 150
  ** Wave 3 — final push, strongest elementals
  if var $self wave < 3
    event varset wave number 3
    event phaseevent
    event gecho {M[Convergence] {RThe portals surge with violent energy! Greater elementals pour through!{x
  endif
  ** Spawn greater elementals
  if rand 40
    event mload 7503
    event mload 7504
  endif
else
  if eventprogress $self >= 75
    ** Wave 2 — mid-tier elementals
    if var $self wave < 2
      event varset wave number 2
      event phaseevent
      event gecho {M[Convergence] {YThe portals widen! Stronger elementals are emerging!{x
    endif
    ** Spawn mid-tier elementals
    if rand 50
      event mload 7501
      event mload 7502
    endif
  else
    ** Wave 1 — basic elementals
    if rand 60
      event mload 7500
    endif
  endif
endif

** Periodic global announcements
if var $self tick_count == 30
  event gecho {M[Convergence] {WThe elemental assault shows no sign of stopping...{x
endif

if var $self tick_count == 60
  event gecho {M[Convergence] {WThe portals are weakening! Keep fighting!{x
endif

** Check if goal reached
if eventprogress $self >= 200
  event gecho {M[Convergence] {GThe portals collapse! The elemental invasion has been repelled!{x
  event stopevent
endif
```

### Script 2: Portal Spawns on Reset

```
** Elemental Convergence — area reset spawner
** Trigger: reset 100

** Spawn visual portal objects in key locations
event oload 7600 1001
event oload 7600 2001
event oload 7600 3001

** Spawn a few elementals at portal sites
event mload 7500 1001
event mload 7500 2001
```

### Script 3: Cleanup on Event End

```
** Elemental Convergence — cleanup via tick
** This script is called via xcall from the stopevent handler
** or can be the last action before stopevent

** Reset altered rooms
event resetroom 1001
event resetroom 2001
event resetroom 3001

** Clear tracking variables
event varclear wave
event varclear tick_count
```

### Trigger Attachments

```
evtedit <event vnum>
addeprog <script 1 vnum> tick 100
addeprog <script 2 vnum> reset 100
```

### How It Works

1. The event starts on a recurring schedule (every 1440 minutes with 120-minute variance).
2. With `autoadd` flag, all eligible players in the world are automatically enrolled.
3. With `scaling` flag, spawn rates and difficulty adjust based on participant count.
4. The `tick` handler drives three waves:
   - **Wave 1 (0–74 kills):** Basic elementals spawn at 60% chance per tick.
   - **Wave 2 (75–149 kills):** Mid-tier elementals at 50% chance, with a stage advance announcement.
   - **Wave 3 (150+ kills):** Greater elementals at 40% chance for the final push.
5. The `reset` handler plants portal objects and base elementals at key locations each area cycle.
6. When 200 kills are reached, the event declares victory and stops.

---

## Example 3: Seasonal Boss Spawn — The Frost Wyrm

A seasonal boss event that spawns a powerful creature in a specific location. The event announces the boss's approach, spawns minions as guards, and tracks the boss kill as the completion condition.

### Event Definition (evtedit)

```
evtedit create
name The Frost Wyrm Awakens
description An ancient frost wyrm emerges from the northern glaciers.
type boss
scope area
scopeanchor <glacier area>
schedule manual
duration 60
minlevel 30
maxlevel 0
minplayers 6
maxplayers 0
goal 1
stages 3
flags joinlate scaling all_parts
announce {CThe ground trembles in the Northern Glaciers! The Frost Wyrm stirs!{x
endmsg {CThe Frost Wyrm has been defeated! Heroes of the realm, claim your rewards!{x
joinmsg {CYou feel the bitter cold of the Frost Wyrm's domain wash over you.{x
```

### Script 1: Boss Spawn Sequence (tick trigger)

```
** Frost Wyrm — main tick handler
** Trigger: tick 100

** Initialize
if varexists $self phase
else
  event varset phase number 0
  event varset phase_ticks number 0
endif

event varset phase_ticks number $regmath($getvar($self phase_ticks) + 1)

** Phase 0: Approach — environmental effects
if var $self phase == 0
  ** Atmospheric buildup for 6 ticks (~3 minutes)
  if var $self phase_ticks == 1
    event gecho {C[Frost Wyrm] {wA chill wind sweeps across the Northern Glaciers...{x
    event alterroom 4500 sector ice
  endif

  if var $self phase_ticks == 3
    event gecho {C[Frost Wyrm] {wThe glacier cracks. Something ancient stirs beneath.{x
    ** Spawn minion scouts
    event mload 9600 4501
    event mload 9600 4502
  endif

  if var $self phase_ticks >= 6
    event varset phase number 1
    event varset phase_ticks number 0
    event phaseevent
  endif
endif

** Phase 1: Minion wave — guard spawns
if var $self phase == 1
  if var $self phase_ticks == 1
    event gecho {C[Frost Wyrm] {RIce drakes burst from the glacier, defending their master!{x
    ** Spawn ice drake guards
    event mload 9601 4500
    event mload 9601 4501
    event mload 9601 4502
    event mload 9602 4500
  endif

  if var $self phase_ticks == 4
    event gecho {C[Frost Wyrm] {WMore drakes swarm from the crevasse!{x
    event mload 9601 4503
    event mload 9601 4504
    event mload 9602 4501
  endif

  ** Move to boss phase after 8 ticks
  if var $self phase_ticks >= 8
    event varset phase number 2
    event varset phase_ticks number 0
    event phaseevent
  endif
endif

** Phase 2: Boss active
if var $self phase == 2
  if var $self phase_ticks == 1
    ** Spawn the Frost Wyrm boss
    event mload 9650 4500
    event gecho {C[Frost Wyrm] {RTHE FROST WYRM ERUPTS FROM THE GLACIER!{x
    event gecho {C[Frost Wyrm] {YAll brave heroes — converge on the Northern Glaciers!{x
    event varset boss_spawned number 1
  endif

  ** Check if boss is defeated (goal = 1 kill)
  if eventprogress $self >= 1
    event gecho {C[Frost Wyrm] {GThe Frost Wyrm crashes to the ground, defeated!{x
    event gecho {C[Frost Wyrm] {WThe glaciers grow still. The ancient menace is no more.{x
    ** Spawn reward chest
    event oload 9660 4500
    event gecho {C[Frost Wyrm] {YA shimmering chest materializes where the wyrm fell!{x
    ** Reset altered terrain
    event resetroom 4500
    event stopevent
  endif

  ** Enrage timer — if boss is alive too long
  if var $self phase_ticks >= 80
    event gecho {C[Frost Wyrm] {RThe Frost Wyrm grows enraged! Ice storms batter the entire glacier!{x
    ** Could alter more rooms, spawn more adds, etc.
  endif

  ** Periodic roar effects
  if rand 15
    event gecho {C[Frost Wyrm] {wA deafening roar echoes across the glaciers!{x
  endif
endif
```

### Script 2: Minion Respawns on Reset

```
** Frost Wyrm — reset handler
** Trigger: reset 100

** Only respawn minions if we're in the boss phase and boss is still alive
if varexists $self boss_spawned
  ** Respawn some ice drake guards
  event mload 9601 4501
  event mload 9601 4502
endif
```

### Script 3: Moon Phase Enhancement

```
** Frost Wyrm — moon handler
** Trigger: moon 100

** If the event is active during a full moon, add frost effects
event gecho {C[Frost Wyrm] {wThe moon casts an eerie glow over the glaciers. The cold intensifies.{x
** Could buff the boss, spawn extra minions, etc.
event mload 9600 4503
event mload 9600 4504
```

### Trigger Attachments

```
evtedit <event vnum>
addeprog <script 1 vnum> tick 100
addeprog <script 2 vnum> reset 100
addeprog <script 3 vnum> moon 50
```

### How It Works

1. The event is started manually by staff (schedule set to `manual`) — ideal for seasonal events that run at specific times.
2. The `tick` handler drives a three-phase sequence:
   - **Phase 0 (Approach):** 3 minutes of atmospheric buildup. Environmental descriptions, terrain changes, and scout spawns create tension.
   - **Phase 1 (Minions):** 4 minutes of ice drake guard spawns. Players must fight through the guards before the boss appears.
   - **Phase 2 (Boss):** The Frost Wyrm spawns. The event tracks the single boss kill as its goal. Periodic roar effects and an enrage timer at ~40 minutes keep pressure on.
3. When the boss dies (progress reaches 1), the event spawns a reward chest, resets the terrain, and ends.
4. The `reset` handler keeps minion guards flowing during the boss phase.
5. The `moon` handler adds flavor — if a moon phase change happens during the event, extra minions spawn and atmospheric text fires.
6. With the `all_parts` flag, every participant receives rewards, not just the killing blow.
7. With `scaling`, the boss difficulty adjusts based on how many players are present.
