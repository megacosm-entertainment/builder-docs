---
layout: default
title: Commands
nav_order: 2
parent: Quest Programs
grand_parent: Scripting
---

# Quest Program Commands
{: .no_toc }

Quest programs use the **same command table** as area programs (`area_cmd_table`) — all 44 commands available to area progs are also available to quest progs. The only difference is the command prefix: quest programs use `quest` instead of `area`.

For the full area command reference, see [Area Script Commands](../area-programs/aprog-commands.md). This page focuses on the commands most relevant to quest scripting, organized by usage pattern.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Command Prefix

All commands in a quest program must be prefixed with `quest`:

```
quest echoat $n You feel the quest calling to you.
quest varset quest_stage 2
quest mload 50100
```

{: .warning }
> Using the wrong prefix (e.g., `mob` or `area`) will cause the command to fail. Always use `quest` in quest programs.

---

## Complete Command List

All 44 commands available to quest programs:

| # | Command | Description |
|--:|---------|-------------|
| 1 | `alterroom` | Modify properties of a room (flags, sector, name, etc.) |
| 2 | `call` | Call another script as a subroutine with return value |
| 3 | `churchannouncetheft` | Announce a theft to members of a church/religion organization |
| 4 | `dungeoncommence` | Start/begin a dungeon encounter or phase |
| 5 | `dungeoncomplete` | Mark a dungeon as successfully completed |
| 6 | `dungeonfailure` | Mark a dungeon as failed |
| 7 | `echoat` | Send a message to a specific target character only |
| 8 | `event` | Interact with the event system (create, modify, query, manage events) |
| 9 | `instancecomplete` | Mark the current dungeon instance as completed |
| 10 | `instancefailure` | Mark the current dungeon instance as failed |
| 11 | `mail` | Send an in-game mail message to a player |
| 12 | `mload` | Load/create a mobile by vnum into the game world |
| 13 | `mute` | Mute a character (prevent them from speaking) |
| 14 | `oload` | Load/create an object by vnum into the game world |
| 15 | `phaseevent` | Advance an event to its next phase |
| 16 | `quest` | Interact with the quest system (start, complete, set step, etc.) |
| 17 | `questechoat` | Send a quest-related message to a specific character |
| 18 | `reckoning` | Trigger a reckoning event (world event system) |
| 19 | `resetroom` | Reset a room to its default/template state |
| 20 | `sendfloor` | Send a dungeon floor transition to players |
| 21 | `specialkey` | Create or manage a special key item (dungeon keys) |
| 22 | `stageevent` | Advance an event to its next stage |
| 23 | `startevent` | Start a world/area event |
| 24 | `startreckoning` | Start a reckoning world event sequence |
| 25 | `stopevent` | Stop/end an active world/area event |
| 26 | `stopreckoning` | Stop an active reckoning event |
| 27 | `treasuremap` | Create or interact with a treasure map item |
| 28 | `unlockarea` | Unlock a locked area (make it accessible) |
| 29 | `unlockdungeon` | Unlock a dungeon (make it accessible to players) |
| 30 | `unmute` | Unmute a previously muted character |
| 31 | `varclear` | Clear/delete a script variable from the current entity |
| 32 | `varclearon` | Clear a variable on a different entity |
| 33 | `varcopy` | Copy a variable from one name to another on the current entity |
| 34 | `varsave` | Mark a variable for persistent save |
| 35 | `varsaveon` | Mark a variable on a different entity for persistent save |
| 36 | `varset` | Set a variable value on the current entity |
| 37 | `varseton` | Set a variable on a different entity |
| 38 | `wildernessmap` | Generate or modify a wilderness map overlay |
| 39 | `wildsanchor` | Set an anchor point on the wilderness map |
| 40 | `wildsoverlay` | Apply a tile overlay to the wilderness map |
| 41 | `wildstile` | Set or modify a specific wilderness tile |
| 42 | `wildsvlink` | Create a virtual link between wilderness and interior rooms |
| 43 | `wiznet` | Send a message to the immortal communication channel |
| 44 | `xcall` | Execute a cross-entity script call (call script on another entity) |

---

## Commands by Quest Usage Pattern

### Player Communication

The most common quest scripting task is sending messages to the player as quest events occur.

#### `echoat`

Send a message to a specific character. This is the primary way to communicate quest status to the player.

```
quest echoat $n {CYou have accepted the quest: The Lost Artifact.{x
quest echoat $n {cSeek the ancient ruins north of Millhaven.{x
```

#### `questechoat`

