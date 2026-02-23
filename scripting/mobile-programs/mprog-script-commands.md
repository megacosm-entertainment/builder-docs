---
layout: default
title: Mobile Script Commands
nav_order: 3
parent: Mobile Scripts
grand_parent: Scripting
---

# Mobile Script Command Reference
{: .no_toc }

This page documents all commands available in mobile scripts (mprogs). Most commands also work in room progs, object progs, and token progs — see the specific trigger documentation for any exceptions.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

{: .notice }
Commands that send output to players may use color codes. Wrap text in `{Y...{x` for yellow, `{R...{x` for red, etc.

---

## Output Commands

### echo

```
echo <text>
```

Sends a message to everyone in the current room. No attribution — not "NPC says".

```
echo A cold wind sweeps through the tavern.
echo {Y$N stands at the entrance, looking weary.{x
```

---

### echoaround

```
echoaround <victim> <text>
```

Sends a message to everyone in the room **except** the specified character.

```
echoaround $n The innkeeper winks to the other patrons.
```

---

### echoat

```
echoat <victim> <text>
```

Sends a private message to a specific character only.

```
echoat $n {WThe old man whispers: 'Meet me at midnight.'{x
```

---

### echobattlespam

```
echobattlespam <victim> <text>
```

Sends a combat-formatted message to the specified character (uses the combat output channel).

---

### echochurch

```
echochurch <church_name> <text>
```

Broadcasts a message to all online members of the named church.

```
echochurch Paladin {WThe high priest calls you to arms!{x
```

---

### echogrouparound

```
echogrouparound <character> <text>
```

Sends a message to all group members of the specified character **except** that character.

---

### echogroupat

```
echogroupat <character> <text>
```

Sends a message to all group members of the specified character, including the character themselves.

```
echogroupat $n The dungeon rumbles — the boss approaches!
```

---

### echoleadaround / echoleadat

```
echoleadaround <character> <text>
echoleadat <character> <text>
```

Like `echogrouparound`/`echogroupat` but targets the group **leader** and their followers.

---

### echonotvict

```
echonotvict <victim> <text>
```

Sends to everyone in the room except the victim (synonym for `echoaround`).

---

### echoroom

```
echoroom <room_vnum> <text>
```

Sends a message to all characters in a specific room.

```
echoroom 3200 A distant explosion shakes the walls.
```

---

### gecho

```
gecho <text>
```

Broadcasts a message to all players on the entire game (global echo). Use sparingly.

```
gecho {RThe sky turns blood-red as the ritual begins!{x
```

---

### zecho

```
zecho <text>
```

Broadcasts to all players in the current **zone/area**.

```
zecho A howl echoes through the Thornwood.
```

---

### pageat

```
pageat <victim> <text>
```

Sends a paged message to the specified character (for long text displays).

---

## Mob Interaction Commands

### say

```
say <text>
```

The NPC says something aloud to the room.

```
say Welcome to the guild, $n!
```

---

### emote

```
emote <action text>
```

The NPC performs a third-person emote visible to the room.

```
emote waves a hand toward the northern corridor.
```

---

### input

```
input <character> <text>
```

Forces the specified character to interpret a line of text through the script parser.

---

### inputstring

```
inputstring <character> <string>
```

Like `input` but passes the full raw string.

---

## Movement and Positioning

### goto

```
goto <vnum>
```

Instantly teleports the NPC (self) to a room by widevnum.

```
goto 3100
```

---

### at

```
at <vnum> <command>
```

Executes a script command from the context of a different room without moving the NPC.

```
at 3200 echo A shadow passes over the courtyard.
at 3100 mload 500
```

---

### transfer

```
transfer <victim> [room_vnum]
```

Moves a character to a room. If no room given, moves them to the NPC's current room.

```
transfer $n 3001
transfer $n
```

---

### teleport

```
teleport <victim> <vnum>
```

