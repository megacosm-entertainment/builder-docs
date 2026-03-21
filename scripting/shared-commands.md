---
layout: default
title: Shared Commands Reference
nav_order: 8
parent: Scripting
---

# Shared Commands Reference
{: .no_toc }

Commands listed here are available across most or all entity types (mob, obj, room, token, area, instance, dungeon, event scripts). Use the appropriate prefix for your script type — for example, `mob echo` in a mob prog, `obj echo` in an object prog, etc.

Throughout this reference, `<prefix>` stands for the entity type prefix (`mob`, `obj`, `room`, `token`, `area`, `instance`, `dungeon`, `event`). Replace it with the correct prefix for your script.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Conventions Used in This Reference

- **`<prefix>`** — Replace with your script's entity type: `mob`, `obj`, `room`, `token`, `area`, `instance`, `dungeon`, or `event`.
- **`<required_arg>`** — A required argument. You must supply a value.
- **`[optional_arg]`** — An optional argument. You may omit it.
- **`$n`** — The character who triggered the script (the "actor"). See [Variables and Tokens](variables-and-tokens.md) for the full list of quick-code variables.
- **`lastreturn`** — Many commands set a special variable called `lastreturn` to indicate success or failure. You can check it with `if lastreturn == 1`.
- **Unrestricted command** — A command that does not require elevated script security to use.
- **Color codes** — Inline color tags such as `{R` (red), `{G` (green), `{x` (reset). See [Quick Codes](quick-codes.md) for the full list.

---

## Output Commands

### echo

```
<prefix> echo <message>
```

Sends a message to all characters in the current room. Supports quick-code variables (`$n`, `$N`, etc.) and color codes (`{R`, `{G`, etc.).

**Example:**
```
mob echo The ground trembles beneath your feet!
```

### echoat

```
<prefix> echoat <target> <message>
```

Sends a message only to the specified target. The target can be a character (by name or variable), a room, an area, an instance, a dungeon, or a church.

**Example:**
```
mob echoat $n You feel a chill run down your spine.
```

### echoaround

```
<prefix> echoaround <target> <message>
```

Sends a message to everyone in the room **except** the target.

**Example:**
```
mob echoaround $n $N shivers uncontrollably.
```

### echoroom

```
<prefix> echoroom <room> <message>
```

Sends a message to a specific room, which may differ from the script's current room. The room argument can be a mobile (uses their room), an object (uses its room), a room reference, or an exit.

**Example:**
```
mob echoroom $r The door creaks open from the other side.
```

### gecho

```
<prefix> gecho <message>
```

Broadcasts a message to **all** players in the entire game world. Immortals see a `Mob echo>` prefix for debugging purposes. Use sparingly — this reaches every connected player.

**Example:**
```
mob gecho {RThe sky darkens as a terrible storm approaches!{x
```

### zecho

```
<prefix> zecho <message>
```

Sends a message to all players in the same area/zone as the calling entity.

**Example:**
```
mob zecho The village bell rings three times.
```

### echobattlespam

```
<prefix> echobattlespam <message>
```

Sends a combat-flagged message to the current room. Players who have battlespam filtering enabled may not see this message. Use for repetitive combat flavor text.

**Example:**
```
mob echobattlespam Sparks fly as blades clash!
```

### echochurch

```
<prefix> echochurch <church> <message>
```

Sends a message to all online members of a specified church or religion.

**Example:**
```
mob echochurch $i {YA divine blessing washes over the faithful.{x
```

### echogroupat

```
<prefix> echogroupat <target> <message>
```

Sends a message to all members of the target's group.

**Example:**
```
mob echogroupat $n Your group receives a blessing of protection.
```

### echogrouparound

```
<prefix> echogrouparound <target> <message>
```

Sends a message to everyone in the room **except** the target's group members.

**Example:**
```
mob echogrouparound $n The adventuring party huddles together.
```

### echoleadat

```
<prefix> echoleadat <target> <message>
```

Sends a message to the group leader of the target's group only.

**Example:**
```
mob echoleadat $n As the leader, you sense danger ahead.
```

### echoleadaround

```
<prefix> echoleadaround <target> <message>
```

Sends a message to everyone in the room **except** the target's group leader.

**Example:**
```
mob echoleadaround $n The group leader signals silently.
```

### echonotvict

```
<prefix> echonotvict <victim> <message>
```

Sends a message to everyone in the room except the specified victim. Similar to `echoaround` but uses "victim" context instead of "target."

**Example:**
```
mob echonotvict $n $N doesn't notice the shadow behind them.
```

### asound

```
<prefix> asound <message>
```

Sends a sound or message to all **adjacent** rooms (not the current room). Useful for sounds that carry through walls.

**Example:**
```
mob asound You hear a loud crash from nearby!
```

### crier

```
<prefix> crier <message>
```

Announces a message through the town crier system (game-wide announcement).

**Example:**
```
mob crier The king has declared a day of celebration!
```

### pageat

```
<prefix> pageat <player> <message>
```

Sends a paginated (scrollable) message to a specific player. Only works on player characters (not NPCs).

Sets `lastreturn` to 1 on success, 0 on failure.

**Example:**
```
mob pageat $n {WDetailed quest information here...{x
```

### prompt

```
<prefix> prompt <player> <type> <value>
```

Sets or displays a custom prompt for a player. Only works on player characters.