Send a quest-related message to a character. Similar to `echoat` but specifically designed for quest system messages.

```
quest questechoat $n {YQuest Updated: Stage 2 has begun.{x
```

#### `mail`

Send an in-game mail to a player. Useful for delayed quest notifications or rewards.

```
quest mail $n The Council of Elders Your bravery in completing the quest has not gone unnoticed. Report to the council chambers at your earliest convenience.
```

#### `wiznet`

Send a message to the immortal communication channel. Useful for debugging quest scripts.

```
quest wiznet Quest prog: $n accepted quest, level $j
```

### Variable Management

Variables are essential for tracking quest state — custom data that persists across trigger firings.

#### `varset` / `varseton`

Set a variable on the quest entity or on another entity.

```
** Track quest start time on the quest itself
quest varset quest_start_time $T

** Set a variable on the player's token
quest varseton $n token_quest_progress 1
```

#### `varclear` / `varclearon`

Clear a variable from the quest or another entity.

```
** Clean up temporary quest state
quest varclear temp_target
quest varclearon $n token_quest_progress
```

#### `varcopy`

Copy a variable value to a new name.

```
quest varcopy old_stage_target saved_target
```

#### `varsave` / `varsaveon`

Mark a variable for persistent save (survives server restarts).

```
quest varset quest_completions 1
quest varsave quest_completions
```

### World Interaction

Quest programs can spawn NPCs, create objects, and modify the game world.

#### `mload`

Load a mobile into the game world. Useful for spawning quest-specific NPCs when a stage begins.

```
** Spawn the quest boss when stage 3 starts
quest mload 52100
```

#### `oload`

Load an object into the game world. Useful for creating quest items or rewards.

```
** Create a reward item
quest oload 65200
```

#### `alterroom`

Modify room properties. Useful for changing room descriptions or flags during quest progression.

```
** Open the sealed door when stage 2 completes
quest alterroom 31050 flags -sealed
```

#### `resetroom`

Reset a room to its default state. Useful for cleaning up after quest events.

```
quest resetroom 31050
```

### Script Flow

#### `call`

Call another script as a subroutine. Useful for sharing logic between quest triggers.

```
** Call a shared reward calculation script
quest call 50010
```

#### `xcall`

Execute a script on another entity. Useful for triggering behavior on NPCs or objects as part of quest progression.

```
** Tell the quest NPC to react
quest xcall 52100 mob 50020
```

### Quest System Integration

#### `quest`

Interact with the quest system directly — start quests, complete quests, set steps, and more. This command is available within quest progs for self-referential quest manipulation.

```
quest quest complete $n
```

### Event and Dungeon Integration

Quest programs can interact with the event and dungeon systems, enabling quests that span world events or dungeon instances.

#### `startevent` / `stopevent` / `phaseevent` / `stageevent`

Manage world events from quest scripts:

```
** Start a world event when the quest reaches stage 3
quest startevent 80010
```

#### `dungeoncommence` / `dungeoncomplete` / `dungeonfailure`

Control dungeon encounters:

```
** Begin the dungeon encounter for this quest stage
quest dungeoncommence 90001
```

#### `instancecomplete` / `instancefailure`

Manage dungeon instances:

```
quest instancecomplete
```

---

## What Quest Programs Cannot Do

Because quest programs share the area command table, they do **not** have access to commands that are specific to other entity types. Notable commands that are NOT available in quest programs:

| Not Available | Entity Types | Reason |
|--------------|-------------|--------|
| `echo`, `echoaround` | Mob, Obj, Room, Tok | Use `echoat` or `questechoat` instead |
| `kill`, `damage`, `flee` | Mob | Combat commands are mob-only |
| `transfer`, `teleport` | Mob | Movement commands are mob-only |
| `give`, `take`, `junk` | Mob | Inventory manipulation is mob-only |
| `remember`, `forget` | Mob | Target memory is mob-only |
| `cast` | Mob | Spellcasting is mob-only |
| `force` | Mob | Forcing commands is mob-only |
| `tokengive`, `tokenremove` | Mob, Obj, Room, Tok | Token management is not in area table |

To perform these actions from a quest program, use `xcall` to call a mob program on an NPC, or use the related quest triggers on mob/obj/room/token entities.

---

## See Also

- [Area Script Commands](../area-programs/aprog-commands.md) — Full reference for the shared command table
- [Shared Commands](../shared-commands.md) — Commands available across all entity types
- [Quest Triggers](qprog-triggers.md) — All 13 quest-exclusive triggers
- [Quest Examples](qprog-examples.md) — Practical quest scripting examples
