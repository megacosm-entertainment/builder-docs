---
layout: default
title: Commands
nav_order: 2
parent: Token Programs
grand_parent: Scripting
---

# Token Program Commands
{: .no_toc }

Token programs have access to **162 commands** in the standard token context, plus **3 TokenOther commands** (`adjust`, `give`, `junk`) that manipulate tokens on host entities. Most commands are shared with other entity types and documented in the [Shared Commands Reference](../shared-commands.html). This page lists every available command with brief descriptions, and provides full documentation for the 5 commands unique to tokens.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Token-Specific Commands

These commands are unique to token programs or have token-specific behavior not found in other entity types.

### adjust

```
token adjust <target> <token_vnum> <slot> <operator> <value>
token adjust <token_ref> <slot> <operator> <value>
```

Adjusts a numeric property on a token. Works in both the standard token context and the TokenOther context.

**Target:** A character, object, or room that carries the token, or a direct token reference (`$i` for self).

**Slot:** A value index (`0`–`7`), `timer`, or `tempstore1`–`tempstore4`.

**Operators:**

| Operator | Operation |
|:---------|:----------|
| `+` | Add |
| `-` | Subtract |
| `*` | Multiply |
| `/` | Divide (errors on zero) |
| `%` | Modulo (errors on zero) |
| `=` | Set (assign) |
| `&` | Bitwise AND |
| `\|` | Bitwise OR |
| `!` | Clear bits (AND NOT) |
| `^` | Bitwise XOR |

**Examples:**
```
** Increment this token's value[0] by 1
token adjust $i 0 + 1

** Set value[3] on token vnum 5001 on $n to 50
token adjust $n 5001 3 = 50

** Subtract 5 from a token's timer
token adjust $i timer - 5

** Double value[1] on a room's token
token adjust $here 5001 1 * 2
```

---

### castfailure

```
token castfailure
```

Handles spell casting failure for token-defined spells. Called when a spell cast from this token fails (e.g., the skill check fails). Token-exclusive — only available in token programs.

**Example:**
```
token echoat $n {RYour spell fizzles and dissipates!{x
token echoaround $n {R$N's spell fizzles harmlessly.{x
token castfailure
```

---

### castrecover

```
token castrecover
```

Handles spell casting recovery/cooldown for token-defined spells. Called after a successful cast to handle any recovery or cooldown logic. Token-exclusive — only available in token programs.

**Example:**
```
token echoat $n {YYou feel drained from the casting.{x
token castrecover
```

---

### give

```
token give <target> <token_vnum> [variable_name]
```

Creates a new instance of the specified token and attaches it to the target character, object, or room. If a variable name is provided, stores a reference to the new token in that variable.

Respects the `SINGULAR` flag — if the target already has a copy of a singular token, the command silently fails.

Fires the `token_given` trigger on the newly created token.

**Examples:**
```
** Give token 5001 to the triggering player
token give $n 5001

** Give token 5002 to the room, store reference in $new_tok
token give $here 5002 new_tok

** Give token 5003 to an object
token give $p 5003
```

---

### junk

```
token junk <target> <token_vnum>
token junk <token_ref>
token junk self
```

Destroys a token. Can target a specific token on a host entity by vnum, destroy a token by direct reference, or destroy the currently running token with `self`.

Using `self` should be the **last command** in the script — execution stops immediately after the token is extracted.

Fires the `token_removed` trigger before extraction.

**Examples:**
```
** Remove token 5001 from $n
token junk $n 5001

** Destroy a specific token reference
token junk $q

** Self-destruct (must be last command)
token junk self
```

---

## Shared Commands Quick Reference

The following commands are available in token programs and documented in detail in the [Shared Commands Reference](../shared-commands.html). Use the `token` prefix for all commands.

### Output Commands

