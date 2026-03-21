---
layout: default
title: Examples
nav_order: 3
parent: Quest Programs
grand_parent: Scripting
---

# Quest Program Examples
{: .no_toc }

Three practical examples of quest programs, showing common patterns. Each example includes the complete script, trigger attachment, and a line-by-line explanation.

For the full trigger and command references, see [Triggers](qprog-triggers.md) and [Commands](qprog-commands.md).

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Example 1: Quest Acceptance Handler

A quest program that welcomes the player when they accept the quest, initializes tracking variables, and spawns a quest-specific NPC.

**Trigger:** `quest_accepted` — Fires when the player accepts the quest.

**Attachment:**

```
qedit <quest vnum>
addprog <script vnum> quest_accepted 100
```

The phrase `100` means the script fires every time (100% chance).

**Script:**

```
** Quest: The Lost Artifact
** Fires when the player accepts the quest
** Initializes quest state and provides the opening briefing

** Send the quest briefing
quest echoat $n {x
quest echoat $n {C================================================================{x
quest echoat $n {C  The Lost Artifact{x
quest echoat $n {C================================================================{x
quest echoat $n {x
quest echoat $n {cAn ancient artifact has been stolen from the Museum of Antiquities.{x
quest echoat $n {cThe thief was last seen heading toward the Darkwood Forest.{x
quest echoat $n {x
quest echoat $n {cYour objectives:{x
quest echoat $n {c  1. Travel to the Darkwood Forest entrance{x
quest echoat $n {c  2. Track down the thief{x
quest echoat $n {c  3. Recover the artifact and return it to the museum{x
quest echoat $n {x

** Initialize quest tracking variables
quest varset quest_start_time $T
quest varset quest_stage 1
quest varset bonus_eligible 1

** Spawn the quest contact NPC at the forest entrance
quest mload 52050

** Notify immortals for debugging
quest wiznet Quest: $n accepted 'The Lost Artifact'
```

**How it works:**

| Line | What It Does |
|------|-------------|
| `quest echoat $n ...` | Sends formatted quest briefing text to the player. `{C` is bright cyan, `{c` is dark cyan, `{x` resets color. |
| `quest varset quest_start_time $T` | Stores the current timestamp in a quest variable. `$T` is the time quick code. |
| `quest varset quest_stage 1` | Tracks which stage the player is on (starts at 1). |
| `quest varset bonus_eligible 1` | Flags the player as eligible for a speed bonus — cleared if they take too long. |
| `quest mload 52050` | Spawns mob vnum 52050 (the quest contact NPC) into the world. |
| `quest wiznet ...` | Sends a debug message to the immortal channel. `$n` is replaced with the player's name. |

{: .note }
> The `quest varset` commands store data on the quest entity itself. These variables persist across trigger firings for the duration of the quest and can be read by other quest triggers or by if-checks.

---

## Example 2: Quest Completion with Reward Scaling

A quest program that fires when the quest is completed, calculates scaled rewards based on player level and completion speed, and grants bonus items to qualifying players.

**Trigger:** `quest_completed` — Fires after standard awards have been given.

**Attachment:**

```
qedit <quest vnum>
addprog <script vnum> quest_completed 100
```

**Script:**

```
** Quest: The Lost Artifact — Completion handler
** Fires AFTER standard quest rewards are delivered
** Adds scaled bonus rewards based on level and speed

** Congratulatory message
quest echoat $n {x
quest echoat $n {Y================================================================{x
quest echoat $n {Y  Quest Complete: The Lost Artifact{x
quest echoat $n {Y================================================================{x
quest echoat $n {x

** Level-based bonus rewards
** Higher level players get progressively better bonus items
if level $n >= 50
  ** Elite reward: rare enchanted accessory
  quest echoat $n {yAs a seasoned adventurer, you receive a special reward...{x
  quest oload 65300
  quest echoat $n {YYou receive the Amulet of the Curator!{x
elseif level $n >= 30
  ** Mid-tier reward: enchanted gem
  quest echoat $n {yYour growing skills have earned you an extra reward...{x
  quest oload 65250
  quest echoat $n {YYou receive an Enchanted Sapphire!{x
elseif level $n >= 10
  ** Low-tier reward: basic consumable
  quest echoat $n {yFor your efforts, you receive a small bonus...{x
  quest oload 65200
  quest echoat $n {YYou receive a Potion of Restoration!{x
else
  ** Beginner: encouragement only
  quest echoat $n {yWell done for a new adventurer! Keep at it.{x
endif

** Speed bonus: check if the player completed quickly
** bonus_eligible is set to 1 on accept; a timed stage_completed
** script could clear it if the player took too long
if questvar $(self) bonus_eligible == 1
  quest echoat $n {x
  quest echoat $n {GSPEED BONUS: You completed the quest swiftly!{x
  quest echoat $n {gYou receive bonus experience!{x
  quest echoat $n {x
endif

** Clean up quest variables
quest varclear quest_start_time
quest varclear quest_stage
quest varclear bonus_eligible

** Log completion
quest wiznet Quest: $n completed 'The Lost Artifact' at level $j
```