**Example:**
```
mob prompt $n title Defender of the Realm
```

### mail

```
<prefix> mail <sender_name> <recipient_name> <message> [object1] [object2] ...
```

Sends an in-game mail message to a player, optionally with item attachments. Requires script security ≥ 9.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob mail 'The Blacksmith' $n 'Your repaired sword is ready.'
```

### wiznet

```
<prefix> wiznet <message>
```

Sends a message to the immortal wiznet channel. Useful for debugging scripts or alerting immortals.

**Example:**
```
mob wiznet [QUEST] Player $n completed the dragon quest.
```

---

## Movement and Transfer

### transfer

```
<prefix> transfer <victim|all> <destination> [force|quiet]
```

Teleports one or more characters to a destination room.

- `all` transfers everyone in the current room.
- `force` bypasses private room restrictions.
- `quiet` suppresses arrival and departure messages.

**Example:**
```
mob transfer $n 3001
mob transfer all 3054 quiet
```

### gtransfer

```
<prefix> gtransfer <victim> <destination> [force|quiet]
```

Transfers the victim's **entire group** to the destination room.

**Example:**
```
mob gtransfer $n 5020
```

### otransfer

```
<prefix> otransfer <object> <destination>
```

Transfers an object to a different location. The destination can be a character, a room, or a container.

**Example:**
```
mob otransfer sword $n
mob otransfer key 3001
```

### flee

```
<prefix> flee [target] [direction] [conceal] [pursue]
```

Forces a character to flee from combat. This is an unrestricted command.

| Parameter | Description |
|-----------|-------------|
| target | Character to force to flee. Defaults to the scripting entity. |
| direction | `north`, `south`, `east`, `west`, `up`, `down`, `random`, `any`, `none`, `anyway`, `wimpy` |
| conceal | Boolean. If true, the flee is hidden from the opponent. |
| pursue | Boolean (default true). If true, the enemy may pursue. |

Sets `lastreturn` to the exit direction on success, or a negative value on failure.

**Example:**
```
mob flee
mob flee $n north true false
```

---

## Loading and Creation

### mload

```
<prefix> mload <mobile_vnum> [count]
```

Loads (creates) one or more mobiles by vnum into the current room.

**Example:**
```
mob mload 3001
mob mload 3001 3
```

### oload

```
<prefix> oload <object_vnum> [count]
```

Loads (creates) one or more objects by vnum into the current location.

**Example:**
```
mob oload 5010
mob oload 5010 5
```

### cloneroom

```
<prefix> cloneroom <room_vnum> <environment|none> <variable_name>
```

Creates a dynamic copy of a room template. The new room's reference is stored in the named variable for later use. Use `environment` to set a parent environment, or `none` for a standalone room.

**Example:**
```
mob cloneroom 10050 none myroom
```

### destroyroom

```
<prefix> destroyroom <room>
```

Removes a dynamically created (cloned) room. Cannot destroy template or original rooms.

**Example:**
```
mob destroyroom $myroom
```

### purge

```
<prefix> purge [target]
```

Removes entities from the room. This is an unrestricted command.

- With no argument: removes all NPCs and objects from the current room.
- With a target: removes that specific entity.
- Cannot purge player characters.
- The scripting entity cannot purge itself — use `selfdestruct` instead.

**Example:**
```
mob purge
mob purge goblin
```

### junk

```
<prefix> junk <object>
```

Silently destroys an object. No message is shown to anyone.

**Example:**
```
mob junk sword
```

### remove

```
<prefix> remove <character> <object>
```

Unequips and extracts an object from a character.

**Example:**
```
mob remove $n helmet
```

### resetroom

```
<prefix> resetroom <room>
```

Resets a room to its default template state. Useful for dungeon rooms that need to be refreshed.

**Example:**
```
mob resetroom 5001
```

### loadinstanced

```
<prefix> loadinstanced <mobile|object> <vnum> [args]
```

Loads a mob or object specifically scoped to the current dungeon instance. The calling entity must be inside a valid instance or dungeon.

**Example:**
```
instance loadinstanced mobile 8001
```

---

## Combat

### damage

```
<prefix> damage <victim|all> <low> <high> [kill] [damage_class] [attacker]
<prefix> damage <victim|all> level <level_num> [kill] [damage_class] [attacker]
<prefix> damage <victim|all> remort [kill] [damage_class] [attacker]
```

Inflicts hit point damage on a target. There are three modes:

| Mode | Description |
|------|-------------|
| Number | Random damage between `low` and `high`. |
| Level | Damage scales based on the victim's level. |
| Remort | Damage scales for remort-tier characters. |

| Parameter | Description |
|-----------|-------------|
| kill / lethal | Allows the damage to actually kill. By default, damage stops at 1 HP. |
| damage_class | Type of damage (`fire`, `cold`, etc.). Defaults to TYPE_UNDEFINED. |
| attacker | Who is credited with the damage. Defaults to the script entity. |

**Example:**
```
mob damage $n 50 100
mob damage $n 100 200 kill fire
mob damage all level 10 kill
```

### entercombat

```
<prefix> entercombat [attacker] <victim> [silent]
```

Forces a character into combat with another. If the attacker is omitted, the scripting mob is used. The `silent` flag suppresses combat initiation messages.

Sets `lastreturn` to 1 on success, 0 on failure.

**Example:**
```
mob entercombat $n
mob entercombat guard $n true
```

### startcombat

```
<prefix> startcombat <victim> [attacker]
```

Initiates combat between two characters. Similar to `entercombat` but with different argument order.

**Example:**
```
mob startcombat $n
```

### stopcombat

```
<prefix> stopcombat <mobile> [both]
```

Stops combat for a specific character. Set `both` to true to also stop combat for their opponent.

**Example:**
```
mob stopcombat $n true
```

### peace

```
<prefix> peace
```

Stops **all** combat in the current room. This is an unrestricted command.

**Example:**
```
mob peace
```

### breathe

```
<prefix> breathe
```

Forces the entity to perform a breath attack. The entity must have a breath weapon defined.

**Example:**
```
mob breathe
```

### rawkill

```
<prefix> rawkill <victim> <corpse_type> [head] [show]
```

Kills a character instantly, bypassing normal death processing.

| Parameter | Description |
|-----------|-------------|
| corpse_type | Determines the type of corpse left behind. |
| head | Boolean. Whether to create a severed head. |
| show | Boolean. Whether to display death messages. |

**Example:**
```
mob rawkill $n normal false true
```

### zot

```
<prefix> zot <victim>
```

Strikes a character with divine punishment. Reduces HP, mana, and movement to 1 (does not kill). Displays dramatic lightning-bolt messages.

**Example:**
```
mob zot $n
```

### interrupt

```
<prefix> interrupt <target>
```

Interrupts a character's current action (spellcasting, skills, etc.).

**Example:**
```
mob interrupt $n
```

---

## Variables

For comprehensive documentation on the variable system, see [Variables and Tokens](variables-and-tokens.md).
{: .note }

### varset

```
<prefix> varset <name> <value>
```

Sets a variable on the calling entity's script. The special target names `quest`, `questindex`, `event`, and `eventindex` redirect to `varseton`.

**Example:**
```
mob varset quest_stage 3
mob varset player_name $n
```

### varseton

```
<prefix> varseton <target> <name> <value>
```

Sets a variable on a **different** entity. Target can be `quest`, `questindex`, `event`, `eventindex`, or a mobile/object/room/token reference.

**Example:**
```
mob varseton quest current_step 2
```

### varclear

```
<prefix> varclear [name]
```

With a name: deletes that specific variable from the calling entity. Without a name: clears **all** variables on the calling entity.

**Example:**
```
mob varclear quest_stage
mob varclear
```

### varclearon

```
<prefix> varclearon <target> [name]
```

Clears variable(s) on a different entity.

**Example:**
```
mob varclearon quest current_step
```

### varcopy

```
<prefix> varcopy <old_name> <new_name>
```

Copies a variable value from one name to another on the calling entity.

**Example:**
```
mob varcopy temp_score final_score
```

### varsave

```
<prefix> varsave <name> <on|off>
```

Marks a variable for persistent storage (survives reboots and copyovers). Accepts `on`/`off`, `true`/`false`, `yes`/`no`.

**Example:**
```
mob varsave quest_stage on
```

### varsaveon

```
<prefix> varsaveon <target> <name> <on|off>
```

Marks a variable on a different entity for persistent storage.

**Example:**
```
mob varsaveon quest current_step on
```

### persist

```
<prefix> persist <entity> <true|false|toggle>
```

Marks an entity (mobile, object, or room) for persistence across reboots. Requires maximum script security to enable. Fails on instanced entities or blueprint rooms.

**Example:**
```
mob persist self true
```

---

## Affects and Auras

### addaffect

```
<prefix> addaffect <target> <where> <group> <skill> <level> <location> <modifier> <hours> [bitvector] [bitvector2]
```

Adds a spell-like affect to a character or object.

| Parameter | Description |
|-----------|-------------|
| target | Character or object to receive the affect. |
| where | `TO_AFFECTS`, `TO_OBJECT`, `TO_OBJECT2`, `TO_OBJECT3`, `TO_OBJECT4`, `TO_WEAPON` |
| group | Affect group flag. |
| skill | Built-in skill name (e.g., `'armor'`). |
| level | Affect level. |
| location | Apply type (e.g., `APPLY_STR`, `APPLY_DEX`, `APPLY_AC`). |
| modifier | Numeric modifier value. |
| hours | Duration in hours. |
| bitvectors | Required for `TO_AFFECTS` and `TO_OBJECT` variants. |

Sets `lastreturn` to 1 on success.

**Example:**
```
mob addaffect $n to_affects none 'armor' 20 apply_ac -20 24 none
```

### addaffectname

```
<prefix> addaffectname <target> <affect_name> <level> <location> <modifier> <hours>
```

Adds a named affect using a string name instead of a skill reference. Simpler than `addaffect` — no where, group, or bitvector arguments needed.

**Example:**
```
mob addaffectname $n 'divine blessing' 30 apply_hitroll 5 12
```

### stripaffect

```
<prefix> stripaffect <target> <skill_name>
```

Removes all affects from a specific skill on the target.

**Example:**
```
mob stripaffect $n 'armor'
```

### stripaffectname

```
<prefix> stripaffectname <target> <affect_name>
```

Removes affects by name string instead of skill reference.

**Example:**
```
mob stripaffectname $n 'divine blessing'
```

### alteraffect

```
<prefix> alteraffect <target> <affect_name> <field> <operator> <value>
```

Modifies fields of an existing affect on a target.

| Parameter | Values |
|-----------|--------|
| field | `location`, `modifier`, `duration`, `hours`, `level`, etc. |
| operator | `=`, `+`, `-`, `*`, `/`, `%`, `&`, `\|`, `!`, `^` |

**Example:**
```
mob alteraffect $n 'armor' modifier + 10
mob alteraffect $n 'blessing' hours = 24
```

### fixaffects

```
<prefix> fixaffects <character>
```

Recalculates and repairs all affects on a character. Use after complex affect manipulation to ensure consistency.

**Example:**
```
mob fixaffects $n
```

### addaura

```
<prefix> addaura <target> <aura_name>
```

Adds a magical aura effect to a character.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob addaura $n 'holy radiance'
```

