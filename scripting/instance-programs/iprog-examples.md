---
layout: default
title: Examples
nav_order: 3
parent: Instance Programs
grand_parent: Scripting
---

# Instance Program Examples
{: .no_toc }

Practical examples of instance programs covering common use cases. Each example includes the blueprint setup, complete script code, trigger attachments, and an explanation of how the pieces fit together.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

{: .important }
> All instance program commands use the `instance` prefix. Comments use `**` on their own line.

---

## Example 1: Instance Completion Tracking

A three-boss dungeon where the instance tracks boss kills using variables. When all three bosses are defeated, the instance is marked complete, a reward chest spawns, and players receive a congratulatory message.

### Blueprint Setup

The dungeon blueprint defines three boss rooms (4200, 4210, 4220) and an exit portal room (4230). Each boss mob has a mob program that increments the instance's kill counter on death (via `xcall` to the instance). The instance program monitors the counter and triggers completion.

### Script 1: Boss Kill Tracker (tick trigger)

```
** Instance completion tracker — check boss kill count
** Trigger: tick 100

** Initialize on first tick
if varexists $self bosses_killed
else
  instance varset bosses_killed number 0
  instance varset total_bosses number 3
  instance varset completion_announced number 0
endif

** Skip if already completed
if var $self completion_announced >= 1
  break
endif

** Check if all bosses are down
if var $self bosses_killed >= 3
  ** Mark the instance complete
  instance instancecomplete $self
  instance varset completion_announced number 1
endif
```

### Script 2: Initial Setup (repop trigger)

```
** Instance initialization — spawn bosses and set up rooms
** Trigger: repop 100

** Spawn the three bosses using instance-scoped loading
instance loadinstanced mobile 9501 4200 boss1
instance makeinstanced $lastloaded

instance loadinstanced mobile 9502 4210 boss2
instance makeinstanced $lastloaded

instance loadinstanced mobile 9503 4220 boss3
instance makeinstanced $lastloaded

** Initialize tracking
instance varset bosses_killed number 0
instance varset total_bosses number 3
instance varset start_time number $time

** Announce to players
instance echoat $n {W*** Welcome to the Crypt of the Three Lords ***{x
instance echoat $n {wDefeat all three bosses to claim your reward.{x
```

### Script 3: Completion Handler (completed trigger)

```
** Instance completed — distribute rewards
** Trigger: completed 100

** Spawn the reward chest in the central chamber
instance loadinstanced object 9550 50 room 4215 reward_chest

** Open the exit portal
instance alterroom 4230 flags noflee

** Announce completion
instance echoat $n {G*** DUNGEON COMPLETE ***{x
instance echoat $n {YThe crypt falls silent. A {Wglowing portal{Y appears in the final chamber.{x
instance echoat $n {YA {Wchest of ancient treasure{Y materializes before you.{x

** Log the completion
instance wiznet Crypt of Three Lords completed in $regmath($time - $getvar($self start_time)) seconds
```

### Script 4: Boss Death Handler (on each boss mob)

This mob program runs on each boss. When the boss dies, it calls back to the instance to increment the kill counter.

```
** Boss death handler — mob program on each boss mob
** Trigger: death 100

** Increment the instance kill counter via the mob's instance context
mob varset $instance bosses_killed number $regmath($getvar($instance bosses_killed) + 1)

** Announce the kill
mob echo {RThe {W$shortdesc($i){R crumbles to dust!{x
mob echo {Y[$getvar($instance bosses_killed) of $getvar($instance total_bosses) bosses defeated]{x
```

### Trigger Attachments

```
** On the instance blueprint:
ipedit <blueprint>
addiprog <script 1 vnum> tick 100
addiprog <script 2 vnum> repop 100
addiprog <script 3 vnum> completed 100

** On each boss mob:
medit 9501
addprog <script 4 vnum> death 100
medit 9502
addprog <script 4 vnum> death 100
medit 9503
addprog <script 4 vnum> death 100
```

### How It Works

1. When the instance is created, the `repop` trigger fires and spawns three instance-scoped bosses using `loadinstanced` and `makeinstanced`.
2. Each boss has a mob program on its `death` trigger that increments the instance variable `bosses_killed`.
3. The `tick` trigger checks `bosses_killed` every tick. When it reaches 3, it calls `instancecomplete`.
4. The `completed` trigger fires, spawning a reward chest and opening the exit portal.
5. The `wiznet` command logs the completion time for staff monitoring.

