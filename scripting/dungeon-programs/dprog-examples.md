---
layout: default
title: Examples
nav_order: 3
parent: Dungeon Programs
grand_parent: Scripting
---

# Dungeon Program Examples
{: .no_toc }

Three practical examples of dungeon programs, from straightforward to complex. Each example includes the complete script, trigger attachment, and a line-by-line explanation.

For the full trigger and command references, see [Triggers](dprog-triggers.md) and [Commands](dprog-commands.md).

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Example 1: Dungeon Completion Handler

A dungeon that tracks boss kills across two floors and completes when both bosses are defeated. Uses variables to track state and the `completed` trigger to reward players.

### Scripts Required

This example uses three scripts:
1. A **`dungeon_commenced`** script to initialize the dungeon
2. A **`random`** script that checks boss kill conditions each tick
3. A **`completed`** script to handle rewards

### Script 1: Initialization (dungeon_commenced)

**Attachment:**
```
dedit <dungeon vnum>
addprog <script vnum> dungeon_commenced 100
```

**Script:**
```
** Initialize boss tracking variables when dungeon starts
** Fires on dungeon_commenced trigger (100% chance)
dungeon varset $self boss1_dead 0
dungeon varset $self boss2_dead 0
dungeon varset $self dungeon_phase started
dungeon echoat $self {YThe ancient dungeon awakens! Defeat both guardians to claim your reward.{x
```

**How it works:**

- `dungeon varset $self boss1_dead 0` — Initializes tracking variables for both bosses. The boss mobs themselves will set these to 1 via their own mob programs when they die (using `mob varseton $(dungeon) boss1_dead 1`).
- `dungeon echoat $self ...` — Broadcasts a message to everyone in the dungeon. When the target is `$self` (the dungeon), all players receive the message.

### Script 2: Boss Kill Check (random)

**Attachment:**
```
addprog <script vnum> random 100
```

**Script:**
```
** Check if both bosses are dead each tick
** Fires on random trigger (100% chance = every tick)
if var $self dungeon_phase != started
  break
endif
if var $self boss1_dead == 1
  if var $self boss2_dead == 1
    ** Both bosses are dead — complete the dungeon
    dungeon varset $self dungeon_phase completing
    dungeon dungeoncomplete $self
  endif
endif
```

**How it works:**

- `if var $self dungeon_phase != started` — Guards against running after the dungeon is already completing or completed. Without this check, the script would call `dungeoncomplete` every tick.
- The nested `if` checks whether both boss variables are set to 1. Boss mob programs are responsible for setting these variables when they die.
- `dungeon dungeoncomplete $self` — Marks the dungeon as completed and fires the `completed` trigger.

### Script 3: Completion Rewards (completed)

**Attachment:**
```
addprog <script vnum> completed 100
```

**Script:**
```
** Handle dungeon completion — reward all players
** Fires on completed trigger (100% chance)
dungeon echoat $self {x
dungeon echoat $self {G═══════════════════════════════════════════{x
dungeon echoat $self {G  DUNGEON COMPLETE! Both guardians have fallen!{x
dungeon echoat $self {G═══════════════════════════════════════════{x
dungeon echoat $self {x
dungeon varset $self dungeon_phase completed
```

**How it works:**

- Multiple `dungeon echoat $self` lines display a formatted completion banner to all players.
- `dungeon varset $self dungeon_phase completed` — Updates the phase variable so the random-tick script stops checking.

{: .note }
> The boss mobs need their own mob programs with a `death` trigger that sets the dungeon variable:
> ```
> ** In boss mob's death trigger script:
> mob varseton $(dungeon) boss1_dead 1
> mob echoat $(dungeon) {RThe first guardian has been slain!{x
> ```

---

## Example 2: Progressive Difficulty Scaling

A dungeon that gets harder over time. Every 30 seconds, the difficulty phase increases — spawning additional mobs, modifying room properties, and warning players. After phase 5, the dungeon fails if not completed.

### Script: Difficulty Escalation (tick)

**Attachment:**
```
dedit <dungeon vnum>
addprog <script vnum> tick 100
```

