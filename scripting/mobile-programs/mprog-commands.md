---
layout: default
title: Commands
nav_order: 2
parent: Mobile Programs
grand_parent: Scripting
---

# Mobile Program Commands
{: .no_toc }

This page documents all 167 commands available in mobile programs. Most commands are shared across all entity types and are documented in detail in the [Shared Commands Reference](../shared-commands.md). This page provides a quick reference for every mob-available command plus full documentation for mob-only commands.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

{: .important }
> All mob program commands use the `mob` prefix. For example: `mob echo`, `mob transfer`, `mob oload`.

---

## Command Quick Reference

All 167 commands available in mob programs:

| Command | Description | Mob-Only | Restricted |
|---------|------------|:--------:|:----------:|
| [`addaffect`](#addaffect) | Add a spell-like affect (by skill number) to a character or object |  | yes |
| [`addaffectname`](#addaffectname) | Add a named affect (by string name) to a character or object |  | yes |
| [`addaura`](#addaura) | Add a magical aura effect to a character |  | yes |
| [`addspell`](#addspell) | Add a spell to a character's known spell list |  | yes |
| [`addstache`](#addstache) | Add a mustache/cosmetic customization to a character |  | yes |
| [`alteraffect`](#alteraffect) | Modify fields of an existing affect on a target entity |  | yes |
| [`alterexit`](#alterexit) | Modify properties of a room exit (flags, key, destination, etc.) |  | yes |
| [`altermob`](#altermob) | Modify properties of a mobile/NPC (stats, flags, race, etc.) |  | yes |
| [`alterobj`](#alterobj) | Modify properties of an object (values, flags, weight, etc.) |  | yes |
| [`alterroom`](#alterroom) | Modify properties of a room (flags, sector, name, etc.) |  | yes |
| [`appear`](#appear) | Make the scripting entity become visible (remove invisibility) | yes |  |
| [`applytoxin`](#applytoxin) | Apply a poison/toxin effect to a target character |  | yes |
| [`asound`](#asound) | Send a sound/message to all adjacent rooms (not the current room) |  | yes |
| [`assist`](#assist) | Force the mobile to assist another character in combat | yes | yes |
| [`at`](#at) | Execute a command at a different room location temporarily |  | yes |
| [`attach`](#attach) | Attach a script program to an entity (mob, obj, room, token) |  | yes |
| [`award`](#award) | Award experience points or quest completion credit to a player |  | yes |
| [`breathe`](#breathe) | Force the entity to perform a breath attack |  | yes |
| [`call`](#call) | Call another script as a subroutine with return value |  | yes |
| [`cancel`](#cancel) | Cancel a previously set delay timer on the current entity |  |  |
| [`cast`](#cast) | Force the entity to cast a spell on a target |  | yes |
| [`chargebank`](#chargebank) | Deduct money from a player's bank account |  | yes |
| [`chargemoney`](#chargemoney) | Deduct money from a player's carried coins | yes | yes |
| [`checkpoint`](#checkpoint) | Set a dungeon/instance checkpoint for player respawn |  | yes |
| [`churchannouncetheft`](#churchannouncetheft) | Announce a theft to members of a church/religion organization |  | yes |
| [`cloneroom`](#cloneroom) | Create a dynamic copy/clone of a room template |  | yes |
| [`condition`](#condition) | Set or modify the condition/durability value of an object or NPC stat |  | yes |
| [`crier`](#crier) | Make a town crier NPC announce a message |  | yes |
| [`damage`](#damage) | Inflict hit point damage on a target character |  | yes |
| [`deduct`](#deduct) | Deduct currency, XP, or other resource from a player |  | yes |
| [`delay`](#delay) | Set a delayed trigger (in pulses) on the current entity |  | yes |
| [`dequeue`](#dequeue) | Remove all queued/owned events from the current entity |  |  |
| [`destroyroom`](#destroyroom) | Remove/destroy a dynamically created room |  | yes |
| [`detach`](#detach) | Detach/remove a script program from an entity |  | yes |
| [`disappear`](#disappear) | Make the entity disappear (become invisible/hidden) | yes |  |
| [`dungeoncommence`](#dungeoncommence) | Start/begin a dungeon encounter or phase |  | yes |
| [`dungeoncomplete`](#dungeoncomplete) | Mark a dungeon as successfully completed |  | yes |
| [`dungeonfailure`](#dungeonfailure) | Mark a dungeon as failed |  | yes |
| [`echo`](#echo) | Send a message to all characters in the current room |  | yes |
| [`echoaround`](#echoaround) | Send a message to all characters in the room except the target |  | yes |
| [`echoat`](#echoat) | Send a message to a specific target character only |  | yes |
| [`echobattlespam`](#echobattlespam) | Send a combat-flagged message (respects battlespam filtering) |  | yes |
| [`echochurch`](#echochurch) | Send a message to all online members of a church/religion |  | yes |
| [`echogrouparound`](#echogrouparound) | Send a message to everyone in the room except target's group |  | yes |
| [`echogroupat`](#echogroupat) | Send a message to all members of the target's group |  | yes |
| [`echoleadaround`](#echoleadaround) | Send a message to everyone in room except the group leader |  | yes |
| [`echoleadat`](#echoleadat) | Send a message to the group leader only |  | yes |
| [`echonotvict`](#echonotvict) | Send a message to everyone in the room except the victim |  | yes |
| [`echoroom`](#echoroom) | Send a message to a specific room (may differ from entity's room) |  | yes |
| [`ed`](#ed) | Add or modify an extra description on an entity |  | yes |
| [`entercombat`](#entercombat) | Force a character into combat state with a target |  | yes |
| [`event`](#event) | Interact with the event system (create, modify, query, manage events) |  | yes |
| [`fade`](#fade) | Gradually make the entity disappear (fade out effect) |  | yes |
| [`fixaffects`](#fixaffects) | Recalculate and fix all affects on a character |  | yes |
| [`flee`](#flee) | Force the entity to flee from combat |  |  |
| [`force`](#force) | Force another character to execute a command against their will |  | yes |
| [`forget`](#forget) | Clear the entity's remembered target (set by remember) |  | yes |
| [`gdamage`](#gdamage) | Inflict damage on all characters in a group or area |  | yes |
| [`gecho`](#gecho) | Send a message to all players in the entire game world |  | yes |
| [`gforce`](#gforce) | Force all members of a target's group to execute a command |  | yes |
| [`goto`](#goto) | Teleport the scripting entity to a specified room |  | yes |
| [`grantclass`](#grantclass) | Grant a character access to a class |  | yes |
| [`grantskill`](#grantskill) | Grant a character knowledge of a specific skill |  | yes |
| [`grantsong`](#grantsong) | Grant a character knowledge of a bard song |  | yes |
| [`group`](#group) | Force grouping operations (join, add, leave) |  | yes |
| [`gtransfer`](#gtransfer) | Transfer an entire group of characters to a destination room |  | yes |
| [`hunt`](#hunt) | Set the mobile to track/hunt a target character | yes | yes |
| [`input`](#input) | Queue a script command as if the entity typed it (delayed input) |  | yes |
| [`inputstring`](#inputstring) | Send a string as if the entity typed it (raw input injection) |  | yes |
| [`instancecomplete`](#instancecomplete) | Mark the current dungeon instance as completed |  | yes |
| [`instancefailure`](#instancefailure) | Mark the current dungeon instance as failed |  | yes |
| [`interrupt`](#interrupt) | Interrupt a target character's current action (casting, etc.) |  | yes |
| [`junk`](#junk) | Silently destroy an object in the entity's inventory |  | yes |
| [`kill`](#kill) | Force the entity to attack a target (alias for hit) | yes | yes |
| [`link`](#link) | Create or modify an exit link between two rooms |  | yes |
| [`loadinstanced`](#loadinstanced) | Load a mob/object into the current dungeon instance context |  | yes |
| [`lockadd`](#lockadd) | Add a lock to a door or container |  | yes |
| [`lockremove`](#lockremove) | Remove a lock from a door or container |  | yes |
| [`mail`](#mail) | Send an in-game mail message to a player |  | yes |
| [`mload`](#mload) | Load/create a mobile by vnum into the game world |  | yes |
| [`mute`](#mute) | Mute a character (prevent them from speaking) |  | yes |
| [`oload`](#oload) | Load/create an object by vnum into the game world |  | yes |
| [`otransfer`](#otransfer) | Transfer an object to a different location (char, room, container) |  | yes |
| [`pageat`](#pageat) | Send a paginated message to a specific character |  | yes |
| [`peace`](#peace) | Stop all combat in the current room |  |  |
| [`persist`](#persist) | Mark a script variable for persistent storage (survives reboots) |  | yes |
| [`phaseevent`](#phaseevent) | Advance an event to its next phase |  | yes |
| [`prompt`](#prompt) | Display a custom prompt message to a target character |  | yes |
| [`purge`](#purge) | Remove entities (mobs/objects) from a room, optionally by vnum |  |  |
| [`questaccept`](#questaccept) | Accept/begin a quest for a player |  | yes |
| [`questcancel`](#questcancel) | Cancel an active quest for a player |  | yes |
| [`questcomplete`](#questcomplete) | Mark a quest as completed for a player |  | yes |
| [`questgenerate`](#questgenerate) | Procedurally generate a quest (random quest system) |  | yes |
| [`questpartcustom`](#questpartcustom) | Add a custom quest objective/part |  | yes |
| [`questpartgetitem`](#questpartgetitem) | Add a "collect item" quest objective |  | yes |
| [`questpartgoto`](#questpartgoto) | Add a "go to location" quest objective |  | yes |
| [`questpartrescue`](#questpartrescue) | Add a "rescue NPC" quest objective |  | yes |
| [`questpartslay`](#questpartslay) | Add a "kill mob" quest objective |  | yes |
| [`questscroll`](#questscroll) | Create a quest scroll item for a player |  | yes |
| [`queue`](#queue) | Schedule a delayed script execution via the event queue system |  | yes |
| [`raisedead`](#raisedead) | Resurrect a dead character |  | yes |
| [`rawkill`](#rawkill) | Kill a character instantly bypassing normal death processing |  | yes |
| [`reckoning`](#reckoning) | Trigger a reckoning event (world event system) |  | yes |
| [`remaura`](#remaura) | Remove a magical aura from a character |  | yes |
| [`remember`](#remember) | Store a target character in the entity's memory for later recall |  | yes |
| [`remort`](#remort) | Remort a character (rebirth at higher tier) |  | yes |
| [`remove`](#remove) | Remove (unequip and extract) an object from a character |  | yes |
| [`remspell`](#remspell) | Remove a spell from a character's known spell list |  | yes |
| [`remstache`](#remstache) | Remove a mustache/cosmetic customization from a character |  | yes |
| [`resetdice`](#resetdice) | Reset the dice/random values on a target object |  | yes |
| [`resetroom`](#resetroom) | Reset a room to its default/template state |  | yes |
| [`restore`](#restore) | Fully restore a character's HP, mana, and movement to maximum |  | yes |
| [`revokeclass`](#revokeclass) | Revoke a character's access to a class |  | yes |
| [`revokeskill`](#revokeskill) | Revoke a character's knowledge of a skill |  | yes |
| [`revokesong`](#revokesong) | Revoke a character's knowledge of a bard song |  | yes |
| [`saveplayer`](#saveplayer) | Force-save a player character to disk |  | yes |
| [`scriptwait`](#scriptwait) | Pause script execution for a specified duration |  | yes |
| [`selfdestruct`](#selfdestruct) | Cause the scripting entity to destroy itself |  |  |
| [`sendfloor`](#sendfloor) | Send a dungeon floor transition to players |  | yes |
| [`setclass`](#setclass) | Set a character's class |  | yes |
| [`setclasslevel`](#setclasslevel) | Set a character's level in a specific class |  | yes |
| [`setposition`](#setposition) | Set a character's position (standing, sitting, sleeping, etc.) |  | yes |
| [`setrace`](#setrace) | Set a character's race |  | yes |
| [`setrecall`](#setrecall) | Set a player's recall point (home room) |  | yes |
| [`settimer`](#settimer) | Set a countdown timer on a token or entity |  | yes |
| [`settrait`](#settrait) | Set a specific trait value on a character |  | yes |
| [`shop`](#shop) | Interact with the shop system (buy, sell, list) |  | yes |
| [`showcommand`](#showcommand) | Display a command's output to a player without executing it |  | yes |
| [`showroom`](#showroom) | Display a remote room's description to a player (scrying/preview) |  | yes |
| [`skimprove`](#skimprove) | Trigger a skill improvement check for a character |  | yes |
| [`spawndungeon`](#spawndungeon) | Create and initialize a new dungeon instance |  | yes |
| [`specialkey`](#specialkey) | Create or manage a special key item (dungeon keys) |  | yes |
| [`startcombat`](#startcombat) | Initiate combat between two characters |  | yes |
| [`startevent`](#startevent) | Start a world/area event |  | yes |
| [`startreckoning`](#startreckoning) | Start a reckoning world event sequence |  | yes |
| [`stopcombat`](#stopcombat) | Force stop combat for a character |  | yes |
| [`stopevent`](#stopevent) | Stop/end an active world/area event |  | yes |
| [`stopreckoning`](#stopreckoning) | Stop an active reckoning event |  | yes |
| [`stringmob`](#stringmob) | Modify string properties (name, short, long desc) of a mobile |  | yes |
| [`stringobj`](#stringobj) | Modify string properties (name, short, long desc) of an object |  | yes |
| [`stripaffect`](#stripaffect) | Remove an affect (by skill number) from a character or object |  | yes |
| [`stripaffectname`](#stripaffectname) | Remove a named affect (by string name) from a character or object |  | yes |
| [`take`](#take) | Force the mobile to take an object from a character | yes | yes |
| [`teleport`](#teleport) | Teleport all characters in the room to a destination | yes |  |
| [`transfer`](#transfer) | Teleport one or more characters to a destination room |  | yes |
| [`treasuremap`](#treasuremap) | Create or interact with a treasure map item |  | yes |
| [`ungroup`](#ungroup) | Force a character to leave their group |  | yes |
| [`unlockarea`](#unlockarea) | Unlock a locked area (make it accessible) |  | yes |
| [`unlockdungeon`](#unlockdungeon) | Unlock a dungeon (make it accessible to players) |  | yes |
| [`unmute`](#unmute) | Unmute a previously muted character |  | yes |
| [`usecatalyst`](#usecatalyst) | Consume a catalyst component and check if requirements are met |  | yes |
| [`varclear`](#varclear) | Clear/delete a script variable from the current entity |  | yes |
| [`varclearon`](#varclearon) | Clear a variable on a different entity (quest, event, mob, etc.) |  | yes |
| [`varcopy`](#varcopy) | Copy a variable from one name to another on the current entity |  | yes |
| [`varsave`](#varsave) | Mark a variable for persistent save (supports quest/event targets) |  | yes |
| [`varsaveon`](#varsaveon) | Mark a variable on a different entity for persistent save |  | yes |
| [`varset`](#varset) | Set a variable value (supports quest/event variable targeting) |  | yes |
| [`varseton`](#varseton) | Set a variable on a different entity (quest, event target) |  | yes |
| [`vforce`](#vforce) | Force all instances of a mobile vnum to execute a command |  | yes |
| [`wildernessmap`](#wildernessmap) | Generate or modify a wilderness map overlay |  | yes |
| [`wildsoverlay`](#wildsoverlay) | Apply a tile overlay to the wilderness map |  | yes |
| [`wildstile`](#wildstile) | Set or modify a specific wilderness tile |  | yes |
| [`wiretransfer`](#wiretransfer) | Transfer money between bank accounts |  | yes |
| [`wiznet`](#wiznet) | Send a message to the immortal communication channel (wiznet) |  | yes |
| [`xcall`](#xcall) | Execute a cross-entity script call (call script on another entity) |  | yes |
| [`zecho`](#zecho) | Send a message to all players in the same area/zone |  | yes |
| [`zot`](#zot) | Zot (smite) a character with divine punishment damage |  | yes |

{: .note }
> **Restricted** commands require elevated script security to use. See [Scripting Basics](../scripting-basics.md) for details on security levels.

---

## Mob-Only Commands

These commands are exclusive to mobile programs and are not available in other script types.

### appear

```
mob appear
```

Makes the NPC become visible if it was invisible or hidden. Removes any invisibility flags from the mobile. Often used with `disappear` to create appearing/vanishing NPCs.

**Example:**

```
** Make a ghost appear when players enter
mob appear
mob echo {WA spectral figure materializes before you!{x
```

---

### assist

```
mob assist <target>
```

Forces the mobile to join combat on the side of the specified target. The mob will begin attacking whoever the target is fighting.

**Example:**

```
** Guard assists the king when he is attacked
mob assist king
say I will defend you, my liege!
```

---

### chargemoney

```
mob chargemoney <target> <amount> <currency>
```

Deducts money directly from a player's carried coins. Unlike `chargebank` (which takes from the bank), this removes coins the player is physically carrying. Sets `lastreturn` to indicate success or failure.

**Example:**

```
** Charge the player 500 gold for a service
mob chargemoney $n 500 gold
if lastreturn == 1
  say Thank you for your payment, $n.
else
  say You don't have enough gold, $n.
endif
```

---

### disappear

```
mob disappear
```

Makes the NPC vanish from sight. Sets invisibility flags on the mobile so players cannot see or interact with it. Useful for scripted events where an NPC should appear only at certain times.

**Example:**

```
** Vanish after delivering a message
say Remember what I told you...
mob echo {D$I fades into the shadows.{x
mob disappear
```

---

### hunt

```
mob hunt <target>
```

Sets the mobile to track and pursue the specified target character across rooms. The mob will pathfind toward the target each tick until it reaches them or the hunt is cancelled. Use `mob forget` to stop hunting.

**Example:**

```
** Start hunting a player who stole something
say Thief! You will not escape!
mob hunt $n
```

---

### kill

```
mob kill <target>
```

Forces the mobile to initiate combat with the specified target. This is the standard way to make an NPC attack a player or another NPC from within a script.

**Example:**

```
** Attack intruders who don't have the right token
if tokenvalue $n 50100 0 < 1
  say You are not welcome here!
  mob kill $n
endif
```

---

### take

```
mob take <object> <target>
```

Forces the mobile to take an object directly from a character's inventory. Unlike `mob remove` (which extracts and destroys), this transfers the object to the mob's inventory.

**Example:**

```
** Confiscate a forbidden item
say That weapon is not allowed here, $n.
mob take sword $n
mob junk sword
```

---

### teleport

```
mob teleport <room vnum>
```

Teleports all player characters in the mob's current room to the specified destination room. The mob itself does not move. Useful for trap rooms or scripted transitions.

**Example:**

```
** Teleport all players to the arena
mob echo {YA blinding flash envelopes the room!{x
mob teleport 50200
```

---

## Shared Commands by Category

The following commands are shared across multiple entity types. Each entry links to the full documentation in the [Shared Commands Reference](../shared-commands.md).

### Output

| Command | Description |
|---------|------------|
| [`mob asound`](../shared-commands.md#asound) | Send a sound/message to all adjacent rooms (not the current room) |
| [`mob crier`](../shared-commands.md#crier) | Make a town crier NPC announce a message |
| [`mob echo`](../shared-commands.md#echo) | Send a message to all characters in the current room |
| [`mob echoaround`](../shared-commands.md#echoaround) | Send a message to all characters in the room except the target |
| [`mob echoat`](../shared-commands.md#echoat) | Send a message to a specific target character only |
| [`mob echobattlespam`](../shared-commands.md#echobattlespam) | Send a combat-flagged message (respects battlespam filtering) |
| [`mob echochurch`](../shared-commands.md#echochurch) | Send a message to all online members of a church/religion |
| [`mob echogrouparound`](../shared-commands.md#echogrouparound) | Send a message to everyone in the room except target's group |
| [`mob echogroupat`](../shared-commands.md#echogroupat) | Send a message to all members of the target's group |
| [`mob echoleadaround`](../shared-commands.md#echoleadaround) | Send a message to everyone in room except the group leader |
| [`mob echoleadat`](../shared-commands.md#echoleadat) | Send a message to the group leader only |
| [`mob echonotvict`](../shared-commands.md#echonotvict) | Send a message to everyone in the room except the victim |
| [`mob echoroom`](../shared-commands.md#echoroom) | Send a message to a specific room (may differ from entity's room) |
| [`mob gecho`](../shared-commands.md#gecho) | Send a message to all players in the entire game world |
| [`mob mail`](../shared-commands.md#mail) | Send an in-game mail message to a player |
| [`mob pageat`](../shared-commands.md#pageat) | Send a paginated message to a specific character |
| [`mob prompt`](../shared-commands.md#prompt) | Display a custom prompt message to a target character |
| [`mob wiznet`](../shared-commands.md#wiznet) | Send a message to the immortal communication channel (wiznet) |
| [`mob zecho`](../shared-commands.md#zecho) | Send a message to all players in the same area/zone |

### Movement & Transfer

| Command | Description |
|---------|------------|
| [`mob at`](../shared-commands.md#at) | Execute a command at a different room location temporarily |
| [`mob flee`](../shared-commands.md#flee) | Force the entity to flee from combat |
| [`mob goto`](../shared-commands.md#goto) | Teleport the scripting entity to a specified room |
| [`mob gtransfer`](../shared-commands.md#gtransfer) | Transfer an entire group of characters to a destination room |
| [`mob otransfer`](../shared-commands.md#otransfer) | Transfer an object to a different location (char, room, container) |
| [`mob transfer`](../shared-commands.md#transfer) | Teleport one or more characters to a destination room |

### Combat

| Command | Description |
|---------|------------|
| [`mob applytoxin`](../shared-commands.md#applytoxin) | Apply a poison/toxin effect to a target character |
| [`mob breathe`](../shared-commands.md#breathe) | Force the entity to perform a breath attack |
| [`mob cast`](../shared-commands.md#cast) | Force the entity to cast a spell on a target |
| [`mob damage`](../shared-commands.md#damage) | Inflict hit point damage on a target character |
| [`mob entercombat`](../shared-commands.md#entercombat) | Force a character into combat state with a target |
| [`mob gdamage`](../shared-commands.md#gdamage) | Inflict damage on all characters in a group or area |
| [`mob interrupt`](../shared-commands.md#interrupt) | Interrupt a target character's current action (casting, etc.) |
| [`mob peace`](../shared-commands.md#peace) | Stop all combat in the current room |
| [`mob rawkill`](../shared-commands.md#rawkill) | Kill a character instantly bypassing normal death processing |
| [`mob startcombat`](../shared-commands.md#startcombat) | Initiate combat between two characters |
| [`mob stopcombat`](../shared-commands.md#stopcombat) | Force stop combat for a character |
| [`mob zot`](../shared-commands.md#zot) | Zot (smite) a character with divine punishment damage |

### Entity Creation & Destruction

| Command | Description |
|---------|------------|
| [`mob cloneroom`](../shared-commands.md#cloneroom) | Create a dynamic copy/clone of a room template |
| [`mob destroyroom`](../shared-commands.md#destroyroom) | Remove/destroy a dynamically created room |
| [`mob fade`](../shared-commands.md#fade) | Gradually make the entity disappear (fade out effect) |
| [`mob junk`](../shared-commands.md#junk) | Silently destroy an object in the entity's inventory |
| [`mob loadinstanced`](../shared-commands.md#loadinstanced) | Load a mob/object into the current dungeon instance context |
| [`mob mload`](../shared-commands.md#mload) | Load/create a mobile by vnum into the game world |
| [`mob oload`](../shared-commands.md#oload) | Load/create an object by vnum into the game world |
| [`mob purge`](../shared-commands.md#purge) | Remove entities (mobs/objects) from a room, optionally by vnum |
| [`mob remove`](../shared-commands.md#remove) | Remove (unequip and extract) an object from a character |
| [`mob resetroom`](../shared-commands.md#resetroom) | Reset a room to its default/template state |

### Entity Modification

| Command | Description |
|---------|------------|
| [`mob alterexit`](../shared-commands.md#alterexit) | Modify properties of a room exit (flags, key, destination, etc.) |
| [`mob altermob`](../shared-commands.md#altermob) | Modify properties of a mobile/NPC (stats, flags, race, etc.) |
| [`mob alterobj`](../shared-commands.md#alterobj) | Modify properties of an object (values, flags, weight, etc.) |
| [`mob alterroom`](../shared-commands.md#alterroom) | Modify properties of a room (flags, sector, name, etc.) |
| [`mob condition`](../shared-commands.md#condition) | Set or modify the condition/durability value of an object or NPC stat |
| [`mob ed`](../shared-commands.md#ed) | Add or modify an extra description on an entity |
| [`mob link`](../shared-commands.md#link) | Create or modify an exit link between two rooms |
| [`mob lockadd`](../shared-commands.md#lockadd) | Add a lock to a door or container |
| [`mob lockremove`](../shared-commands.md#lockremove) | Remove a lock from a door or container |
| [`mob resetdice`](../shared-commands.md#resetdice) | Reset the dice/random values on a target object |
| [`mob setposition`](../shared-commands.md#setposition) | Set a character's position (standing, sitting, sleeping, etc.) |
| [`mob shop`](../shared-commands.md#shop) | Interact with the shop system (buy, sell, list) |
| [`mob stringmob`](../shared-commands.md#stringmob) | Modify string properties (name, short, long desc) of a mobile |
| [`mob stringobj`](../shared-commands.md#stringobj) | Modify string properties (name, short, long desc) of an object |

### Affects & Auras

| Command | Description |
|---------|------------|
| [`mob addaffect`](../shared-commands.md#addaffect) | Add a spell-like affect (by skill number) to a character or object |
| [`mob addaffectname`](../shared-commands.md#addaffectname) | Add a named affect (by string name) to a character or object |
| [`mob addaura`](../shared-commands.md#addaura) | Add a magical aura effect to a character |
| [`mob alteraffect`](../shared-commands.md#alteraffect) | Modify fields of an existing affect on a target entity |
| [`mob fixaffects`](../shared-commands.md#fixaffects) | Recalculate and fix all affects on a character |
| [`mob remaura`](../shared-commands.md#remaura) | Remove a magical aura from a character |
| [`mob stripaffect`](../shared-commands.md#stripaffect) | Remove an affect (by skill number) from a character or object |
| [`mob stripaffectname`](../shared-commands.md#stripaffectname) | Remove a named affect (by string name) from a character or object |

### Character Resources

| Command | Description |
|---------|------------|
| [`mob award`](../shared-commands.md#award) | Award experience points or quest completion credit to a player |
| [`mob chargebank`](../shared-commands.md#chargebank) | Deduct money from a player's bank account |
| [`mob deduct`](../shared-commands.md#deduct) | Deduct currency, XP, or other resource from a player |
| [`mob restore`](../shared-commands.md#restore) | Fully restore a character's HP, mana, and movement to maximum |
| [`mob wiretransfer`](../shared-commands.md#wiretransfer) | Transfer money between bank accounts |

### Skills & Classes

| Command | Description |
|---------|------------|
| [`mob addspell`](../shared-commands.md#addspell) | Add a spell to a character's known spell list |
| [`mob grantclass`](../shared-commands.md#grantclass) | Grant a character access to a class |
| [`mob grantskill`](../shared-commands.md#grantskill) | Grant a character knowledge of a specific skill |
| [`mob grantsong`](../shared-commands.md#grantsong) | Grant a character knowledge of a bard song |
| [`mob remspell`](../shared-commands.md#remspell) | Remove a spell from a character's known spell list |
| [`mob revokeclass`](../shared-commands.md#revokeclass) | Revoke a character's access to a class |
| [`mob revokeskill`](../shared-commands.md#revokeskill) | Revoke a character's knowledge of a skill |
| [`mob revokesong`](../shared-commands.md#revokesong) | Revoke a character's knowledge of a bard song |
| [`mob setclass`](../shared-commands.md#setclass) | Set a character's class |
| [`mob setclasslevel`](../shared-commands.md#setclasslevel) | Set a character's level in a specific class |
| [`mob setrace`](../shared-commands.md#setrace) | Set a character's race |
| [`mob settrait`](../shared-commands.md#settrait) | Set a specific trait value on a character |
| [`mob skimprove`](../shared-commands.md#skimprove) | Trigger a skill improvement check for a character |

### Variables & Data

| Command | Description |
|---------|------------|
| [`mob persist`](../shared-commands.md#persist) | Mark a script variable for persistent storage (survives reboots) |
| [`mob varclear`](../shared-commands.md#varclear) | Clear/delete a script variable from the current entity |
| [`mob varclearon`](../shared-commands.md#varclearon) | Clear a variable on a different entity (quest, event, mob, etc.) |
| [`mob varcopy`](../shared-commands.md#varcopy) | Copy a variable from one name to another on the current entity |
| [`mob varsave`](../shared-commands.md#varsave) | Mark a variable for persistent save (supports quest/event targets) |
| [`mob varsaveon`](../shared-commands.md#varsaveon) | Mark a variable on a different entity for persistent save |
| [`mob varset`](../shared-commands.md#varset) | Set a variable value (supports quest/event variable targeting) |
| [`mob varseton`](../shared-commands.md#varseton) | Set a variable on a different entity (quest, event target) |

### Script Control

| Command | Description |
|---------|------------|
| [`mob call`](../shared-commands.md#call) | Call another script as a subroutine with return value |
| [`mob cancel`](../shared-commands.md#cancel) | Cancel a previously set delay timer on the current entity |
| [`mob delay`](../shared-commands.md#delay) | Set a delayed trigger (in pulses) on the current entity |
| [`mob dequeue`](../shared-commands.md#dequeue) | Remove all queued/owned events from the current entity |
| [`mob input`](../shared-commands.md#input) | Queue a script command as if the entity typed it (delayed input) |
| [`mob inputstring`](../shared-commands.md#inputstring) | Send a string as if the entity typed it (raw input injection) |
| [`mob queue`](../shared-commands.md#queue) | Schedule a delayed script execution via the event queue system |
| [`mob scriptwait`](../shared-commands.md#scriptwait) | Pause script execution for a specified duration |
| [`mob selfdestruct`](../shared-commands.md#selfdestruct) | Cause the scripting entity to destroy itself |
| [`mob xcall`](../shared-commands.md#xcall) | Execute a cross-entity script call (call script on another entity) |

### Script Entity Management

| Command | Description |
|---------|------------|
| [`mob attach`](../shared-commands.md#attach) | Attach a script program to an entity (mob, obj, room, token) |
| [`mob detach`](../shared-commands.md#detach) | Detach/remove a script program from an entity |
| [`mob forget`](../shared-commands.md#forget) | Clear the entity's remembered target (set by remember) |
| [`mob remember`](../shared-commands.md#remember) | Store a target character in the entity's memory for later recall |
| [`mob settimer`](../shared-commands.md#settimer) | Set a countdown timer on a token or entity |

### Quest System

| Command | Description |
|---------|------------|
| [`mob event`](../shared-commands.md#event) | Interact with the event system (create, modify, query, manage events) |
| [`mob questaccept`](../shared-commands.md#questaccept) | Accept/begin a quest for a player |
| [`mob questcancel`](../shared-commands.md#questcancel) | Cancel an active quest for a player |
| [`mob questcomplete`](../shared-commands.md#questcomplete) | Mark a quest as completed for a player |
| [`mob questgenerate`](../shared-commands.md#questgenerate) | Procedurally generate a quest (random quest system) |
| [`mob questpartcustom`](../shared-commands.md#questpartcustom) | Add a custom quest objective/part |
| [`mob questpartgetitem`](../shared-commands.md#questpartgetitem) | Add a "collect item" quest objective |
| [`mob questpartgoto`](../shared-commands.md#questpartgoto) | Add a "go to location" quest objective |
| [`mob questpartrescue`](../shared-commands.md#questpartrescue) | Add a "rescue NPC" quest objective |
| [`mob questpartslay`](../shared-commands.md#questpartslay) | Add a "kill mob" quest objective |
| [`mob questscroll`](../shared-commands.md#questscroll) | Create a quest scroll item for a player |

### Dungeon & Instance

| Command | Description |
|---------|------------|
| [`mob checkpoint`](../shared-commands.md#checkpoint) | Set a dungeon/instance checkpoint for player respawn |
| [`mob dungeoncommence`](../shared-commands.md#dungeoncommence) | Start/begin a dungeon encounter or phase |
| [`mob dungeoncomplete`](../shared-commands.md#dungeoncomplete) | Mark a dungeon as successfully completed |
| [`mob dungeonfailure`](../shared-commands.md#dungeonfailure) | Mark a dungeon as failed |
| [`mob instancecomplete`](../shared-commands.md#instancecomplete) | Mark the current dungeon instance as completed |
| [`mob instancefailure`](../shared-commands.md#instancefailure) | Mark the current dungeon instance as failed |
| [`mob sendfloor`](../shared-commands.md#sendfloor) | Send a dungeon floor transition to players |
| [`mob spawndungeon`](../shared-commands.md#spawndungeon) | Create and initialize a new dungeon instance |
| [`mob specialkey`](../shared-commands.md#specialkey) | Create or manage a special key item (dungeon keys) |

### Events & Reckoning

| Command | Description |
|---------|------------|
| [`mob phaseevent`](../shared-commands.md#phaseevent) | Advance an event to its next phase |
| [`mob reckoning`](../shared-commands.md#reckoning) | Trigger a reckoning event (world event system) |
| [`mob startevent`](../shared-commands.md#startevent) | Start a world/area event |
| [`mob startreckoning`](../shared-commands.md#startreckoning) | Start a reckoning world event sequence |
| [`mob stopevent`](../shared-commands.md#stopevent) | Stop/end an active world/area event |
| [`mob stopreckoning`](../shared-commands.md#stopreckoning) | Stop an active reckoning event |

### Social & Appearance

| Command | Description |
|---------|------------|
| [`mob addstache`](../shared-commands.md#addstache) | Add a mustache/cosmetic customization to a character |
| [`mob remstache`](../shared-commands.md#remstache) | Remove a mustache/cosmetic customization from a character |
| [`mob showcommand`](../shared-commands.md#showcommand) | Display a command's output to a player without executing it |
| [`mob showroom`](../shared-commands.md#showroom) | Display a remote room's description to a player (scrying/preview) |

### Group & Targeting

| Command | Description |
|---------|------------|
| [`mob force`](../shared-commands.md#force) | Force another character to execute a command against their will |
| [`mob gforce`](../shared-commands.md#gforce) | Force all members of a target's group to execute a command |
| [`mob group`](../shared-commands.md#group) | Force grouping operations (join, add, leave) |
| [`mob ungroup`](../shared-commands.md#ungroup) | Force a character to leave their group |
| [`mob vforce`](../shared-commands.md#vforce) | Force all instances of a mobile vnum to execute a command |

### Religion & Organizations

| Command | Description |
|---------|------------|
| [`mob churchannouncetheft`](../shared-commands.md#churchannouncetheft) | Announce a theft to members of a church/religion organization |
| [`mob setrecall`](../shared-commands.md#setrecall) | Set a player's recall point (home room) |

### Player Management

| Command | Description |
|---------|------------|
| [`mob mute`](../shared-commands.md#mute) | Mute a character (prevent them from speaking) |
| [`mob remort`](../shared-commands.md#remort) | Remort a character (rebirth at higher tier) |
| [`mob saveplayer`](../shared-commands.md#saveplayer) | Force-save a player character to disk |
| [`mob unmute`](../shared-commands.md#unmute) | Unmute a previously muted character |

### Wilderness

| Command | Description |
|---------|------------|
| [`mob treasuremap`](../shared-commands.md#treasuremap) | Create or interact with a treasure map item |
| [`mob wildernessmap`](../shared-commands.md#wildernessmap) | Generate or modify a wilderness map overlay |
| [`mob wildsoverlay`](../shared-commands.md#wildsoverlay) | Apply a tile overlay to the wilderness map |
| [`mob wildstile`](../shared-commands.md#wildstile) | Set or modify a specific wilderness tile |

### Misc

| Command | Description |
|---------|------------|
| [`mob unlockarea`](../shared-commands.md#unlockarea) | Unlock a locked area (make it accessible) |
| [`mob unlockdungeon`](../shared-commands.md#unlockdungeon) | Unlock a dungeon (make it accessible to players) |
| [`mob usecatalyst`](../shared-commands.md#usecatalyst) | Consume a catalyst component and check if requirements are met |

### Other Commands

| Command | Description |
|---------|------------|
| [`mob raisedead`](../shared-commands.md#raisedead) | Resurrect a dead character |