Silently teleports a character with no room exit message.

```
teleport $n 3500
```

---

### gtransfer

```
gtransfer <character> <vnum>
```

Transfers the entire group of a character to a room.

```
gtransfer $n 3001
```

---

### otransfer

```
otransfer <object_keyword> <destination>
```

Moves objects by keyword to a destination entity or room.

```
otransfer gold $i
otransfer magic_key 3100
```

---

### sendfloor

```
sendfloor <character> <vnum>
```

Transfers a character to a dungeon floor by number.

---

## Loading and Creation

### mload

```
mload <mob_vnum>
```

Loads (spawns) a mobile into the current room.

```
mload 500
mload 2#500
```

---

### oload

```
oload <obj_vnum>
```

Loads an object into the current room.

```
oload 800
oload 2#800
```

---

### loadinstanced

```
loadinstanced <mob_vnum>
```

Loads a mobile configured for blueprint instances.

---

### cloneroom

```
cloneroom <room_vnum>
```

Creates a clone of a room (used in blueprint and dungeon systems).

---

## Destruction

### purge

```
purge [target]
```

Removes an object or NPC from the game. Without a target, purges the NPC itself.

```
purge $n
purge rusted_key
purge
```

---

### destroyroom

```
destroyroom <vnum>
```

Permanently removes a cloned room.

---

### junk

```
junk <object_keyword>
```

Destroys an object in the NPC's inventory without dropping it in the room.

```
junk gold_coin
```

---

### selfdestruct

```
selfdestruct
```

The NPC removes itself from the world at the end of the current script execution.

---

## Combat Commands

### kill

```
kill <victim>
```

Initiates combat between the NPC and the specified target.

```
kill $n
```

---

### rawkill

```
rawkill <victim>
```

Instantly kills the target without going through normal combat. No XP awarded, no corpse processing.

---

### damage

```
damage <victim> <amount> <type>
```

Deals direct damage to a character. Type is a damage class (e.g. `fire`, `weapon`, `mental`).

```
damage $n 50 fire
damage $n 100 weapon
```

---

### gdamage

```
gdamage <character> <amount> <type>
```

Deals damage to all members of a character's group.

---

### startcombat

```
startcombat <attacker> <defender>
```

Forces two characters into combat with each other.

```
startcombat $i $n
```

---

### stopcombat

```
stopcombat <character>
```

Removes a character from combat.

```
stopcombat $n
```

---

### assist

```
assist <character>
```

Makes the NPC join combat assisting the specified character.

---

### peace

```
peace
```

Stops all combat in the current room.

---

### flee

```
flee
```

Makes the NPC flee combat.

---

### fade

```
fade
```

Makes the NPC fade out (become temporarily invisible).

---

### appear

```
appear
```

Makes the NPC reappear after fading.

---

### disappear

```
disappear
```

Causes the NPC to vanish from the room description.

---

### entercombat

```
entercombat <target>
```

Engages combat with target (similar to `kill`).

---

## Forces and Commands

### force

```
force <character> <command>
```

Forces a character to execute a game command as if they typed it.

```
force $n north
force $n say Hello!
force $n drop all
```

---

### vforce

```
vforce <vnum> <command>
```

Forces all NPCs in the current room with the specified vnum to execute a command.

---

### gforce

```
gforce <character> <command>
```

Forces all members of a character's group to execute a command.

```
gforce $n follow me
```

---

## Quests

### questaccept

```
questaccept <character> <quest_vnum>
```

Starts a quest for a character.

```
questaccept $n 1001
```

---

### questcomplete

```
questcomplete <character> <quest_vnum>
```

Completes a quest and triggers its completion rewards.

```
questcomplete $n 1001
```

---

### questcancel

```
questcancel <character> <quest_vnum>
```

Cancels an in-progress quest for the character.

```
questcancel $n 1001
```

---

### questgenerate

```
questgenerate <character>
```