**How it works:**

| Line | What It Does |
|------|-------------|
| `if level $n >= 50` | Checks the player's level using the `level` if-check. Rewards scale in tiers. |
| `quest oload 65300` | Creates item vnum 65300 in the world. The object will need to be given to the player by an NPC or placed in their inventory via another mechanism. |
| `if questvar $(self) bonus_eligible == 1` | Reads a variable stored on the quest entity (`$(self)`). If the speed bonus flag is still set, the player qualifies. |
| `quest varclear ...` | Cleans up temporary quest variables now that the quest is done. |

{: .important }
> The `quest_completed` trigger fires **after** standard awards (experience, gold, quest points) have already been given. The rewards in this script are *bonus* rewards layered on top of the standard ones.

---

## Example 3: Multi-Part Quest Progression

A set of quest programs that manage a three-stage quest with dynamic target resolution, stage transitions, and per-objective notifications. This example shows how multiple triggers work together across the quest lifecycle.

**Quest structure:**

| Stage | Description | Objectives |
|-------|------------|-----------|
| Stage 1 | Travel to the Darkwood Forest | Go to the forest entrance |
| Stage 2 | Track the thief | Slay the thief's scout, collect the trail map |
| Stage 3 | Recover the artifact | Defeat the thief, collect the artifact |

**Triggers and attachments:**

```
qedit <quest vnum>
addprog <script A vnum> quest_accepted 100
addprog <script B vnum> stage_commenced 100
addprog <script C vnum> stage_target_resolved 100
addprog <script D vnum> objective_completed 100
addprog <script E vnum> stage_completed 100
addprog <script F vnum> quest_completed 100
```

---

### Script A: Quest Accepted

**Trigger:** `quest_accepted`

```
** The Lost Artifact — Quest acceptance
** Initialize the quest and brief the player

quest echoat $n {cThe curator wrings his hands in worry.{x
quest echoat $n {c'Please, you must help us recover the Sunstone Pendant!'{x
quest echoat $n {c'The thief fled north toward the Darkwood. Please hurry!'{x

** Track total objectives completed for stat reporting
quest varset objectives_done 0
quest varset quest_start_time $T
```

---

### Script B: Stage Commenced

**Trigger:** `stage_commenced`

This single script handles all stage transitions by checking the current stage number.

```
** The Lost Artifact — Stage introductions
** Fires whenever a new stage begins

if questvar $(self) quest_stage == 1
  quest echoat $n {x
  quest echoat $n {c--- Stage 1: Into the Darkwood ---{x
  quest echoat $n {cFind the path to the Darkwood Forest entrance.{x
  quest echoat $n {cThe curator said the thief went north.{x
  quest echoat $n {x
elseif questvar $(self) quest_stage == 2
  quest echoat $n {x
  quest echoat $n {c--- Stage 2: On the Trail ---{x
  quest echoat $n {cThe thief left scouts behind. Defeat them and find the trail.{x
  quest echoat $n {x
  ** Spawn the scout mob for this stage
  quest mload 52060
elseif questvar $(self) quest_stage == 3
  quest echoat $n {x
  quest echoat $n {c--- Stage 3: Confrontation ---{x
  quest echoat $n {cThe thief is cornered! Defeat them and recover the artifact.{x
  quest echoat $n {x
  ** Spawn the thief boss
  quest mload 52070
endif
```

---

### Script C: Stage Target Resolved

