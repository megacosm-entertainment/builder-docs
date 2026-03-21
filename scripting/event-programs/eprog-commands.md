---
layout: default
title: Commands
nav_order: 2
parent: Event Programs
grand_parent: Scripting
---

# Event Program Commands
{: .no_toc }

Event programs have access to **44 commands**. Most are shared with other script types and are documented in the [Shared Commands Reference](../shared-commands.md). This page provides a brief index of all commands with links, plus full documentation for event-specific behavior.

All commands use the `event` prefix: `event echoat`, `event mload`, `event startevent`, etc.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

{: .important }
> Comments in scripts use the `**` prefix. Place comments on their own line:
> ```
> ** This is a comment
> event echoat $n The event has begun!
> ```

---

## Commands Not Available to Events

Event programs have a focused command set. Many commands available to mob, object, and room programs are **not** available in event context. Notable exclusions:

| Command | Available To | Why Not Event |
|---------|-------------|---------------|
| `appear` / `disappear` | mob | Events have no visibility state |
| `assist` / `kill` / `flee` | mob | Events don't participate in combat directly |
| `at` | mob, obj, room, token | Events have no location |
| `cast` | mob, obj | Events don't cast spells |
| `damage` / `gdamage` | mob, obj, room, token | Use mob scripts for damage |
| `delay` / `cancel` | mob, obj, room | Events use tick-based timing |
| `echo` / `echoaround` | mob, obj, room, token | Use `echoat` or `gecho` instead |
| `force` / `gforce` | mob, obj, room, token | Events don't force player commands |
| `goto` / `transfer` / `gtransfer` | mob, obj, room, token | Events have no location to move from |
| `hunt` | mob | Events don't track targets |
| `purge` | mob, obj, room, token | Use mob/room scripts for purging |
| `selfdestruct` | mob, obj | Events end via `stopevent` |
| `dequeue` / `queue` | mob, obj, room, token | Events use their own scheduling |

---

## Complete Command List

All 44 commands available to event programs, organized by category.

### Event Control (8 commands)

These commands directly manage event lifecycle and progression.

#### event

```
event event <subcommand> [parameters]
```

Interact with the event system — create, modify, query, and manage events. This is the general-purpose event command for operations not covered by the specific commands below.

```
** Query event state
event event status
```

#### startevent

```
event startevent
```

Start a world or area event. When called from within an event program, starts the current event or a referenced event. Triggers the event's start sequence, sends announcements, and begins tracking participants.

```
** Start the event from a tick trigger
event startevent
event gecho {WThe invasion has begun! Defend the realm!{x
```

#### stopevent

```
event stopevent
```

Stop and end the currently active event. Triggers end-of-event processing including reward distribution and cleanup announcements.

```
** End the event when goal is reached
if eventprogress $self >= 100
  event stopevent
endif
```

#### phaseevent

```
event phaseevent
```

Advance the event to its next phase or stage. Events can have multiple stages defined in `evtedit`, and each phase transition can trigger different behavior in your scripts.

```
** Advance to next phase when progress milestone is hit
if eventprogress $self >= 50
  event phaseevent
  event gecho {YThe event has entered its next phase!{x
endif
```

#### stageevent

```
event stageevent
```

Advance the event to its next stage. This is an alias for `phaseevent` — both commands perform the same action.

#### reckoning

```
event reckoning <subcommand>
```

Interact with the reckoning world event system. Trigger reckoning phases, check reckoning state, and manage reckoning progression.

- **Requires argument:** Yes (restricted)

```
** Trigger a reckoning phase
event reckoning advance
```

#### startreckoning

```
event startreckoning <parameters>
```

Start a reckoning world event sequence. Reckonings are large-scale world events with global effects.

- **Requires argument:** Yes (restricted)

```
** Begin a new reckoning
event startreckoning
event gecho {RThe Reckoning has begun! The world trembles!{x
```

#### stopreckoning

```
event stopreckoning
```

Stop an active reckoning event. Ends the reckoning and triggers any cleanup logic.

- **Requires argument:** Yes (restricted)

```
** End the reckoning
event stopreckoning
event gecho {WThe Reckoning has ended. Peace returns to the land.{x
```

---

### Communication (4 commands)

Commands for sending messages to players.

#### echoat

```
event echoat <target> <message>
```

Send a message to a specific target character. This is the primary communication command for events, since events have no "room" to echo to.

```
** Notify a player of their event progress
event echoat $n {CYou have contributed 15 kills to the war effort!{x
```

#### questechoat

```
event questechoat <target> <message>
```

Send a quest-themed message to a specific character. Displays with quest formatting.

```
** Quest-style notification
event questechoat $n {YObjective Update: Collect 10 more moonberries.{x
```

#### mail

```
event mail <player> <subject>~ <body>~
```

Send an in-game mail message to a player. Useful for notifying offline participants of event results.