{: .note }
> The boss death handler uses mob program variable commands to write to the instance. This cross-entity communication pattern is common — mobs report state changes, and the instance program acts on the aggregated state.

---

## Example 2: Scaled Mob Spawning

An instance that adjusts difficulty based on the number of players in the group. More players mean more mobs, stronger bosses, and better rewards. The instance checks party size on entry and dynamically adjusts spawn counts.

### Script 1: Scaled Initialization (repop trigger)

```
** Scaled dungeon — adjust difficulty to party size
** Trigger: repop 100

** Count players in the instance
instance varset player_count number 0
instance varset difficulty number 1

** Default assumption: scale will be set on first entry
instance varset scaled number 0
instance varset mobs_per_room number 2
instance varset boss_hp_mult number 1
```

### Script 2: Scale on First Entry (entry trigger)

```
** Scale difficulty when first player enters
** Trigger: entry 100

** Only scale once
if var $self scaled >= 1
  break
endif

** Only count player characters
if ispc $n
else
  break
endif

** Count group members
if varexists $self player_count
  instance varset player_count number $regmath($getvar($self player_count) + 1)
else
  instance varset player_count number 1
endif
```

### Script 3: Spawn Scaled Content (tick trigger)

```
** Scaled spawner — run once after players have arrived
** Trigger: tick 100

** Wait for players to arrive, then scale
if var $self scaled >= 1
  break
endif

** Don't scale until at least one player is present
if var $self player_count < 1
  break
endif

** Lock in the difficulty based on player count
instance varset scaled number 1

** Determine difficulty tier
if var $self player_count >= 6
  ** Full group — hardest difficulty
  instance varset difficulty number 3
  instance varset mobs_per_room number 5
  instance varset boss_hp_mult number 3
  instance echoat $n {RThe dungeon senses a large group. It responds with fury!{x
else
  if var $self player_count >= 3
    ** Mid group — medium difficulty
    instance varset difficulty number 2
    instance varset mobs_per_room number 3
    instance varset boss_hp_mult number 2
    instance echoat $n {YThe dungeon stirs, sensing intruders...{x
  else
    ** Solo or duo — easy difficulty
    instance varset difficulty number 1
    instance varset mobs_per_room number 2
    instance varset boss_hp_mult number 1
    instance echoat $n {wThe dungeon is quiet... for now.{x
  endif
endif

** Spawn mobs based on difficulty
** Room 4100 — Entry hall
instance loadinstanced mobile 8100 4100
instance makeinstanced $lastloaded
if var $self mobs_per_room >= 3
  instance loadinstanced mobile 8100 4100
  instance makeinstanced $lastloaded
endif
if var $self mobs_per_room >= 4
  instance loadinstanced mobile 8101 4100
  instance makeinstanced $lastloaded
endif
if var $self mobs_per_room >= 5
  instance loadinstanced mobile 8101 4100
  instance makeinstanced $lastloaded
endif

** Room 4110 — Corridor
instance loadinstanced mobile 8100 4110
instance makeinstanced $lastloaded
instance loadinstanced mobile 8102 4110
instance makeinstanced $lastloaded
if var $self mobs_per_room >= 3
  instance loadinstanced mobile 8100 4110
  instance makeinstanced $lastloaded
endif

** Room 4120 — Boss chamber
instance loadinstanced mobile 9500 4120 final_boss
instance makeinstanced $lastloaded

** Scale the boss name to reflect difficulty
if var $self difficulty >= 3
  instance stringmob $getvar($self final_boss) short {RThe Empowered Crypt Guardian{x
else
  if var $self difficulty >= 2
    instance stringmob $getvar($self final_boss) short {yThe Crypt Guardian{x
  else
    instance stringmob $getvar($self final_boss) short {wThe Weakened Crypt Guardian{x
  endif
endif

instance echoat $n {wDifficulty set to tier $getvar($self difficulty) for $getvar($self player_count) players.{x
```

### Trigger Attachments

```
ipedit <blueprint>
addiprog <script 1 vnum> repop 100
addiprog <script 2 vnum> entry 100
addiprog <script 3 vnum> tick 100
```

