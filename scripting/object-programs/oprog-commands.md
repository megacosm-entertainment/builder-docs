---
layout: default
title: Commands
nav_order: 2
parent: Object Programs
grand_parent: Scripting
---

# Object Program Commands
{: .no_toc }

This page lists every command available in object programs. All object program commands use the `obj` prefix. Most commands are shared across all entity types — for those, a brief description and a link to the [Shared Commands Reference](../shared-commands.md) are provided. Commands that are unique to objects or that behave differently in object context receive full documentation here.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## How to Use This Reference

Every command follows this pattern:

```
obj <command> [arguments...]
```

- **`$n`** refers to the character who triggered the script.
- **`$i`** refers to the object itself (the script owner).
- **`lastreturn`** is set by many commands (1 = success, 0 = failure). Check it with `if lastreturn == 1`.

For the full explanation of variables, quick codes, and color codes, see [Variables and Tokens](../variables-and-tokens.md) and [Scripting Basics](../scripting-basics.md).

---

## Output Commands

Commands that send messages to characters.

### echo

Send a message to everyone in the object's room. → [Shared Reference](../shared-commands.md#echo)

```
obj echo The sword hums with power.
```

### echoat

Send a message to one specific target. → [Shared Reference](../shared-commands.md#echoat)

```
obj echoat $n You feel the ring tighten around your finger.
```

### echoaround

Send a message to everyone in the room except the target. → [Shared Reference](../shared-commands.md#echoaround)

```
obj echoaround $n $N's ring begins to glow.
```

### echoroom

Send a message to a specific room (not necessarily the object's room). → [Shared Reference](../shared-commands.md#echoroom)

```
obj echoroom 3001 A distant rumble echoes from the cavern.
```

### gecho

Broadcast a message to all players in the game. → [Shared Reference](../shared-commands.md#gecho)

```
obj gecho {RThe sky darkens as an ancient artifact awakens!{x
```

### zecho

Send a message to all players in the same area. → [Shared Reference](../shared-commands.md#zecho)

```
obj zecho {YA bell tolls three times from the temple.{x
```

### echonotvict

Send a message to everyone in the room except the victim. → [Shared Reference](../shared-commands.md#echonotvict)

```
obj echonotvict $(victim) $N is struck by the artifact's power!
```

### echobattlespam

Send a combat-flagged message (respects player battlespam settings). → [Shared Reference](../shared-commands.md#echobattlespam)

```
obj echobattlespam {RSparks fly from the enchanted blade!{x
```

### echochurch

Send a message to all online members of a church. → [Shared Reference](../shared-commands.md#echochurch)

```
obj echochurch $i {YThe relic speaks to the faithful.{x
```

### echogroupat

Send a message to all members of the target's group. → [Shared Reference](../shared-commands.md#echogroupat)

```
obj echogroupat $n {CThe banner rallies your group!{x
```

### echogrouparound

Send a message to the room except the target's group. → [Shared Reference](../shared-commands.md#echogrouparound)

```
obj echogrouparound $n The enemy group's banner pulses with light.
```

### echoleadat

Send a message to the group leader. → [Shared Reference](../shared-commands.md#echoleadat)

```
obj echoleadat $n {YYour commander's insignia glows with authority.{x
```

### echoleadaround

Send a message to everyone except the group leader. → [Shared Reference](../shared-commands.md#echoleadaround)

```
obj echoleadaround $n The leader's insignia flares with power.
```

### asound

Send a message to all adjacent rooms (not the current room). → [Shared Reference](../shared-commands.md#asound)

```
obj asound A deep horn blast echoes from nearby.
```

### crier

Make a town crier announce a message. → [Shared Reference](../shared-commands.md#crier)

```
obj crier Hear ye! The ancient artifact has been found!
```

### mail

Send in-game mail to a player. → [Shared Reference](../shared-commands.md#mail)

```
obj mail $n The artifact whispers a message into your mailbox.
```

### pageat

Send a paginated message to a target. → [Shared Reference](../shared-commands.md#pageat)

```
obj pageat $n {CLong inscription text from the ancient tablet...{x
```

### prompt

Display a custom prompt to a target. → [Shared Reference](../shared-commands.md#prompt)

```
obj prompt $n {Y[Artifact Charge: 75%%]{x
```

### wiznet

Send a message to the immortal wiznet channel. → [Shared Reference](../shared-commands.md#wiznet)

```
obj wiznet Debug: artifact triggered by $n in room $R
```

---

## Entity Modification Commands

Commands that change properties of mobiles, objects, rooms, and exits.

### alterobj

Modify object properties (values, flags, weight, etc.). → [Shared Reference](../shared-commands.md#alterobj)

```
obj alterobj $i v0 5
obj alterobj $i addname glowing
```

### altermob

Modify mobile/NPC properties. → [Shared Reference](../shared-commands.md#altermob)

```
obj altermob $n maxhp 500
```

### alterroom

Modify room properties. → [Shared Reference](../shared-commands.md#alterroom)

```
obj alterroom $(here) addflag dark
```

### alterexit

Modify exit properties. → [Shared Reference](../shared-commands.md#alterexit)

```
obj alterexit north flags remove closed
```

### condition

Set or modify object condition/durability. → [Shared Reference](../shared-commands.md#condition)

```
obj condition $i 100
```

### ed

Add or modify extra descriptions on an entity. → [Shared Reference](../shared-commands.md#ed)

```
obj ed $i runes Ancient runes cover the blade's surface.
```

### resetdice

Reset dice or random values on an object. → [Shared Reference](../shared-commands.md#resetdice)

```
obj resetdice $i
```

### stringmob

Modify mobile name, short, or long descriptions. → [Shared Reference](../shared-commands.md#stringmob)

```
obj stringmob $n short a battle-scarred warrior
```

### stringobj

Modify object name, short, or long descriptions. → [Shared Reference](../shared-commands.md#stringobj)

```
obj stringobj $i short a {Rbloodstained{x dagger
```

### setclass

Set a character's class. → [Shared Reference](../shared-commands.md#setclass)

```
obj setclass $n warrior
```

### setclasslevel

Set a character's level in a specific class. → [Shared Reference](../shared-commands.md#setclasslevel)

```
obj setclasslevel $n warrior 10
```

### setposition

Set a character's position (standing, sitting, sleeping, etc.). → [Shared Reference](../shared-commands.md#setposition)

```
obj setposition $n sleeping
```

### setrace

Set a character's race. → [Shared Reference](../shared-commands.md#setrace)

```
obj setrace $n elf
```

### setrecall

Set a player's recall (home) room. → [Shared Reference](../shared-commands.md#setrecall)

```
obj setrecall $n 3001
```

---

## Affects and Auras

Commands that add, modify, or remove spell-like effects.

### addaffect

Add a spell-like affect by skill number. → [Shared Reference](../shared-commands.md#addaffect)

```
obj addaffect $n 1 200 -2 17 0
```

### addaffectname

Add a named affect (by string name). → [Shared Reference](../shared-commands.md#addaffectname)

```
obj addaffectname $n fire_shield 100
```

### addaura

Add a magical aura effect. → [Shared Reference](../shared-commands.md#addaura)

```
obj addaura $n holy_aura 200
```

### alteraffect

Modify an existing affect on a character. → [Shared Reference](../shared-commands.md#alteraffect)

```
obj alteraffect $n fire_shield duration 300
```

### applytoxin

Apply a poison or toxin effect. → [Shared Reference](../shared-commands.md#applytoxin)

```
obj applytoxin $n venom 50
```

### fixaffects

Recalculate all affects on a character. → [Shared Reference](../shared-commands.md#fixaffects)

```
obj fixaffects $n
```

### remaura

Remove a magical aura from a character. → [Shared Reference](../shared-commands.md#remaura)

```
obj remaura $n holy_aura
```

### stripaffect

Remove an affect by skill number. → [Shared Reference](../shared-commands.md#stripaffect)

```
obj stripaffect $n 1
```

### stripaffectname

Remove a named affect by string name. → [Shared Reference](../shared-commands.md#stripaffectname)

```
obj stripaffectname $n fire_shield
```

---

## Combat Commands

Commands related to fighting, damage, and combat state.

### damage

Inflict HP damage on a character. → [Shared Reference](../shared-commands.md#damage)

```
obj damage $n 50
obj damage $(victim) 100
```

### gdamage

Inflict damage on an entire group. → [Shared Reference](../shared-commands.md#gdamage)

```
obj gdamage $n 25
```

### entercombat

Force a character into combat. → [Shared Reference](../shared-commands.md#entercombat)

```
obj entercombat $n $(victim)
```

### startcombat

Initiate combat between two characters. → [Shared Reference](../shared-commands.md#startcombat)

```
obj startcombat $n $(victim)
```

### stopcombat

Force a character to stop fighting. → [Shared Reference](../shared-commands.md#stopcombat)

```
obj stopcombat $n
```

### peace

Stop all combat in the current room. → [Shared Reference](../shared-commands.md#peace)

```
obj peace
```

### rawkill

Instantly kill a character (bypasses death processing). → [Shared Reference](../shared-commands.md#rawkill)

```
obj rawkill $n
```

### interrupt

Interrupt a character's current action (casting, etc.). → [Shared Reference](../shared-commands.md#interrupt)

```
obj interrupt $n
```

### breathe

Force an entity to perform a breath attack. → [Shared Reference](../shared-commands.md#breathe)

```
obj breathe $n fire
```

### cast

Force the object's context to cast a spell. This is one of the few commands that behaves specially for objects — the object itself can be the caster.

```
obj cast 'fireball' $(victim)
obj cast 'cure light' $n
```

### zot

Smite a character with divine punishment. → [Shared Reference](../shared-commands.md#zot)

```
obj zot $n 500
```

### flee

Force the holder to flee from combat. → [Shared Reference](../shared-commands.md#flee)

```
obj flee
```

---

## Movement and Transfer

Commands that move characters and objects between locations.

### goto

Teleport the object to a specified room. In object programs, this moves the object itself.

```
obj goto 3001
```

### at

Execute a command at a different location without moving the object.

```
obj at 3001 obj echo A distant rumble is heard.
```

### transfer

Teleport a character to a destination room. → [Shared Reference](../shared-commands.md#transfer)

```
obj transfer $n 3001
```

### gtransfer

Transfer an entire group to a destination. → [Shared Reference](../shared-commands.md#gtransfer)

```
obj gtransfer $n 3001
```

### otransfer

Transfer an object to a different location or character. → [Shared Reference](../shared-commands.md#otransfer)

```
obj otransfer $i $n
obj otransfer 3050 3001
```

---

## Entity Creation and Destruction

Commands that create or remove entities.

### oload

Load (create) an object by vnum. → [Shared Reference](../shared-commands.md#oload)

```
obj oload 3050
obj oload 3050 $n
```

### mload

Load (create) a mobile by vnum. → [Shared Reference](../shared-commands.md#mload)

```
obj mload 3001
```

### purge

Remove entities from the room. → [Shared Reference](../shared-commands.md#purge)

```
obj purge
obj purge 3050
```

### junk

Silently destroy an object. In object programs, commonly used to destroy other objects or the object itself.

```
obj junk $i
obj junk $(obj1)
```

### selfdestruct

Cause the object to destroy itself. This is the standard way for one-use items to clean themselves up after activation.

```
** Potion is consumed after use
obj echoat $n {GYou feel the healing energy flow through you.{x
obj restore $n
obj selfdestruct
```

**Notes:**
- The script stops executing after `selfdestruct` — any commands after it will not run.
- Use this for consumables, one-time quest items, and objects that should disappear after triggering.

### remove

Remove/unequip an object from a character. → [Shared Reference](../shared-commands.md#remove)

```
obj remove $n
```

### cloneroom

Create a dynamic copy of a room. → [Shared Reference](../shared-commands.md#cloneroom)

```
obj cloneroom 3001 9001
```

### destroyroom

Destroy a dynamically created room. → [Shared Reference](../shared-commands.md#destroyroom)

```
obj destroyroom 9001
```

### resetroom

Reset a room to its default state. → [Shared Reference](../shared-commands.md#resetroom)

```
obj resetroom 3001
```

### loadinstanced

Load an entity into a dungeon instance context. → [Shared Reference](../shared-commands.md#loadinstanced)

```
obj loadinstanced 3001
```

### makeinstanced

Convert an entity to instance-specific scope. → [Shared Reference](../shared-commands.md#makeinstanced)

```
obj makeinstanced $i
```

---

## Skills, Spells, and Traits

Commands that modify character abilities.

### addspell

Add a spell to a character's spell list. → [Shared Reference](../shared-commands.md#addspell)

```
obj addspell $n fireball
```

### remspell

Remove a spell from a character's spell list. → [Shared Reference](../shared-commands.md#remspell)

```
obj remspell $n fireball
```

### grantskill

Grant a character knowledge of a skill. → [Shared Reference](../shared-commands.md#grantskill)

```
obj grantskill $n dodge
```

### revokeskill

Revoke a character's knowledge of a skill. → [Shared Reference](../shared-commands.md#revokeskill)

```
obj revokeskill $n dodge
```

### grantsong

Grant a character knowledge of a bard song. → [Shared Reference](../shared-commands.md#grantsong)

```
obj grantsong $n lullaby
```

### revokesong

Revoke a character's knowledge of a bard song. → [Shared Reference](../shared-commands.md#revokesong)

```
obj revokesong $n lullaby
```

### grantclass

Grant a character access to a class. → [Shared Reference](../shared-commands.md#grantclass)

```
obj grantclass $n ranger
```

### revokeclass

Revoke a character's access to a class. → [Shared Reference](../shared-commands.md#revokeclass)

```
obj revokeclass $n ranger
```

### skimprove

Trigger a skill improvement check. → [Shared Reference](../shared-commands.md#skimprove)

```
obj skimprove $n swordsmanship
```

### addtrait

Add a trait (racial/class feature) to a character.

```
obj addtrait $n night_vision
```

### adjusttrait

Adjust a trait's value on a character.

```
obj adjusttrait $n strength_bonus 2
```

### removetrait

Remove a trait from a character.

```
obj removetrait $n night_vision
```

### settrait

Set a specific trait value on a character. → [Shared Reference](../shared-commands.md#settrait)

```
obj settrait $n fire_resist 50
```

---

## Character Resources

Commands that modify currency, experience, and vitals.

### award

Award experience points or quest credit. → [Shared Reference](../shared-commands.md#award)

```
obj award $n 500
```

### deduct

Deduct currency, XP, or other resources. → [Shared Reference](../shared-commands.md#deduct)

```
obj deduct $n gold 100
```

### chargebank

Deduct money from a player's bank account. → [Shared Reference](../shared-commands.md#chargebank)

```
obj chargebank $n 500
```

### restore

Fully restore a character's HP, mana, and movement. → [Shared Reference](../shared-commands.md#restore)

```
obj restore $n
```

### wiretransfer

Transfer money between player bank accounts. → [Shared Reference](../shared-commands.md#wiretransfer)

```
obj wiretransfer $n merchant 1000
```

### remort

Remort a character (rebirth at higher tier). → [Shared Reference](../shared-commands.md#remort)

```
obj remort $n
```

---

## Variable Commands

Commands for storing and retrieving data. Variables are how objects remember state across trigger firings.

### varset

Set a variable on the object. → [Shared Reference](../shared-commands.md#varset)

```
obj varset kill_count 0
obj varset last_user $n
```

### varclear

Delete a variable from the object. → [Shared Reference](../shared-commands.md#varclear)

```
obj varclear kill_count
```

### varseton

Set a variable on a different entity. → [Shared Reference](../shared-commands.md#varseton)

```
obj varseton $n has_artifact 1
```

### varclearon

Clear a variable on a different entity. → [Shared Reference](../shared-commands.md#varclearon)

```
obj varclearon $n has_artifact
```

### varcopy

Copy a variable value to a new variable name. → [Shared Reference](../shared-commands.md#varcopy)

```
obj varcopy old_count new_count
```

### persist

Mark a variable for persistent storage (survives reboots). → [Shared Reference](../shared-commands.md#persist)

```
obj persist kill_count
```

### varsave

Mark a variable for persistent save. → [Shared Reference](../shared-commands.md#varsave)

```
obj varsave kill_count
```

### varsaveon

Mark a variable on a different entity for persistent save. → [Shared Reference](../shared-commands.md#varsaveon)

```
obj varsaveon $n quest_progress
```

---

## Script Control

Commands that control script execution flow.

### call

Call another script as a subroutine. → [Shared Reference](../shared-commands.md#call)

```
obj call 3001 argument1 argument2
```

### xcall

Execute a cross-entity script call. → [Shared Reference](../shared-commands.md#xcall)

```
obj xcall $(victim) 3005
```

### delay

Set a delay timer (in pulses). When the delay expires, the `delay` trigger fires. → [Shared Reference](../shared-commands.md#delay)

```
obj delay 10
```

### cancel

Cancel a previously set delay timer. → [Shared Reference](../shared-commands.md#cancel)

```
obj cancel
```

### queue

Schedule a command for delayed execution. → [Shared Reference](../shared-commands.md#queue)

```
obj queue 5 obj echo {RBoom!{x
```

### dequeue

Remove all queued events owned by this entity. → [Shared Reference](../shared-commands.md#dequeue)

```
obj dequeue
```

### input

Queue a command as if the entity typed it. → [Shared Reference](../shared-commands.md#input)

```
obj input drop all
```

### inputstring

Send raw input as if typed. → [Shared Reference](../shared-commands.md#inputstring)

```
obj inputstring say Hello there!
```

### scriptwait

Pause script execution for a duration. In object programs, this pauses the current script and resumes after the specified time.

```
obj scriptwait 3
obj echo {xThree seconds later...{x
```

### settimer

Set a countdown timer on a token or entity. → [Shared Reference](../shared-commands.md#settimer)

```
obj settimer bomb_timer 30
```

---

## Script Entity Management

Commands for managing script attachment and memory.

### attach

Attach a script program to an entity. → [Shared Reference](../shared-commands.md#attach)

```
obj attach 3005
```

### detach

Detach a script program from an entity. → [Shared Reference](../shared-commands.md#detach)

```
obj detach 3005
```

### remember

Store a target in the object's memory for later reference. → [Shared Reference](../shared-commands.md#remember)

```
obj remember $n
```

### forget

Clear the object's remembered target. → [Shared Reference](../shared-commands.md#forget)

```
obj forget
```

---

## Force Commands

Commands that make characters perform actions.

### force

Force a character to execute a command. → [Shared Reference](../shared-commands.md#force)

```
obj force $n say I cannot resist the artifact's command!
```

### gforce

Force all members of a group to execute a command. → [Shared Reference](../shared-commands.md#gforce)

```
obj gforce $n stand
```

### vforce

Force all instances of a mobile vnum to execute a command. → [Shared Reference](../shared-commands.md#vforce)

```
obj vforce 3001 say The artifact commands us!
```

### group

Force grouping operations. → [Shared Reference](../shared-commands.md#group)

```
obj group $n add $(victim)
```

### ungroup

Force a character to leave their group. → [Shared Reference](../shared-commands.md#ungroup)

```
obj ungroup $n
```

---

## Quest Commands

Commands for the quest system.

### questaccept

Accept or begin a quest for a player. → [Shared Reference](../shared-commands.md#questaccept)

```
obj questaccept $n 100
```

### questcomplete

Mark a quest as completed. → [Shared Reference](../shared-commands.md#questcomplete)

```
obj questcomplete $n 100
```

### questcancel

Cancel an active quest. → [Shared Reference](../shared-commands.md#questcancel)

```
obj questcancel $n 100
```

### questgenerate

Procedurally generate a random quest. → [Shared Reference](../shared-commands.md#questgenerate)

```
obj questgenerate $n
```

### questpartcustom

Add a custom quest objective. → [Shared Reference](../shared-commands.md#questpartcustom)

```
obj questpartcustom 100 Speak the ancient password
```

### questpartgetitem

Add a "collect item" quest objective. → [Shared Reference](../shared-commands.md#questpartgetitem)

```
obj questpartgetitem 100 3050 5
```

### questpartgoto

Add a "go to location" quest objective. → [Shared Reference](../shared-commands.md#questpartgoto)

```
obj questpartgoto 100 3001
```

### questpartrescue

Add a "rescue NPC" quest objective. → [Shared Reference](../shared-commands.md#questpartrescue)

```
obj questpartrescue 100 3010
```

### questpartslay

Add a "kill mob" quest objective. → [Shared Reference](../shared-commands.md#questpartslay)

```
obj questpartslay 100 3005 10
```

### questscroll

Create a quest scroll item for a player. → [Shared Reference](../shared-commands.md#questscroll)

```
obj questscroll $n 100
```

### questsetindex

Set the quest step/index. Available on objects, rooms, and tokens.

```
obj questsetindex 100 3
```

---

## Dungeon and Instance Commands

Commands for the dungeon and instance systems.

### spawndungeon

Create and initialize a new dungeon instance. → [Shared Reference](../shared-commands.md#spawndungeon)

```
obj spawndungeon 5001
```

### dungeoncommence

Start the dungeon encounter phase. → [Shared Reference](../shared-commands.md#dungeoncommence)

```
obj dungeoncommence 5001
```

### dungeoncomplete

Mark a dungeon as successfully completed. → [Shared Reference](../shared-commands.md#dungeoncomplete)

```
obj dungeoncomplete 5001
```

### dungeonfailure

Mark a dungeon as failed. → [Shared Reference](../shared-commands.md#dungeonfailure)

```
obj dungeonfailure 5001
```

### instancecomplete

Mark the current instance as completed. → [Shared Reference](../shared-commands.md#instancecomplete)

```
obj instancecomplete
```

### instancefailure

Mark the current instance as failed. → [Shared Reference](../shared-commands.md#instancefailure)

```
obj instancefailure
```

### checkpoint

Set a dungeon checkpoint for respawning. → [Shared Reference](../shared-commands.md#checkpoint)

```
obj checkpoint 5010
```

### sendfloor

Send a dungeon floor transition. → [Shared Reference](../shared-commands.md#sendfloor)

```
obj sendfloor 2
```

### showroom

Display a remote room description to a player (scrying). → [Shared Reference](../shared-commands.md#showroom)

```
obj showroom 3001 $n
```

### specialkey

Create or manage a special key item. → [Shared Reference](../shared-commands.md#specialkey)

```
obj specialkey ancient_key
```

### unlockarea

Unlock a locked area. → [Shared Reference](../shared-commands.md#unlockarea)

```
obj unlockarea 50
```

### unlockdungeon

Unlock a dungeon (make it accessible). → [Shared Reference](../shared-commands.md#unlockdungeon)

```
obj unlockdungeon 5001
```

---

## Event and Reckoning Commands

Commands for the world event system.

### event

Interact with the event system. → [Shared Reference](../shared-commands.md#event)

```
obj event start festival
```

### startevent

Start a world or area event. → [Shared Reference](../shared-commands.md#startevent)

```
obj startevent 200
```

### stopevent

Stop an active event. → [Shared Reference](../shared-commands.md#stopevent)

```
obj stopevent 200
```

### phaseevent

Advance an event to the next phase. → [Shared Reference](../shared-commands.md#phaseevent)

```
obj phaseevent 200
```

### reckoning

Trigger a reckoning world event. → [Shared Reference](../shared-commands.md#reckoning)

```
obj reckoning start
```

### startreckoning

Start a reckoning event sequence. → [Shared Reference](../shared-commands.md#startreckoning)

```
obj startreckoning 1
```

### stopreckoning

Stop an active reckoning event. → [Shared Reference](../shared-commands.md#stopreckoning)

```
obj stopreckoning 1
```

---

## Lock Commands

Commands for managing locks on doors and containers.

### lockadd

Add a lock to a door or container. → [Shared Reference](../shared-commands.md#lockadd)

```
obj lockadd north 3050
```

### lockremove

Remove a lock from a door or container. → [Shared Reference](../shared-commands.md#lockremove)

```
obj lockremove north
```

---

## Exit and Link Commands

### link

Create or modify an exit link between rooms. → [Shared Reference](../shared-commands.md#link)

```
obj link north 3002
```

---

## Appearance and Social

### addstache

Add a cosmetic customization to a character. → [Shared Reference](../shared-commands.md#addstache)

```
obj addstache $n handlebar
```

### remstache

Remove a cosmetic customization. → [Shared Reference](../shared-commands.md#remstache)

```
obj remstache $n handlebar
```

### fade

Gradually make the object disappear (fade-out effect). → [Shared Reference](../shared-commands.md#fade)

```
obj fade 5
```

---

## Player Management

### mute

Prevent a character from speaking. → [Shared Reference](../shared-commands.md#mute)

```
obj mute $n
```

### unmute

Unmute a previously muted character. → [Shared Reference](../shared-commands.md#unmute)

```
obj unmute $n
```

### saveplayer

Force-save a player character to disk. → [Shared Reference](../shared-commands.md#saveplayer)

```
obj saveplayer $n
```

### showcommand

Display command output to a player without executing. → [Shared Reference](../shared-commands.md#showcommand)

```
obj showcommand $n score
```

---

## Religion and Organization

### churchannouncetheft

Announce a theft to church members. → [Shared Reference](../shared-commands.md#churchannouncetheft)

```
obj churchannouncetheft temple The sacred relic has been stolen!
```

### shop

Interact with the shop system. → [Shared Reference](../shared-commands.md#shop)

```
obj shop list $n
```

---

## Catalyst and Alchemy

### usecatalyst

Consume a catalyst and check requirements. → [Shared Reference](../shared-commands.md#usecatalyst)

```
obj usecatalyst 3080
```

---

## Wilderness

Commands for the wilderness map system.

### treasuremap

Create or interact with a treasure map. → [Shared Reference](../shared-commands.md#treasuremap)

```
obj treasuremap $n 150 200
```

### wildernessmap

Generate or modify a wilderness map overlay. → [Shared Reference](../shared-commands.md#wildernessmap)

```
obj wildernessmap generate
```

### wildsanchor

Set an anchor point on the wilderness map. → [Shared Reference](../shared-commands.md#wildsanchor)

```
obj wildsanchor 150 200
```

### wildsoverlay

Apply a tile overlay to the wilderness map. → [Shared Reference](../shared-commands.md#wildsoverlay)

```
obj wildsoverlay add forest 150 200
```

### wildstile

Set or modify a wilderness tile. → [Shared Reference](../shared-commands.md#wildstile)

```
obj wildstile 150 200 mountain
```

### wildsvlink

Create a virtual link between wilderness and interior rooms. → [Shared Reference](../shared-commands.md#wildsvlink)

```
obj wildsvlink 150,200 3001
```

---

## Command Quick Reference

Alphabetical list of all 162 object program commands:

| Command | Category | Command | Category |
|:--------|:---------|:--------|:---------|
| `addaffect` | Affects | `mload` | Creation |
| `addaffectname` | Affects | `mute` | Player Mgmt |
| `addaura` | Affects | `oload` | Creation |
| `addspell` | Skills | `otransfer` | Movement |
| `addstache` | Appearance | `pageat` | Output |
| `addtrait` | Skills | `peace` | Combat |
| `adjusttrait` | Skills | `persist` | Variables |
| `alteraffect` | Affects | `phaseevent` | Events |
| `alterexit` | Modification | `prompt` | Output |
| `altermob` | Modification | `purge` | Creation |
| `alterobj` | Modification | `questaccept` | Quest |
| `alterroom` | Modification | `questcancel` | Quest |
| `applytoxin` | Affects | `questcomplete` | Quest |
| `asound` | Output | `questgenerate` | Quest |
| `at` | Movement | `questpartcustom` | Quest |
| `attach` | Script Mgmt | `questpartgetitem` | Quest |
| `award` | Resources | `questpartgoto` | Quest |
| `breathe` | Combat | `questpartrescue` | Quest |
| `call` | Script Control | `questpartslay` | Quest |
| `cancel` | Script Control | `questscroll` | Quest |
| `cast` | Combat | `questsetindex` | Quest |
| `chargebank` | Resources | `queue` | Script Control |
| `checkpoint` | Dungeon | `rawkill` | Combat |
| `churchannouncetheft` | Religion | `reckoning` | Events |
| `cloneroom` | Creation | `remember` | Script Mgmt |
| `condition` | Modification | `remort` | Resources |
| `crier` | Output | `remove` | Creation |
| `damage` | Combat | `removetrait` | Skills |
| `deduct` | Resources | `remspell` | Skills |
| `delay` | Script Control | `remstache` | Appearance |
| `dequeue` | Script Control | `resetdice` | Modification |
| `destroyroom` | Creation | `resetroom` | Creation |
| `detach` | Script Mgmt | `restore` | Resources |
| `dungeoncommence` | Dungeon | `revokeclass` | Skills |
| `dungeoncomplete` | Dungeon | `revokeskill` | Skills |
| `dungeonfailure` | Dungeon | `revokesong` | Skills |
| `echo` | Output | `saveplayer` | Player Mgmt |
| `echoaround` | Output | `scriptwait` | Script Control |
| `echoat` | Output | `selfdestruct` | Creation |
| `echobattlespam` | Output | `sendfloor` | Dungeon |
| `echochurch` | Output | `setclass` | Modification |
| `echogrouparound` | Output | `setclasslevel` | Modification |
| `echogroupat` | Output | `setposition` | Modification |
| `echoleadaround` | Output | `setrace` | Modification |
| `echoleadat` | Output | `setrecall` | Modification |
| `echonotvict` | Output | `settimer` | Script Control |
| `echoroom` | Output | `settrait` | Skills |
| `ed` | Modification | `shop` | Religion |
| `entercombat` | Combat | `showcommand` | Player Mgmt |
| `event` | Events | `showroom` | Dungeon |
| `fade` | Appearance | `skimprove` | Skills |
| `fixaffects` | Affects | `spawndungeon` | Dungeon |
| `flee` | Combat | `specialkey` | Dungeon |
| `force` | Force | `startcombat` | Combat |
| `forget` | Script Mgmt | `startevent` | Events |
| `gdamage` | Combat | `startreckoning` | Events |
| `gecho` | Output | `stopcombat` | Combat |
| `gforce` | Force | `stopevent` | Events |
| `goto` | Movement | `stopreckoning` | Events |
| `grantclass` | Skills | `stringmob` | Modification |
| `grantskill` | Skills | `stringobj` | Modification |
| `grantsong` | Skills | `stripaffect` | Affects |
| `group` | Force | `stripaffectname` | Affects |
| `gtransfer` | Movement | `transfer` | Movement |
| `input` | Script Control | `treasuremap` | Wilderness |
| `inputstring` | Script Control | `ungroup` | Force |
| `instancecomplete` | Dungeon | `unlockarea` | Dungeon |
| `instancefailure` | Dungeon | `unlockdungeon` | Dungeon |
| `interrupt` | Combat | `unmute` | Player Mgmt |
| `junk` | Creation | `usecatalyst` | Alchemy |
| `link` | Exits | `varclear` | Variables |
| `loadinstanced` | Creation | `varclearon` | Variables |
| `lockadd` | Locks | `varcopy` | Variables |
| `lockremove` | Locks | `varsave` | Variables |
| `mail` | Output | `varsaveon` | Variables |
| `makeinstanced` | Creation | `varset` | Variables |
| `wildernessmap` | Wilderness | `varseton` | Variables |
| `wildsanchor` | Wilderness | `vforce` | Force |
| `wildsoverlay` | Wilderness | `wiretransfer` | Resources |
| `wildstile` | Wilderness | `wiznet` | Output |
| `wildsvlink` | Wilderness | `xcall` | Script Control |
| `zecho` | Output | `zot` | Combat |
