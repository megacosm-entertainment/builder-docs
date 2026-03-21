---
layout: default
title: Triggers
nav_order: 1
parent: Room Programs
grand_parent: Scripting
---

# Room Program Triggers
{: .no_toc }

Room programs support **71 triggers** that fire in response to movement, speech, combat, environmental events, and more. This page documents every trigger available to room programs.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## How Triggers Work

Each trigger is attached to a room program with a **phrase** that controls when it fires. The phrase meaning depends on the trigger type:

| Phrase Type | Meaning | Example |
|-------------|---------|---------|
| Percent chance | Fires N% of the time | `50` = 50% chance |
| Keyword substring | Fires when text contains the keyword | `hello` matches "say hello there" |
| Direction number | Matches an exit direction | `0`=north, `1`=east, `2`=south, `3`=west, `4`=up, `5`=down |
| Direction name | Matches a direction by name string | `north`, `east`, `portal` |
| Vnum/name match | Matches an object vnum or keyword | `3001` or `sword` |
| Exact name match | Case-insensitive exact match | `fireball` |
| Direct execution | Always fires (no phrase needed) | — |

### Variable Bindings

When a trigger fires, the script receives context through variables:

| Variable | Alias | Description |
|----------|-------|-------------|
| `$self` / `$this` | `$i` | The scripted room |
| `$enactor` | `$n` | Character who triggered the event |
| `$victim` | `$t` | Victim or target |
| `$victim2` | — | Secondary victim (rare) |
| `$random` | — | Random character in the room |
| `$obj1` | `$p` | Primary object parameter |
| `$obj2` | — | Secondary object |
| `$token` | `$q` | Token parameter |
| `$here` | — | Current room (same as `$self`) |
| `$phrase` | — | Matched phrase/argument |
| `$trigger` | — | Trigger phrase pattern that matched |
| `$register1`–`$register5` | — | Numeric registers for passing data |

---

## Movement Triggers

### greet

Fires when a character enters the room and the room can "see" them (character is visible and standing).

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = entering character
- **Notes:** Most common room trigger. Does not fire for invisible or sneaking characters.

```
** Welcome message for visible entrants
room echo {CThe crystals in the walls pulse with a soft glow as $n enters.{x
```

### grall

Fires when a character enters the room, regardless of visibility or position. Like `greet` but ignores sight checks.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = entering character
- **Notes:** Use when the room should react to all entrants, including invisible or sneaking characters.

```
** Magical ward detects all passage
room echo {MThe ward flickers — something has passed through.{x
```

### entry

Fires when the scripted entity itself enters a room. For room programs, this fires when the room is loaded or entered in an instance/dungeon context.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = entering character
- **Notes:** Primarily useful in dungeon/instance contexts where rooms are dynamically loaded.

### exit

Fires when a character attempts to leave the room in a specific direction. The character must be visible and in a valid position.

- **Phrase:** Direction number (`0`=north, `1`=east, `2`=south, `3`=west, `4`=up, `5`=down)
- **Bindings:** `$n` = departing character
- **Notes:** Can prevent movement by returning a blocking result. Use `*` to match all directions.

```
** Block exit south unless puzzle is solved
if varexists $n puzzle_complete
else
  room echoat $n {RThe stone door will not budge.{x
  break
endif
```

### exall

Like `exit` but fires regardless of the character's visibility or position.

- **Phrase:** Direction number (exact match)
- **Bindings:** `$n` = departing character
- **Notes:** Use when the room should detect all movement attempts, including invisible characters.

### preenter

Fires before a character enters through a portal or special entrance. Can prevent the entry.

- **Phrase:** Direction name string or `portal`
- **Bindings:** `$n` = entering character
- **Notes:** Use to gate access to portals or special room transitions.

### move_char

Fires when a character is moved programmatically (by a script, not by walking).

- **Phrase:** Direction name string
- **Bindings:** `$n` = moved character
- **Notes:** Only fires on scripted movement (e.g., `transfer`, `gtransfer`), not player-initiated movement.

---

## Speech Triggers

### speech

Fires when a character says something in the room that matches the phrase.

- **Phrase:** Keyword substring match
- **Bindings:** `$n` = speaker, `$phrase` = what was said
- **Notes:** Matches if the spoken text contains the phrase as a substring. Case-insensitive.

