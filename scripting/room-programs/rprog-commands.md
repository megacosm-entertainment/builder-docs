---
layout: default
title: Commands
nav_order: 2
parent: Room Programs
grand_parent: Scripting
---

# Room Program Commands
{: .no_toc }

Room programs have access to **157 commands**. Most are shared with other script types and are documented in the [Shared Commands Reference](../shared-commands.md). This page provides a brief index of all shared commands with links, plus full documentation for room-specific behavior.

All commands use the `room` prefix: `room echo`, `room transfer`, `room alterexit`, etc.

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
> room echo This is a command.
> ```

---

## Commands Not Available to Rooms

The following commands exist for other entity types but are **not** available in room programs:

| Command | Available To | Why Not Room |
|---------|-------------|--------------|
| `appear` | mob | Mob visibility toggle |
| `assist` | mob | Mob combat join |
| `cast` | mob, obj | Mob/obj spellcasting |
| `disappear` | mob | Mob visibility toggle |
| `gdamage` | mob, obj, token | Group damage |
| `goto` | mob, obj, token | Entity self-movement (rooms don't move) |
| `hunt` | mob | Mob tracking AI |
| `junk` | mob, obj, token | Inventory destruction (rooms have no inventory) |
| `kill` | mob | Mob combat initiation |
| `makeinstanced` | instance, dungeon | Instance conversion |
| `quest` | mob | Quest system (use `questaccept` etc. instead) |
| `raisedead` | mob, token | Resurrection |
| `scriptwait` | mob, obj, token | Script pause |
| `selfdestruct` | mob, obj | Self-destruction (rooms persist) |
| `stageevent` | mob | Event staging |
| `teleport` | mob | Teleport all in room (use `transfer` instead) |
| `wildsanchor` | mob | Wilderness anchor |
| `wildsvlink` | mob | Wilderness virtual link |

---

## Room-Specific Command Behavior

Several shared commands have special behavior or are particularly relevant when used in room programs.

### alterexit

```
room alterexit <direction> <field> <value>
```

Modify properties of a room exit. This is one of the most important room prog commands for puzzles, locked doors, and dynamic pathways.

**Directions:** `north`, `south`, `east`, `west`, `up`, `down` (or `n`, `s`, `e`, `w`, `u`, `d`)

**Fields:**

| Field | Description | Example Values |
|-------|-------------|----------------|
| `door` | Door state | `open`, `closed`, `locked` |
| `flags` | Exit flags | `isdoor`, `closed`, `locked`, `hidden`, `nopass` |
| `key` | Key vnum required | Object vnum |
| `dest` | Destination room | Room vnum |
| `name` | Exit name/keyword | Text string |

**Examples:**
```
** Open the north door
room alterexit north door open

** Lock the south door
room alterexit south door locked

** Create a hidden exit
room alterexit east flags hidden

** Change where east leads
room alterexit east dest 3500
```

### alterroom

```
room alterroom <field> <value>
```

Modify properties of the current room.

**Fields:**

| Field | Description | Example Values |
|-------|-------------|----------------|
| `flags` | Room flags | `dark`, `norecall`, `nomob`, `safe`, `indoors` |
| `sector` | Sector/terrain type | `inside`, `city`, `forest`, `water`, `air` |
| `name` | Room name/title | Text string |
| `desc` | Room description | Text string |
| `heal` | HP regen modifier | Numeric value |
| `mana` | Mana regen modifier | Numeric value |

**Examples:**
```
** Make the room dark
room alterroom flags dark

** Change sector to underwater
room alterroom sector underwater