### How It Works

1. The `repop` trigger initializes default values when the instance is created.
2. As players enter, the `entry` trigger counts them via the `player_count` variable. The `ispc` check ensures only player characters are counted.
3. On the first `tick` after players arrive, the script locks in the difficulty tier:
   - **Tier 1 (1–2 players):** 2 mobs per room, standard boss
   - **Tier 2 (3–5 players):** 3 mobs per room, 2× boss HP multiplier
   - **Tier 3 (6+ players):** 5 mobs per room, 3× boss HP multiplier
4. Mobs are spawned using `loadinstanced` and `makeinstanced`, then the boss name is customized with `stringmob` to reflect difficulty.
5. The `scaled` variable prevents the spawner from running again on subsequent ticks.

{: .important }
> In this example, the boss HP multiplier variable is set on the instance but the actual HP modification would be handled by a mob program on the boss itself (reading the instance variable). Instance programs coordinate state; mob programs apply it to individual entities.

---

## Example 3: Instance-Wide Variable State

A puzzle dungeon that tracks shared state across multiple rooms. Players must activate switches in different rooms in the correct order. The instance tracks which switches have been activated, validates the sequence, and opens the final door when the puzzle is solved.

### Script 1: Initialize Puzzle State (repop trigger)

```
** Puzzle dungeon — initialize state
** Trigger: repop 100

** The correct switch order is: red (1), blue (2), green (3)
instance varset puzzle_solution string "1,2,3"
instance varset switches_activated number 0
instance varset last_switch number 0
instance varset puzzle_solved number 0
instance varset puzzle_failed number 0

** Lock the final door
instance alterroom 4300 flags norecall

instance echoat $n {W*** The Chamber of Sequences ***{x
instance echoat $n {wThree switches lie within. Activate them in the correct order.{x
instance echoat $n {wChoose wrongly, and the dungeon resets its defenses.{x
```

### Script 2: Switch Monitoring (tick trigger)

```
** Puzzle state monitor — check for completion or failure
** Trigger: tick 100

** Skip if puzzle already solved
if var $self puzzle_solved >= 1
  break
endif

** Check for failed sequence — reset if wrong order was triggered
if var $self puzzle_failed >= 1
  ** Reset all switches
  instance varset switches_activated number 0
  instance varset last_switch number 0
  instance varset puzzle_failed number 0

  ** Reset the rooms
  instance resetroom 4310
  instance resetroom 4320
  instance resetroom 4330

  ** Re-spawn switch mobs
  instance loadinstanced mobile 8400 4310
  instance makeinstanced $lastloaded
  instance loadinstanced mobile 8401 4320
  instance makeinstanced $lastloaded
  instance loadinstanced mobile 8402 4330
  instance makeinstanced $lastloaded

  instance echoat $n {RThe switches reset with a grinding sound.{x
  instance echoat $n {wThe sequence was incorrect. Try again.{x
endif

** Check for completion
if var $self switches_activated >= 3
  if var $self puzzle_solved < 1
    instance varset puzzle_solved number 1

    ** Open the final door
    instance alterroom 4300 flags none
    instance echoat $n {GAll three switches activated in the correct order!{x
    instance echoat $n {YThe sealed door in the central chamber slides open with a rumble.{x

    ** Spawn the reward behind the door
    instance loadinstanced object 9600 50 room 4300 puzzle_reward
  endif
endif
```

### Script 3: Switch Activation Handler

Each switch is a mob in its respective room. When a player interacts with the switch (via a verb trigger on the mob), the mob program validates the sequence and updates the instance state.

**Red switch mob (room 4310) — expected first (value 1):**

```
** Red switch handler — mob program
** Trigger: verb "activate" 100

** Check if this is the right order
if var $instance last_switch == 0
  ** Correct! Red is first
  mob varset $instance switches_activated number $regmath($getvar($instance switches_activated) + 1)
  mob varset $instance last_switch number 1
  mob echo {RThe red crystal glows brightly!{x
  mob echo {Y[Switch 1 of 3 activated]{x
else
  ** Wrong order
  mob varset $instance puzzle_failed number 1
  mob echo {RThe red crystal flickers and dies. Wrong sequence!{x
endif
```