```
** React to the magic word
room echo {YThe ancient runes on the wall begin to glow!{x
room alterexit north door open
room echo {GThe northern passage slides open with a grinding of stone.{x
```

---

## Action Triggers

### act

Fires when an action message (from `act()`) visible to the room matches the phrase.

- **Phrase:** Keyword substring match
- **Bindings:** `$n` = actor
- **Notes:** Matches against the raw act message string. Useful for reacting to combat messages, emotes, social commands, etc.

```
** React to someone praying
room echo {WA warm light fills the room in response.{x
```

---

## Combat Triggers

### fight

Fires each combat round while fighting is happening in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = one of the combatants
- **Notes:** Fires repeatedly during combat. Use low percentages to avoid spam.

```
** Occasional combat narration
room echo {rBlood spatters across the stone floor.{x
```

### afterkill

Fires after a character kills another character in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = killer, `$t` = victim (now dead)

### startcombat

Fires when combat begins between two characters in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = attacker, `$t` = defender

### prewimpy

Fires before a character attempts to wimpy (auto-flee at low HP). Can prevent it.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character attempting wimpy

### wimpy

Fires after a character successfully wimpies.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character who wimpied

---

## Damage Triggers

### damage

Fires when a character in the room takes damage.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = damaged character
- **Notes:** Fires on all damage sources (combat, spells, scripts, environment).

### check_damage

Fires before damage is applied, allowing the script to modify the damage amount.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character about to take damage
- **Notes:** Use `$register1` to read/modify the damage amount.

---

## Spell Triggers

### spellcast

Fires when a spell is cast in the room.

- **Phrase:** Exact spell name match (case-insensitive)
- **Bindings:** `$n` = caster, `$t` = target
- **Notes:** Use to create anti-magic zones, spell reflection effects, or special reactions to specific spells.

```
** Anti-magic zone
room echoat $n {DYour magic fizzles and dies in this dead zone.{x
break
```

---

## Item Interaction Triggers

### drop

Fires when a character drops an object in the room.

- **Phrase:** Object vnum, keyword, or percent chance
- **Bindings:** `$n` = character, `$o` = dropped object
- **Notes:** In room programs, the phrase matches against the object's vnum or keyword.

```
** React to dropping the gem into the pedestal room
room echo {YThe gem begins to glow as it touches the ancient stone!{x
```

### get

Fires when a character picks up an object from the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character, `$o` = picked-up object

### throw

Fires when a character throws an object in the room.

- **Phrase:** Vnum or keyword match
- **Bindings:** `$n` = thrower, `$o` = thrown object

### wear

Fires when a character equips an item while in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character, `$o` = worn object

### eat

Fires when a character eats something in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### preeat

Fires before eating. Can prevent the action.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### drink

Fires when a character drinks in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### predrink

Fires before drinking. Can prevent the action.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### board

Fires when a character boards a vehicle or conveyance in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### bury

Fires when a character buries an object in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character, `$o` = buried object

---

## Door and Exit Triggers

### knock

Fires in the room where someone knocks on a door.

- **Phrase:** Direction number of the knocked door
- **Bindings:** `$n` = knocker
- **Notes:** Room-exclusive trigger (only room and token). Fires in the room where the knock originates.

```
** React to knocking on the north door
room echo {wA hollow echo reverberates from the north door.{x
```

### knocking

Fires in the **adjacent** room when someone knocks on a connecting door.

