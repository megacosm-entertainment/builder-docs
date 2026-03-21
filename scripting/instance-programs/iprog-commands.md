---
layout: default
title: Commands
nav_order: 2
parent: Instance Programs
grand_parent: Scripting
---

# Instance Program Commands
{: .no_toc }

Instance programs have access to **48 commands**. Most are shared with other script types and are documented in the [Shared Commands Reference](../shared-commands.md). This page provides a complete index of all commands with full documentation, organized by category.

All commands use the `instance` prefix: `instance echoat`, `instance mload`, `instance loadinstanced`, etc.

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
> instance echoat $n The dungeon shakes!
> ```

---

## Commands Not Available to Instances

Instance programs have a focused command set oriented around dungeon lifecycle management. Many commands available to mob, object, and room programs are **not** available in instance context. Notable exclusions:

| Command | Available To | Why Not Instance |
|---------|-------------|------------------|
| `appear` / `disappear` | mob | Instances have no visibility state |
| `assist` / `kill` / `flee` | mob | Instances don't participate in combat directly |
| `at` | mob, obj, room, token | Use `loadinstanced` for instance-scoped placement |
| `cast` | mob, obj | Instances don't cast spells |
| `damage` / `gdamage` | mob, obj, room, token | Use mob scripts for damage |
| `delay` / `cancel` | mob, obj, room | Instances use tick-based timing |
| `echo` / `echoaround` | mob, obj, room, token | Use `echoat` for targeted messaging |
| `force` / `gforce` | mob, obj, room, token | Instances don't force player commands |
| `goto` / `transfer` / `gtransfer` | mob, obj, room, token | Use `sendfloor` for floor transitions |
| `purge` | mob, obj, room, token | Instance cleanup is automatic on teardown |
| `selfdestruct` | mob, obj | Instances end via completion/failure |

---

## Complete Command List

All 48 commands available to instance programs, organized by category.

### Instance Lifecycle (2 commands)

These commands control the instance's completion state.

#### instancecomplete

```
instance instancecomplete <instance>
```

Mark the current dungeon instance as successfully completed. Sets the `INSTANCE_COMPLETED` flag and fires the `completed` trigger. Prevents duplicate completion — calling this on an already-completed instance has no effect.

- **Requires argument:** Yes
- **Restricted:** Yes

```
** Mark the instance complete after all bosses are killed
if var $self bosses_remaining <= 0
  instance instancecomplete $self
