---
layout: default
title: Commands
nav_order: 2
parent: Area Programs
grand_parent: Scripting
---

# Area Program Commands
{: .no_toc }

This page documents all 44 commands available in area programs. For detailed syntax and examples of shared commands, see the [Shared Commands Reference](../shared-commands.md). This page provides a quick reference plus context for how each command works in the area program context.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

{: .important }
> All area program commands use the `area` prefix. For example: `area echo`, `area mload`, `area varset`.

{: .note }
> **Quest programs share this exact command table.** Every command listed here is also available in quest programs (with the `quest` prefix). The engine dispatches both area and quest scripts through the same command table.

---

## Command Quick Reference

All 44 commands available in area programs:

| Command | Description | Restricted |
|---------|------------|:----------:|
| [`alterroom`](#alterroom) | Modify properties of a room (flags, sector, name, etc.) | yes |
| [`call`](#call) | Call another script as a subroutine with return value | yes |
| [`churchannouncetheft`](#churchannouncetheft) | Announce a theft to members of a church/religion | yes |
| [`dungeoncommence`](#dungeoncommence) | Start/begin a dungeon encounter or phase | yes |
| [`dungeoncomplete`](#dungeoncomplete) | Mark a dungeon as successfully completed | yes |
| [`dungeonfailure`](#dungeonfailure) | Mark a dungeon as failed | yes |
| [`echoat`](#echoat) | Send a message to a specific target character | yes |
| [`event`](#event) | Interact with the event system | yes |
| [`instancecomplete`](#instancecomplete) | Mark the current dungeon instance as completed | yes |
| [`instancefailure`](#instancefailure) | Mark the current dungeon instance as failed | yes |
| [`mail`](#mail) | Send an in-game mail message to a player | yes |
| [`mload`](#mload) | Load/create a mobile by vnum into the game world | yes |
| [`mute`](#mute) | Mute a character (prevent them from speaking) | yes |
| [`oload`](#oload) | Load/create an object by vnum into the game world | yes |
| [`phaseevent`](#phaseevent) | Advance an event to its next phase | yes |
| [`quest`](#quest) | Interact with the quest system | yes |
| [`questechoat`](#questechoat) | Send a quest-related message to a character | yes |
| [`reckoning`](#reckoning) | Trigger a reckoning event | yes |
| [`resetroom`](#resetroom) | Reset a room to its default/template state | yes |
| [`sendfloor`](#sendfloor) | Send a dungeon floor transition to players | yes |
| [`specialkey`](#specialkey) | Create or manage a special key item (dungeon keys) | yes |
| [`stageevent`](#stageevent) | Advance an event to its next stage (alias for phaseevent) | yes |
| [`startevent`](#startevent) | Start a world/area event | yes |
| [`startreckoning`](#startreckoning) | Start a reckoning world event sequence | yes |
| [`stopevent`](#stopevent) | Stop/end an active world/area event | yes |
| [`stopreckoning`](#stopreckoning) | Stop an active reckoning event | yes |
| [`treasuremap`](#treasuremap) | Create or interact with a treasure map item | yes |
| [`unlockarea`](#unlockarea) | Unlock a locked area (make it accessible) | yes |
| [`unlockdungeon`](#unlockdungeon) | Unlock a dungeon (make it accessible to players) | yes |
| [`unmute`](#unmute) | Unmute a previously muted character | yes |
| [`varclear`](#varclear) | Clear/delete a variable from the area | yes |
| [`varclearon`](#varclearon) | Clear a variable on a different entity | yes |
| [`varcopy`](#varcopy) | Copy a variable from one name to another | yes |
| [`varsave`](#varsave) | Mark a variable for persistent save | yes |
| [`varsaveon`](#varsaveon) | Mark a variable on a different entity for persistent save | yes |
| [`varset`](#varset) | Set a variable value on the area | yes |
| [`varseton`](#varseton) | Set a variable on a different entity | yes |
| [`wildernessmap`](#wildernessmap) | Generate or modify a wilderness map overlay | yes |
| [`wildsanchor`](#wildsanchor) | Set an anchor point on the wilderness map | yes |
| [`wildsoverlay`](#wildsoverlay) | Apply a tile overlay to the wilderness map | yes |
| [`wildstile`](#wildstile) | Set or modify a specific wilderness tile | yes |
| [`wildsvlink`](#wildsvlink) | Create a virtual link between wilderness and interior rooms | yes |
| [`wiznet`](#wiznet) | Send a message to the immortal channel | yes |
| [`xcall`](#xcall) | Execute a cross-entity script call | yes |

{: .note }
> All area commands are **restricted** — they require elevated script security to execute. This prevents untrusted scripts from modifying zone-wide state.

---

## What Area Programs Cannot Do

Compared to mob programs (167 commands), area programs have a deliberately limited set. Notably absent:

- **No `echo`** — Use `echoat` to message specific players, or use mob/room programs for room-level messages
- **No movement commands** — No `goto`, `transfer`, `teleport` (areas don't have a location)
- **No combat commands** — No `kill`, `damage`, `cast`, `assist` (areas can't fight)
- **No object manipulation** — No `give`, `take`, `junk`, `purge` (use mob programs for these)
- **No affect commands** — No `addaffect`, `stripaffect` (target through mob/token programs)
- **No self-modification** — No `appear`, `disappear`, `selfdestruct`

This is intentional. Area programs coordinate zone-wide behavior; detailed entity interaction belongs in mob, object, room, or token programs. Use `area call` or `area xcall` to delegate to entity-level scripts.

---

## Command Details

### alterroom

```
area alterroom <room vnum> <field> <value>
```

Modify properties of a room within the area — flags, sector type, name, description, etc. See [Shared Commands: alterroom](../shared-commands.md#alterroom) for full field list.

**Example:**

```
** Collapse a mine entrance during the reckoning
area alterroom 3045 addflag dark
area alterroom 3045 sector underground
```

---

### call

```
area call <script vnum> [arguments]
```

Call another area script as a subroutine. The called script runs immediately, and execution returns to the caller when it finishes. The called script can set `lastreturn` to pass a value back.

**Example:**

```
** Call a shared utility script to check zone state
area call 12400
if lastreturn == 1
  area echoat $n The zone is ready for the event.
endif
```

See [Shared Commands: call](../shared-commands.md#call) for full details.

---

### churchannouncetheft

```
area churchannouncetheft <church name> <thief name>
```

Announce a theft to all online members of a church or religious organization. Used by anti-theft systems in church-controlled areas.

See [Shared Commands: churchannouncetheft](../shared-commands.md#churchannouncetheft) for full details.

---

### dungeoncommence

```
area dungeoncommence
```

Signal that a dungeon encounter or phase should begin. Used in dungeon orchestration scripts to transition from setup to active gameplay.

See [Shared Commands: dungeoncommence](../shared-commands.md#dungeoncommence) for full details.

---

### dungeoncomplete

```
area dungeoncomplete
```

Mark the current dungeon as successfully completed. Triggers completion rewards and cleanup.

See [Shared Commands: dungeoncomplete](../shared-commands.md#dungeoncomplete) for full details.

---

### dungeonfailure

```
area dungeonfailure
```

Mark the current dungeon as failed. Triggers failure handling and cleanup.

See [Shared Commands: dungeonfailure](../shared-commands.md#dungeonfailure) for full details.

---

### echoat

```
area echoat <target> <message>
```

Send a message to a specific character. This is the primary output command for area programs — since areas have no inherent room, you must target messages to specific players.

**Example:**

```
** Send a personalized message to a player
area echoat $n {YYou feel the land itself watching you...{x
```

See [Shared Commands: echoat](../shared-commands.md#echoat) for full details.

---

### event

```
area event <subcommand> [arguments]
```

Interact with the event system — create, modify, query, or manage events. Events are zone-level or world-level occurrences that can span multiple areas and trigger cascading scripts.

See [Shared Commands: startevent](../shared-commands.md#startevent) for event lifecycle commands.

---

### instancecomplete

```
area instancecomplete
```

Mark the current dungeon instance as completed. Similar to `dungeoncomplete` but operates on the instance level rather than the dungeon template.

See [Shared Commands: instancecomplete](../shared-commands.md#instancecomplete) for full details.

---

### instancefailure

```
area instancefailure
```

Mark the current dungeon instance as failed.

See [Shared Commands: instancefailure](../shared-commands.md#instancefailure) for full details.

---

### mail

```
area mail <recipient> <subject>~<body>~
```

Send an in-game mail message to a player. Useful for notifying players about zone events, quest updates, or administrative messages triggered by area scripts.

**Example:**

```
** Notify a player that their land claim has expired
area mail $n Land Claim Expired~Your claim in the Northern Forest has expired due to inactivity.~
```

See [Shared Commands: mail](../shared-commands.md#mail) for full details.

---

### mload

```
area mload <mob vnum> <room vnum>
```

Load a mobile into a specific room. In area programs, you **must** specify the destination room since the area has no inherent location.

**Example:**

```
** Spawn a patrol guard at the north gate
area mload 5032 3001
```

See [Shared Commands: mload](../shared-commands.md#mload) for full details.

---

### mute

```
area mute <target>
```

Mute a character, preventing them from using communication channels. Useful for scripted silence effects or enforcing zone rules.

See [Shared Commands: mute](../shared-commands.md#mute) for full details.

---

### oload

```
area oload <obj vnum> <room vnum>
```

Load an object into a specific room. Like `mload`, the destination room must be specified explicitly in area programs.

**Example:**

```
** Place a quest item in the treasure chamber
area oload 7200 3050
```

See [Shared Commands: oload](../shared-commands.md#oload) for full details.

---

### phaseevent

```
area phaseevent <event id>
```

Advance an event to its next phase. Events progress through phases, each of which can trigger different scripts and behavior.

See [Shared Commands: phaseevent](../shared-commands.md#phaseevent) for full details.

---

### quest

```
area quest <subcommand> [arguments]
```

Interact with the quest system — start quests, complete objectives, set steps, and manage quest state for players.

**Example:**

```
** Complete a zone-wide quest objective for all participants
area quest complete 5001 $n
```

---

### questechoat

```
area questechoat <target> <message>
```

Send a quest-related message to a specific character. Functions like `echoat` but is categorized as quest output, which may be filtered differently by client settings.

**Example:**

```
area questechoat $n {GQuest Update: The forest has been cleansed!{x
```

---

### reckoning

```
area reckoning <subcommand> [arguments]
```

Trigger or interact with the reckoning event system. Reckonings are major world events that affect entire zones.

See [Shared Commands: reckoning](../shared-commands.md#reckoning) for full details.

---

### resetroom

```
area resetroom <room vnum>
```

Reset a specific room to its default template state — restoring mobs, objects, exits, and flags to their original values.

**Example:**

```
** Reset the boss chamber after the event ends
area resetroom 3099
```

See [Shared Commands: resetroom](../shared-commands.md#resetroom) for full details.

---

### sendfloor

```
area sendfloor <arguments>
```

Send a dungeon floor transition to players, moving them between floors in a multi-level dungeon instance.

See [Shared Commands: sendfloor](../shared-commands.md#sendfloor) for full details.

---

### specialkey

```
area specialkey <arguments>
```

Create or manage a special key item used in dungeon mechanics. Special keys are temporary items that unlock specific dungeon doors or triggers.

See [Shared Commands: specialkey](../shared-commands.md#specialkey) for full details.

---

### stageevent

```
area stageevent <event id>
```

Advance an event to its next stage. This is an alias for `phaseevent` — both commands are identical in function.

See [Shared Commands: phaseevent](../shared-commands.md#phaseevent) for full details.

---

### startevent

```
area startevent <event vnum> [arguments]
```

Start a world or area event. Events are multi-phase scripted occurrences that can spawn mobs, modify rooms, and drive narrative across the zone.

**Example:**

```
** Start the harvest festival event
area startevent 9001
area echoat $n {GThe Harvest Festival has begun!{x
```

See [Shared Commands: startevent](../shared-commands.md#startevent) for full details.

---

### startreckoning

```
area startreckoning <arguments>
```

Start a reckoning world event sequence. This triggers the `prereckoning` phase first, then transitions to the active `reckoning` phase.

See [Shared Commands: startreckoning](../shared-commands.md#startreckoning) for full details.

---

### stopevent

```
area stopevent <event id>
```

Stop an active world or area event, ending all associated triggers and cleanup.

See [Shared Commands: stopevent](../shared-commands.md#stopevent) for full details.

---

### stopreckoning

```
area stopreckoning
```

Stop an active reckoning event, ending the reckoning phase and ceasing all reckoning triggers.

See [Shared Commands: stopreckoning](../shared-commands.md#stopreckoning) for full details.

---

### treasuremap

```
area treasuremap <arguments>
```

Create or interact with a treasure map item. Treasure maps lead players to hidden locations in the wilderness.

See [Shared Commands: treasuremap](../shared-commands.md#treasuremap) for full details.

---

### unlockarea

```
area unlockarea <area vnum>
```

Unlock a locked area, making it accessible to players. Areas can be locked to prevent entry until certain conditions are met.

**Example:**

```
** Unlock the forbidden zone after the ritual completes
area unlockarea 50
area echoat $n {YThe magical barrier around the Forbidden Zone shimmers and fades!{x
```

See [Shared Commands: unlockarea](../shared-commands.md#unlockarea) for full details.

---

### unlockdungeon

```
area unlockdungeon <dungeon vnum>
```

Unlock a dungeon, making it accessible to players who meet the entry requirements.

See [Shared Commands: unlockdungeon](../shared-commands.md#unlockdungeon) for full details.

---

### unmute

```
area unmute <target>
```

Unmute a previously muted character, restoring their ability to use communication channels.

See [Shared Commands: unmute](../shared-commands.md#unmute) for full details.

---

### varclear

```
area varclear <variable name>
```

Clear (delete) a script variable from the area entity. The variable is removed entirely, not just set to zero.

**Example:**

```
** Clean up event state after the event ends
area varclear event_stage
area varclear wave_count
area varclear merchant_spawned
```

See [Shared Commands: varclear](../shared-commands.md#varclear) for full details.

---

### varclearon

```
area varclearon <target type> <target id> <variable name>
```

Clear a variable on a different entity — a mob, room, token, quest, or event.

See [Shared Commands: varclearon](../shared-commands.md#varclearon) for full details.

---

### varcopy

```
area varcopy <source variable> <destination variable>
```

Copy a variable value from one name to another on the area entity.

**Example:**

```
** Save the previous state before updating
area varcopy current_stage previous_stage
area varset current_stage 3
```

See [Shared Commands: varcopy](../shared-commands.md#varcopy) for full details.

---

### varsave

```
area varsave <variable name>
```

Mark a variable for persistent save. Persistent variables survive area resets and server reboots. Use this for long-term zone state.

**Example:**

```
** Track total completions across reboots
area varset total_completions $@(total_completions + 1)
area varsave total_completions
```

See [Shared Commands: varsave](../shared-commands.md#varsave) for full details.

---

### varsaveon

```
area varsaveon <target type> <target id> <variable name>
```

Mark a variable on a different entity for persistent save.

See [Shared Commands: varsaveon](../shared-commands.md#varsaveon) for full details.

---

### varset

```
area varset <variable name> <value>
```

Set a variable value on the area entity. Variables can store numbers, strings, or expressions.

**Example:**

```
** Track event progress
area varset invasion_phase 2
area varset defender_count $@(defender_count + 1)
area varset last_reset_time $T
```

See [Shared Commands: varset](../shared-commands.md#varset) for full details.

---

### varseton

```
area varseton <target type> <target id> <variable name> <value>
```

Set a variable on a different entity — a mob, room, token, quest, or event.

See [Shared Commands: varseton](../shared-commands.md#varseton) for full details.

---

### wildernessmap

```
area wildernessmap <arguments>
```

Generate or modify a wilderness map overlay. Wilderness maps are large-scale outdoor environments rendered dynamically.

See [Shared Commands: wildernessmap](../shared-commands.md#wildernessmap) for full details.

---

### wildsanchor

```
area wildsanchor <x> <y> <room vnum>
```

Set an anchor point on the wilderness map, linking a specific coordinate to an interior room. Anchors create entry points from the wilderness into buildings, caves, or other interior spaces.

See [Shared Commands: wildsanchor](../shared-commands.md#wildsanchor) for full details.

---

### wildsoverlay

```
area wildsoverlay <arguments>
```

Apply a tile overlay to the wilderness map. Overlays change the visual appearance and properties of a region without modifying the base map.

See [Shared Commands: wildsoverlay](../shared-commands.md#wildsoverlay) for full details.

---

### wildstile

```
area wildstile <x> <y> <tile type>
```

Set or modify a specific tile on the wilderness map. Changes the terrain type at a given coordinate.

See [Shared Commands: wildstile](../shared-commands.md#wildstile) for full details.

---

### wildsvlink

```
area wildsvlink <arguments>
```

Create a virtual link between a wilderness map coordinate and an interior room. Virtual links allow seamless transitions between the outdoor map and indoor areas.

See [Shared Commands: wildsvlink](../shared-commands.md#wildsvlink) for full details.

---

### wiznet

```
area wiznet <message>
```

Send a message to the immortal communication channel (wiznet). Useful for debugging area scripts or notifying staff about zone events.

**Example:**

```
** Debug message for staff
area wiznet Area reset script fired — reset_count is now $@(reset_count)
```

See [Shared Commands: wiznet](../shared-commands.md#wiznet) for full details.

---

### xcall

```
area xcall <entity type> <entity id> <script vnum> [arguments]
```

Execute a cross-entity script call — run a script on a different entity (mob, object, room, token) from within the area program. This is how area programs delegate detailed work to entity-level scripts.

**Example:**

```
** Tell the town crier mob to announce the event
area xcall mob 5001 12500 invasion_started
```

See [Shared Commands: xcall](../shared-commands.md#xcall) for full details.

---

## Commands by Category

### Output
{: .no_toc }

| Command | Purpose |
|---------|---------|
| `echoat` | Message a specific player |
| `questechoat` | Quest-flagged message to a player |
| `wiznet` | Message to immortal channel |
| `mail` | Send in-game mail |

### Entity Creation
{: .no_toc }

| Command | Purpose |
|---------|---------|
| `mload` | Spawn a mobile in a room |
| `oload` | Spawn an object in a room |
| `resetroom` | Reset a room to defaults |
| `alterroom` | Modify room properties |

### Variables
{: .no_toc }

| Command | Purpose |
|---------|---------|
| `varset` | Set a variable |
| `varseton` | Set variable on another entity |
| `varclear` | Delete a variable |
| `varclearon` | Delete variable on another entity |
| `varcopy` | Copy variable to new name |
| `varsave` | Mark variable as persistent |
| `varsaveon` | Mark remote variable as persistent |

### Script Control
{: .no_toc }

| Command | Purpose |
|---------|---------|
| `call` | Call a subroutine script |
| `xcall` | Call a script on another entity |

### Events & Reckonings
{: .no_toc }

| Command | Purpose |
|---------|---------|
| `event` | Interact with event system |
| `startevent` | Start an event |
| `stopevent` | End an event |
| `phaseevent` | Advance event phase |
| `stageevent` | Advance event stage (alias) |
| `reckoning` | Reckoning interaction |
| `startreckoning` | Begin a reckoning |
| `stopreckoning` | End a reckoning |

### Dungeons & Instances
{: .no_toc }

| Command | Purpose |
|---------|---------|
| `dungeoncommence` | Start dungeon encounter |
| `dungeoncomplete` | Mark dungeon complete |
| `dungeonfailure` | Mark dungeon failed |
| `instancecomplete` | Mark instance complete |
| `instancefailure` | Mark instance failed |
| `sendfloor` | Floor transition |
| `specialkey` | Manage dungeon keys |
| `unlockdungeon` | Unlock a dungeon |

### Wilderness
{: .no_toc }

| Command | Purpose |
|---------|---------|
| `wildernessmap` | Map overlay generation |
| `wildsanchor` | Set map anchor point |
| `wildsoverlay` | Apply tile overlay |
| `wildstile` | Modify a map tile |
| `wildsvlink` | Create interior link |

### Other
{: .no_toc }

| Command | Purpose |
|---------|---------|
| `churchannouncetheft` | Church theft announcement |
| `treasuremap` | Treasure map management |
| `unlockarea` | Unlock a locked area |
| `mute` | Mute a character |
| `unmute` | Unmute a character |
| `quest` | Quest system interaction |