- **Requires argument:** Yes (restricted)

```
** Mail results to the winner
event mail $n Event Results~ Congratulations! You won the Harvest Moon event with 47 contributions!~
```

#### wiznet

```
event wiznet <message>
```

Send a message to the immortal communication channel. Useful for logging event state changes visible to staff.

```
** Log event milestones to wiznet
event wiznet Event 'Harvest Moon' reached 50% progress.
```

---

### Entity Loading (2 commands)

Commands for creating mobs and objects in the game world.

#### mload

```
event mload <mobile vnum> [room vnum]
```

Load a mobile (NPC) by vnum into the game world. When a room vnum is specified, the mob spawns there. Without a room argument, the mob spawns at a default location within the event scope.

```
** Spawn the event boss
event mload 9500 3001
event gecho {RThe Dread Warden has appeared in the Town Square!{x
```

#### oload

```
event oload <object vnum> [location]
```

Load an object by vnum into the game world. Can place objects in rooms or on mobs.

```
** Spawn a collection item
event oload 5001
```

---

### Room Management (2 commands)

#### alterroom

```
event alterroom <room vnum> <field> <value>
```

Modify properties of a room. Events can alter rooms within their scope to create environmental effects.

- **Requires argument:** Yes (restricted)

```
** Make the arena room safe during setup phase
event alterroom 3500 flags safe
```

#### resetroom

```
event resetroom <room vnum>
```

Reset a room to its default template state. Useful for cleaning up after an event.

- **Requires argument:** Yes (restricted)

```
** Clean up the arena after the event
event resetroom 3500
event resetroom 3501
event resetroom 3502
```

---

### Dungeon and Instance Integration (6 commands)

Events can interact with the dungeon and instance systems.

#### dungeoncommence

```
event dungeoncommence <dungeon id>
```

Start a dungeon encounter or phase. Used when an event integrates with a dungeon.

- **Requires argument:** Yes (restricted)

#### dungeoncomplete

```
event dungeoncomplete <dungeon id>
```

Mark a dungeon as successfully completed.

- **Requires argument:** Yes (restricted)

#### dungeonfailure

```
event dungeonfailure <dungeon id>
```

Mark a dungeon as failed.

- **Requires argument:** Yes (restricted)

#### instancecomplete

```
event instancecomplete <instance id>
```

Mark the current dungeon instance as completed.

- **Requires argument:** Yes (restricted)

#### instancefailure

```
event instancefailure <instance id>
```

Mark the current dungeon instance as failed.

- **Requires argument:** Yes (restricted)

#### sendfloor

```
event sendfloor <parameters>
```

Send a dungeon floor transition to players. Moves participants to the next dungeon floor.

---

### Quest Integration (2 commands)

#### quest

```
event quest <subcommand> <parameters>
```

Interact with the quest system. Start, complete, or modify quests for event participants.

```
** Complete the event quest for a player
event quest complete $n harvest_quest
```

#### specialkey

```
event specialkey <parameters>
```

Create or manage a special key item for dungeon access. Used when events gate dungeon entry.

---

### Player Management (2 commands)

#### mute

```
event mute <target>
```

Mute a character, preventing them from using speech channels. Can be used to enforce event rules.

```
** Mute a disruptive player
event mute $n
```

#### unmute

```
event unmute <target>
```

Remove a mute from a previously muted character.

```
** Unmute at event end
event unmute $n
```

---

### Area and Dungeon Unlocking (2 commands)

#### unlockarea

```
event unlockarea <area vnum>
```

Unlock a locked area, making it accessible to players. Events can gate content behind event progress.

- **Requires argument:** Yes (restricted)

```
** Unlock the bonus zone when event reaches phase 3
event unlockarea 45
event gecho {GThe sealed gates of the Shadow Realm swing open!{x
```

#### unlockdungeon

```
event unlockdungeon <dungeon id>
```

Unlock a dungeon, making it accessible to players.

- **Requires argument:** Yes (restricted)

---

### Entity Modification (1 command)

#### churchannouncetheft

```
event churchannouncetheft <parameters>
```

Announce a theft to members of a church or religion. Specialized for religion-based war events.

- **Requires argument:** Yes (restricted)

---

### Variable Management (7 commands)

Variables let event scripts store and retrieve data that persists across trigger firings.

#### varset

```
event varset <name> <type> <value>
```

Set a variable on the event entity. Supports `number`, `string`, and `room` types.

```
** Track the current phase
event varset current_phase number 2

** Store the boss room
event varset boss_room room 3001
```

#### varseton

```
event varseton <target> <name> <type> <value>
```

Set a variable on a different entity (player, mob, quest, another event).

```
** Mark a player as having participated
event varseton $n event_participated number 1
```

#### varclear

```
event varclear <name>
```

Remove a variable from the event entity.