- **Phrase:** Direction number (from the adjacent room's perspective)
- **Bindings:** `$n` = knocker
- **Notes:** Room-exclusive trigger. Fires on the other side of the knocked door.

```
** The room on the other side hears the knock
room echo {wYou hear a faint knocking from the south.{x
```

### open

Fires when a character opens a door or container in the room.

- **Phrase:** Direction number or percent chance
- **Bindings:** `$n` = character

### close

Fires when a character closes a door or container in the room.

- **Phrase:** Direction number or percent chance
- **Bindings:** `$n` = character

---

## Position Triggers

### rest

Fires when a character rests in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### prerest

Fires before resting. Can prevent the action.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### sit

Fires when a character sits in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### presit

Fires before sitting. Can prevent the action.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### sleep

Fires when a character goes to sleep in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### presleep

Fires before sleeping. Can prevent the action.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### stand

Fires when a character stands up in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### prestand

Fires before standing. Can prevent the action.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### wake

Fires when a character wakes up in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### prewake

Fires before waking. Can prevent the action.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

---

## Timed and Random Triggers

### random

Fires periodically with a percent chance each tick.

- **Phrase:** Percent chance (1–100)
- **Bindings:** None (no specific enactor)
- **Notes:** Fires on the room's random pulse. Use for atmospheric effects, periodic events, and environmental narration.

```
** Atmospheric description
room echo {DA cold draft whispers through the corridor.{x
```

### tick

Fires every game tick.

- **Phrase:** Percent chance (1–100)
- **Bindings:** None
- **Notes:** Fires more frequently than `random`. Use for time-sensitive mechanics.

### delay

Fires after a previously set delay timer expires on the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** Preserved from the script that set the delay
- **Notes:** Set a delay with `room delay <pulses>`, then the `delay` trigger fires when it expires.

---

## Death and Repop Triggers

### death

Fires when a character dies in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = dying character

```
** Death narration
room echo {DThe room grows colder as $n falls.{x
```

### death_protection

Fires when a death protection effect activates for a character in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = protected character

### death_timer

Fires when a character's death timer expires in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### extract

Fires when an entity is extracted (removed from the game world) in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = extracted entity

### preanimate

Fires before an animate/resurrection spell is cast in the room. Can prevent it.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = caster

### preresurrect

Fires before a resurrection attempt in the room. Can prevent it.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character being resurrected

### reset

Fires when the room's area resets (repop).

- **Phrase:** Percent chance (1–100)
- **Bindings:** None
- **Notes:** Room-focused trigger. Fires during the area reset cycle. Use to restore room state, respawn environmental objects, or reinitialize puzzles.

```
** Reset the puzzle state
room varset puzzle_state number 0
room alterexit north door closed
room echo The mechanisms click as they reset.
```

---

## Recall Triggers

### recall

Fires when a character recalls from the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = recalling character
- **Notes:** Room-exclusive trigger (only room and token).

### prerecall

Fires before a character attempts to recall from the room. Can prevent recall.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character attempting recall
- **Notes:** Room-exclusive trigger. Use to create no-recall zones.

```
** Prevent recall in the boss room
room echoat $n {RThe dark magic here prevents your escape!{x
break
```

---

## Regeneration Triggers

### regen

Fires on custom regeneration events in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = regenerating character

### regen_hp

Fires when HP regeneration occurs for a character in the room. Can modify the amount.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character, `$register1` = regen amount

### regen_mana

Fires when mana regeneration occurs for a character in the room. Can modify the amount.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character, `$register1` = regen amount

### regen_move

Fires when movement regeneration occurs for a character in the room. Can modify the amount.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character, `$register1` = regen amount

---

## Reckoning Triggers

### prereckoning

Fires before a reckoning world event begins in the room. Can prevent it.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = affected character

### reckoning

Fires when a reckoning world event is active in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = affected character

---

## Quest Triggers

### quest_cancel

Fires when a player cancels a quest while in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = player

### quest_complete

Fires when a player completes a quest while in the room (before rewards are given). Can modify rewards.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = player

### quest_incomplete

Fires when a player turns in an incomplete quest while in the room (before rewards are given). Can modify rewards.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = player

---

## Miscellaneous Triggers

### kill

Fires when a character initiates an attack on another character in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = attacker, `$t` = target

### flee

Fires when a character flees combat in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = fleeing character

### login

Fires when a player logs in while in the room.

- **Phrase:** Direct execution (no phrase)
- **Bindings:** `$n` = player

### clone_extract

Fires when a cloned room is being extracted/destroyed.

- **Phrase:** Percent chance (1–100)
- **Bindings:** None
- **Notes:** Room-exclusive trigger (only room and token). Fires during cleanup of dynamically created rooms.

### xpgain

Fires when a character gains experience points in the room.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character gaining XP

### prebite

Fires before a bite attack in the room. Can prevent it.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = biter

### prerecite

Fires before a character recites a scroll in the room. Can prevent it.

- **Phrase:** Percent chance (1–100)
- **Bindings:** `$n` = character

### verb

Fires when a custom verb command is used in the room.

- **Phrase:** Keyword match (the custom verb keyword)
- **Bindings:** `$n` = character using the verb
- **Notes:** Allows creating custom room commands. Players type `verb <keyword>` or just `<keyword>` to activate.

```
** Custom "pull lever" command
room echo {YYou pull the rusty lever. Gears grind overhead.{x
room alterexit east door open
```

### showcommands

Fires to display available custom verb commands in the room.

- **Phrase:** Direct execution
- **Bindings:** `$n` = character requesting command list
- **Notes:** Paired with `verb` triggers. Output here appears when a player checks available room commands.

---

## Complete Trigger List

All 71 room triggers in alphabetical order:

| # | Trigger | Phrase Type | Category |
|---|---------|-------------|----------|
| 1 | `act` | keyword substring | Action |
| 2 | `afterkill` | percent | Combat |
| 3 | `board` | percent | Item |
| 4 | `bury` | percent | Item |
| 5 | `check_damage` | percent | Damage |
| 6 | `clone_extract` | percent | Death/Repop |
| 7 | `close` | direction/percent | Door |
| 8 | `damage` | percent | Damage |
| 9 | `death` | percent | Death/Repop |
| 10 | `death_protection` | percent | Death/Repop |
| 11 | `death_timer` | percent | Death/Repop |
| 12 | `delay` | percent | Timed |
| 13 | `drink` | percent | Item |
| 14 | `drop` | vnum/keyword | Item |
| 15 | `eat` | percent | Item |
| 16 | `entry` | percent | Movement |
| 17 | `exall` | direction number | Movement |
| 18 | `exit` | direction number | Movement |
| 19 | `extract` | percent | Death/Repop |
| 20 | `fight` | percent | Combat |
| 21 | `flee` | percent | Misc |
| 22 | `get` | percent | Item |
| 23 | `grall` | percent | Movement |
| 24 | `greet` | percent | Movement |
| 25 | `kill` | percent | Misc |
| 26 | `knock` | direction number | Door |
| 27 | `knocking` | direction number | Door |
| 28 | `login` | direct execution | Misc |
| 29 | `move_char` | direction name | Movement |
| 30 | `open` | direction/percent | Door |
| 31 | `preanimate` | percent | Death/Repop |
| 32 | `prebite` | percent | Misc |
| 33 | `predrink` | percent | Item |
| 34 | `preeat` | percent | Item |
| 35 | `preenter` | direction/portal | Movement |
| 36 | `prerecall` | percent | Recall |
| 37 | `prerecite` | percent | Misc |
| 38 | `prereckoning` | percent | Reckoning |
| 39 | `prerest` | percent | Position |
| 40 | `preresurrect` | percent | Death/Repop |
| 41 | `presit` | percent | Position |
| 42 | `presleep` | percent | Position |
| 43 | `prestand` | percent | Position |
| 44 | `prewake` | percent | Position |
| 45 | `prewimpy` | percent | Combat |
| 46 | `quest_cancel` | percent | Quest |
| 47 | `quest_complete` | percent | Quest |
| 48 | `quest_incomplete` | percent | Quest |
| 49 | `random` | percent | Timed |
| 50 | `recall` | percent | Recall |
| 51 | `reckoning` | percent | Reckoning |
| 52 | `regen` | percent | Regen |
| 53 | `regen_hp` | percent | Regen |
| 54 | `regen_mana` | percent | Regen |
| 55 | `regen_move` | percent | Regen |
| 56 | `reset` | percent | Death/Repop |
| 57 | `rest` | percent | Position |
| 58 | `sit` | percent | Position |
| 59 | `sleep` | percent | Position |
| 60 | `speech` | keyword substring | Speech |
| 61 | `spellcast` | exact spell name | Spell |
| 62 | `stand` | percent | Position |
| 63 | `startcombat` | percent | Combat |
| 64 | `throw` | vnum/keyword | Item |
| 65 | `tick` | percent | Timed |
| 66 | `verb` | keyword match | Misc |
| 67 | `wake` | percent | Position |
| 68 | `wear` | percent | Item |
| 69 | `wimpy` | percent | Combat |
| 70 | `xpgain` | percent | Misc |
| 71 | `showcommands` | direct execution | Misc |

### Room-Exclusive Triggers

These triggers are only available to room (and token) programs — not mob or object programs:

- **`knock`** — Door knock in the room
- **`knocking`** — Door knock heard from adjacent room
- **`prerecall`** — Before recall (can prevent)
- **`recall`** — After recall
- **`clone_extract`** — Cloned room being destroyed
- **`reset`** — Area reset cycle (also available to area/instance/dungeon/event)