**Trigger:** `stage_target_resolved`

Dynamically assigns targets for the slay objectives in stages 2 and 3.

```
** The Lost Artifact — Dynamic target resolution
** Assigns mob targets based on the current stage

if questvar $(self) quest_stage == 2
  ** Stage 2 target: the thief's scout
  ** Pick a tougher scout for higher-level players
  if level $n >= 30
    quest varset stage_target 52062
  else
    quest varset stage_target 52060
  endif
elseif questvar $(self) quest_stage == 3
  ** Stage 3 target: the thief boss
  quest varset stage_target 52070
endif
```

---

### Script D: Objective Completed

**Trigger:** `objective_completed`

Fires each time an individual objective is completed, providing per-objective feedback.

```
** The Lost Artifact — Objective completion notifications
** Provides feedback for each objective

** Increment our objective counter
quest varset objectives_done $+1

** Send a notification
quest echoat $n {G[Objective Complete]{x
quest echoat $n {gYou feel one step closer to recovering the artifact.{x
```

---

### Script E: Stage Completed

**Trigger:** `stage_completed`

Handles transitions between stages — grants intermediate rewards and advances the stage counter.

```
** The Lost Artifact — Stage completion handler
** Advances the stage counter and provides transition narrative

if questvar $(self) quest_stage == 1
  quest echoat $n {x
  quest echoat $n {YStage 1 Complete!{x
  quest echoat $n {yYou have reached the Darkwood Forest.{x
  quest echoat $n {yFresh tracks lead deeper into the woods...{x
  quest echoat $n {x
  quest varset quest_stage 2

elseif questvar $(self) quest_stage == 2
  quest echoat $n {x
  quest echoat $n {YStage 2 Complete!{x
  quest echoat $n {yThe scout's trail map reveals the thief's hideout!{x
  quest echoat $n {yPrepare yourself for the final confrontation.{x
  quest echoat $n {x
  quest varset quest_stage 3

elseif questvar $(self) quest_stage == 3
  quest echoat $n {x
  quest echoat $n {YFinal Stage Complete!{x
  quest echoat $n {yYou have defeated the thief and recovered the Sunstone Pendant!{x
  quest echoat $n {yReturn to the museum to claim your reward.{x
  quest echoat $n {x
endif
```

---

### Script F: Quest Completed

**Trigger:** `quest_completed`

Final wrap-up — summary and cleanup.

```
** The Lost Artifact — Final completion
** Summary and cleanup

quest echoat $n {x
quest echoat $n {Y================================================================{x
quest echoat $n {Y  Quest Complete: The Lost Artifact{x
quest echoat $n {Y================================================================{x
quest echoat $n {x
quest echoat $n {yThe curator accepts the Sunstone Pendant with tears of joy.{x
quest echoat $n {y'You have saved our most precious treasure! Thank you, $n!'{x
quest echoat $n {x

** Clean up all quest variables
quest varclear objectives_done
quest varclear quest_stage
quest varclear quest_start_time

quest wiznet Quest: $n completed 'The Lost Artifact'
```

---

## Putting It All Together

The multi-part example demonstrates several key patterns:

| Pattern | Technique |
|---------|----------|
| **Stage tracking** | Use `quest varset quest_stage` to track which stage is active; check it in `stage_commenced` and `stage_completed` triggers |
| **Dynamic targets** | Use `stage_target_resolved` to assign different targets based on player level or quest state |
| **Per-objective feedback** | Use `objective_completed` to notify the player each time an objective is done |
| **Stage transitions** | Use `stage_completed` to advance the `quest_stage` variable and provide narrative bridges |
| **Cleanup** | Always `quest varclear` temporary variables in the `quest_completed` handler |
| **NPC spawning** | Use `quest mload` in `stage_commenced` to spawn stage-specific mobs |

## See Also

- [Quest Triggers](qprog-triggers.md) — All 13 quest-exclusive triggers
- [Quest Commands](qprog-commands.md) — All 44 available commands
- [Variables and Tokens](../variables-and-tokens.md) — Quick codes and variable reference
- [If-Checks Reference](../ifchecks-reference.md) — Conditional logic reference
- [Mobile Program Examples](../mobile-programs/mprog-examples.md) — NPC scripting examples (including quest NPCs)