endif
```

{: .note }
> Completing an instance does not automatically remove players or destroy the instance. Players can continue exploring until the instance times out or they leave voluntarily. Use the `completed` trigger to handle post-completion behavior.

#### instancefailure

```
instance instancefailure <instance>
```

Mark the current dungeon instance as failed. Sets the `INSTANCE_FAILED` flag and fires the `failed` trigger. Prevents duplicate failure — calling this on an already-failed instance has no effect.

- **Requires argument:** Yes
- **Restricted:** Yes

```
** Fail the instance if the timer runs out
if var $self time_remaining <= 0
  instance echoat $n {RTime has expired! The dungeon collapses!{x
  instance instancefailure $self
endif
```

---

### Dungeon Control (3 commands)

Commands that control the parent dungeon from within the instance.

#### dungeoncommence

```
instance dungeoncommence <dungeon>
```

Start or commence a dungeon encounter or phase. Signals the parent dungeon that a phase has begun.

- **Requires argument:** Yes
- **Restricted:** Yes

```
** Begin the dungeon encounter when players enter the boss room
instance dungeoncommence $dungeon($self)
instance echoat $n {RThe doors slam shut behind you!{x
```

#### dungeoncomplete

```
instance dungeoncomplete <dungeon>
```

Mark the parent dungeon as successfully completed from within the instance.

- **Requires argument:** Yes
- **Restricted:** Yes

```
** Complete the dungeon when all instances are done
instance dungeoncomplete $dungeon($self)
```

#### dungeonfailure

```
instance dungeonfailure <dungeon>
```

Mark the parent dungeon as failed from within the instance.

- **Requires argument:** Yes
- **Restricted:** Yes

```
** Signal dungeon failure
instance dungeonfailure $dungeon($self)
```

---

### Instance-Scoped Entity Loading (2 commands)

These commands are exclusive to instance and dungeon contexts. They load and scope entities to the instance lifecycle.

#### loadinstanced

```
instance loadinstanced mobile <vnum> <room>
instance loadinstanced mobile <vnum> <room> <varname>
instance loadinstanced object <vnum> <level> room <room>
instance loadinstanced object <vnum> <level> here
instance loadinstanced object <vnum> <level> wear <mobile>
instance loadinstanced object <vnum> <level> room <room> <varname>
```

Load a mob or object into the current instance context. The loaded entity is automatically scoped to the instance — it will be cleaned up when the instance is destroyed.

- **Requires argument:** Yes
- **Restricted:** Yes
- **Also available on:** Dungeon

**For mobiles:**
- `<vnum>` — The mobile vnum to load
- `<room>` — Room where the mobile spawns
- `<varname>` — Optional variable name to store a reference to the loaded mob

**For objects:**
- `<vnum>` — The object vnum to load
- `<level>` — Level of the object
- `room <room>` — Place in a room
- `here` — Place in the instance entrance
- `wear <mobile>` — Equip on a mobile
- `<varname>` — Optional variable name to store a reference

```
** Spawn a boss mob in the throne room
instance loadinstanced mobile 9500 4200 boss_mob
instance echoat $n {RThe Dungeon Lord rises from his throne!{x

** Spawn a key item
instance loadinstanced object 9510 50 room 4200 boss_key
```

{: .important }
> Entities loaded with `loadinstanced` are automatically tracked by the instance. They will be cleaned up when the instance is destroyed. This is the preferred way to spawn content inside instances.

#### makeinstanced

```
instance makeinstanced <mobile|object>
```

Convert an existing mob or object to be instance-scoped. Sets the `ACT2_INSTANCE_MOB` flag on mobiles or `ITEM_INSTANCE_OBJ` flag on objects. The instance assumes ownership of the entity.

- **Requires argument:** Yes
- **Restricted:** Yes
- **Also available on:** Dungeon
- **Returns:** `$lastreturn` = 1 on success, 0 on failure

Use this when an entity is loaded by standard `mload`/`oload` but needs to be scoped to the instance.

```
** Load a mob normally, then mark it as instance-owned
instance mload 8300
instance makeinstanced $lastloaded
```

{: .note }
> Prefer `loadinstanced` over `mload` + `makeinstanced` for new scripts. Use `makeinstanced` when you need finer control over entity placement before marking it as instance-scoped.

---

### General Entity Loading (2 commands)

Standard entity loading commands, shared with most script types.

#### mload

```
instance mload <mobile vnum> [room vnum]
```

Load a mobile (NPC) by vnum into the game world. When a room vnum is specified, the mob spawns there. Without a room argument, the mob spawns at a default location.

- **Restricted:** Yes

```
** Spawn a patrol guard
instance mload 8200 4100
```

{: .note }
> Mobs loaded with `mload` are not automatically scoped to the instance. Use `loadinstanced` or follow with `makeinstanced` to ensure proper instance ownership.

#### oload

```
instance oload <object vnum> [location]
```

Load an object by vnum into the game world. Can place objects in rooms or on mobs.

- **Restricted:** Yes

```
** Spawn a health potion
instance oload 3001
```

---

### Floor Management (1 command)

#### sendfloor

```
instance sendfloor <mobile> <dungeon> <floor> [mode] [scope]
```

Transfer a character (or group, or all players) to a specific floor within the dungeon. Retrieves the target instance from the dungeon's floor list and moves characters to that instance's entrance room.

- **Restricted:** Yes

**Parameters:**
- `<mobile>` — The character to transfer
- `<dungeon>` — The target dungeon entity
- `<floor>` — Floor number (1-based index)
- `[mode]` — Transfer mode (optional):
  - `silent` — No transfer messages
  - `portal` — Portal visual effect (default)
  - `movement` — Standard movement messages
- `[scope]` — Who to transfer (optional):
  - (omitted) — Transfer only the specified character
  - `group` — Transfer the character's entire group
  - `all` — Transfer all players in the instance

```
** Send the party to floor 2 via a portal effect
instance sendfloor $n $dungeon($self) 2 portal group

** Silently move everyone to the boss floor
instance sendfloor $n $dungeon($self) 5 silent all
```

{: .note }
> The target floor must have a valid entrance room defined. If the floor's instance has no entrance, the transfer silently fails.

---

### Communication (4 commands)

Commands for sending messages to players in the instance.

#### echoat

```
instance echoat <target> <message>
```

Send a message to a specific target character. This is the primary communication command for instance programs, since instances have no single "room" to echo to.

- **Restricted:** Yes

```
** Notify a player
instance echoat $n {YYou feel the dungeon shift around you...{x
```

#### questechoat

```
instance questechoat <target> <message>
```

Send a quest-themed message to a specific character. Displays with quest formatting.

- **Restricted:** Yes

```
** Quest objective update
instance questechoat $n {YObjective: Defeat the remaining 2 bosses.{x
```

#### mail

```
instance mail <player> <subject>~ <body>~
```

Send an in-game mail message to a player. Useful for sending post-dungeon results or rewards to offline players.

- **Requires argument:** Yes
- **Restricted:** Yes

```
** Mail dungeon results
instance mail $n Dungeon Results~ You completed the dungeon in 15 minutes! Reward: 500 gold.~
```

#### wiznet

```
instance wiznet <message>
```

Send a message to the immortal communication channel. Useful for debugging instance scripts or logging significant events.

- **Restricted:** Yes

```
** Log instance events for staff
instance wiznet Instance $blueprintname($self) completed by $name($n)
```

---

### Room Management (2 commands)

#### alterroom

```
instance alterroom <room vnum> <field> <value>
```

Modify properties of a room within the instance. Changes are local to the instance — they don't affect the blueprint or other instances.

- **Requires argument:** Yes
- **Restricted:** Yes

```
** Lock the boss room door
instance alterroom 4200 flags dark
```

#### resetroom

```
instance resetroom <room vnum>
```

Reset a room to its default template state. Restores the room to its blueprint-defined properties.

- **Requires argument:** Yes
- **Restricted:** Yes

```
** Reset a room after an encounter
instance resetroom 4100
```

---

### Quest Integration (2 commands)

#### quest

```
instance quest <subcommand> <parameters>
```

Interact with the quest system. Start, complete, advance, or query quests for players in the instance.

- **Restricted:** Yes

```
** Complete a dungeon quest objective
instance quest complete $n dungeon_clear
```

#### specialkey

```
instance specialkey <dungeon|ship> <key wnum> <varname>
```

Create a special key item for dungeon access. Retrieves the key definition, creates the object, and stores a reference in the specified variable.

- **Restricted:** Yes
- **Returns:** `$lastreturn` = 1 on success

```
** Create the boss room key
instance specialkey $dungeon($self) 9510 boss_key
instance echoat $n {YYou receive a {Wglowing crystal key{Y.{x
```

---

### Entity Modification (3 commands)

#### stringmob

```
instance stringmob <target> <field> <value>
```

Modify string properties (name, short description, long description) of a mobile. Used to customize instance-specific NPCs.

- **Requires argument:** Yes
- **Restricted:** Yes

```
** Scale boss name based on difficulty
if var $self difficulty >= 3
  instance stringmob $t short {RThe Enraged Dungeon Lord{x
else
  instance stringmob $t short {yThe Dungeon Lord{x
endif
```

#### stringobj

```
instance stringobj <target> <field> <value>
```

Modify string properties of an object. Used to customize instance-specific items.

- **Requires argument:** Yes
- **Restricted:** Yes

```
** Customize reward based on performance
instance stringobj $t short {Ya chest of {Wglittering treasure{x
```

#### churchannouncetheft

```
instance churchannouncetheft <parameters>
```

Announce a theft to members of a church or religion organization.

- **Requires argument:** Yes
- **Restricted:** Yes

---

### Variable Management (7 commands)

Variables let instance scripts store and retrieve data that persists across trigger firings for the duration of the instance. This is essential for tracking boss kills, phase state, timers, and other instance-wide data.

#### varset

```
instance varset <name> <type> <value>
```

Set a variable on the instance entity. Supports `number`, `string`, and `room` types.

- **Restricted:** Yes

```
** Track boss kills
instance varset bosses_killed number 0

** Store the current phase
instance varset current_phase string "exploration"

** Remember a room
instance varset boss_room room 4200
```

#### varseton

```
instance varseton <target> <name> <type> <value>
```

Set a variable on a different entity (player, mob, quest, another instance).

- **Restricted:** Yes

```
** Mark a player as having completed this instance
instance varseton $n dungeon_completed number 1
```

#### varclear

```
instance varclear <name>
```

Remove a variable from the instance entity.

- **Restricted:** Yes

```
** Clean up tracking variables
instance varclear bosses_killed
instance varclear current_phase
```

#### varclearon

```
instance varclearon <target> <name>
```

Clear a variable on a different entity.

- **Restricted:** Yes

```
** Remove a temporary marker from a player
instance varclearon $n dungeon_temp_flag
```

#### varcopy

```
instance varcopy <source_name> <dest_name>
```

Copy a variable value from one name to another on the instance entity.

- **Restricted:** Yes

```
** Save the current kill count before resetting
instance varcopy bosses_killed bosses_killed_backup
```

#### varsave

```
instance varsave <name>
```

Mark a variable for persistent storage. Saved variables survive server reboots.

- **Restricted:** Yes

```
** Persist the instance completion record
instance varsave completion_time
```

#### varsaveon

```
instance varsaveon <target> <name>
```

Mark a variable on a different entity for persistent save.

- **Restricted:** Yes

```
** Persist dungeon completion on the player
instance varsaveon $n dungeon_completed
```

---

### Event Control (5 commands)

Instance programs can interact with the game-wide event system.

#### startevent

```
instance startevent
```

Start a world or area event from the instance context.

- **Restricted:** Yes

#### stopevent

```
instance stopevent
```

Stop an active world or area event.

- **Restricted:** Yes

#### phaseevent

```
instance phaseevent
```

Advance an event to its next phase or stage.

- **Restricted:** Yes

#### stageevent

```
instance stageevent
```

Advance an event to its next stage. This is an alias for `phaseevent`.

- **Restricted:** Yes

#### event

```
instance event <subcommand> <parameters>
```

Interact with the event system (create, modify, query, manage events).

- **Restricted:** Yes

---

### Reckoning (3 commands)

Commands for the world-event reckoning system.

#### reckoning

```
instance reckoning <subcommand>
```

Trigger a reckoning event phase.

- **Requires argument:** Yes
- **Restricted:** Yes

#### startreckoning

```
instance startreckoning <parameters>
```

Start a reckoning world event sequence.

- **Requires argument:** Yes
- **Restricted:** Yes

#### stopreckoning

```
instance stopreckoning <parameters>
```

Stop an active reckoning event.

- **Requires argument:** Yes
- **Restricted:** Yes

---

### Area and Dungeon Unlocking (2 commands)

#### unlockarea

```
instance unlockarea <area vnum>
```

Unlock a locked area, making it accessible to players. Instances can gate progression by unlocking content as objectives are met.

- **Requires argument:** Yes
- **Restricted:** Yes

```
** Unlock the hidden wing after the puzzle is solved
instance unlockarea 45
instance echoat $n {GThe sealed passage grinds open!{x
```

#### unlockdungeon

```
instance unlockdungeon <dungeon>
```

Unlock a dungeon, making it accessible to players.

- **Requires argument:** Yes
- **Restricted:** Yes

---

### Player Management (2 commands)

#### mute

```
instance mute <target>
```

Mute a character, preventing them from using speech channels. Can be used for cinematic sequences or encounter mechanics.

- **Restricted:** Yes

```
** Mute during a cutscene
instance mute $n
```

#### unmute

```
instance unmute <target>
```

Remove a mute from a previously muted character.

- **Restricted:** Yes

```
** Unmute after the cutscene
instance unmute $n
```

---

### Script Control (2 commands)

#### call

```
instance call <script vnum> [arguments]
```

Call another script as a subroutine. The called script runs and returns control to the caller. Useful for reusing common logic across multiple instance scripts.

- **Restricted:** Yes

```
** Call the reward distribution subroutine
instance call 505
```

#### xcall

```
instance xcall <entity> <script vnum> [arguments]
```

Execute a cross-entity script call — run a script on another entity (mob, room, area, etc.) from the instance context.

- **Restricted:** Yes

```
** Trigger a boss mob's special behavior
instance xcall mob:9500 600
```

---

### Wilderness (5 commands)

Instances can modify the wilderness map within their scope.

#### wildernessmap

```
instance wildernessmap <parameters>
```

Generate or modify a wilderness map overlay.

- **Restricted:** Yes

#### wildsanchor

```
instance wildsanchor <parameters>
```

Set an anchor point on the wilderness map.

- **Restricted:** Yes

#### wildsoverlay

```
instance wildsoverlay <parameters>
```

Apply a tile overlay to the wilderness map.

- **Restricted:** Yes

#### wildstile

```
instance wildstile <parameters>
```

Set or modify a specific wilderness tile.

- **Restricted:** Yes

#### wildsvlink

```
instance wildsvlink <parameters>
```

Create a virtual link between wilderness tiles and interior rooms.

- **Restricted:** Yes

---

### Miscellaneous (1 command)

#### treasuremap

```
instance treasuremap <parameters>
```

Create or interact with a treasure map item.

- **Restricted:** Yes

---

## Command Summary Table

| Command | Category | Req | Description |
|---------|----------|:---:|-------------|
| `instancecomplete` | Instance Lifecycle | ✓ | Mark instance as completed |
| `instancefailure` | Instance Lifecycle | ✓ | Mark instance as failed |
| `dungeoncommence` | Dungeon Control | ✓ | Start a dungeon encounter |
| `dungeoncomplete` | Dungeon Control | ✓ | Mark dungeon complete |
| `dungeonfailure` | Dungeon Control | ✓ | Mark dungeon failed |
| `loadinstanced` | Instance Loading | ✓ | Load entity into instance context |
| `makeinstanced` | Instance Loading | ✓ | Mark entity as instance-owned |
| `mload` | Entity Loading | | Load a mobile by vnum |
| `oload` | Entity Loading | | Load an object by vnum |
| `sendfloor` | Floor Management | | Send players to a dungeon floor |
| `echoat` | Communication | | Message a specific character |
| `questechoat` | Communication | | Quest-formatted message |
| `mail` | Communication | ✓ | Send in-game mail |
| `wiznet` | Communication | | Message the immortal channel |
| `alterroom` | Room Management | ✓ | Modify room properties |
| `resetroom` | Room Management | ✓ | Reset a room to defaults |
| `quest` | Quest Integration | | Interact with quest system |
| `specialkey` | Quest Integration | | Create/manage special keys |
| `stringmob` | Entity Modification | ✓ | Modify mob strings |
| `stringobj` | Entity Modification | ✓ | Modify object strings |
| `churchannouncetheft` | Entity Modification | ✓ | Announce church theft |
| `varset` | Variables | | Set a variable |
| `varseton` | Variables | | Set variable on another entity |
| `varclear` | Variables | | Clear a variable |
| `varclearon` | Variables | | Clear variable on another entity |
| `varcopy` | Variables | | Copy a variable |
| `varsave` | Variables | | Persist a variable |
| `varsaveon` | Variables | | Persist variable on another entity |
| `startevent` | Event Control | | Start a world/area event |
| `stopevent` | Event Control | | Stop an active event |
| `phaseevent` | Event Control | | Advance to next phase |
| `stageevent` | Event Control | | Advance to next stage |
| `event` | Event Control | | Interact with event system |
| `reckoning` | Reckoning | ✓ | Trigger reckoning event |
| `startreckoning` | Reckoning | ✓ | Start a reckoning sequence |
| `stopreckoning` | Reckoning | ✓ | Stop an active reckoning |
| `unlockarea` | Unlocking | ✓ | Unlock a locked area |
| `unlockdungeon` | Unlocking | ✓ | Unlock a dungeon |
| `mute` | Player Management | | Mute a character |
| `unmute` | Player Management | | Unmute a character |
| `call` | Scripting | | Call a subroutine script |
| `xcall` | Scripting | | Cross-entity script call |
| `wildernessmap` | Wilderness | | Modify wilderness map |
| `wildsanchor` | Wilderness | | Set wilderness anchor |
| `wildsoverlay` | Wilderness | | Apply tile overlay |
| `wildstile` | Wilderness | | Set a wilderness tile |
| `wildsvlink` | Wilderness | | Create virtual link |
| `treasuremap` | Miscellaneous | | Create/manage treasure map |

{: .note }
> All 48 commands are restricted (require `MIN_SCRIPT_SECURITY`). The "Req" column indicates whether the command requires an argument — commands without `✓` accept optional arguments or no arguments.
