---
layout: default
title: Commands
nav_order: 2
parent: Dungeon Programs
grand_parent: Scripting
---

# Dungeon Program Commands
{: .no_toc }

This page documents all 48 commands available in dungeon programs. Most commands are shared across all entity types and are documented in detail in the [Shared Commands Reference](../shared-commands.md). This page provides a quick reference for every dungeon-available command plus full documentation for dungeon-specific and dungeon-relevant commands.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

{: .important }
> All dungeon program commands use the `dungeon` prefix. For example: `dungeon echoat`, `dungeon mload`, `dungeon varset`.

---

## Command Quick Reference

All 48 commands available in dungeon programs:

| Command | Description | Dungeon-Specific | Restricted |
|---------|------------|:---------:|:----------:|
| [`alterroom`](#alterroom) | Modify properties of a room (flags, sector, name, etc.) | | yes |
| [`call`](#call) | Call another script as a subroutine with return value | | yes |
| [`churchannouncetheft`](#churchannouncetheft) | Announce a theft to members of a church/religion organization | | yes |
| [`dungeoncommence`](#dungeoncommence) | Start/begin a dungeon encounter | ✓ | yes |
| [`dungeoncomplete`](#dungeoncomplete) | Mark a dungeon as successfully completed | ✓ | yes |
| [`dungeonfailure`](#dungeonfailure) | Mark a dungeon as failed | ✓ | yes |
| [`echoat`](#echoat) | Send a message to a specific target (player, room, dungeon, etc.) | | yes |
| [`event`](#event) | Interact with the event system | | yes |
| [`instancecomplete`](#instancecomplete) | Mark a dungeon instance as completed | ✓ | yes |
| [`instancefailure`](#instancefailure) | Mark a dungeon instance as failed | ✓ | yes |
| [`loadinstanced`](#loadinstanced) | Load a mob/object into the dungeon instance context | ✓ | yes |
| [`mail`](#mail) | Send an in-game mail message to a player | | yes |
| [`makeinstanced`](#makeinstanced) | Convert an entity to be instance-scoped | ✓ | yes |
| [`mload`](#mload) | Load/create a mobile by vnum into the game world | | yes |
| [`mute`](#mute) | Mute a character (prevent them from speaking) | | yes |
| [`oload`](#oload) | Load/create an object by vnum into the game world | | yes |
| [`phaseevent`](#phaseevent) | Advance an event to its next phase | | yes |
| [`quest`](#quest) | Interact with the quest system | | yes |
| [`questechoat`](#questechoat) | Send a quest-context message to a target | | yes |
| [`reckoning`](#reckoning) | Modify reckoning event parameters | ✓ | yes |
| [`resetroom`](#resetroom) | Reset a room to its default state | ✓ | yes |
| [`sendfloor`](#sendfloor) | Send players to a different dungeon floor | ✓ | yes |
| [`specialkey`](#specialkey) | Create a special key item from a dungeon key definition | ✓ | yes |
| [`stageevent`](#stageevent) | Advance an event to its next stage (alias for phaseevent) | | yes |
| [`startevent`](#startevent) | Start a world/area event | | yes |
| [`startreckoning`](#startreckoning) | Start a reckoning world event | ✓ | yes |
| [`stopevent`](#stopevent) | Stop/end an active event | | yes |
| [`stopreckoning`](#stopreckoning) | Stop an active reckoning event | ✓ | yes |
| [`stringmob`](#stringmob) | Modify the string properties of a mobile | | yes |
| [`stringobj`](#stringobj) | Modify the string properties of an object | | yes |
| [`treasuremap`](#treasuremap) | Create or interact with a treasure map item | | yes |
| [`unlockarea`](#unlockarea) | Unlock a locked area for a player | | yes |
| [`unlockdungeon`](#unlockdungeon) | Unlock a dungeon for a player | ✓ | yes |
| [`unmute`](#unmute) | Unmute a previously muted character | | yes |
| [`varclear`](#varclear) | Clear/delete a variable | | yes |
| [`varclearon`](#varclearon) | Clear/delete a variable on another entity | | yes |
| [`varcopy`](#varcopy) | Copy a variable from one entity to another | | yes |
| [`varsave`](#varsave) | Mark a variable for persistent storage | | yes |
| [`varsaveon`](#varsaveon) | Mark a variable on another entity for persistent storage | | yes |
| [`varset`](#varset) | Set a variable value | | yes |
| [`varseton`](#varseton) | Set a variable on another entity | | yes |
| [`wildsanchor`](#wildsanchor) | Set an anchor point on the wilderness map | | yes |
| [`wildernessmap`](#wildernessmap) | Generate or modify a wilderness map overlay | | yes |
| [`wildsoverlay`](#wildsoverlay) | Apply a tile overlay to the wilderness map | | yes |
| [`wildstile`](#wildstile) | Set or modify a specific wilderness tile | | yes |
| [`wildsvlink`](#wildsvlink) | Create a virtual link between wilderness and interior rooms | | yes |
| [`wiznet`](#wiznet) | Send a message to the immortal wiznet channel | | yes |
| [`xcall`](#xcall) | Cross-entity call — execute a script on another entity | | yes |

---

## Dungeon State Commands

These commands control the dungeon lifecycle — starting, completing, and failing dungeons and instances.

### dungeoncommence

```
dungeon dungeoncommence <dungeon>
```

Marks the dungeon as officially started by setting the `DUNGEON_COMMENCED` flag. Fires the `dungeon_commenced` trigger on the dungeon. Has no effect if the dungeon is already commenced.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `<dungeon>` | dungeon | The dungeon instance to commence (typically `$self`) |

**Example:**
```
dungeon dungeoncommence $self
```

{: .note }
> This command both sets the flag and fires the `dungeon_commenced` trigger. If you are already inside a `dungeon_commenced` trigger handler, calling this again has no effect (the flag is already set).

### dungeoncomplete

```
dungeon dungeoncomplete <dungeon>
```

Marks the dungeon as successfully completed by setting the `DUNGEON_COMPLETED` flag. Fires the `completed` trigger on the dungeon before setting the flag. Has no effect if the dungeon is already completed.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `<dungeon>` | dungeon | The dungeon instance to complete (typically `$self`) |

**Example:**
```
** Check if boss is dead, then complete the dungeon
if var $self boss_defeated == 1
  dungeon dungeoncomplete $self
endif
```

### dungeonfailure

```
dungeon dungeonfailure <dungeon>
```

Marks the dungeon as failed by setting the `DUNGEON_FAILED` flag. Fires the `failed` trigger on the dungeon after setting the flag. Has no effect if the dungeon is already marked as failed.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `<dungeon>` | dungeon | The dungeon instance to fail (typically `$self`) |

**Example:**
```
** Timer ran out — fail the dungeon
dungeon dungeonfailure $self
```

### instancecomplete

```
dungeon instancecomplete <instance>
```

Marks a specific instance (floor) as completed by setting the `INSTANCE_COMPLETED` flag. Fires the `completed` trigger on the instance before setting the flag. Has no effect if the instance is already completed.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `<instance>` | instance | The instance to mark as completed |

**Example:**
```
dungeon instancecomplete $(instance)
```

### instancefailure

```
dungeon instancefailure <instance>
```

Marks a specific instance (floor) as failed by setting the `INSTANCE_FAILED` flag. Fires the `failed` trigger on the instance after setting the flag. Has no effect if the instance is already failed.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `<instance>` | instance | The instance to mark as failed |

**Example:**
```
dungeon instancefailure $(instance)
```

---

## Floor and Room Commands

These commands manage dungeon floor transitions, room resets, and room modifications.

### sendfloor

```
dungeon sendfloor <player> <dungeon> <floor_number> <transfer_mode> [group|all]
```

Transfers a player (and optionally their group or all dungeon players) to a specific dungeon floor. The player is moved to the floor's entrance room.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `<player>` | mobile | The player to transfer |
| `<dungeon>` | dungeon/room | The dungeon (or a room in the dungeon) |
| `<floor_number>` | number | Floor index (1-based) |
| `<transfer_mode>` | string | Transfer animation: `portal`, `teleport`, `walk`, etc. |
| `[group]` | string | Optional — `group` transfers the player's group; `all` transfers everyone |

**Transfer modes:** `portal`, `teleport`, `walk`, and other modes defined in the transfer mode table.

**Example:**
```
** Send the player and their group to floor 2
dungeon sendfloor $n $self 2 portal group
```

**Example — advance all players to the next floor:**
```
dungeon sendfloor $n $self 3 teleport all
```

{: .warning }
> Floor numbers are 1-indexed. Specifying a floor number less than 1 or greater than the dungeon's total floor count will silently fail. Always validate floor numbers before calling this command.

### resetroom

```
dungeon resetroom <room>
```

Resets a room to its default state, re-running all resets defined for that room. This restores mobs, objects, exits, and room properties to their initial configuration.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `<room>` | room | The room to reset |

**Example:**
```
** Reset the boss room after the encounter ends
dungeon resetroom $self.boss_room
```

### alterroom

```
dungeon alterroom <room> <field> <value>
```

Modifies a property of a room — flags, sector type, name, description, etc. See [Shared Commands Reference](../shared-commands.md#alterroom) for full field documentation.

**Example:**
```
** Lock the boss room doors
dungeon alterroom $self.boss_room flags +norecall
```

---

## Entity Loading Commands

These commands load mobs and objects into the dungeon, with optional instance scoping.

### loadinstanced

```
dungeon loadinstanced mobile <vnum> <room> [variable_name]
dungeon loadinstanced object <vnum> <level> room|here|wear <target> [variable_name]
```

Loads a mobile or object into the game world, automatically marking it as instance-scoped. Instance-scoped entities are tracked by the dungeon instance and cleaned up when the instance is destroyed.

The calling entity must be within a dungeon or instance context for this command to work.

**Arguments (mobile):**

| Argument | Type | Description |
|----------|------|-------------|
| `mobile` | string | Literal keyword `mobile` |
| `<vnum>` | number/mobile | The mobile vnum to load |
| `<room>` | room | The room to load the mobile into |
| `[variable_name]` | string | Optional — store a reference to the loaded mob in this variable |

**Arguments (object):**

| Argument | Type | Description |
|----------|------|-------------|
| `object` | string | Literal keyword `object` |
| `<vnum>` | number/object | The object vnum to load |
| `<level>` | number | The object level |
| `room\|here\|wear` | string | Where to place the object |
| `<target>` | entity | The target (room, character, or container) |
| `[variable_name]` | string | Optional — store a reference to the loaded object in this variable |

**Example:**
```
** Spawn boss mob in the boss room, save reference
dungeon loadinstanced mobile 50100 $self.boss_room boss_mob
** Spawn treasure chest in the same room
dungeon loadinstanced object 50200 100 room $self.boss_room
```

**Returns:** Sets `lastreturn` to 1 on success, 0 on failure.

{: .note }
> Prefer `loadinstanced` over `mload`/`oload` for dungeon content. Instance-scoped entities are automatically cleaned up when the dungeon instance is destroyed, preventing entity leaks.

### makeinstanced

```
dungeon makeinstanced <mobile|object>
```

Converts an existing entity (mobile or object) to be instance-scoped. The entity must be in a room that belongs to the calling dungeon's instance. Only available in dungeon programs and instance programs.

After conversion, the entity is removed from the instance's "external entities" tracking list, meaning it will be treated as part of the dungeon and cleaned up on instance destruction.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `<mobile\|object>` | mobile/object | The entity to convert to instance scope |

**Example:**
```
** Make a dynamically spawned mob part of the instance
dungeon mload 50105
dungeon makeinstanced $mob_loaded
```

**Returns:** Sets `lastreturn` to 1 on success, 0 on failure.

### mload

```
dungeon mload <vnum> [room] [variable_name]
```

Loads a mobile by vnum into the game world. See [Shared Commands Reference](../shared-commands.md#mload) for full documentation.

{: .note }
> In dungeon programs, prefer [`loadinstanced`](#loadinstanced) to ensure spawned entities are tracked by the instance.

### oload

```
dungeon oload <vnum> <level> <location> [target] [variable_name]
```

Loads an object by vnum into the game world. See [Shared Commands Reference](../shared-commands.md#oload) for full documentation.

---

## Communication Commands

### echoat

```
dungeon echoat <target> <message>
```

Sends a message to a specific target. The target can be a player (by name or variable), a room, an area, an instance, a dungeon, or a church. When the target is a dungeon, the message is sent to all players in the dungeon.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `<target>` | mobile/room/area/instance/dungeon/church | The message recipient |
| `<message>` | string | The message text (supports color codes and quick-code variables) |

**Example — message to entire dungeon:**
```
dungeon echoat $self {YA terrible roar echoes through the dungeon!{x
```

**Example — message to a single player:**
```
dungeon echoat $n {cYou sense something watching you from the shadows.{x
```

### questechoat

```
dungeon questechoat <target> <message>
```

Sends a quest-context message to a target. Functions identically to `echoat` but tagged for quest display filtering.

### wiznet

```
dungeon wiznet <message>
```

Sends a message to the immortal wiznet channel. Useful for debugging dungeon scripts.

**Example:**
```
dungeon wiznet Dungeon $self phase transition: phase 2 starting
```

---

## Dungeon Key and Unlock Commands

### specialkey

```
dungeon specialkey <dungeon> <key_id> [arguments...]
```

Creates a special key object from a dungeon's special key definition. The key is loaded from the dungeon's configured key vnum for the specified key ID.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `<dungeon>` | dungeon | The dungeon whose key definitions to use |
| `<key_id>` | number | The special key identifier |

**Returns:** Sets `lastreturn` to 1 on success, 0 on failure.

{: .note }
> Special keys are defined in the dungeon editor's key configuration. This command instantiates a key object from that definition.

### unlockdungeon

```
dungeon unlockdungeon <player> <dungeon>
```

Unlocks a dungeon for a specific player, granting them access to enter it. The dungeon can be specified as a dungeon entity, a room within a dungeon, a vnum, or a widevnum string.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `<player>` | mobile | The player to grant access (must be a PC, not an NPC) |
| `<dungeon>` | dungeon/room/number/string | The dungeon to unlock |

**Returns:** Sets `lastreturn` to 1 on success, 0 on failure.

**Example:**
```
** Unlock the next dungeon tier for the player
dungeon unlockdungeon $n 50200
```

### unlockarea

```
dungeon unlockarea <player> <area>
```

Unlocks a locked area for a player, granting them access. See [Shared Commands Reference](../shared-commands.md#unlockarea) for full documentation.

---

## Reckoning Commands

The reckoning system is a world-event mechanic that provides timed challenges. These commands require script security level 9.

### startreckoning

```
dungeon startreckoning [duration] [skip_warning]
```

Starts a reckoning world event. Duration defaults to 30 minutes, clamped to 15–60.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `[duration]` | number | Duration in minutes (15–60, default: 30) |
| `[skip_warning]` | string | `true` or `yes` to skip the pre-reckoning warning phase |

**Returns:** Sets `lastreturn` to the duration on success, 0 if a reckoning is already active.

**Example:**
```
dungeon startreckoning 45
```

### stopreckoning

```
dungeon stopreckoning [immediate_or_duration]
```

Stops an active reckoning event. Can end it immediately or set a shortened remaining duration.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `[immediate_or_duration]` | boolean/number | `true`/`yes`/`immediate` for instant end; a number sets remaining minutes |

**Returns:** Sets `lastreturn` to remaining minutes, -1 for immediate stop, 0 on failure.

**Example:**
```
** End the reckoning immediately
dungeon stopreckoning immediate
```

### reckoning

```
dungeon reckoning <field> <operator> <value>
```

Modifies reckoning event parameters while a reckoning is active.

**Arguments:**

| Argument | Type | Description |
|----------|------|-------------|
| `<field>` | string | Parameter to modify: `chance`, `cooldown`, `duration`, `intensity` |
| `<operator>` | string | `=` (set), `+` (add), or `-` (subtract) |
| `<value>` | number | The value to apply |

Available fields depend on reckoning state:
- **During active reckoning:** only `cooldown` can be modified
- **During cooldown:** `chance`, `cooldown`, `duration`, `intensity` can be modified

**Example:**
```
** Increase reckoning intensity by 5
dungeon reckoning intensity + 5
```

---

## Variable Commands

Variable commands store and retrieve data on entities. See [Variables and Tokens](../variables-and-tokens.md) for concepts.

### varset

```
dungeon varset <entity> <variable_name> <value>
```

Sets a variable on an entity. See [Shared Commands Reference](../shared-commands.md#varset) for full documentation.

**Example:**
```
dungeon varset $self boss_phase 1
dungeon varset $self timer 300
```

### varseton

```
dungeon varseton <entity> <variable_name> <value>
```

Sets a variable on another entity. See [Shared Commands Reference](../shared-commands.md#varseton).

### varclear

```
dungeon varclear <variable_name>
```

Clears (deletes) a variable on the dungeon. See [Shared Commands Reference](../shared-commands.md#varclear).

### varclearon

```
dungeon varclearon <entity> <variable_name>
```

Clears a variable on another entity. See [Shared Commands Reference](../shared-commands.md#varclearon).

### varcopy

```
dungeon varcopy <source_entity> <source_var> <dest_entity> <dest_var>
```

Copies a variable from one entity to another. See [Shared Commands Reference](../shared-commands.md#varcopy).

### varsave

```
dungeon varsave <variable_name>
```

Marks a variable for persistent storage (survives reboots). See [Shared Commands Reference](../shared-commands.md#varsave).

### varsaveon

```
dungeon varsaveon <entity> <variable_name>
```

Marks a variable on another entity for persistent storage. See [Shared Commands Reference](../shared-commands.md#varsaveon).

---

## Script Control Commands

### call

```
dungeon call <script_vnum> [arguments...]
```

Calls another script as a subroutine. The called script runs to completion, then control returns. The called script's `lastreturn` value is available after the call. See [Shared Commands Reference](../shared-commands.md#call).

**Example:**
```
** Call a shared boss-spawn utility script
dungeon call 50150
if lastreturn == 1
  dungeon echoat $self {RThe boss has arrived!{x
endif
```

### xcall

```
dungeon xcall <entity> <script_vnum> [arguments...]
```

Cross-entity call — executes a script on another entity (mob, room, object, etc.) as if that entity were running it. See [Shared Commands Reference](../shared-commands.md#xcall).

---

## Event Commands

### event

```
dungeon event <subcommand> [arguments...]
```

Interacts with the event system. See [Shared Commands Reference](../shared-commands.md#event) for subcommands.

### startevent

```
dungeon startevent <event_vnum> [arguments...]
```

Starts a world or area event. See [Shared Commands Reference](../shared-commands.md#startevent).

### stopevent

```
dungeon stopevent <event>
```

Stops an active event. See [Shared Commands Reference](../shared-commands.md#stopevent).

### phaseevent

```
dungeon phaseevent <event>
```

Advances an event to its next phase. See [Shared Commands Reference](../shared-commands.md#phaseevent).

### stageevent

Alias for [`phaseevent`](#phaseevent).

---

## Quest Commands

### quest

```
dungeon quest <subcommand> [arguments...]
```

Interacts with the quest system. See [Shared Commands Reference](../shared-commands.md#quest) for subcommands.

---

## Player Management Commands

### mute

```
dungeon mute <player>
```

Mutes a character, preventing them from speaking. See [Shared Commands Reference](../shared-commands.md#mute).

### unmute

```
dungeon unmute <player>
```

Unmutes a previously muted character. See [Shared Commands Reference](../shared-commands.md#unmute).

### mail

```
dungeon mail <player> <subject> <body>
```

Sends an in-game mail message. See [Shared Commands Reference](../shared-commands.md#mail).

---

## Entity Modification Commands

### stringmob

```
dungeon stringmob <mobile> <field> <value>
```

Modifies string properties of a mobile (name, short description, long description, etc.). See [Shared Commands Reference](../shared-commands.md#stringmob).

### stringobj

```
dungeon stringobj <object> <field> <value>
```

Modifies string properties of an object. See [Shared Commands Reference](../shared-commands.md#stringobj).

---

## Religion and Organization Commands

### churchannouncetheft

```
dungeon churchannouncetheft <church> <thief> <item>
```

Announces a theft to members of a church/religion organization. See [Shared Commands Reference](../shared-commands.md#churchannouncetheft).

---

## Wilderness Commands

These commands manipulate the wilderness map system.

### wildernessmap

```
dungeon wildernessmap <arguments...>
```

Generates or modifies a wilderness map overlay. See [Shared Commands Reference](../shared-commands.md#wildernessmap).

### wildsoverlay

```
dungeon wildsoverlay <arguments...>
```

Applies a tile overlay to the wilderness map. See [Shared Commands Reference](../shared-commands.md#wildsoverlay).

### wildsanchor

```
dungeon wildsanchor <arguments...>
```

Sets an anchor point on the wilderness map. See [Shared Commands Reference](../shared-commands.md#wildsanchor).

### wildstile

```
dungeon wildstile <arguments...>
```

Sets or modifies a specific wilderness tile. See [Shared Commands Reference](../shared-commands.md#wildstile).

### wildsvlink

```
dungeon wildsvlink <arguments...>
```

Creates a virtual link between wilderness and interior rooms. See [Shared Commands Reference](../shared-commands.md#wildsvlink).

---

## Miscellaneous Commands

### treasuremap

```
dungeon treasuremap <arguments...>
```

Creates or interacts with a treasure map item. See [Shared Commands Reference](../shared-commands.md#treasuremap).