**Blue switch mob (room 4320) — expected second (value 2):**

```
** Blue switch handler — mob program
** Trigger: verb "activate" 100

if var $instance last_switch == 1
  ** Correct! Blue is second
  mob varset $instance switches_activated number $regmath($getvar($instance switches_activated) + 1)
  mob varset $instance last_switch number 2
  mob echo {BThe blue crystal hums with energy!{x
  mob echo {Y[Switch 2 of 3 activated]{x
else
  mob varset $instance puzzle_failed number 1
  mob echo {BThe blue crystal sputters. Wrong sequence!{x
endif
```

**Green switch mob (room 4330) — expected third (value 3):**

```
** Green switch handler — mob program
** Trigger: verb "activate" 100

if var $instance last_switch == 2
  ** Correct! Green is third
  mob varset $instance switches_activated number $regmath($getvar($instance switches_activated) + 1)
  mob varset $instance last_switch number 3
  mob echo {GThe green crystal blazes with light!{x
  mob echo {Y[Switch 3 of 3 activated — the sequence is complete!]{x
else
  mob varset $instance puzzle_failed number 1
  mob echo {GThe green crystal dims. Wrong sequence!{x
endif
```

### Trigger Attachments

```
** On the instance blueprint:
ipedit <blueprint>
addiprog <script 1 vnum> repop 100
addiprog <script 2 vnum> tick 100

** On the switch mobs:
medit 8400
addprog <red switch script vnum> verb activate 100
medit 8401
addprog <blue switch script vnum> verb activate 100
medit 8402
addprog <green switch script vnum> verb activate 100
```

### How It Works

1. The `repop` trigger initializes the puzzle state: `switches_activated = 0`, `last_switch = 0`, and the final door is locked.
2. Players explore three rooms and "activate" switches by using the `activate` verb on switch mobs.
3. Each switch mob checks the instance variable `last_switch` to validate the sequence:
   - Red must be first (`last_switch == 0`)
   - Blue must be second (`last_switch == 1`)
   - Green must be third (`last_switch == 2`)
4. If the wrong switch is activated, `puzzle_failed` is set to 1.
5. The `tick` trigger monitors state:
   - If `puzzle_failed`, it resets all switches, respawns the switch mobs, and clears the state.
   - If `switches_activated` reaches 3, it opens the final door and spawns the reward.
6. Each instance has its own independent puzzle state — multiple groups can attempt the puzzle simultaneously without interfering with each other.

{: .note }
> This pattern of instance variables as shared state + mob programs as input handlers is the standard approach for multi-room puzzles in instances. The instance program is the coordinator; individual mob and room programs are the sensors.

---

## Tips for Writing Instance Programs

1. **Use `loadinstanced` + `makeinstanced` for all spawned content.** This ensures entities are cleaned up when the instance ends. Standard `mload`/`oload` creates content that may leak into the wider game world.

2. **Initialize state in `repop`, not `tick`.** The `repop` trigger fires once when the instance is created — it's the right place for one-time setup. Use `tick` for ongoing monitoring.

3. **Guard against repeated execution.** Use flag variables (e.g., `scaled`, `completion_announced`) to prevent logic from running multiple times on successive ticks.

4. **Keep `entry` triggers lightweight.** The entry trigger fires for every character movement in the instance, including NPCs. Heavy logic in entry triggers can cause performance issues.

5. **Use `echoat` instead of `echo`.** Instance programs have no inherent room context, so `echo` is not available. Target specific players with `echoat`.

6. **Coordinate with mob programs.** Instance programs manage state; mob programs handle individual entity behavior. Use instance variables as the communication bridge between the two.

7. **Log significant events to `wiznet`.** Staff need visibility into instance behavior for debugging. Log completions, failures, and unusual states.

---

## See Also

- [Instance Triggers](iprog-triggers.md) — Complete trigger reference
- [Instance Commands](iprog-commands.md) — Full command documentation
- [Dungeon Programs](../dungeon-programs/) — Blueprint-level scripting
- [Shared Commands](../shared-commands.md) — Commands shared across all script types
- [Variables and Tokens](../variables-and-tokens.md) — Variable system reference
- [If-Checks Reference](../ifchecks-reference.md) — Conditional checks for scripts