** Set a custom room name
room alterroom name {CThe Crystal Chamber — Illuminated{x
```

### link

```
room link <direction> <destination_vnum>
```

Create or modify an exit linking this room to another. Room programs have a dedicated handler for this command.

**Example:**
```
** Open a new passage east
room link east 3500
room echo {GA hidden passage reveals itself to the east!{x
```

### cloneroom

```
room cloneroom <source_vnum>
```

Create a dynamic clone of a room template. Used in dungeon and instance systems for procedural generation.

**Example:**
```
room cloneroom 5000
```

### destroyroom

```
room destroyroom <vnum>
```

Remove a dynamically created (cloned) room. Only works on rooms created with `cloneroom`.

### resetroom

```
room resetroom [room_vnum]
```

Reset a room to its default template state. Without a vnum, resets the current room.

**Example:**
```
** On area reset, restore the puzzle room
room resetroom
```

### purge

```
room purge [target]
```

Remove entities from the room. Without a target, purges all non-player entities. With a target, purges the matching mob or object.

**Examples:**
```
** Remove all mobs and objects
room purge

** Remove a specific mob
room purge guard

** Remove a specific object
room purge rusty_key
```

### altermob

```
room altermob <target> <field> <value>
```

Modify properties of a mobile in the room. Room programs have a dedicated handler for this.

### remove

```
room remove <target> <object>
```

Remove (unequip and extract) an object from a character. Room programs have a dedicated handler.

### group / ungroup

```
room group <character1> <character2>
room ungroup <character>
```

Force grouping/ungrouping. Room programs have a dedicated group handler.

---

## Shared Commands Quick Reference

All shared commands below work identically in room programs as in other script types. Each links to the full documentation in the [Shared Commands Reference](../shared-commands.md).

### Output Commands

| Command | Syntax | Description |
|---------|--------|-------------|
| [`echo`](../shared-commands.md#echo) | `room echo <message>` | Message to all in room |
| [`echoat`](../shared-commands.md#echoat) | `room echoat <target> <message>` | Message to one character |
| [`echoaround`](../shared-commands.md#echoaround) | `room echoaround <target> <message>` | Message to all except target |
| [`echonotvict`](../shared-commands.md#echonotvict) | `room echonotvict <victim> <message>` | Synonym for echoaround |
| [`echoroom`](../shared-commands.md#echoroom) | `room echoroom <vnum> <message>` | Message to a specific room |
| [`echobattlespam`](../shared-commands.md#echobattlespam) | `room echobattlespam <message>` | Combat-filtered message |
| [`echochurch`](../shared-commands.md#echochurch) | `room echochurch <message>` | Message to church members |
| [`echogroupat`](../shared-commands.md#echogroupat) | `room echogroupat <target> <message>` | Message to target's group |
| [`echogrouparound`](../shared-commands.md#echogrouparound) | `room echogrouparound <target> <message>` | Message to room except group |
| [`echoleadat`](../shared-commands.md#echoleadat) | `room echoleadat <message>` | Message to group leader |
| [`echoleadaround`](../shared-commands.md#echoleadaround) | `room echoleadaround <message>` | Message to all except leader |
| [`gecho`](../shared-commands.md#gecho) | `room gecho <message>` | Global message (all players) |
| [`zecho`](../shared-commands.md#zecho) | `room zecho <message>` | Zone/area-wide message |
| [`asound`](../shared-commands.md#asound) | `room asound <message>` | Message to adjacent rooms |
| [`crier`](../shared-commands.md#crier) | `room crier <message>` | Town crier announcement |
| [`pageat`](../shared-commands.md#pageat) | `room pageat <target> <message>` | Paged message to target |
| [`prompt`](../shared-commands.md#prompt) | `room prompt <target> <text>` | Custom prompt display |
| [`mail`](../shared-commands.md#mail) | `room mail <player> <subject> <body>` | Send in-game mail |
| [`wiznet`](../shared-commands.md#wiznet) | `room wiznet <message>` | Message to immortals |

### Movement and Transfer

| Command | Syntax | Description |
|---------|--------|-------------|
| [`transfer`](../shared-commands.md#transfer) | `room transfer <target> [vnum]` | Move character to a room |
| [`gtransfer`](../shared-commands.md#gtransfer) | `room gtransfer <target> <vnum>` | Move entire group to a room |
| [`otransfer`](../shared-commands.md#otransfer) | `room otransfer <object> <dest>` | Move object to location |
| [`at`](../shared-commands.md#at) | `room at <vnum> <command>` | Execute command at another room |
| [`flee`](../shared-commands.md#flee) | `room flee` | Force flee from combat |

### Entity Loading and Destruction

| Command | Syntax | Description |
|---------|--------|-------------|
| [`mload`](../shared-commands.md#mload) | `room mload <mob_vnum>` | Spawn a mobile in the room |
| [`oload`](../shared-commands.md#oload) | `room oload <obj_vnum>` | Load an object in the room |
| [`purge`](../shared-commands.md#purge) | `room purge [target]` | Remove entities from room |
| [`cloneroom`](../shared-commands.md#cloneroom) | `room cloneroom <vnum>` | Clone a room template |
| [`destroyroom`](../shared-commands.md#destroyroom) | `room destroyroom <vnum>` | Destroy a cloned room |
| [`resetroom`](../shared-commands.md#resetroom) | `room resetroom [vnum]` | Reset room to template |
| [`loadinstanced`](../shared-commands.md#loadinstanced) | `room loadinstanced <vnum>` | Load entity for instance |
| [`remove`](../shared-commands.md#remove) | `room remove <target> <obj>` | Unequip and extract object |
| [`usecatalyst`](../shared-commands.md#usecatalyst) | `room usecatalyst <char> <name>` | Consume a catalyst |

### Combat

| Command | Syntax | Description |
|---------|--------|-------------|
| [`damage`](../shared-commands.md#damage) | `room damage <target> <amount>` | Deal direct damage |
| [`entercombat`](../shared-commands.md#entercombat) | `room entercombat <char1> <char2>` | Force two into combat |
| [`startcombat`](../shared-commands.md#startcombat) | `room startcombat <char1> <char2>` | Initiate combat |
| [`stopcombat`](../shared-commands.md#stopcombat) | `room stopcombat <target>` | End combat for character |
| [`peace`](../shared-commands.md#peace) | `room peace` | Stop all combat in room |
| [`breathe`](../shared-commands.md#breathe) | `room breathe <target>` | Breath weapon attack |
| [`rawkill`](../shared-commands.md#rawkill) | `room rawkill <target>` | Instant kill (no processing) |
| [`interrupt`](../shared-commands.md#interrupt) | `room interrupt <target>` | Interrupt casting/action |
| [`zot`](../shared-commands.md#zot) | `room zot <target> <amount>` | Divine punishment damage |

### Affects and Auras

| Command | Syntax | Description |
|---------|--------|-------------|
| [`addaffect`](../shared-commands.md#addaffect) | `room addaffect <target> <skill#> [dur] [mod]` | Apply affect by skill number |
| [`addaffectname`](../shared-commands.md#addaffectname) | `room addaffectname <target> <name> [dur] [mod]` | Apply affect by name |
| [`stripaffect`](../shared-commands.md#stripaffect) | `room stripaffect <target> <skill#>` | Remove affect by skill number |
| [`stripaffectname`](../shared-commands.md#stripaffectname) | `room stripaffectname <target> <name>` | Remove affect by name |
| [`alteraffect`](../shared-commands.md#alteraffect) | `room alteraffect <target> <idx> <field> <val>` | Modify existing affect |
| [`fixaffects`](../shared-commands.md#fixaffects) | `room fixaffects <target>` | Recalculate all affects |
| [`addaura`](../shared-commands.md#addaura) | `room addaura <target> <type> [power]` | Add aura effect |
| [`remaura`](../shared-commands.md#remaura) | `room remaura <target> <type>` | Remove aura effect |
| [`applytoxin`](../shared-commands.md#applytoxin) | `room applytoxin <target> <type> [severity]` | Apply poison/toxin |

### Entity Modification

| Command | Syntax | Description |
|---------|--------|-------------|
| [`alterexit`](../shared-commands.md#alterexit) | `room alterexit <dir> <field> <value>` | Modify room exit |
| [`alterroom`](../shared-commands.md#alterroom) | `room alterroom <field> <value>` | Modify room properties |
| [`altermob`](../shared-commands.md#altermob) | `room altermob <target> <field> <value>` | Modify a mobile |
| [`alterobj`](../shared-commands.md#alterobj) | `room alterobj <target> <field> <value>` | Modify an object |
| [`stringmob`](../shared-commands.md#stringmob) | `room stringmob <target> <field> <text>` | Change mob string fields |
| [`stringobj`](../shared-commands.md#stringobj) | `room stringobj <target> <field> <text>` | Change obj string fields |
| [`ed`](../shared-commands.md#ed) | `room ed <key> <description>` | Add/modify extra description |
| [`condition`](../shared-commands.md#condition) | `room condition <target> <value>` | Set condition/durability |
| [`link`](../shared-commands.md#link) | `room link <dir> <dest_vnum>` | Create/modify exit link |
| [`lockadd`](../shared-commands.md#lockadd) | `room lockadd <dir\|obj> <key_vnum>` | Add a lock |
| [`lockremove`](../shared-commands.md#lockremove) | `room lockremove <dir\|obj>` | Remove a lock |
| [`setposition`](../shared-commands.md#setposition) | `room setposition <target> <pos>` | Set character position |
| [`resetdice`](../shared-commands.md#resetdice) | `room resetdice <object>` | Reset dice values |

### Variables and Persistence

| Command | Syntax | Description |
|---------|--------|-------------|
| [`varset`](../shared-commands.md#varset) | `room varset <name> <value>` | Set a variable |
| [`varseton`](../shared-commands.md#varseton) | `room varseton <target> <name> <value>` | Set variable on entity |
| [`varclear`](../shared-commands.md#varclear) | `room varclear <name>` | Clear a variable |
| [`varclearon`](../shared-commands.md#varclearon) | `room varclearon <target> <name>` | Clear variable on entity |
| [`varcopy`](../shared-commands.md#varcopy) | `room varcopy <old_name> <new_name>` | Copy variable |
| [`varsave`](../shared-commands.md#varsave) | `room varsave <name>` | Persist variable to disk |
| [`varsaveon`](../shared-commands.md#varsaveon) | `room varsaveon <target> <name>` | Persist variable on entity |
| [`persist`](../shared-commands.md#persist) | `room persist <name> <value>` | Mark variable persistent |

### Script Control and Timing

| Command | Syntax | Description |
|---------|--------|-------------|
| [`call`](../shared-commands.md#call) | `room call <script_vnum> [args]` | Call another script |
| [`xcall`](../shared-commands.md#xcall) | `room xcall <target> <script> [args]` | Cross-entity script call |
| [`delay`](../shared-commands.md#delay) | `room delay <pulses>` | Set delay timer |
| [`cancel`](../shared-commands.md#cancel) | `room cancel` | Cancel delay timer |
| [`queue`](../shared-commands.md#queue) | `room queue <pulses> <script> [args]` | Schedule delayed script |
| [`dequeue`](../shared-commands.md#dequeue) | `room dequeue` | Cancel queued events |
| [`input`](../shared-commands.md#input) | `room input <command>` | Queue script input |
| [`inputstring`](../shared-commands.md#inputstring) | `room inputstring <raw_input>` | Queue raw input |
| [`settimer`](../shared-commands.md#settimer) | `room settimer <name> <ms>` | Set countdown timer |

### Script Entity Management

| Command | Syntax | Description |
|---------|--------|-------------|
| [`attach`](../shared-commands.md#attach) | `room attach <script_vnum> [args]` | Attach script to entity |
| [`detach`](../shared-commands.md#detach) | `room detach <script_vnum>` | Remove script from entity |
| [`remember`](../shared-commands.md#remember) | `room remember <target>` | Store target in memory |
| [`forget`](../shared-commands.md#forget) | `room forget` | Clear remembered target |
| [`showroom`](../shared-commands.md#showroom) | `room showroom <player> <vnum>` | Show remote room description |

### Skills, Classes, and Traits

| Command | Syntax | Description |
|---------|--------|-------------|
| [`addspell`](../shared-commands.md#addspell) | `room addspell <target> <spell>` | Grant a spell |
| [`remspell`](../shared-commands.md#remspell) | `room remspell <target> <spell>` | Remove a spell |
| [`grantskill`](../shared-commands.md#grantskill) | `room grantskill <target> <skill>` | Grant a skill |
| [`revokeskill`](../shared-commands.md#revokeskill) | `room revokeskill <target> <skill>` | Revoke a skill |
| [`grantclass`](../shared-commands.md#grantclass) | `room grantclass <target> <class>` | Grant class access |
| [`revokeclass`](../shared-commands.md#revokeclass) | `room revokeclass <target> <class>` | Revoke class access |
| [`grantsong`](../shared-commands.md#grantsong) | `room grantsong <target> <song>` | Grant a bard song |
| [`revokesong`](../shared-commands.md#revokesong) | `room revokesong <target> <song>` | Revoke a bard song |
| [`setclass`](../shared-commands.md#setclass) | `room setclass <target> <class>` | Set character class |
| [`setclasslevel`](../shared-commands.md#setclasslevel) | `room setclasslevel <target> <class> <lvl>` | Set class level |
| [`setrace`](../shared-commands.md#setrace) | `room setrace <target> <race>` | Set character race |
| [`settrait`](../shared-commands.md#settrait) | `room settrait <target> <trait> <value>` | Set trait value |
| [`addtrait`](../shared-commands.md#addtrait) | `room addtrait <target> <trait> [value]` | Add a trait |
| [`adjusttrait`](../shared-commands.md#adjusttrait) | `room adjusttrait <target> <trait> <adj>` | Adjust trait value |
| [`removetrait`](../shared-commands.md#removetrait) | `room removetrait <target> <trait>` | Remove a trait |
| [`skimprove`](../shared-commands.md#skimprove) | `room skimprove <target> <skill> [amt]` | Trigger skill improvement |

### Character Resources and Economy

| Command | Syntax | Description |
|---------|--------|-------------|
| [`award`](../shared-commands.md#award) | `room award <target> <amount> <type>` | Award XP, gold, QP, etc. |
| [`deduct`](../shared-commands.md#deduct) | `room deduct <target> <type> <amount>` | Deduct resources |
| [`chargebank`](../shared-commands.md#chargebank) | `room chargebank <target> <amount>` | Deduct from bank |
| [`wiretransfer`](../shared-commands.md#wiretransfer) | `room wiretransfer <from> <to> <amount>` | Transfer between accounts |
| [`restore`](../shared-commands.md#restore) | `room restore <target>` | Full HP/mana/move restore |

### Quest System

| Command | Syntax | Description |
|---------|--------|-------------|
| [`questaccept`](../shared-commands.md#questaccept) | `room questaccept <target> <quest>` | Start a quest |
| [`questcomplete`](../shared-commands.md#questcomplete) | `room questcomplete <target> <quest>` | Complete a quest |
| [`questcancel`](../shared-commands.md#questcancel) | `room questcancel <target> <quest>` | Cancel a quest |
| [`questgenerate`](../shared-commands.md#questgenerate) | `room questgenerate <target> [template]` | Generate random quest |
| [`questscroll`](../shared-commands.md#questscroll) | `room questscroll <target> <quest>` | Create quest scroll |
| [`questpartcustom`](../shared-commands.md#questpartcustom) | `room questpartcustom <quest> <text>` | Custom quest objective |
| [`questpartgetitem`](../shared-commands.md#questpartgetitem) | `room questpartgetitem <quest> <vnum> <qty>` | Collect item objective |
| [`questpartgoto`](../shared-commands.md#questpartgoto) | `room questpartgoto <quest> <location>` | Go-to-location objective |
| [`questpartrescue`](../shared-commands.md#questpartrescue) | `room questpartrescue <quest> <npc>` | Rescue NPC objective |
| [`questpartslay`](../shared-commands.md#questpartslay) | `room questpartslay <quest> <mob> <qty>` | Slay mob objective |
| [`questsetindex`](../shared-commands.md#questsetindex) | `room questsetindex <template> <step>` | Set quest step index |

### Dungeon and Instance

| Command | Syntax | Description |
|---------|--------|-------------|
| [`spawndungeon`](../shared-commands.md#spawndungeon) | `room spawndungeon <vnum> [seed]` | Create dungeon instance |
| [`dungeoncommence`](../shared-commands.md#dungeoncommence) | `room dungeoncommence <dungeon>` | Start dungeon encounter |
| [`dungeoncomplete`](../shared-commands.md#dungeoncomplete) | `room dungeoncomplete [dungeon]` | Mark dungeon completed |
| [`dungeonfailure`](../shared-commands.md#dungeonfailure) | `room dungeonfailure [dungeon]` | Mark dungeon failed |
| [`instancecomplete`](../shared-commands.md#instancecomplete) | `room instancecomplete` | Mark instance completed |
| [`instancefailure`](../shared-commands.md#instancefailure) | `room instancefailure` | Mark instance failed |
| [`checkpoint`](../shared-commands.md#checkpoint) | `room checkpoint <vnum>` | Set dungeon checkpoint |
| [`sendfloor`](../shared-commands.md#sendfloor) | `room sendfloor <floor>` | Dungeon floor transition |
| [`specialkey`](../shared-commands.md#specialkey) | `room specialkey <player> <key> [area]` | Create special key |
| [`unlockarea`](../shared-commands.md#unlockarea) | `room unlockarea <area>` | Unlock a locked area |
| [`unlockdungeon`](../shared-commands.md#unlockdungeon) | `room unlockdungeon <dungeon>` | Unlock a dungeon |
| [`loadinstanced`](../shared-commands.md#loadinstanced) | `room loadinstanced <vnum>` | Load instance entity |

### Events and World Systems

| Command | Syntax | Description |
|---------|--------|-------------|
| [`event`](../shared-commands.md#event) | `room event <op> <event_id> [params]` | Interact with events |
| [`startevent`](../shared-commands.md#startevent) | `room startevent <event>` | Start a world event |
| [`stopevent`](../shared-commands.md#stopevent) | `room stopevent <event>` | Stop a world event |
| [`phaseevent`](../shared-commands.md#phaseevent) | `room phaseevent <event>` | Advance event phase |
| [`reckoning`](../shared-commands.md#reckoning) | `room reckoning <op> [target] [value]` | Reckoning event system |
| [`startreckoning`](../shared-commands.md#startreckoning) | `room startreckoning <type> [params]` | Start reckoning event |
| [`stopreckoning`](../shared-commands.md#stopreckoning) | `room stopreckoning` | Stop reckoning event |

### Player Management

| Command | Syntax | Description |
|---------|--------|-------------|
| [`force`](../shared-commands.md#force) | `room force <target> <command>` | Force character to act |
| [`gforce`](../shared-commands.md#gforce) | `room gforce <leader> <command>` | Force entire group |
| [`vforce`](../shared-commands.md#vforce) | `room vforce <mob_vnum> <command>` | Force all mobs of vnum |
| [`mute`](../shared-commands.md#mute) | `room mute <target>` | Silence a character |
| [`unmute`](../shared-commands.md#unmute) | `room unmute <target>` | Un-silence a character |
| [`remort`](../shared-commands.md#remort) | `room remort <target> [class]` | Force remort |
| [`saveplayer`](../shared-commands.md#saveplayer) | `room saveplayer <target>` | Force save to disk |
| [`showcommand`](../shared-commands.md#showcommand) | `room showcommand <target> <cmd>` | Display command output |
| [`group`](../shared-commands.md#group) | `room group <char1> <char2>` | Force grouping |
| [`ungroup`](../shared-commands.md#ungroup) | `room ungroup <target>` | Force ungrouping |

### Appearance and Cosmetics

| Command | Syntax | Description |
|---------|--------|-------------|
| [`fade`](../shared-commands.md#fade) | `room fade <target>` | Fade out effect |
| [`addstache`](../shared-commands.md#addstache) | `room addstache <target> <type>` | Add cosmetic |
| [`remstache`](../shared-commands.md#remstache) | `room remstache <target>` | Remove cosmetic |

### Wilderness

| Command | Syntax | Description |
|---------|--------|-------------|
| [`wildernessmap`](../shared-commands.md#wildernessmap) | `room wildernessmap <op> [params]` | Wilderness map operations |
| [`wildsoverlay`](../shared-commands.md#wildsoverlay) | `room wildsoverlay <name> [params]` | Apply tile overlay |
| [`wildstile`](../shared-commands.md#wildstile) | `room wildstile <x> <y> <type>` | Set wilderness tile |
| [`treasuremap`](../shared-commands.md#treasuremap) | `room treasuremap <target> <map> [loc]` | Create treasure map |

### Religion and Commerce

| Command | Syntax | Description |
|---------|--------|-------------|
| [`churchannouncetheft`](../shared-commands.md#churchannouncetheft) | `room churchannouncetheft <thief> <item>` | Announce theft |
| [`shop`](../shared-commands.md#shop) | `room shop <op> [params]` | Shop system operations |
| [`setrecall`](../shared-commands.md#setrecall) | `room setrecall <target> <vnum>` | Set recall point |

---

## Command Count Summary

| Category | Count |
|----------|-------|
| Output | 19 |
| Movement and Transfer | 5 |
| Entity Loading and Destruction | 9 |
| Combat | 9 |
| Affects and Auras | 9 |
| Entity Modification | 13 |
| Variables and Persistence | 8 |
| Script Control and Timing | 9 |
| Script Entity Management | 5 |
| Skills, Classes, and Traits | 16 |
| Character Resources and Economy | 5 |
| Quest System | 11 |
| Dungeon and Instance | 12 |
| Events and World Systems | 7 |
| Player Management | 10 |
| Appearance and Cosmetics | 3 |
| Wilderness | 4 |
| Religion and Commerce | 3 |
| **Total** | **157** |