Generates a random quest for the character using this NPC's questor data.

---

### questscroll

```
questscroll <character>
```

Shows the character their current quest status.

---

### questpartcustom

```
questpartcustom <character> <quest_vnum> <part_id> <value>
```

Updates a custom quest part value for the character.

---

### questpartgetitem

```
questpartgetitem <character> <quest_vnum> <part_id> <obj_vnum>
```

Gives a quest item to the character and marks the quest part as complete.

---

### questpartgoto

```
questpartgoto <character> <quest_vnum> <part_id> <room_vnum>
```

Marks a "go to this room" quest part as complete.

---

### questpartslay

```
questpartslay <character> <quest_vnum> <part_id>
```

Marks a "slay this mob" quest part as complete.

---

### questpartrescue

```
questpartrescue <character> <quest_vnum> <part_id>
```

Marks a "rescue this NPC" quest part as complete.

---

## Variables

### varset / varseton

```
varset <name> number <value>
varset <name> string <text>
varset <name> room <vnum>
varset <name> obj <vnum>
varset <name> mob <vnum>
```

Sets a named variable on the specified entity. `varseton` sets on a different entity than self:

```
varset $n kills number 0
varseton $r room_state number 1
```

---

### varclear / varclearon

```
varclear <name>
varclearon <entity> <name>
```

Removes a named variable.

```
varclear $n kills
varclearon $r room_state
```

---

### varcopy

```
varcopy <source_entity> <source_var> <dest_entity> <dest_var>
```

Copies a variable from one entity to another.

```
varcopy $n score $n total_score
```

---

### varsave

```
varsave <entity> <name>
```

Persists a variable to the entity's save file (survives reboots).

```
varsave $n quest_stage
```

---

## Affects and Spells

### addaffect

```
addaffect <victim> <affect_name> <duration> <modifier>
```

Applies an affect to a character.

```
addaffect $n blind 10 0
addaffect $n fly 20 0
```

---

### addaffectname

```
addaffectname <victim> <affect_name> <duration> <modifier> <display_name>
```

Like `addaffect` but with a custom display name in the `affects` list.

---

### addspell

```
addspell <victim> <spell_name>
```

Permanently grants a spell to a character.

---

### remspell

```
remspell <victim> <spell_name>
```

Removes a permanently granted spell.

---

### stripaffect

```
stripaffect <victim> <affect_name>
```

Removes a specific active affect from a character.

```
stripaffect $n blind
```

---

### stripaffectname

```
stripaffectname <victim> <affect_name>
```

Like `stripaffect` but matches by display name.

---

### alteraffect

```
alteraffect <victim> <affect_name> <new_duration>
```

Changes the duration of an existing affect.

---

### fixaffects

```
fixaffects <character>
```

Recalculates and reapplies all affects for a character.

---

### cast

```
cast <spell_name> [victim]
```

The NPC casts a spell, optionally targeting a victim.

```
cast fireball $n
cast heal $n
```

---

### breathe

```
breathe <damage_type> [victim]
```

Simulates a breath weapon attack.

```
breathe fire $n
breathe cold
```

---

## Mob State Commands

### restore

```
restore <character>
```

Fully restores a character's HP, mana, and move.

```
restore $i
restore $n
```

---

### altermob

```
altermob <target_mob_vnum> <field> <value>
```

Changes a field on all copies of a mob in the current room.

---

### alterobj

```
alterobj <target_obj_keyword> <field> <value>
```

Changes a field on an object in the current room.

---

### alterroom

```
alterroom <field> <value>
```

Changes a field on the current room.

---

### alterexit

```
alterexit <direction> <field> <value>
```

Changes an exit property (e.g. open/close a door).

```
alterexit north door closed
alterexit north door open
```

---

### stringmob

```
stringmob <target_vnum> <field> <text>
```

Changes a string field (name, short desc, long desc, etc.) on all matching mobs in the room.

---

