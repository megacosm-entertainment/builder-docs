---
layout: default
title: Triggers
nav_order: 1
parent: Quest Programs
grand_parent: Scripting
---

# Quest Program Triggers
{: .no_toc }

This page documents all 13 triggers available for quest programs. Every trigger listed here is **exclusive to quests** — none of these triggers exist on any other entity type (mobiles, objects, rooms, tokens, areas, etc.).

For general trigger concepts and phrase matching, see [Scripting Basics](../scripting-basics.md). For the commands you can use inside a triggered script, see [Commands](qprog-commands.md).

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## How Quest Triggers Work

When you attach a script to a quest, you specify:

1. **Trigger type** — The quest event name (e.g., `quest_accepted`, `stage_completed`)
2. **Phrase** — A percent chance (1–100) that the script fires when the event occurs

All 13 quest triggers use **percent chance** as their phrase type. A phrase of `100` means the script always fires; `50` means a 50% chance each time the event occurs.

**Attachment example:**

```
qedit <quest vnum>
addprog <script vnum> quest_accepted 100
```

## Entity Bindings

When a quest trigger fires, your script can reference these entities:

| Variable | Type | Description |
|----------|------|-------------|
| `$i` / `$(self)` | Quest | The quest itself |
| `$n` / `$(enactor)` | Mobile | The player involved in the quest event |
| `$(here)` | Room | The current room of the player |
| `$(phrase)` | String | The matched phrase/argument |

See [Variables and Tokens](../variables-and-tokens.md) for the complete reference.

---

## Trigger Quick Reference

All 13 quest triggers at a glance, organized by lifecycle phase:

| Trigger | When It Fires | Phrase |
|---------|--------------|-------|
| `quest_accepted` | Player accepts the quest | percent chance |
| `quest_focused` | Player focuses/selects the quest | percent chance |
| `quest_completed` | Quest completion is finalized (after awards) | percent chance |
| `quest_failed` | Quest fails | percent chance |
| `stage_commenced` | A quest stage begins | percent chance |
| `stage_completed` | A quest stage completes | percent chance |
| `stage_failed` | A quest stage fails | percent chance |
| `stage_target_resolved` | A stage target is resolved | percent chance |
| `stage_dest_resolved` | A stage destination is resolved | percent chance |
| `objective_completed` | A quest objective completes | percent chance |
| `objective_failed` | A quest objective fails | percent chance |
| `objective_target_resolved` | An objective target is resolved | percent chance |
| `objective_dest_resolved` | An objective destination is resolved | percent chance |

---

## Quest-Level Triggers

These triggers fire in response to events on the quest as a whole.

### `quest_accepted`

**Enum:** `TRIG_QUEST_ACCEPTED` (153)
{: .text-grey-dk-000 }

Fires when a player accepts the quest. This is the earliest point in the quest lifecycle and is ideal for initialization — setting variables, sending introductory messages, or dynamically configuring quest parameters.

**Alias:** `quest_accept`

**Phrase:** Percent chance (1–100).

**When it fires:**
- The player accepts the quest through a quest giver or quest interface
- Fires once per acceptance

**Common uses:**
- Send a welcome message or quest briefing
- Initialize quest variables (`quest varset`)
- Load objects or NPCs needed for the quest
- Log quest acceptance for tracking

**Example:**

```
** Welcome the player and set up initial quest state
quest echoat $n {CYou have accepted the quest: The Lost Artifact.{x
quest echoat $n {cSeek the ancient ruins to the north...{x
quest varset quest_start_time $T
```

---

### `quest_focused`

**Enum:** `TRIG_QUEST_FOCUSED` (156)
{: .text-grey-dk-000 }

Fires when a player focuses or selects the quest — typically through a quest journal or quest tracking interface. This trigger is useful for providing contextual reminders or updating quest state when the player reviews their progress.

**Phrase:** Percent chance (1–100).

**When it fires:**
- The player selects or focuses this quest in their quest interface
- May fire multiple times as the player switches between quests

**Common uses:**
- Display current quest progress or hints
- Update quest tracking information
- Provide contextual guidance based on current stage

**Example:**

```
** Remind the player of their current objective
quest echoat $n {cQuest: The Lost Artifact{x
quest echoat $n {cObjective: Find the ancient ruins north of Millhaven.{x
```

---

### `quest_completed`

**Enum:** `TRIG_QUEST_COMPLETED` (154)
{: .text-grey-dk-000 }

Fires after the quest completion is finalized and all standard awards have been given. This is the post-completion hook — use it for follow-up actions like unlocking new quests, sending congratulatory messages, or cleaning up quest state.

**Phrase:** Percent chance (1–100).

**When it fires:**
- After all standard quest rewards and messages have been delivered
- Fires once per completion

{: .note }
> This fires **after** awards are given. If you need to modify rewards before they are delivered, use `quest_complete` on a mob/obj/room/token program instead — that trigger fires on other entity types *before* awards.

**Common uses:**
- Send completion messages and lore
- Unlock follow-up quests
- Grant bonus rewards based on custom conditions
- Clean up quest variables and temporary state
- Record completion statistics

**Example:**