| Command | Description |
|:--------|:------------|
| [`echo`](../shared-commands.html#echo) | Send a message to all characters in the current room |
| [`echoat`](../shared-commands.html#echoat) | Send a message to a specific target character |
| [`echoaround`](../shared-commands.html#echoaround) | Send a message to everyone except the target |
| [`echoroom`](../shared-commands.html#echoroom) | Send a message to a specific room |
| [`gecho`](../shared-commands.html#gecho) | Broadcast a message to all players in the game |
| [`zecho`](../shared-commands.html#zecho) | Send a message to all players in the same area |
| [`echobattlespam`](../shared-commands.html#echobattlespam) | Send a combat-flagged message |
| [`echochurch`](../shared-commands.html#echochurch) | Send a message to all members of a church |
| [`echogroupat`](../shared-commands.html#echogroupat) | Send a message to the target's group |
| [`echogrouparound`](../shared-commands.html#echogrouparound) | Send a message to everyone except the target's group |
| [`echoleadat`](../shared-commands.html#echoleadat) | Send a message to the group leader |
| [`echoleadaround`](../shared-commands.html#echoleadaround) | Send a message to everyone except the group leader |
| [`echonotvict`](../shared-commands.html#echonotvict) | Send a message to everyone except the victim |
| [`asound`](../shared-commands.html#asound) | Send a message to all adjacent rooms |
| [`crier`](../shared-commands.html#crier) | Make a town crier announce a message |
| [`pageat`](../shared-commands.html#pageat) | Send a paginated message to a character |
| [`prompt`](../shared-commands.html#prompt) | Display a custom prompt to a character |
| [`wiznet`](../shared-commands.html#wiznet) | Send a message to the immortal channel |
| [`mail`](../shared-commands.html#mail) | Send in-game mail to a player |
| [`showcommand`](../shared-commands.html#showcommand) | Display a command's output without executing |

### Movement & Transfer

| Command | Description |
|:--------|:------------|
| [`goto`](../shared-commands.html#goto) | Teleport the token to a specified room |
| [`transfer`](../shared-commands.html#transfer) | Move characters to a destination room |
| [`gtransfer`](../shared-commands.html#gtransfer) | Transfer an entire group |
| [`otransfer`](../shared-commands.html#otransfer) | Transfer an object to a different location |
| [`sendfloor`](../shared-commands.html#sendfloor) | Send a dungeon floor transition |
| [`flee`](../shared-commands.html#flee) | Force the entity to flee from combat |

### Entity Creation & Destruction

| Command | Description |
|:--------|:------------|
| [`mload`](../shared-commands.html#mload) | Load a mobile by vnum |
| [`oload`](../shared-commands.html#oload) | Load an object by vnum |
| [`loadinstanced`](../shared-commands.html#loadinstanced) | Load a mob/object into a dungeon instance |
| [`cloneroom`](../shared-commands.html#cloneroom) | Create a dynamic room clone |
| [`destroyroom`](../shared-commands.html#destroyroom) | Destroy a dynamically created room |
| [`purge`](../shared-commands.html#purge) | Remove entities from a room |
| [`remove`](../shared-commands.html#remove) | Unequip and extract an object |
| [`resetroom`](../shared-commands.html#resetroom) | Reset a room to its default state |

### Combat

| Command | Description |
|:--------|:------------|
| [`damage`](../shared-commands.html#damage) | Inflict HP damage on a target |
| [`gdamage`](../shared-commands.html#gdamage) | Inflict damage on a group or area |
| [`rawkill`](../shared-commands.html#rawkill) | Kill a character instantly (bypass death processing) |
| [`startcombat`](../shared-commands.html#startcombat) | Initiate combat between two characters |
| [`stopcombat`](../shared-commands.html#stopcombat) | Force stop combat for a character |
| [`entercombat`](../shared-commands.html#entercombat) | Force a character into combat state |
| [`peace`](../shared-commands.html#peace) | Stop all combat in the room |
| [`breathe`](../shared-commands.html#breathe) | Perform a breath attack |
| [`interrupt`](../shared-commands.html#interrupt) | Interrupt a character's current action |
| [`zot`](../shared-commands.html#zot) | Smite a character with divine damage |

### Entity Modification

| Command | Description |
|:--------|:------------|
| [`altermob`](../shared-commands.html#altermob) | Modify properties of a mobile |
| [`alterobj`](../shared-commands.html#alterobj) | Modify properties of an object |
| [`alterroom`](../shared-commands.html#alterroom) | Modify properties of a room |
| [`alterexit`](../shared-commands.html#alterexit) | Modify properties of a room exit |
| [`alteraffect`](../shared-commands.html#alteraffect) | Modify an existing affect |
| [`condition`](../shared-commands.html#condition) | Set condition/durability on an entity |
| [`ed`](../shared-commands.html#ed) | Add or modify extra descriptions |
| [`stringmob`](../shared-commands.html#stringmob) | Modify a mobile's string properties |
| [`stringobj`](../shared-commands.html#stringobj) | Modify an object's string properties |
| [`setposition`](../shared-commands.html#setposition) | Set a character's position |
| [`setrace`](../shared-commands.html#setrace) | Set a character's race |
| [`setclass`](../shared-commands.html#setclass) | Set a character's class |
| [`setclasslevel`](../shared-commands.html#setclasslevel) | Set a character's class level |
| [`setrecall`](../shared-commands.html#setrecall) | Set a player's recall point |
| [`resetdice`](../shared-commands.html#resetdice) | Reset dice values on an object |

### Affects & Auras

| Command | Description |
|:--------|:------------|
| [`addaffect`](../shared-commands.html#addaffect) | Add a spell-like affect by skill number |
| [`addaffectname`](../shared-commands.html#addaffectname) | Add a named affect by string name |
| [`addaura`](../shared-commands.html#addaura) | Add a magical aura to a character |
| [`remaura`](../shared-commands.html#remaura) | Remove a magical aura |
| [`stripaffect`](../shared-commands.html#stripaffect) | Remove an affect by skill number |
| [`stripaffectname`](../shared-commands.html#stripaffectname) | Remove a named affect by string name |
| [`fixaffects`](../shared-commands.html#fixaffects) | Recalculate all affects on a character |
| [`applytoxin`](../shared-commands.html#applytoxin) | Apply a poison/toxin effect |

### Skills, Spells & Classes

| Command | Description |
|:--------|:------------|
| [`addspell`](../shared-commands.html#addspell) | Add a spell to a character's known list |
| [`remspell`](../shared-commands.html#remspell) | Remove a spell from a character's known list |
| [`grantskill`](../shared-commands.html#grantskill) | Grant a skill to a character |
| [`revokeskill`](../shared-commands.html#revokeskill) | Revoke a character's skill |
| [`grantsong`](../shared-commands.html#grantsong) | Grant a bard song |
| [`revokesong`](../shared-commands.html#revokesong) | Revoke a bard song |
| [`grantclass`](../shared-commands.html#grantclass) | Grant access to a class |
| [`revokeclass`](../shared-commands.html#revokeclass) | Revoke access to a class |
| [`skimprove`](../shared-commands.html#skimprove) | Trigger a skill improvement check |
| [`addtrait`](../shared-commands.html#addtrait) | Add a trait to a character |
| [`removetrait`](../shared-commands.html#removetrait) | Remove a trait from a character |
| [`adjusttrait`](../shared-commands.html#adjusttrait) | Adjust a trait value |
| [`settrait`](../shared-commands.html#settrait) | Set a trait value |
| [`addstache`](../shared-commands.html#addstache) | Add a cosmetic customization |
| [`remstache`](../shared-commands.html#remstache) | Remove a cosmetic customization |

### Variables

| Command | Description |
|:--------|:------------|
| [`varset`](../shared-commands.html#varset) | Set a variable on the current entity |
| [`varseton`](../shared-commands.html#varseton) | Set a variable on another entity |
| [`varclear`](../shared-commands.html#varclear) | Delete a variable from the current entity |
| [`varclearon`](../shared-commands.html#varclearon) | Delete a variable on another entity |
| [`varcopy`](../shared-commands.html#varcopy) | Copy a variable to a new name |
| [`varsave`](../shared-commands.html#varsave) | Mark a variable for persistent save |
| [`varsaveon`](../shared-commands.html#varsaveon) | Mark a variable on another entity for save |
| [`persist`](../shared-commands.html#persist) | Mark a variable for persistence |

### Script Control

| Command | Description |
|:--------|:------------|
| [`call`](../shared-commands.html#call) | Call another script as a subroutine |
| [`xcall`](../shared-commands.html#xcall) | Cross-entity script call |
| [`settimer`](../shared-commands.html#settimer) | Set a countdown timer on a token |
| [`queue`](../shared-commands.html#queue) | Schedule delayed script execution |
| [`dequeue`](../shared-commands.html#dequeue) | Remove queued events |
| [`input`](../shared-commands.html#input) | Queue a command as entity input |
| [`inputstring`](../shared-commands.html#inputstring) | Send raw input to the entity |
| [`scriptwait`](../shared-commands.html#scriptwait) | Pause script execution |
| [`attach`](../shared-commands.html#attach) | Attach a script to an entity |
| [`detach`](../shared-commands.html#detach) | Remove a script from an entity |
| [`remember`](../shared-commands.html#remember) | Store a target in memory |
| [`forget`](../shared-commands.html#forget) | Clear the remembered target |

### Character Resources

| Command | Description |
|:--------|:------------|
| [`restore`](../shared-commands.html#restore) | Fully restore HP, mana, and movement |
| [`chargebank`](../shared-commands.html#chargebank) | Deduct money from a bank account |
| [`deduct`](../shared-commands.html#deduct) | Deduct currency, XP, or resources |
| [`wiretransfer`](../shared-commands.html#wiretransfer) | Transfer money between bank accounts |
| [`award`](../shared-commands.html#award) | Award XP or quest credit |
| [`raisedead`](../shared-commands.html#raisedead) | Resurrect a dead character |

### Forcing Actions

| Command | Description |
|:--------|:------------|
| [`force`](../shared-commands.html#force) | Force a character to execute a command |
| [`gforce`](../shared-commands.html#gforce) | Force a group to execute a command |
| [`vforce`](../shared-commands.html#vforce) | Force all instances of a vnum to execute |
| [`group`](../shared-commands.html#group) | Force group join/leave operations |
| [`ungroup`](../shared-commands.html#ungroup) | Force a character to leave their group |

### Quest System

| Command | Description |
|:--------|:------------|
| [`questaccept`](../shared-commands.html#questaccept) | Accept a quest for a player |
| [`questcomplete`](../shared-commands.html#questcomplete) | Mark a quest as completed |
| [`questcancel`](../shared-commands.html#questcancel) | Cancel an active quest |
| [`questgenerate`](../shared-commands.html#questgenerate) | Generate a random quest |
| [`questscroll`](../shared-commands.html#questscroll) | Create a quest scroll item |
| [`questpartcustom`](../shared-commands.html#questpartcustom) | Add a custom quest objective |
| [`questpartgetitem`](../shared-commands.html#questpartgetitem) | Add a "collect item" objective |
| [`questpartgoto`](../shared-commands.html#questpartgoto) | Add a "go to location" objective |
| [`questpartrescue`](../shared-commands.html#questpartrescue) | Add a "rescue NPC" objective |
| [`questpartslay`](../shared-commands.html#questpartslay) | Add a "kill mob" objective |
| [`questsetindex`](../shared-commands.html#questsetindex) | Set a quest step index |

### Dungeon & Instance

| Command | Description |
|:--------|:------------|
| [`spawndungeon`](../shared-commands.html#spawndungeon) | Create a dungeon instance |
| [`dungeoncommence`](../shared-commands.html#dungeoncommence) | Start a dungeon encounter |
| [`dungeoncomplete`](../shared-commands.html#dungeoncomplete) | Mark a dungeon as completed |
| [`dungeonfailure`](../shared-commands.html#dungeonfailure) | Mark a dungeon as failed |
| [`instancecomplete`](../shared-commands.html#instancecomplete) | Mark an instance as completed |
| [`instancefailure`](../shared-commands.html#instancefailure) | Mark an instance as failed |
| [`checkpoint`](../shared-commands.html#checkpoint) | Set a dungeon checkpoint |
| [`specialkey`](../shared-commands.html#specialkey) | Create or manage a dungeon key |
| [`showroom`](../shared-commands.html#showroom) | Display a remote room's description |
| [`unlockarea`](../shared-commands.html#unlockarea) | Unlock a locked area |
| [`unlockdungeon`](../shared-commands.html#unlockdungeon) | Unlock a dungeon |

### Events & Reckoning

| Command | Description |
|:--------|:------------|
| [`event`](../shared-commands.html#event) | Interact with the event system |
| [`startevent`](../shared-commands.html#startevent) | Start a world event |
| [`stopevent`](../shared-commands.html#stopevent) | Stop an active event |
| [`phaseevent`](../shared-commands.html#phaseevent) | Advance an event to the next phase |
| [`reckoning`](../shared-commands.html#reckoning) | Trigger a reckoning event |
| [`startreckoning`](../shared-commands.html#startreckoning) | Start a reckoning sequence |
| [`stopreckoning`](../shared-commands.html#stopreckoning) | Stop an active reckoning |

### Player Management

| Command | Description |
|:--------|:------------|
| [`saveplayer`](../shared-commands.html#saveplayer) | Force-save a player to disk |
| [`mute`](../shared-commands.html#mute) | Mute a character |
| [`unmute`](../shared-commands.html#unmute) | Unmute a character |
| [`remort`](../shared-commands.html#remort) | Remort a character |
| [`fade`](../shared-commands.html#fade) | Gradually fade an entity out |

### World & Wilderness

| Command | Description |
|:--------|:------------|
| [`link`](../shared-commands.html#link) | Create or modify room exit links |
| [`lockadd`](../shared-commands.html#lockadd) | Add a lock to a door or container |
| [`lockremove`](../shared-commands.html#lockremove) | Remove a lock |
| [`usecatalyst`](../shared-commands.html#usecatalyst) | Consume a catalyst component |
| [`treasuremap`](../shared-commands.html#treasuremap) | Create or manage a treasure map |
| [`wildernessmap`](../shared-commands.html#wildernessmap) | Generate a wilderness map overlay |
| [`wildsoverlay`](../shared-commands.html#wildsoverlay) | Apply a wilderness tile overlay |
| [`wildstile`](../shared-commands.html#wildstile) | Set a wilderness tile |
| [`churchannouncetheft`](../shared-commands.html#churchannouncetheft) | Announce a theft to church members |
| [`shop`](../shared-commands.html#shop) | Interact with the shop system |

---

## Commands Not Available to Tokens

The following commands exist for other entity types but are **not** available in token programs:

| Command | Available In | Description |
|:--------|:-------------|:------------|
| `appear` | mob | Make the entity visible |
| `assist` | mob | Force a mobile to assist in combat |
| `at` | mob, obj, room | Execute a command at another location |
| `cancel` | mob, obj, room | Cancel a delay timer |
| `cast` | mob, obj | Force spell casting |
| `chargemoney` | mob | Deduct carried coins |
| `delay` | mob, obj, room | Set a delayed trigger |
| `disappear` | mob | Make the entity invisible |
| `hunt` | mob | Track a target |
| `kill` | mob | Force an attack |
| `quest` | area, instance, dungeon | Direct quest system interaction |
| `selfdestruct` | mob, obj | Destroy the scripting entity |
| `take` | mob | Take an object from a character |
| `teleport` | mob | Teleport all characters in the room |