### stringobj

```
stringobj <target_keyword> <field> <text>
```

Changes a string field on a matching object in the room.

---

### ed

```
ed <target> <keyword> <text>
```

Adds or updates an extra description on a mob or object.

---

## Skills and Levels

### grantskill

```
grantskill <character> <skill_name>
```

Permanently grants a skill to a character.

---

### revokeskill

```
revokeskill <character> <skill_name>
```

Permanently removes a granted skill.

---

### skimprove

```
skimprove <character> <skill_name> <amount>
```

Improves a character's skill percentage by the given amount.

---

### remort

```
remort <character>
```

Forces a remort on the character (resets level, applies remort bonuses).

---

## Economy

### chargemoney

```
chargemoney <character> <amount>
```

Deducts gold from a character's inventory.

```
chargemoney $n 500
```

---

### chargebank

```
chargebank <character> <amount>
```

Deducts silver from a character's bank account.

---

### deduct

```
deduct <character> <type> <amount>
```

Deducts a resource from a character. Types: `questpoints`, `practices`, `trains`, etc.

---

### wiretransfer

```
wiretransfer <character> <amount>
```

Transfers gold directly to a character's bank account.

---

### award

```
award <character> <amount> <type>
```

Awards a resource to a character. Types: `xp`, `questpoints`, `gold`, `practices`, `trains`.

```
award $n 500 xp
award $n 200 gold
award $n 5 questpoints
```

---

## Dungeon and Blueprint Commands

### spawndungeon

```
spawndungeon <dungeon_vnum> <character>
```

Creates a dungeon instance for the specified character.

```
spawndungeon 100 $n
```

---

### dungeoncomplete

```
dungeoncomplete
```

Marks the current dungeon instance as completed.

---

### instancecomplete

```
instancecomplete
```

Marks the current blueprint instance as completed.

---

## Timers and Delays

### delay

```
delay <pulses>
```

Suspends the script for N pulses (1 pulse ≈ 0.25 seconds) then resumes execution.

```
say I'll be right back.
delay 20
say I'm back!
```

---

### cancel

```
cancel
```

Cancels any pending delayed script on the NPC.

---

### settimer

```
settimer <character> <ticks>
```

Sets the character's idle timer to N ticks.

---

### queue / dequeue

```
queue <delay_pulses> <command>
dequeue
```

Schedules a command for execution after N pulses. `dequeue` cancels queued commands.

---

### scriptwait

```
scriptwait <pulses>
```

Pauses script execution for N pulses without fully yielding.

---

### interrupt

```
interrupt
```

Cancels any pending delayed or queued scripts on the current NPC.

---

## Group Commands

### group

```
group <character>
```

Adds a character to the NPC's group.

```
group $n
```

---

### ungroup

```
ungroup <character>
```

Removes a character from the NPC's group.

---

## Miscellaneous

### attach

```
attach <script_vnum>
```

Attaches a script to the current NPC at runtime.

---

### detach

```
detach <script_vnum>
```

Removes a script from the current NPC at runtime.

---

### call

```
call <script_vnum>
```

Calls another script on the current entity as a sub-script. Execution continues after return.

---

### xcall

```
xcall <entity_type> <entity_vnum> <script_vnum>
```

Calls a script on another entity. Entity type is one of: `mob`, `obj`, `room`, `token`.

```
xcall mob 500 201
xcall obj 800 301
```

---

### checkpoint

```
checkpoint <character> <checkpoint_id>
```

Records a checkpoint for the character (used in dungeon/progress tracking).

---

### persist

```
persist <character>
```

Marks the NPC as persistent — it won't be purged on area reset.

---

### remember

```
remember <character>
```

Stores the character as the NPC's remembered/hunting target.

---

### forget

```
forget
```

Clears the NPC's remembered target.

---

### hunt

```
hunt <character>
```

Causes the NPC to begin actively hunting a character.

---

### mute / unmute