```
** Clean up tracking variables at event end
event varclear current_phase
event varclear boss_spawned
```

#### varclearon

```
event varclearon <target> <name>
```

Clear a variable on a different entity.

```
** Remove participation marker from a player
event varclearon $n event_participated
```

#### varcopy

```
event varcopy <source_name> <dest_name>
```

Copy a variable from one name to another on the event entity.

#### varsave

```
event varsave <name>
```

Mark a variable for persistent storage. Saved variables survive server reboots.

```
** Persist the event's historical win count
event varsave total_completions
```

#### varsaveon

```
event varsaveon <target> <name>
```

Mark a variable on a different entity for persistent save.

---

### Scripting Control (2 commands)

#### call

```
event call <script vnum> [arguments]
```

Call another script as a subroutine. The called script runs and returns control to the caller. Useful for reusing common logic across multiple event scripts.

```
** Call the reward distribution subroutine
event call 505
```

#### xcall

```
event xcall <entity> <script vnum> [arguments]
```

Execute a cross-entity script call — run a script on another entity (mob, room, area, etc.) from the event context.

```
** Trigger a room script to open the arena gates
event xcall room:3500 600
```

---

### Wilderness (5 commands)

Events can modify the wilderness map.

#### wildernessmap

```
event wildernessmap <parameters>
```

Generate or modify a wilderness map overlay.

#### wildsanchor

```
event wildsanchor <parameters>
```

Set an anchor point on the wilderness map.

#### wildsoverlay

```
event wildsoverlay <parameters>
```

Apply a tile overlay to the wilderness map. Used to change terrain during events.

```
** Turn the plains to scorched earth during the invasion
event wildsoverlay 50 50 100 100 scorched
```

#### wildstile

```
event wildstile <parameters>
```

Set or modify a specific wilderness tile.

#### wildsvlink

```
event wildsvlink <parameters>
```

Create a virtual link between wilderness tiles and interior rooms.

---

### Miscellaneous (1 command)

#### treasuremap

```
event treasuremap <parameters>
```

Create or interact with a treasure map item. Used for treasure-hunt style events.

---

## Command Summary Table

| Command | Category | Description |
|---------|----------|-------------|
| `alterroom` | Room Management | Modify room properties |
| `call` | Scripting | Call a subroutine script |
| `churchannouncetheft` | Entity Modification | Announce church theft |
| `dungeoncommence` | Dungeon/Instance | Start a dungeon encounter |
| `dungeoncomplete` | Dungeon/Instance | Mark dungeon complete |
| `dungeonfailure` | Dungeon/Instance | Mark dungeon failed |
| `echoat` | Communication | Message a specific character |
| `event` | Event Control | Interact with the event system |
| `instancecomplete` | Dungeon/Instance | Mark instance complete |
| `instancefailure` | Dungeon/Instance | Mark instance failed |
| `mail` | Communication | Send in-game mail |
| `mload` | Entity Loading | Load a mobile by vnum |
| `mute` | Player Management | Mute a character |
| `oload` | Entity Loading | Load an object by vnum |
| `phaseevent` | Event Control | Advance to next phase |
| `quest` | Quest Integration | Interact with quest system |
| `questechoat` | Communication | Quest-formatted message to a character |
| `reckoning` | Event Control | Trigger reckoning event |
| `resetroom` | Room Management | Reset a room to defaults |
| `sendfloor` | Dungeon/Instance | Send floor transition |
| `specialkey` | Quest Integration | Manage special keys |
| `stageevent` | Event Control | Advance to next stage (alias for phaseevent) |
| `startevent` | Event Control | Start a world/area event |
| `startreckoning` | Event Control | Start a reckoning sequence |
| `stopevent` | Event Control | End the active event |
| `stopreckoning` | Event Control | Stop an active reckoning |
| `treasuremap` | Miscellaneous | Create/manage treasure map |
| `unlockarea` | Unlocking | Unlock a locked area |
| `unlockdungeon` | Unlocking | Unlock a dungeon |
| `unmute` | Player Management | Unmute a character |
| `varclear` | Variables | Clear a variable |
| `varclearon` | Variables | Clear variable on another entity |
| `varcopy` | Variables | Copy a variable |
| `varsave` | Variables | Persist a variable |
| `varsaveon` | Variables | Persist variable on another entity |
| `varset` | Variables | Set a variable |
| `varseton` | Variables | Set variable on another entity |
| `wildernessmap` | Wilderness | Modify wilderness map |
| `wildsanchor` | Wilderness | Set wilderness anchor |
| `wildsoverlay` | Wilderness | Apply tile overlay |
| `wildstile` | Wilderness | Set a wilderness tile |
| `wildsvlink` | Wilderness | Create virtual link |
| `wiznet` | Communication | Message the immortal channel |
| `xcall` | Scripting | Cross-entity script call |