**Script:**
```
** Progressive difficulty scaling
** Fires on tick trigger (100% every pulse)
** Uses a countdown timer to advance phases every ~30 ticks
if dungeonflag $self commenced
else
  ** Dungeon hasn't started yet
  break
endif
if dungeonflag $self completed
  break
endif
if dungeonflag $self failed
  break
endif

** Decrement the phase timer
if varexists $self phase_timer
  dungeon varset $self phase_timer -= 1
  if var $self phase_timer > 0
    break
  endif
else
  ** First run — initialize
  dungeon varset $self difficulty_phase 0
  dungeon varset $self phase_timer 30
  break
endif

** Timer hit zero — advance to next phase
dungeon varset $self difficulty_phase += 1
dungeon varset $self phase_timer 30

if var $self difficulty_phase == 1
  dungeon echoat $self {yThe air grows thick with dark energy...{x
  dungeon echoat $self {yDifficulty increased: Phase 1{x
endif

if var $self difficulty_phase == 2
  dungeon echoat $self {YShadows gather in the corridors! Additional creatures emerge!{x
  dungeon echoat $self {YDifficulty increased: Phase 2{x
  ** Spawn additional mobs on each floor
  dungeon loadinstanced mobile 50110 $self.entry_room
  dungeon loadinstanced mobile 50110 $self.entry_room
endif

if var $self difficulty_phase == 3
  dungeon echoat $self {rThe dungeon trembles with fury! The walls close in!{x
  dungeon echoat $self {RDifficulty increased: Phase 3{x
  dungeon loadinstanced mobile 50115 $self.entry_room
  dungeon loadinstanced mobile 50115 $self.entry_room
  dungeon loadinstanced mobile 50115 $self.entry_room
endif

if var $self difficulty_phase == 4
  dungeon echoat $self {R*** WARNING: The dungeon is becoming unstable! ***{x
  dungeon echoat $self {RDifficulty increased: Phase 4 — Complete the dungeon NOW!{x
  dungeon loadinstanced mobile 50120 $self.entry_room
  dungeon loadinstanced mobile 50120 $self.entry_room
endif

if var $self difficulty_phase >= 5
  dungeon echoat $self {D*** THE DUNGEON COLLAPSES! ***{x
  dungeon echoat $self {DYou have run out of time!{x
  dungeon dungeonfailure $self
endif
```

**How it works:**

- **Guard clauses** — The first three `if` blocks ensure the script only runs when the dungeon is active (commenced but not completed or failed).
- **Countdown timer** — `phase_timer` starts at 30 and decrements each tick. When it reaches 0, the difficulty phase advances and the timer resets.
- **Phase progression** — Each phase spawns increasingly dangerous mobs and sends escalating warning messages.
- **Phase 5 = failure** — If players haven't completed the dungeon by phase 5, it fails automatically via `dungeon dungeonfailure`.
- **`loadinstanced`** — All spawned mobs are instance-scoped, ensuring cleanup on dungeon destruction.

{: .important }
> The tick rate depends on the game's pulse speed. Adjust the `phase_timer` value to match your desired real-time intervals. A value of 30 at the default pulse rate is approximately 30 seconds.

---

## Example 3: Boss Room Setup

A dungeon program that sets up a boss encounter when players reach a specific floor. It seals the room, spawns the boss with custom properties, starts a combat timer, and handles the boss defeat.

This example uses two scripts: one for floor entry and one for periodic monitoring.

### Script 1: Boss Room Activation (entry)

**Attachment:**
```
dedit <dungeon vnum>
addprog <script vnum> entry 100
```

**Script:**
```
** Boss room setup — activates when a player enters the dungeon
** Checks if the player is on floor 3 (the boss floor)
** Fires on entry trigger (100% chance)
if dungeonflag $self completed
  break
endif
if var $self boss_active == 1
  ** Boss already spawned
  break
endif

** Check if the entering player is on floor 3
** We track this by having the floor 3 entry room set a variable
if roomvnum $(here) != $self.boss_floor_entry
  break
endif

** Seal the boss room
dungeon alterroom $(here) flags +norecall
dungeon alterroom $(here) flags +nomob_exit
dungeon echoat $(here) {x
dungeon echoat $(here) {R══════════════════════════════════════{x
dungeon echoat $(here) {R  The doors slam shut behind you!{x
dungeon echoat $(here) {R══════════════════════════════════════{x
dungeon echoat $(here) {x

** Spawn the boss
dungeon loadinstanced mobile 50200 $(here) boss_ref
dungeon stringmob $boss_ref short {RKhar'goth the Undying{x
dungeon echoat $(here) {DA massive figure rises from the shadows...{x
dungeon echoat $(here) {RKhar'goth the Undying bellows: "YOU DARE ENTER MY DOMAIN?!"{x

** Set boss tracking variables
dungeon varset $self boss_active 1
dungeon varset $self boss_timer 120

** Start combat timer
dungeon echoat $(here) {yYou have 2 minutes to defeat Khar'goth!{x
```