```
mute <character>
unmute <character>
```

Silences (or un-silences) a character.

---

### link

```
link <mob_vnum>
```

Links the NPC to another mobile (for follow/guard behavior).

---

### raisedead

```
raisedead <character> [room_vnum]
```

Resurrects a dead character, optionally moving them to a room.

---

### reckoning

```
reckoning <character>
```

Triggers a reckoning event on a character.

---

### resetdice

```
resetdice <character>
```

Resets a character's combat dice rolls.

---

### saveplayer

```
saveplayer <character>
```

Forces an immediate save of the character's data.

---

### setrecall

```
setrecall <character> <room_vnum>
```

Sets the character's recall point.

```
setrecall $n 3001
```

---

### showroom

```
showroom <room_vnum> <character>
```

Shows a room description to a character without moving them.

---

### asound

```
asound <text>
```

Sends a sound string to adjacent rooms (as a room "ambient sound").

---

### zot

```
zot <character>
```

Deals lethal damage to the character.

---

### crier

```
crier <text>
```

Broadcasts a town-crier style announcement to the area.

---

### prompt

```
prompt <character> <prompt_string>
```

Sets a custom prompt for the character.

---

### specialkey

```
specialkey <character> <key_id>
```

Grants temporary scripted-door access to a character.

---

### lockadd / lockremove

```
lockadd <exit_direction>
lockremove <exit_direction>
```

Adds or removes a script lock on a door exit.

---

### unlockarea

```
unlockarea <area_uid>
```

Marks an area as unlocked for the current player progression.

---

### treasuremap

```
treasuremap <character> <map_vnum>
```

Gives the character a treasure map token.

---

### wildernessmap

```
wildernessmap <character>
```

Displays the wilderness map to the character.

---

### applytoxin

```
applytoxin <victim> <toxin_type> <strength>
```

Applies a toxin effect to a character.

---

### usecatalyst

```
usecatalyst <character> <catalyst_vnum>
```

Triggers a catalyst item's effect on a character.

### addaffectname

### addspell

### alteraffect

### alterexit

### altermob

### alterobj

### alterroom

### appear

### applytoxin

### asound

### assist

### at

### attach

### award

### breathe

### call

### cancel

### cast

### chargebank

### chargemoney

### checkpoint

### cloneroom

### condition

### crier

### damage

### deduct

### delay

### dequeue

### destroyroom

### detach

### disappear

### dungeoncomplete

### echo

### echoaround

### echoat

### echobattlespam

### echochurch

### echogrouparound

### echogroupat

### echoleadaround

### echoleadat

### echonotvict

### echoroom

### ed

### entercombat

### fade

### fixaffects

### flee

### force

### forget

### gdamage

### gecho

### gforce

### goto

### grantskill

### group

### gtransfer

### hunt

### input

### inputstring

### instancecomplete

### interrupt

### junk

### kill

### link

### loadinstanced

### lockadd

### lockremove

### mload

### mute

### oload

### otransfer

### pageat

### peace

### persist

### prompt

### purge

### questaccept

### questcancel

### questcomplete

### questgenerate

### questpartcustom

### questpartgetitem

### questpartgoto

### questpartrescue

### questpartslay

### questscroll

### queue

### raisedead

### rawkill

### reckoning

### remember

### remort

### remove

### remspell

### resetdice

### restore

### revokeskill

### saveplayer

### scriptwait

### selfdestruct

### sendfloor

### setrecall

### settimer

### showroom

### skimprove

### spawndungeon

### specialkey

### startcombat

### stopcombat

### stringmob

### stringobj

### stripaffect

### stripaffectname

### take

### teleport

### transfer

### treasuremap

### ungroup

### unlockarea

### unmute

### usecatalyst

### varclear

### varclearon

### varcopy

### varsave

### varseton

### vforce

### wildernessmap

### wiretransfer

### xcall

### zecho

### zot