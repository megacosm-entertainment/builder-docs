---
layout: default
title: Examples
nav_order: 3
parent: Area Programs
grand_parent: Scripting
---

# Area Program Examples
{: .no_toc }

Three practical examples of area programs, progressing from simple to complex. Each example includes the complete script, trigger attachment, and a line-by-line explanation.

For the full trigger and command references, see [Triggers](aprog-triggers.md) and [Commands](aprog-commands.md).

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Example 1: Area-Wide Weather and Announcement System

A program that broadcasts periodic ambient weather messages and zone announcements on a random tick, cycling through different messages based on a rotating counter.

**Trigger:** `tick` — Fires on each regular game tick with a percent chance.

**Attachment:**

```
aedit
addprog <script vnum> tick 20
```

The phrase `20` means a 20% chance of firing each tick — roughly once every 5 ticks on average.

**Script:**

```
** Area-wide ambient weather / announcement system
** Cycles through 5 different messages using a counter variable

** Increment the message counter
area varset msg_index $@(msg_index + 1)

** Reset counter after reaching 5
if var msg_index > 5
  area varset msg_index 1
endif

** Send the appropriate weather message to all rooms via xcall
if var msg_index == 1
  area echoat * {cA cold wind sweeps down from the northern peaks, carrying flurries of snow.{x
elseif var msg_index == 2
  area echoat * {yThunder rumbles in the distance as dark clouds gather overhead.{x
elseif var msg_index == 3
  area echoat * {CThe fog thickens, reducing visibility to mere feet.{x
elseif var msg_index == 4
  area echoat * {YA break in the clouds lets golden sunlight stream across the valley.{x
elseif var msg_index == 5
  area echoat * {GThe rain eases to a gentle drizzle, and the air smells of wet earth.{x
endif

** Log for debugging (only visible to immortals)
area wiznet Weather script fired: message index = $@(msg_index)
```

**How it works:**

| Line | Purpose |
|------|---------|
| `area varset msg_index $@(msg_index + 1)` | Increments the counter. `$@(...)` evaluates an arithmetic expression using the current variable value. |
| `if var msg_index > 5` | Checks whether the counter has exceeded the number of messages. |
| `area varset msg_index 1` | Resets the counter to cycle back to the first message. |
| `area echoat * ...` | Sends a message to all players. The `*` target broadcasts to everyone in the area. |
| `area wiznet ...` | Sends a debug message visible only to immortals on the wiznet channel. |

{: .note }
> The `tick` trigger with a 20% phrase creates organic-feeling timing. Messages arrive at irregular intervals rather than a predictable rhythm, making the atmosphere more immersive.

---

## Example 2: Reset-Triggered Repopulation Logic

A program that runs on each area reset to conditionally spawn mobs and objects based on zone state variables, rather than relying solely on the static reset list.

**Trigger:** `reset` — Fires when the area resets (repopulation cycle).

**Attachment:**

```
aedit
addprog <script vnum> reset 100
```

The phrase `100` means the script fires on every reset — the logic inside controls what actually happens.

**Script:**

```
** Custom reset repopulation script
** Spawns different configurations based on zone state

** Track total reset count (persistent across reboots)
area varset reset_count $@(reset_count + 1)
area varsave reset_count

** Always reset the town square and main gate
area resetroom 3001
area resetroom 3002

** Spawn a rare traveling merchant every 5th reset
if mathmod $@(reset_count) 5 == 0
  area mload 5050 3010
  area varset merchant_present 1
  area echoat * {GA mysterious merchant has set up shop at the crossroads!{x
else
  area varset merchant_present 0
endif

** If the invasion event is active, spawn extra guards
if var invasion_active == 1
  area mload 5031 3001
  area mload 5031 3002
  area mload 5032 3003
  area mload 5032 3004
  area echoat * {WAdditional guards take up positions around the town.{x

  ** Escalate defenses every 3rd reset during invasion
  if mathmod $@(reset_count) 3 == 0
    area mload 5040 3001
    area echoat * {YA veteran captain arrives to coordinate the defense!{x
  endif
endif

** Seasonal spawning — check a global variable
if var season == winter
  ** Spawn ice elementals in the northern caves
  area mload 6010 3080
  area mload 6010 3081
  area alterroom 3050 sector ice
elseif var season == summer
  ** Restore normal terrain
  area alterroom 3050 sector forest
endif

** Clean up any temporary event variables from the previous cycle
area varclear temp_event_flag
area varclear last_attacker
```

**How it works:**

| Line | Purpose |
|------|---------|
| `area varset reset_count $@(reset_count + 1)` | Increments a persistent counter tracking total resets. |
| `area varsave reset_count` | Marks the counter for persistent storage so it survives reboots. |
| `area resetroom 3001` | Resets room 3001 to its default state (standard repopulation). |
| `if mathmod $@(reset_count) 5 == 0` | Checks if the reset count is divisible by 5 using modular arithmetic. |
| `area mload 5050 3010` | Spawns mob vnum 5050 in room 3010. |
| `if var invasion_active == 1` | Checks a zone variable to see if the invasion event is running. |
| `area alterroom 3050 sector ice` | Changes room 3050's terrain to ice during winter. |
| `area varclear temp_event_flag` | Removes a temporary variable to prevent state leakage between resets. |

{: .important }
> Use `area varsave` for any variable that should persist across server reboots. Without it, variables are lost on restart — fine for temporary event state, but problematic for long-term tracking like reset counts or seasonal progress.

---

## Example 3: Zone-Wide Variable Tracking

A comprehensive program that manages zone-wide state for a multi-phase event, tracking player participation, managing phase transitions, and coordinating with mob and room programs via cross-entity calls.