**How it works:**

- **Floor check** — Uses `roomvnum $(here)` to compare the player's current room against the boss floor's entry room. The `$self.boss_floor_entry` syntax assumes a special room is configured on the dungeon blueprint.
- **Guard clauses** — Prevents re-spawning the boss if the dungeon is already completed or the boss is already active.
- **Room sealing** — `alterroom` adds the `norecall` and `nomob_exit` flags, preventing players from escaping via recall or mob movement commands.
- **Boss spawn** — `loadinstanced` spawns the boss and stores a reference in `boss_ref`. `stringmob` customizes the boss's short description.
- **Timer** — Sets `boss_timer` to 120 ticks. The monitoring script (below) decrements this.

### Script 2: Boss Combat Monitor (tick)

**Attachment:**
```
addprog <script vnum> tick 100
```

**Script:**
```
** Boss combat timer and defeat detection
** Fires on tick trigger (100% every pulse)
if var $self boss_active != 1
  break
endif

** Check if boss is dead (mob death trigger sets this)
if var $self boss_defeated == 1
  dungeon echoat $self {x
  dungeon echoat $self {G══════════════════════════════════════{x
  dungeon echoat $self {G  Khar'goth the Undying has been destroyed!{x
  dungeon echoat $self {G══════════════════════════════════════{x
  dungeon echoat $self {x
  dungeon varset $self boss_active 0

  ** Unseal the room
  dungeon alterroom $self.boss_floor_entry flags -norecall
  dungeon alterroom $self.boss_floor_entry flags -nomob_exit

  ** Spawn reward chest
  dungeon loadinstanced object 50300 100 room $self.boss_floor_entry
  dungeon echoat $self.boss_floor_entry {YA gleaming treasure chest materializes!{x

  ** Complete the dungeon
  dungeon dungeoncomplete $self
  break
endif

** Decrement the combat timer
dungeon varset $self boss_timer -= 1

** Warning messages at key thresholds
if var $self boss_timer == 60
  dungeon echoat $self {YOne minute remaining!{x
endif
if var $self boss_timer == 30
  dungeon echoat $self {R*** 30 SECONDS REMAINING! ***{x
endif
if var $self boss_timer == 10
  dungeon echoat $self {D*** 10 SECONDS! ***{x
endif

** Time's up — fail the dungeon
if var $self boss_timer <= 0
  dungeon echoat $self {DTime has run out! Khar'goth's power overwhelms you!{x
  dungeon dungeonfailure $self
endif
```

**How it works:**

- **Defeat detection** — The boss mob's own death trigger sets `boss_defeated` to 1 on the dungeon (via `mob varseton $(dungeon) boss_defeated 1`). This script checks that variable each tick.
- **Room unsealing** — On defeat, removes the `norecall` and `nomob_exit` flags so players can leave.
- **Reward spawning** — `loadinstanced object` places a treasure chest in the boss room.
- **Timer countdown** — Decrements `boss_timer` each tick with warning messages at 60, 30, and 10 ticks remaining.
- **Failure on timeout** — If the timer reaches 0, the dungeon fails.

{: .note }
> The boss mob needs a `death` trigger mob program:
> ```
> ** Boss death handler (mob program on mob vnum 50200)
> ** Trigger: death, phrase: 100
> mob varseton $(dungeon) boss_defeated 1
> ```

### Putting It All Together

The complete dungeon requires:

| Component | Type | Purpose |
|-----------|------|---------|
| Dungeon blueprint | `dedit` | Defines 3 floors, entry/exit rooms, boss floor |
| Entry script | `dpedit` | Spawns and configures boss on floor 3 entry |
| Tick script | `dpedit` | Monitors boss kill and combat timer |
| Boss mob death script | `mpedit` | Sets `boss_defeated` variable on the dungeon |

This pattern — dungeon scripts for orchestration, mob scripts for individual entity behavior — is the standard approach for complex dungeon encounters.