```
** Congratulate and offer a follow-up quest
quest echoat $n {YCongratulations! You have completed The Lost Artifact!{x
quest echoat $n {yThe elder nods approvingly... perhaps there is more work to be done.{x
```

---

### `quest_failed`

**Enum:** `TRIG_QUEST_FAILED` (155)
{: .text-grey-dk-000 }

Fires when the quest fails — whether due to a timer expiring, a critical objective being lost, or any other failure condition. Use this to clean up quest state, inform the player, or offer a chance to retry.

**Phrase:** Percent chance (1–100).

**When it fires:**
- The quest transitions to a failed state
- Fires once per failure

**Common uses:**
- Send failure messages
- Clean up spawned NPCs or objects
- Reset quest state for retry
- Record failure for tracking

**Example:**

```
** Notify the player of failure and clean up
quest echoat $n {RYou have failed the quest: The Lost Artifact.{x
quest echoat $n {rThe ancient magic fades, and the ruins seal themselves once more...{x
quest varclear quest_start_time
```

---

## Stage-Level Triggers

Quest stages represent major phases of a quest (e.g., "Find the ruins", "Defeat the guardian", "Return the artifact"). These triggers fire when stage events occur.

### `stage_commenced`

**Enum:** `TRIG_STAGE_COMMENCED` (157)
{: .text-grey-dk-000 }

Fires when a quest stage begins. Use this to introduce the next phase of the quest, spawn stage-specific content, or update quest tracking.

**Phrase:** Percent chance (1–100).

**When it fires:**
- A new stage of the quest is activated
- Fires once per stage commencement

**Common uses:**
- Display stage introduction text
- Spawn NPCs or objects for the new stage
- Set stage-specific variables
- Update quest journal entries

**Example:**

```
** Introduce Stage 2: Defeat the Guardian
quest echoat $n {cStage 2: The guardian of the ruins blocks your path.{x
quest echoat $n {cDefeat the ancient guardian to proceed deeper.{x
quest varset current_stage 2
```

---

### `stage_completed`

**Enum:** `TRIG_STAGE_COMPLETED` (158)
{: .text-grey-dk-000 }

Fires when a quest stage completes successfully. Use this to advance the quest, grant intermediate rewards, or transition to the next stage.

**Phrase:** Percent chance (1–100).

**When it fires:**
- All objectives in the stage are satisfied
- The stage transitions to a completed state

**Common uses:**
- Grant intermediate rewards (experience, items, gold)
- Display stage completion messages
- Advance to the next stage programmatically
- Record stage completion time

**Example:**

```
** Stage 1 complete — reward and advance
quest echoat $n {YStage complete! The ruins have been found.{x
quest echoat $n {yYou sense the guardian's presence deeper within...{x
```

---

### `stage_failed`

**Enum:** `TRIG_STAGE_FAILED` (159)
{: .text-grey-dk-000 }

Fires when a quest stage fails. This may or may not fail the entire quest depending on the quest design — some quests allow retrying a stage, while others fail outright.

**Phrase:** Percent chance (1–100).

**When it fires:**
- A stage's failure conditions are met
- Fires once per stage failure

**Common uses:**
- Send stage failure messages
- Clean up stage-specific content
- Reset stage state for retry
- Optionally fail the entire quest

**Example:**

```
** Stage failed — offer context
quest echoat $n {RThe guardian's magic overwhelms you. The stage has failed.{x
quest echoat $n {rYou must gather your strength and try again.{x
```

---

### `stage_target_resolved`

**Enum:** `TRIG_STAGE_TARGET_RESOLVED` (160)
{: .text-grey-dk-000 }

Fires when a stage's target entity is resolved. Target resolution is the process of determining *what* the player needs to interact with for a stage — for example, selecting which mob to kill or which NPC to speak to. This trigger enables **dynamic quest generation** by letting scripts assign targets at runtime.

**Phrase:** Percent chance (1–100).

**When it fires:**
- The engine needs to resolve the target for a stage
- Fires during quest setup or stage transition

**Common uses:**
- Dynamically select a target mob from a pool
- Choose a target based on player level, location, or history
- Set the target vnum via quest variables

**Example:**

```
** Dynamically pick a target mob for a slay stage
** Choose a harder mob for higher-level players
if level $n >= 40
  quest varset stage_target 52010
else
  quest varset stage_target 52005
endif
```

---

### `stage_dest_resolved`

**Enum:** `TRIG_STAGE_DEST_RESOLVED` (161)
{: .text-grey-dk-000 }

Fires when a stage's destination is resolved. Destination resolution determines *where* the player needs to go for a stage — selecting a room vnum dynamically. This enables quests that send players to different locations each time.

**Phrase:** Percent chance (1–100).

**When it fires:**
- The engine needs to resolve the destination for a stage
- Fires during quest setup or stage transition

**Common uses:**
- Randomly select a destination room
- Choose a destination based on player progress or location
- Set the destination vnum via quest variables

**Example:**

```
** Pick a random dungeon entrance for the "go to" stage
quest varset stage_dest 31050
quest echoat $n {cThe oracle points you toward the eastern caverns.{x
```

---

## Objective-Level Triggers

