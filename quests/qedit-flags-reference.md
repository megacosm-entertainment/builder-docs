---
layout: default
title: Quest Flags & Types Reference
nav_order: 3
parent: Quests
---

# Quest Flags & Types Reference

This page lists all valid values for quest properties. Use these when setting fields in `qedit`.

---

## Quest Classes

Set with `qedit <ref> class <value>`.

| Value | Description |
|-------|-------------|
| `narrative` | Story-driven quest; often part of a chain or overarching plot |
| `mission` | Task or job quest; typically standalone with a clear objective |

---

## Quest Types

Set with `qedit <ref> type <value>`.

| Value | Description |
|-------|-------------|
| `main` | Core storyline content; central to the area or world narrative |
| `side` | Optional content; world-building or character flavour |
| `unlock` | Completing this quest unlocks further content (areas, quests, items) |
| `class` | Associated with a specific character class; may require class membership |
| `event` | Temporary content tied to a server event or season |
| `other` | Uncategorised |

---

## Log Categories

Set with `qedit <ref> category <value>`. Controls which journal section the quest appears in.

| Value | Description |
|-------|-------------|
| `none` | No journal category |
| `regional` | Location or zone-specific content |
| `class` | Class advancement or class flavour |
| `story` | Main storyline or narratively significant |
| `church` | Church or guild-related content |
| `dungeon` | Dungeon clearing or instance completion |
| `crafting` | Gathering, crafting, or production tasks |
| `event` | Seasonal or temporary event quests |
| `other` | Miscellaneous |

---

## Scope

Set with `qedit <ref> scope <value>`. Determines who the quest tracks progress for.

| Value | Description |
|-------|-------------|
| `character` | Individual per-character tracking; each player completes separately |
| `group` | Progress is shared across the player's active party |
| `church` | Progress is shared across the player's church/guild |

---

## Quest Flags

Set with `qedit <ref> flags <flag>`. Flags are toggled, not set.

| Flag | Description |
|------|-------------|
| `group_snapshot` | When group scope is lost (e.g. party disbands), the quest state is snaphotted to the individual character instead of being purged. Prevents progress loss on group break. |

---

## Repeat Policy

Set with `qedit <ref> repeat <value>`.

| Value | Description |
|-------|-------------|
| `once` | The quest can only be completed once per character |
| `repeatable` | The quest can be completed multiple times |

---

## Seed Policy

Set with `qedit <ref> seedpolicy <value>`. Used for quests with generated stages.

| Value | Description |
|-------|-------------|
| `auto` | A random seed is automatically assigned when the quest is accepted |
| `fixed` | The seed set via `qedit <ref> seed <number>` is always used |

---

## Stage Source Types

Set with `qedit <ref> stage <id> source <value>`.

| Value | Description |
|-------|-------------|
| `static` | Stage objectives are fixed; target entities are specified by widevnum |
| `generated` | Stage objectives are procedurally generated using the quest seed |

---

## Stage Completion Modes

Set with `qedit <ref> stage <id> complete <value>`.

| Value | Description |
|-------|-------------|
| `all` | All objectives in this stage must be completed before the stage advances |
| `any` | Completing any single objective advances the stage |
| `custom` | Stage completion is controlled entirely by script logic |

---

## Objective Types

Set with `qedit <ref> objective <stage_id> <obj_id> type <value>`.

| Value | Description |
|-------|-------------|
| `kill` | Defeat a mob. Uses `required` for kill count and `target` for the mob entity. |
| `collect` | Gather items. Uses `required` for quantity, `target` for the item vnum, `quantity` for per-hand-in amount. |
| `talk` | Speak with an NPC. Uses `target` for the NPC entity. Satisfied by initiating dialogue. |
| `travel` | Reach a destination. Uses `destination` for the target room widevnum. |
| `locate` | Find and approach a specific entity anywhere in the world. Uses `target`. |
| `rescue` | Escort or recover an NPC and bring it to a destination. Uses `target` and `destination`. |
| `escort` | Guide an NPC from one point to another. Uses `target` and `destination`. |
| `custom` | Completion is script-driven. No automatic trigger; a script must mark this objective complete. |

---

## Target Modes

Set with `qedit <ref> objective <stage_id> <obj_id> target mode <value>`.

| Value | Description |
|-------|-------------|
| `exact` | A single exact entity identified by its widevnum |
| `pool` | Pick one entity randomly from a defined pool of widevnums (weighted). The selection is locked in when the quest is accepted. |

---

## Reward Types

Set with `qedit <ref> reward <index> type <value>`.

| Value | Description |
|-------|-------------|
| `points` | Quest points awarded. Set amount with `reward <index> amount`. |
| `currency` | Gold or silver. Set type with `reward <index> currency <gold\|silver>`, amount with `reward <index> amount`. |
| `reputation` | Faction reputation gain. Set faction with `reward <index> target <widevnum>`, amount with `reward <index> amount`. |
| `token` | A token item given to the player. Set type with `reward <index> target <widevnum>`, quantity with `reward <index> amount`. |
| `item` | A loadable object. Set object with `reward <index> target <widevnum>`. |
| `script` | Runs a script action on delivery. Set script text with `reward <index> script <text>`. |