### remaura

```
<prefix> remaura <target> <aura_name>
```

Removes a magical aura from a character.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob remaura $n 'holy radiance'
```

### applytoxin

```
<prefix> applytoxin <victim> <toxin_name> <level> <duration>
```

Applies a poison or toxin effect to a target character.

| Parameter | Description |
|-----------|-------------|
| level | Minimum potency (1 or higher). |
| duration | Minimum 5 heartbeats. |

Sets `lastreturn` to 1 on success.

**Example:**
```
mob applytoxin $n 'spider venom' 10 30
```

---

## Entity Manipulation

### altermob

```
<prefix> altermob <mobile> <field> [operator] <value>
```

Modifies properties of a mobile or NPC. Checks script security for sensitive fields.

| Parameter | Values |
|-----------|--------|
| field | `name`, `short`, `long`, `race`, `class`, `sex`, `level`, `maxhp`, `hit`, `mana`, `move`, `align`, `spec`, `morph`, `tempstore1`–`tempstore4`, and many more |
| operator | `=`, `+`, `-`, `*`, `/`, `%`, `&`, `\|`, `!`, `^` |

**Example:**
```
mob altermob self maxhp + 100
mob altermob guard level = 30
```

### alterobj

```
<prefix> alterobj <object> <field> [operator] <value>
```

Modifies properties of an object.

| Parameter | Values |
|-----------|--------|
| field | `cond`, `cost`, `extra`, `fixes`, `level`, `repairs`, `tempstore1`–`tempstore4`, `timer`, `type`, `wear`, `wearloc`, `weight`, and numeric value slots (`0`–`7`) |
| operator | `=`, `+`, `-`, `*`, `/`, `%`, `&`, `\|`, `!`, `^` |

**Example:**
```
mob alterobj sword cost = 500
mob alterobj potion timer = 100
```

### alterroom

```
<prefix> alterroom <room> <field> [operator] <value>
```

Modifies properties of a room. Some fields (`environment`, `extern`) only work on clone (dynamic) rooms.

| Parameter | Values |
|-----------|--------|
| field | `name`, `desc`, `light`, `sector`, `heal`, `mana`, `move`, `mapx`, `mapy`, `rsflags`, `rssector`, `rsheal`, `rsmana`, `rsmove`, `tempstore1`–`tempstore4` |
| operator | `=`, `+`, `-`, `*`, `/`, `%`, `&`, `\|`, `!`, `^` |

**Example:**
```
mob alterroom here sector = forest
mob alterroom here heal + 5
```

### alterexit

```
<prefix> alterexit <room> <direction> <field> <value>
```

Modifies properties of a room exit.

| Parameter | Values |
|-----------|--------|
| direction | `north`, `south`, `east`, `west`, `up`, `down` |
| field | `flags`, `key`, `destination`, `name`, `desc` |

**Example:**
```
mob alterexit here north flags + locked
mob alterexit here east key 5010
```

### stringmob

```
<prefix> stringmob <mobile> <field> <value>
```

Modifies string properties of a mobile.

| Field | Description |
|-------|-------------|
| `name` | Keywords used for targeting |
| `short` | Short description (seen in room lists, combat) |
| `long` | Long description (seen when looking at the room) |
| `race` | Race name string |
| `class` | Class name string |
| `title` | Title string |
| `skill` | Skill name |
| `plevel` | Player level string |
| `remort` | Remort string |
| `room_long` | Room-specific long description |
| `full` | Full description (seen with `look at`) |
| `tempstore1`–`tempstore4` | Temporary string storage |

**Example:**
```
mob stringmob self short {Ra powerful demon{x
```

### stringobj

```
<prefix> stringobj <object> <field> <value>
```

Modifies string properties of an object.

| Field | Description |
|-------|-------------|
| `name` | Keywords used for targeting |
| `short` | Short description |
| `long` | Long description (on the ground) |
| `full` | Full description (when examined) |
| `material` | Material name |
| `owner` | Owner name |

**Example:**
```
mob stringobj sword name enchanted sword of flames
mob stringobj sword short {Ran enchanted sword of flames{x
```

### ed

```
<prefix> ed <target> <command> <keyword> [description]
```

Adds, modifies, or removes extra descriptions on an entity.

| Command | Syntax | Description |
|---------|--------|-------------|
| `set` | `ed <target> set <keyword> <description>` | Sets or updates an extra description. |
| `delete` | `ed <target> delete <keyword>` | Removes a specific extra description. |
| `clear` | `ed <target> clear` | Removes **all** extra descriptions. |

Sets `lastreturn` to 1 on success.

**Example:**
```
mob ed sword set inscription 'Forged in dragon fire'
mob ed sword delete inscription
```

### condition

```
<prefix> condition <mobile> <field> <operator> <value>
```

Sets or modifies condition values on a mobile (hunger, thirst, etc.).

**Example:**
```
mob condition $n hunger = 48
```

### setposition

```
<prefix> setposition <mobile> <position>
```

Sets a character's position. Common values: `standing`, `sitting`, `sleeping`, `resting`, `fighting`.

**Example:**
```
mob setposition $n sleeping
```

### resetdice

```
<prefix> resetdice <weapon_object>
```

Re-rolls the damage dice on a weapon object.

**Example:**
```
mob resetdice sword
```

### link

```
<prefix> link <room> <direction> <destination> [flags]
```

Creates or modifies an exit link between two rooms.

**Example:**
```
mob link here north 5020
```

### lockadd

```
<prefix> lockadd <object>
```

Adds a lock to a container, book, or portal object. The object must be of type ITEM_CONTAINER, ITEM_BOOK, or ITEM_PORTAL. Fails if the object already has a lock.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob lockadd chest
```

### lockremove

```
<prefix> lockremove <object>
```

Removes a lock from a container, book, or portal. Only removes locks that were created by scripts.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob lockremove chest
```

---

## Flow Control

### call

```
<prefix> call <script_vnum> [ch] [victim] [obj1] [obj2]
```

Calls another script as a subroutine. Optional parameters are passed as context (`ch`, `victim`, `obj1`, `obj2`) to the called script. Call depth is limited to prevent infinite recursion.

Sets `lastreturn` to the called script's return value.

**Example:**
```
mob call 5001 $n
mob call 5002 $n $r obj1
```

### xcall

```
<prefix> xcall <entity> <script_vnum> [ch] [victim] [obj1] [obj2]
```

Cross-entity script call — executes a script in the context of a **different** entity. The entity can be a mobile, object, room, token, area, instance, or dungeon. Requires script security ≥ 5.

**Example:**
```
mob xcall guard 5001 $n
```

### queue

```
<prefix> queue <delay_seconds> <command>
```

Schedules a command to execute after a delay (0–1000 seconds). The command runs as if the entity typed it.

**Example:**
```
mob queue 10 mob echo The ritual is complete!
```

### dequeue

```
<prefix> dequeue
```

Removes all queued commands from the current entity. This is an unrestricted command.

**Example:**
```
mob dequeue
```

### input

```
<prefix> input <player> <script_vnum> [variable_name] [prompt]
```

Prompts a player for text input, then calls the specified script with their response stored in the named variable.

Sets `lastreturn` to 1 on success, 0 on failure.

**Example:**
```
mob input $n 5010 answer 'What is your name? >'
```

### inputstring

```
<prefix> inputstring <player> <string>
```

Sends a string as if the player typed it (raw input injection).

**Example:**
```
mob inputstring $n say I agree to the terms
```

### event

```
<prefix> event <subcommand> <event> [args...]
```

Complex event management command with multiple subcommands:

| Subcommand | Syntax | Description |
|------------|--------|-------------|
| `progress` | `event progress <event> <operation> <value>` | Modify event progress. |
| `phase` | `event phase <event> next\|set <phase_name>` | Advance or set the event phase. |
| `stage` | `event stage <event> next\|set <stage_name>` | Advance or set the event stage. |
| `objective` | `event objective <event> check\|next\|advance\|complete` | Manage event objectives. |
| `clear` | `event clear <target> [args]` | Clear event state. |
| `inherit` | `event inherit <target> [args]` | Inherit event state. |
| `set` | `event set <target> [args]` | Set event state. |
| `copy` | `event copy <target> [args]` | Copy event state. |

**Example:**
```
mob event progress myevent + 10
mob event phase myevent next
```

---

## Quest and Dungeon

### questaccept

```
<prefix> questaccept <player> [scroll]
```

Accepts and begins a scripted quest for a player. Finalizes a pending quest into the player's journal.

**Example:**
```
mob questaccept $n
```

### questcomplete

```
<prefix> questcomplete <player>
```

Marks the current quest as completed for a player.

**Example:**
```
mob questcomplete $n
```

### questcancel

```
<prefix> questcancel <player>
```

Cancels an active quest for a player.

**Example:**
```
mob questcancel $n
```

### questgenerate

```
<prefix> questgenerate <player>
```

Procedurally generates a random quest for a player.

**Example:**
```
mob questgenerate $n
```

### questscroll

```
<prefix> questscroll <player>
```

Creates a quest scroll item for the player.

**Example:**
```
mob questscroll $n
```

### questpartcustom

```
<prefix> questpartcustom <player> <description>
```

Adds a custom quest objective with a text description.

**Example:**
```
mob questpartcustom $n 'Speak with the village elder'
```

### questpartgetitem

```
<prefix> questpartgetitem <player> <item_vnum>
```

Adds a "collect item" quest objective.

**Example:**
```
mob questpartgetitem $n 5001
```

### questpartgoto

```
<prefix> questpartgoto <player> <room_vnum>
```

Adds a "go to location" quest objective.

**Example:**
```
mob questpartgoto $n 3050
```

### questpartrescue

```
<prefix> questpartrescue <player> <mob_vnum>
```

Adds a "rescue NPC" quest objective.

**Example:**
```
mob questpartrescue $n 4010
```

### questpartslay

```
<prefix> questpartslay <player> <mob_vnum>
```

Adds a "kill mob" quest objective.

**Example:**
```
mob questpartslay $n 4020
```

### quest

```
<prefix> quest <subcommand> [args...]
```

General quest system interaction (start, complete, set step, etc.). This command is available only in area, instance, dungeon, and event scripts.

**Example:**
```
area quest start $n 1001
```

### dungeoncommence

```
<prefix> dungeoncommence <dungeon>
```

Marks a dungeon as COMMENCED, triggering start-of-dungeon scripts.

**Example:**
```
instance dungeoncommence $dungeon
```

### dungeoncomplete

```
<prefix> dungeoncomplete <dungeon>
```

Marks a dungeon as successfully COMPLETED.

**Example:**
```
instance dungeoncomplete $dungeon
```

### dungeonfailure

```
<prefix> dungeonfailure <dungeon>
```

Marks a dungeon as FAILED.

**Example:**
```
instance dungeonfailure $dungeon
```

### instancecomplete

```
<prefix> instancecomplete <instance>
```

Marks the current dungeon instance as completed.

**Example:**
```
instance instancecomplete $instance
```

### instancefailure

```
<prefix> instancefailure <instance>
```

Marks the current dungeon instance as failed.

**Example:**
```
instance instancefailure $instance
```

### spawndungeon

```
<prefix> spawndungeon <player> <dungeon_vnum> <floor> <variable_name>
```

Creates and initializes a new dungeon instance. The spawned entry room reference is stored in the named variable. Does **not** auto-transfer the player — you must use `transfer` separately.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob spawndungeon $n 8001 1 entry_room
```

### checkpoint

```
<prefix> checkpoint <player> <room|none|clear|reset>
```

Sets a player's dungeon respawn point. Use `none`, `clear`, or `reset` to remove the checkpoint.

**Example:**
```
mob checkpoint $n 5050
mob checkpoint $n clear
```

### sendfloor

```
<prefix> sendfloor <player> [floor_number]
```

Sends a dungeon floor transition to a player.

**Example:**
```
mob sendfloor $n 2
```

### specialkey

```
<prefix> specialkey <args>
```

Creates or manages special key items (for dungeon doors, ships, etc.).

**Example:**
```
mob specialkey $n dungeon_key
```

---

## Player Interaction

### force

```
<prefix> force <victim|all> <command>
```

Forces a character to execute a command. Using `all` forces every NPC in the room. Cannot force immortals; respects the command hierarchy.

**Example:**
```
mob force $n say I surrender!
mob force all bow
```

### gforce

```
<prefix> gforce <leader> <command> [all|standing]
```

Forces all members of the leader's group to execute a command.

**Example:**
```
mob gforce $n sit
```

### vforce

```
<prefix> vforce <mob_vnum> <command>
```

Forces all instances of a mobile vnum in the room to execute a command.

**Example:**
```
mob vforce 3001 mob echo We obey!
```

### award

```
<prefix> award <target|church> <type> [subtype] <amount>
```

Awards resources to a player or church.

**Player types:**

| Type | Description |
|------|-------------|
| `silver` | Silver coins |
| `gold` | Gold coins |
| `pneuma` | Pneuma (spirit energy) |
| `deity` / `dp` | Deity points |
| `practice` | Practice sessions |
| `train` | Training sessions |
| `quest` / `qp` | Quest points |
| `experience` / `xp` | Experience points |
| `reputation` | Reputation (requires subtype: reputation index or widevnum) |
| `paragon` | Paragon points (requires subtype: widevnum) |

**Church types:** `gold`, `pneuma`, `deity` / `dp`

Sets `lastreturn` to the actual amount awarded.

**Example:**
```
mob award $n experience 500
mob award $n gold 100
mob award $n reputation 'Dragon Slayers' 50
```

### deduct

```
<prefix> deduct <target|church> <type> [subtype] <amount>
```

Deducts resources from a player or church. Same types as `award`.

Sets `lastreturn` to the actual amount deducted.

**Example:**
```
mob deduct $n gold 50
```

### chargebank

```
<prefix> chargebank <player> <amount>
```

Deducts gold from a player's bank account. Fails if the player has insufficient funds.

**Example:**
```
mob chargebank $n 1000
```

### wiretransfer

```
<prefix> wiretransfer <player> <amount>
```

Adds gold to a player's bank account. Limited to 1000 gold for scripts with security lower than 7. All transfers are logged.

**Example:**
```
mob wiretransfer $n 500
```

### restore

```
<prefix> restore <character> [percent]
```

Restores a character's HP, mana, and movement. The percent argument (1–100) controls how much is restored; it defaults to 100 (full restore).

**Example:**
```
mob restore $n
mob restore $n 50
```

### remort

```
<prefix> remort <player>
```

Initiates the remort (rebirth) process for a player. The player must be at LEVEL_HERO and not busy.

Sets `lastreturn` to 1 if the remort prompt was shown.

**Example:**
```
mob remort $n
```

### saveplayer

```
<prefix> saveplayer <player> [checkpoint_room]
```

Force-saves a player character to disk. The optional checkpoint room sets the player's respawn location.

**Example:**
```
mob saveplayer $n
mob saveplayer $n 3001
```

### mute

```
<prefix> mute <player>
```

Mutes a player by incrementing the mute counter, preventing speech.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob mute $n
```

### unmute

```
<prefix> unmute <player>
```

Unmutes a player by decrementing the mute counter.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob unmute $n
```

### showcommand

```
<prefix> showcommand <player> <command_string>
```

Registers an extra command for a player to see. Only works during the `TRIG_SHOWCOMMANDS` trigger.

Sets `lastreturn` to 1 if added, 0 if the command is a duplicate.

**Example:**
```
mob showcommand $n 'pray at the altar'
```

### showroom

```
<prefix> showroom <viewer> <mode> <args...>
```

Displays a remote room to a player (scrying or preview).

| Mode | Syntax |
|------|--------|
| `map` | `showroom <viewer> map <mapid> <x> <y> <z> <scale> <width> <height> [force]` |
| `room` | `showroom <viewer> room <room_ref> [force]` |

**Example:**
```
mob showroom $n room 5001
```

---

## Skills, Classes, and Traits

### grantskill

```
<prefix> grantskill <player> <skill_name|token_vnum> [rating] [permanent] [flags]
```

Grants a skill to a player.

| Parameter | Description |
|-----------|-------------|
| rating | Proficiency from 1–100 (default 1). |
| permanent | `true` or `false`. |
| flags | `none`, `automatic`, etc. |

Sets `lastreturn` to 1 on success.

**Example:**
```
mob grantskill $n 'dodge' 75 true
```

### revokeskill

```
<prefix> revokeskill <player> <skill_name|token_vnum>
```

Removes a skill from a player.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob revokeskill $n 'dodge'
```

### grantclass

```
<prefix> grantclass <player> <class_name> [level]
```

Grants a player access to a class. Level sets the starting level (default 1). The player's character is saved automatically.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob grantclass $n warrior 5
```

### revokeclass

```
<prefix> revokeclass <player> <class_name>
```

Revokes a class from a player. If it is the player's current class, the system switches to another class first.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob revokeclass $n thief
```

### grantsong

```
<prefix> grantsong <player> <song_name>
```

Grants a bard song to a player. The player's character is saved automatically.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob grantsong $n 'song of valor'
```

### revokesong

```
<prefix> revokesong <player> <song_name>
```

Removes a bard song from a player.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob revokesong $n 'song of valor'
```

### addspell

```
<prefix> addspell <target> <spell_name> [level]
```

Adds a spell to a character's known spell list. The level defaults to the target's current level.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob addspell $n 'fireball' 20
```

### remspell

```
<prefix> remspell <target> <spell_name>
```

Removes a spell from a character's known spell list.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob remspell $n 'fireball'
```

### setclass

```
<prefix> setclass <player> <class_name>
```

Sets a player's currently active class.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob setclass $n mage
```

### setclasslevel

```
<prefix> setclasslevel <player> <level>
```

Sets the player's level in their current class.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob setclasslevel $n 10
```

### setrace

```
<prefix> setrace <player> <race_name>
```

Changes a player's race. The race must be a playable race.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob setrace $n elf
```

### settrait

```
<prefix> settrait <player> <trait_id> <value>
```

Sets a specific trait value on a character.

| Trait type | Accepted values |
|------------|-----------------|
| Boolean | `true` / `false` / `yes` / `no` |
| Integer | Numeric value |
| String | Text value |

Sets `lastreturn` to 1 on success.

**Example:**
```
mob settrait $n night_vision true
```

### skimprove

```
<prefix> skimprove <player> <skill_name> <success|difficulty>
```

Triggers a skill improvement check.

| Parameter | Description |
|-----------|-------------|
| success | Boolean or 0–100 percent success rate. |
| difficulty | 1–10 difficulty level for auto-calculation. |

**Example:**
```
mob skimprove $n 'dodge' 5
```

### addstache

```
<prefix> addstache <player> <keyword> <vnum> [slot]
```

Adds a cosmetic or appearance customization to a character.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob addstache $n 'handlebar' 1001
```

### remstache

```
<prefix> remstache <player> <slot|keyword>
```

Removes a cosmetic customization from a character.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob remstache $n 'handlebar'
```

---

## Script and Entity Management

### attach

```
<prefix> attach <entity> <target> <type> [show]
```

Attaches a script program to an entity.

| Parameter | Values |
|-----------|--------|
| type | `pet`, `master`, `leader`, `reply`, `cart`, `pull`, `on` |
| show | Boolean (default true). Displays the attachment message. |

Sets `lastreturn` to 1 on success.

**Example:**
```
mob attach $n guard pet true
```

### detach

```
<prefix> detach <entity> <type> [show]
```

Removes a script program attachment from an entity. Same types as `attach`.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob detach $n pet
```

### remember

```
<prefix> remember <mobile>
```

Stores a target character in the entity's memory for later recall via the `$q` variable. Persists until `forget` is called.

**Example:**
```
mob remember $n
```

### forget

```
<prefix> forget
```

Clears the entity's remembered target (`$q` becomes empty).

**Example:**
```
mob forget
```

### settimer

```
<prefix> settimer <player> <timer_name> <operator> <value>
```

Sets or modifies a named timer on a player. Operators: `=`, `+`, `-`.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob settimer $n quest_cooldown = 3600
```

### setrecall

```
<prefix> setrecall <player> <room|none>
```

Sets a player's recall point (home room for the `recall` command). Use `none` to clear the recall point.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob setrecall $n 3001
mob setrecall $n none
```

---

## Group Management

### group

```
<prefix> group <follower> [show] [leader]
```

Forces grouping operations. The follower joins the leader's group. The leader defaults to the scripting mob if omitted. Set `show` to display or suppress grouping messages.

**Example:**
```
mob group $n true
```

### ungroup

```
<prefix> ungroup <mobile> [all]
```

Forces a character to leave their group. Set `all` to true to disband the entire group.

**Example:**
```
mob ungroup $n
mob ungroup $n true
```

---

## Events and World Systems

### startevent

```
<prefix> startevent <event_args>
```

Starts a world or area event.

**Example:**
```
mob startevent 'harvest_festival'
```

### stopevent

```
<prefix> stopevent <event>
```

Stops an active world or area event.

**Example:**
```
mob stopevent 'harvest_festival'
```

### phaseevent

```
<prefix> phaseevent <event>
```

Advances an event to its next phase.

**Example:**
```
mob phaseevent 'harvest_festival'
```

### stageevent

```
<prefix> stageevent <event>
```

Advances an event to its next stage. This is an alias for `phaseevent`.

**Example:**
```
mob stageevent 'harvest_festival'
```

### reckoning

```
<prefix> reckoning <field> <operator> <value>
```

Modifies global reckoning system parameters.

| Parameter | Values |
|-----------|--------|
| field | `chance`, `cooldown`, `duration`, `intensity` |
| operator | `=`, `+`, `-` |

Sets `lastreturn` to 1 on success.

**Example:**
```
mob reckoning intensity = 5
```

### startreckoning

```
<prefix> startreckoning [duration] [skip]
```

Starts a reckoning world event sequence. Duration is measured in ticks (default 30). Set `skip` to true to bypass the intro phase.

**Example:**
```
mob startreckoning 60
```

### stopreckoning

```
<prefix> stopreckoning [immediate|duration]
```

Stops an active reckoning event.

**Example:**
```
mob stopreckoning
```

---

## Wilderness

### wildernessmap

```
<prefix> wildernessmap <args>
```

Creates or modifies a wilderness map overlay.

**Example:**
```
mob wildernessmap create 100 100
```

### wildstile

```
<prefix> wildstile <wuid> <x> <y> [dx] [dy] <character>
```

Sets or modifies a specific wilderness tile.

**Example:**
```
mob wildstile $wuid 10 20 'T'
```

### wildsoverlay

```
<prefix> wildsoverlay <subcommand> <args>
```

Manages wilderness overlays. Subcommands: `add`, `remove`, `clearregion`, `cleanup`.

**Example:**
```
mob wildsoverlay add $wuid 10 10 5 5 forest_overlay
```

### wildsanchor

```
<prefix> wildsanchor <variable_name> <wuid> <x> <y> [dx] [dy]
```

Sets an anchor point on the wilderness map and stores it in a variable.

**Example:**
```
mob wildsanchor town_center $wuid 50 50
```

### wildsvlink

```
<prefix> wildsvlink <args>
```

Creates a virtual link between wilderness tiles and interior rooms.

**Example:**
```
mob wildsvlink $wuid 50 50 cave_entrance 3001
```

### treasuremap

```
<prefix> treasuremap <wuid> <area> <treasure> <variable_name>
```

Creates or interacts with a treasure map item. The result is stored in the named variable.

**Example:**
```
mob treasuremap $wuid 'Dark Forest' 'ancient gold' map_obj
```

---

## Religion and Organizations

### churchannouncetheft

```
<prefix> churchannouncetheft <thief> [stolen_object]
```

Announces a theft to members of a church or religion organization.

Sets `lastreturn` to 1 on success.

**Example:**
```
mob churchannouncetheft $n
```

### shop

```
<prefix> shop <mobile> <subcommand> <args>
```

Interacts with the shop system. Subcommands include `add`, `remove`, `deplete`, `discount`, `flags`, `hours`, and more.

**Example:**
```
mob shop self add 5001
mob shop self discount 25
```

---

## Area and Dungeon Unlocking

### unlockarea

```
<prefix> unlockarea <player> <area|area_name>
```

Unlocks a locked area, making it accessible to the player.

**Example:**
```
mob unlockarea $n 'The Dark Caverns'
```

### unlockdungeon

```
<prefix> unlockdungeon <player> <dungeon>
```

Unlocks a dungeon, making it accessible to the player.

**Example:**
```
mob unlockdungeon $n 8001
```

---

## Miscellaneous

### usecatalyst

```
<prefix> usecatalyst <target> <type> <method> <amount> <min> <max> [show]
```

Consumes a catalyst component and checks if requirements are met. Set `show` to true to display the consumption message.

**Example:**
```
mob usecatalyst $n fire consume 1 1 5 true
```

### fade

```
<prefix> fade [target] <door> <rating> [busy_override]
```

Attempts a fading (magical escape) action.

| Parameter | Description |
|-----------|-------------|
| door | `random` / `any` for a random exit, or a specific direction. |
| rating | Success chance from 1–100. |
| busy_override | Boolean. Bypasses busy checks if true. |

Sets `lastreturn` to the door number on success, or -1 on failure. Blocked in NO_FADING areas and for characters pulling carts, in combat, or rifting.

**Example:**
```
mob fade $n random 75
```