Objectives are individual tasks within a stage (e.g., "Kill 5 wolves", "Collect 3 gems", "Visit the shrine"). These triggers fire when objective events occur.

### `objective_completed`

**Enum:** `TRIG_OBJECTIVE_COMPLETED` (162)
{: .text-grey-dk-000 }

Fires when an individual quest objective completes. This is more granular than `stage_completed` — a stage may have multiple objectives, and this fires for each one independently.

**Phrase:** Percent chance (1–100).

**When it fires:**
- A single objective within a stage is satisfied
- Fires once per objective completion

**Common uses:**
- Notify the player of objective progress
- Grant per-objective rewards
- Update quest tracking display
- Trigger conditional logic based on which objectives are done

**Example:**

```
** Notify player of objective completion
quest echoat $n {GObjctive complete!{x
quest echoat $n {gOne step closer to finishing this stage.{x
```

---

### `objective_failed`

**Enum:** `TRIG_OBJECTIVE_FAILED` (163)
{: .text-grey-dk-000 }

Fires when an individual quest objective fails. Depending on quest design, a single failed objective may or may not fail the entire stage or quest.

**Phrase:** Percent chance (1–100).

**When it fires:**
- A single objective within a stage fails
- Fires once per objective failure

**Common uses:**
- Inform the player which objective failed
- Adjust quest difficulty or provide hints
- Clean up objective-specific state

**Example:**

```
** Notify player of objective failure
quest echoat $n {RAn objective has failed. Check your quest journal for details.{x
```

---

### `objective_target_resolved`

**Enum:** `TRIG_OBJECTIVE_TARGET_RESOLVED` (164)
{: .text-grey-dk-000 }

Fires when an objective's target entity is resolved. Similar to `stage_target_resolved` but at the objective level — determines the specific target for an individual objective (e.g., which specific mob to kill for a "slay" objective).

**Phrase:** Percent chance (1–100).

**When it fires:**
- The engine needs to resolve the target for an objective
- Fires during stage setup or objective initialization

**Common uses:**
- Dynamically assign objective targets
- Choose targets based on player context
- Support random or rotating objective targets

**Example:**

```
** Assign a target mob for this kill objective
** Pick from a set of lieutenants based on which zone the player is in
if inroom $n == 31000
  quest varset obj_target 52015
else
  quest varset obj_target 52020
endif
```

---

### `objective_dest_resolved`

**Enum:** `TRIG_OBJECTIVE_DEST_RESOLVED` (165)
{: .text-grey-dk-000 }

Fires when an objective's destination is resolved. Determines *where* the player needs to go for a specific objective (e.g., which room to visit for a "go to" objective).

**Phrase:** Percent chance (1–100).

**When it fires:**
- The engine needs to resolve the destination for an objective
- Fires during stage setup or objective initialization

**Common uses:**
- Dynamically assign objective destinations
- Randomize visit locations for repeatable quests
- Choose destinations based on player level or progress

**Example:**

```
** Assign a destination room for this goto objective
quest varset obj_dest 31075
quest echoat $n {cYour compass points toward the northern watchtower.{x
```

---

## Trigger Lifecycle Summary

The following diagram shows when triggers fire during a typical quest lifecycle:

```
Player accepts quest
  └─ quest_accepted         ← initialization, welcome message

Player focuses quest in journal
  └─ quest_focused          ← progress reminder

Stage begins
  ├─ stage_commenced        ← stage introduction
  ├─ stage_target_resolved  ← dynamic target assignment
  └─ stage_dest_resolved    ← dynamic destination assignment

  Objective setup
    ├─ objective_target_resolved  ← per-objective target
    └─ objective_dest_resolved    ← per-objective destination

  Objective completes/fails
    ├─ objective_completed  ← progress notification
    └─ objective_failed     ← failure handling

Stage completes/fails
  ├─ stage_completed        ← intermediate reward, advance
  └─ stage_failed           ← retry or fail quest

Quest completes/fails
  ├─ quest_completed        ← final reward, unlock follow-ups
  └─ quest_failed           ← cleanup, retry offer
```

---

## Related Triggers on Other Entity Types

Several quest-related triggers exist on **other** entity types (mobs, objects, rooms, tokens) rather than on quests themselves. These are useful for NPCs and objects that participate in the quest system:

| Trigger | Entity Types | Description |
|---------|-------------|-------------|
| `quest_cancel` | Mob, Obj, Room, Tok | Fires when a quest is cancelled |
| `quest_complete` | Mob, Obj, Room, Tok | Fires *before* awards when a quest is turned in complete |
| `quest_incomplete` | Mob, Obj, Room, Tok | Fires *before* awards when a quest is turned in incomplete |
| `quest_part` | Mob, Tok | Used to generate a custom quest part when selected |
| `prequest` | Mob, Tok | Custom checking for questing; allows setting quest part count |
| `postquest` | Mob, Tok | Called after all quest rewards and messages are given |

These triggers are documented in their respective entity type sections ([Mobile Triggers](../mobile-programs/mprog-triggers.md), etc.) and are **not** part of quest programs. They fire on the NPC or object involved in the quest interaction, not on the quest itself.