**Triggers:** Three scripts work together — a `tick` script for monitoring, a `reckoning` script for the active event, and a `prereckoning` script for setup.

### Script 1: Prereckoning Setup

**Attachment:**

```
aedit
addprog <script vnum> prereckoning 100
```

**Script:**

```
** Prereckoning setup — initialize event state
** Fires once when startreckoning is called

** Initialize tracking variables
area varset phase 0
area varset total_kills 0
area varset total_players 0
area varset boss_spawned 0
area varset event_start_time $T

** Announce the coming event
area echoat * {R{/RECKONING WARNING:{x {rThe Shadowrift is destabilizing!{x
area echoat * {rAll defenders report to the Northern Bastion immediately!{x

** Prepare the battlefield rooms
area alterroom 4001 addflag dark
area alterroom 4001 name {RThe Shadowrift - Unstable{x
area alterroom 4002 addflag dark
area alterroom 4002 name {RApproach to the Rift{x

** Spawn initial sentries
area mload 7001 4001
area mload 7001 4002
area mload 7002 4003

** Notify staff
area wiznet Shadowrift reckoning initialized. Variables reset.
```

### Script 2: Active Reckoning — Wave Management

**Attachment:**

```
aedit
addprog <script vnum> reckoning 100
```

**Script:**

```
** Active reckoning — manage event progression each tick
** Fires every tick while the reckoning is active

** Increment tick counter
area varset phase_ticks $@(phase_ticks + 1)

** Phase 0 → Phase 1: After 10 ticks, begin the assault
if var phase == 0
  if var phase_ticks >= 10
    area varset phase 1
    area varset phase_ticks 0
    area echoat * {R{/The first wave of shadow creatures pours through the rift!{x
    area mload 7010 4001
    area mload 7010 4001
    area mload 7011 4002
    area mload 7011 4003
  endif
endif

** Phase 1 → Phase 2: Escalation after 20 ticks
if var phase == 1
  if var phase_ticks >= 20
    area varset phase 2
    area varset phase_ticks 0
    area echoat * {R{/The shadow grows stronger! Elite creatures join the assault!{x
    area mload 7020 4001
    area mload 7020 4002
    area mload 7021 4001
  endif
endif

** Phase 2 → Phase 3: Boss spawn after 15 ticks if enough kills
if var phase == 2
  if var phase_ticks >= 15
    if var total_kills >= 20
      area varset phase 3
      area varset phase_ticks 0
      area varset boss_spawned 1
      area echoat * {R{/THE SHADOW LORD EMERGES FROM THE RIFT!{x
      area mload 7099 4001
      area wiznet Shadowrift boss spawned. Total kills so far: $@(total_kills)
    else
      ** Not enough kills — event fails
      area echoat * {DThe shadow forces overwhelm the defenders...{x
      area echoat * {DThe rift stabilizes, but the shadow's grip on this land grows stronger.{x
      area stopreckoning
    endif
  endif
endif

** Phase 3: Check for boss kill via variable (set by boss mob's death script)
if var phase == 3
  if var boss_killed == 1
    area echoat * {G{/VICTORY! The Shadow Lord is vanquished!{x
    area echoat * {GThe rift collapses in on itself, and light returns to the land!{x

    ** Record the victory persistently
    area varset total_victories $@(total_victories + 1)
    area varsave total_victories

    ** Restore the rooms
    area alterroom 4001 remflag dark
    area alterroom 4001 name The Northern Bastion
    area alterroom 4002 remflag dark
    area alterroom 4002 name Approach to the Bastion

    ** End the reckoning
    area stopreckoning
  endif
endif
```

### Script 3: Tick Monitor — Cross-Entity Coordination

**Attachment:**

```
aedit
addprog <script vnum> tick 100
```

**Script:**

```
** Tick monitor — runs every tick regardless of reckoning state
** Manages long-term zone tracking

** Track days since last reckoning
if var reckoning_cooldown > 0
  area varset reckoning_cooldown $@(reckoning_cooldown - 1)
endif

** Auto-trigger reckoning every ~200 ticks (if not on cooldown)
if var reckoning_cooldown <= 0
  if rand 1
    area startreckoning
    area varset reckoning_cooldown 200
    area varsave reckoning_cooldown
  endif
endif

** Periodic zone health report (every 50 ticks)
if mathmod $@(monitor_tick) 50 == 0
  area wiznet Zone status: victories=$@(total_victories) cooldown=$@(reckoning_cooldown) phase=$@(phase)
endif
area varset monitor_tick $@(monitor_tick + 1)
```

**How the three scripts coordinate:**

| Script | Trigger | Role |
|--------|---------|------|
| Script 1 | `prereckoning` | One-time setup: initialize variables, prepare rooms, spawn sentries |
| Script 2 | `reckoning` | Active event: manage phases, spawn waves, check objectives, end event |
| Script 3 | `tick` | Always running: cooldown tracking, auto-triggering, zone status reporting |

**Key patterns demonstrated:**

- **Phase state machine** — The `phase` variable drives progression through numbered phases, with `phase_ticks` counting time within each phase.
- **Persistent tracking** — `total_victories` and `reckoning_cooldown` use `varsave` to survive reboots.
- **Cross-entity coordination** — The boss mob's death script sets `boss_killed` on the area (via `mob varseton area ... boss_killed 1`), which the reckoning script checks each tick.
- **Conditional escalation** — Phase 2 → Phase 3 requires both time (15 ticks) *and* player performance (20+ kills). If players fail the kill check, the event ends in failure.
- **Cleanup** — Room modifications (dark flag, names) are restored when the event ends.

{: .note }
> In a production setup, the boss mob would have its own mob prog with a `death` trigger that calls `mob varseton area <area uid> boss_killed 1`. This is how entity-level scripts communicate state changes back to area programs.
