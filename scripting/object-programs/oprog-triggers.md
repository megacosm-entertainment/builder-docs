---
layout: default
title: Triggers
nav_order: 1
parent: Object Programs
grand_parent: Scripting
---

# Object Program Triggers
{: .no_toc }

This page lists every trigger available for object programs. A trigger tells the game *when* to run your script. Set the trigger with the `trigger` command inside `opedit`, and set the phrase with the `phrase` command.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## How to Read This Reference

Each trigger entry includes:

- **Fires when** — the event that causes the script to run.
- **Phrase** — what the `phrase` field means for this trigger (percent chance, keyword match, direction, etc.).
- **Bindings** — which entity references are available inside the script (`$n`, `$i`, `$(enactor)`, etc.).
- **Example** — a minimal script showing the trigger in action.

For a refresher on entity references and quick codes, see [Variables and Tokens](../variables-and-tokens.md).

---

## Phrase Types at a Glance

| Type | Meaning | Example |
|:-----|:--------|:--------|
| Percent | Random chance (1–100) the script fires | `phrase 25` → 25% chance |
| Keyword | Fires when phrase text is found in the message | `phrase open sesame` |
| Direction | Fires for a specific exit direction (0–5) | `phrase 0` → north |
| Vnum | Fires for a specific object/mobile vnum | `phrase 3001` |
| Spell name | Fires for a specific spell | `phrase fireball` |
| None | Always fires (no phrase needed) | `phrase 100` or leave blank |

---

## Wearing and Equipment

### wear

- **Fires when:** The object is equipped (worn or wielded).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character equipping it.
- **Example:**
```
** Fire every time someone wears this item
obj echoat $n {CThe ring tightens around your finger and glows softly.{x
obj echoaround $n $N's ring begins to glow with an eerie light.
```

### prewear

- **Fires when:** Just before the object is equipped. Can **prevent** wearing.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to wear.
- **Example:**
```
** Only warriors may wield this weapon
if class($n) != warrior
  obj echoat $n {RThe blade burns your hand! You are not worthy.{x
  break
endif
```

### remove

- **Fires when:** The object is removed (unequipped).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character removing it.
- **Example:**
```
obj echoat $n {xThe warmth fades as you remove the amulet.{x
obj stripaffectname $n amulet_blessing
```

### preremove

- **Fires when:** Just before the object is removed. Can **prevent** removal.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to remove.
- **Example:**
```
** Cursed item — cannot be removed
obj echoat $n {RThe ring refuses to come off!{x
break
```

---

## Interaction and Use

### use

- **Fires when:** The object is used (the `use` command).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character using it.
- **Example:**
```
obj echoat $n {GYou activate the healing stone and feel warmth spread through you.{x
obj restore $n
obj selfdestruct
```

### usewith

- **Fires when:** The object is used with another object or entity.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character, `$(obj1)` = first object, `$(obj2)` = second object.
- **Example:**
```
obj echoat $n You combine the two components and they fuse together.
obj oload 3050
obj selfdestruct
```

### examine

- **Fires when:** The object is examined (looked at closely).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character examining.
- **Example:**
```
obj echoat $n {YAs you look closer, you notice faint runes etched along the blade.{x
```

### inspect

- **Fires when:** The object is inspected.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character inspecting.
- **Example:**
```
obj echoat $n You notice the gem has a tiny crack running through its center.
```

### touch

- **Fires when:** The object is touched.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character touching.
- **Example:**
```
obj echoat $n {CThe crystal is ice-cold to the touch.{x
obj echoaround $n $N reaches out and touches the crystal.
```

### pull

- **Fires when:** The object is pulled.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character pulling.
- **Example:**
```
obj echo {YThe lever clicks into position and a hidden door slides open!{x
obj alterexit north flags remove closed
```

### pullon

- **Fires when:** The `pull` command targets this object with arguments.
- **Phrase:** Keyword substring matched against the argument.
- **Bindings:** `$i` = object, `$n` = character, `$(phrase)` = the argument text.
- **Example:**
```
** trigger phrase: lever
obj echo You pull the lever and hear gears turning.
```

### push

- **Fires when:** The object is pushed.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character pushing.
- **Example:**
```
obj echo {xThe stone block slides forward with a grinding sound.{x
```

### pushon

- **Fires when:** The `push` command targets this object with arguments.
- **Phrase:** Keyword substring matched against the argument.
- **Bindings:** `$i` = object, `$n` = character, `$(phrase)` = the argument text.
- **Example:**
```
** trigger phrase: button
obj echo You press the button and the wall rotates!
```

### turn

- **Fires when:** The object is turned.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character turning.
- **Example:**
```
obj echo {YThe dial clicks to the next position.{x
obj varset dial_position 1
```

### turnon

- **Fires when:** The `turn` command targets this object with arguments.
- **Phrase:** Keyword substring matched against the argument.
- **Bindings:** `$i` = object, `$n` = character, `$(phrase)` = the argument text.
- **Example:**
```
** trigger phrase: clockwise
obj echo You turn the dial clockwise. It locks into place.
```

### verb

- **Fires when:** A custom verb is used on the object.
- **Phrase:** Keyword matched against the verb name.
- **Bindings:** `$i` = object, `$n` = character, `$(phrase)` = the verb arguments.
- **Example:**
```
** trigger phrase: polish
obj echoat $n You polish the blade until it gleams.
obj echoaround $n $N carefully polishes $p until it shines.
```

### showcommands

- **Fires when:** The game queries for custom commands available on this object (used to display verb hints).
- **Phrase:** None (always fires).
- **Bindings:** `$i` = object, `$n` = character.
- **Example:**
```
obj showcommand $n polish
obj showcommand $n engrave
```

---

## Picking Up, Dropping, and Throwing

### get

- **Fires when:** The object is picked up.
- **Phrase:** Percent chance (1–100), vnum, or keyword.
- **Bindings:** `$i` = object, `$n` = character picking it up.
- **Example:**
```
obj echoat $n {RAs you pick up the skull, you hear whispers in your mind.{x
obj damage $n 10
```

### preget

- **Fires when:** Just before the object is picked up. Can **prevent** getting.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to pick up.
- **Example:**
```
** Too heavy unless you are strong enough
if str($n) < 20
  obj echoat $n {xYou strain but cannot lift the massive hammer.{x
  break
endif
```

### drop

- **Fires when:** The object is dropped.
- **Phrase:** Percent chance (1–100), vnum, or keyword.
- **Bindings:** `$i` = object, `$n` = character dropping it.
- **Example:**
```
obj echo {YThe orb shatters on impact, releasing a burst of light!{x
obj selfdestruct
```

### predrop

- **Fires when:** Just before the object is dropped. Can **prevent** dropping.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to drop.
- **Example:**
```
obj echoat $n {RThe ring clings to your finger. You cannot let it go.{x
break
```

### throw

- **Fires when:** The object is thrown.
- **Phrase:** Vnum or keyword of the target.
- **Bindings:** `$i` = object, `$n` = thrower, `$(victim)` = target.
- **Example:**
```
obj echoat $n You hurl the flask!
obj damage $(victim) 75
obj selfdestruct
```

---

## Containers

### put

- **Fires when:** An item is placed inside this container object.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = container, `$n` = character, `$(obj1)` = object being placed.
- **Example:**
```
obj echoat $n The bag glows briefly as you place the item inside.
```

### preput

- **Fires when:** Just before an item is placed in this container. Can **prevent** it.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = container, `$n` = character, `$(obj1)` = object being placed.
- **Example:**
```
** Only gems may be placed in this pouch
if objtype($(obj1)) != gem
  obj echoat $n {xThis pouch only accepts precious gems.{x
  break
endif
```

### open

- **Fires when:** The container or door object is opened.
- **Phrase:** Percent chance (1–100) or direction number.
- **Bindings:** `$i` = object, `$n` = character opening it.
- **Example:**
```
obj echo {xThe chest lid creaks open, releasing a puff of ancient dust.{x
```

### close

- **Fires when:** The container or door object is closed.
- **Phrase:** Percent chance (1–100) or direction number.
- **Bindings:** `$i` = object, `$n` = character closing it.
- **Example:**
```
obj echo The lid slams shut and the lock clicks into place.
```

### prehidein

- **Fires when:** Someone tries to hide an object inside this container. Can **prevent** it.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = container, `$n` = character, `$(obj1)` = object being hidden.
- **Example:**
```
obj echoat $n {xThe chest resists your attempt to conceal something inside it.{x
break
```

---

## Consumption

### eat

- **Fires when:** The object (food) is eaten.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character eating.
- **Example:**
```
obj echoat $n {GThe magical fruit fills you with energy!{x
obj restore $n
```

### preeat

- **Fires when:** Just before the object is eaten. Can **prevent** eating.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to eat.
- **Example:**
```
if level($n) < 10
  obj echoat $n {xThis food is too rich for your inexperienced palate.{x
  break
endif
```

### drink

- **Fires when:** The object (drink container) is drunk from.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character drinking.
- **Example:**
```
obj echoat $n {BThe cool water refreshes you completely.{x
obj restore $n
```

### predrink

- **Fires when:** Just before drinking from the object. Can **prevent** drinking.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to drink.
- **Example:**
```
obj echoat $n {RThe liquid inside looks poisonous. Are you sure?{x
```

### recite

- **Fires when:** The object (scroll) is recited.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character reciting.
- **Example:**
```
obj echoat $n {MThe words on the scroll burn away as you speak them.{x
```

### prerecite

- **Fires when:** Just before a scroll is recited. Can **prevent** recitation.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to recite.
- **Example:**
```
if class($n) != mage
  obj echoat $n {xYou cannot decipher the arcane writing.{x
  break
endif
```

### brandish

- **Fires when:** The object (wand/staff) is brandished.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character brandishing.
- **Example:**
```
obj echo {MA bolt of lightning arcs from the staff!{x
```

### zap

- **Fires when:** The object (wand) is zapped.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character zapping.
- **Example:**
```
obj echoat $n The wand crackles with electricity as you point it.
```

### blow

- **Fires when:** The object (horn, whistle, instrument) is blown into.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character blowing.
- **Example:**
```
obj echo {YA deep note echoes across the valley!{x
obj asound A deep horn blast echoes from nearby.
```

---

## Furniture and Position

### sit

- **Fires when:** Someone sits on or in the object (furniture).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object (furniture), `$n` = character sitting.
- **Example:**
```
obj echoat $n You settle into the ornate throne.
```

### presit

- **Fires when:** Just before sitting on the object. Can **prevent** sitting.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to sit.
- **Example:**
```
** Only the guild leader may sit here
if rank($n) != leader
  obj echoat $n {xThis seat is reserved for the guild leader.{x
  break
endif
```

### rest

- **Fires when:** Someone rests on or near the object.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character resting.
- **Example:**
```
obj echoat $n {GThe enchanted bed soothes your aching muscles.{x
```

### prerest

- **Fires when:** Just before resting on the object. Can **prevent** resting.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to rest.
- **Example:**
```
obj echoat $n {xThe bed is already occupied by something invisible.{x
break
```

### sleep

- **Fires when:** Someone sleeps on or in the object.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character sleeping.
- **Example:**
```
obj echoat $n {BDreams of distant lands fill your mind as you drift off...{x
```

### presleep

- **Fires when:** Just before sleeping on the object. Can **prevent** sleeping.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to sleep.
- **Example:**
```
obj echoat $n {xThe bed shakes violently! You cannot sleep here.{x
break
```

### stand

- **Fires when:** Someone stands up from the object.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character standing.
- **Example:**
```
obj echoat $n You rise from the throne, feeling refreshed.
obj stripaffectname $n throne_blessing
```

### prestand

- **Fires when:** Just before standing from the object. Can **prevent** standing.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to stand.
- **Example:**
```
obj echoat $n {RThe chair holds you fast! You cannot stand.{x
break
```

### wake

- **Fires when:** Someone wakes up from the object.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character waking.
- **Example:**
```
obj echoat $n You wake feeling completely rested and alert.
```

### prewake

- **Fires when:** Just before waking from the object. Can **prevent** waking.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to wake.
- **Example:**
```
** Enchanted sleep — must be dispelled
obj echoat $n {MYou try to wake but the enchantment holds you in slumber.{x
break
```

---

## Combat

### fight

- **Fires when:** Each combat round while the object's holder is fighting.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = fighter (holder), `$(victim)` = opponent.
- **Example:**
```
** 15% chance per round to deal bonus fire damage
obj echoat $n {RYour blade erupts in flames!{x
obj echoat $(victim) {R$N's blade sears you with fire!{x
obj damage $(victim) 25
```

### startcombat

- **Fires when:** Combat begins involving the object's holder.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = fighter, `$(victim)` = opponent.
- **Example:**
```
obj echoat $n {YYour shield hums as it detects the threat.{x
```

### afterkill

- **Fires when:** The object's holder kills an opponent.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = killer, `$(victim)` = killed entity.
- **Example:**
```
obj echoat $n {RThe blade drinks deep of your fallen foe's essence.{x
```

### kill

- **Fires when:** The object's holder kills someone (alternate kill trigger).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = killer, `$(victim)` = killed.
- **Example:**
```
obj echoat $n Another soul claimed by the dark blade.
```

### flee

- **Fires when:** The object's holder flees from combat.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = fleeing character.
- **Example:**
```
obj echoat $n {xThe coward's ring pulses — you feel a burst of speed.{x
```

### wimpy

- **Fires when:** The holder's wimpy threshold is triggered.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character.
- **Example:**
```
obj echoat $n {RYour amulet flares, warning you of danger!{x
```

### prewimpy

- **Fires when:** Just before wimpy flee is triggered. Can **prevent** it.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character, `$(victim)` = combatant.
- **Example:**
```
** Berserker gauntlets prevent fleeing
obj echoat $n {RThe gauntlets burn with fury — you WILL NOT flee!{x
break
```

### weapon_blocked

- **Fires when:** This weapon blocks an attack.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object (weapon), `$n` = attacker, `$(victim)` = target.
- **Example:**
```
obj echo {WThe shield flares with divine light, deflecting the blow!{x
```

### weapon_caught

- **Fires when:** This weapon is caught or trapped during combat.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object (weapon), `$n` = attacker, `$(victim)` = target.
- **Example:**
```
obj echoat $n {xYour weapon is caught! You struggle to free it.{x
```

### weapon_parried

- **Fires when:** This weapon parries an attack.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object (weapon), `$n` = attacker, `$(victim)` = target.
- **Example:**
```
obj echo {YSparks fly as the enchanted blade deflects the strike!{x
```

### check_damage

- **Fires when:** Damage is being calculated (allows modification).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = attacker, `$(victim)` = target. Damage value available via registers.
- **Example:**
```
** Double damage against undead
if isundead($(victim))
  obj echoat $n {WThe holy blade burns the undead creature!{x
endif
```

### damage

- **Fires when:** Damage is dealt or received.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = attacker, `$(victim)` = target, `$register1` = damage amount.
- **Example:**
```
** Absorb some damage when hit
obj echoat $n {CYour shield absorbs part of the blow.{x
```

---

## Speech and Communication

### speech

- **Fires when:** Someone speaks in the same room as the object.
- **Phrase:** Keyword substring matched against the spoken message.
- **Bindings:** `$i` = object, `$n` = speaker, `$(phrase)` = full spoken message.
- **Example:**
```
** trigger phrase: open sesame
obj echo {YThe magical chest clicks and its lid swings open!{x
obj alterexit north flags remove closed
```

### sayto

- **Fires when:** Someone speaks directly to or about the object.
- **Phrase:** Keyword substring matched against the message.
- **Bindings:** `$i` = object, `$n` = speaker, `$(phrase)` = message text.
- **Example:**
```
** trigger phrase: activate
obj echoat $n {CThe crystal responds: "Command acknowledged."{x
```

### act

- **Fires when:** An action message is broadcast in the room.
- **Phrase:** Keyword substring matched against the action text.
- **Bindings:** `$i` = object, `$n` = actor, `$(phrase)` = action text.
- **Example:**
```
** trigger phrase: prays
obj echo {WThe altar glows in response to the prayer.{x
```

---

## Movement and Entry

### entry

- **Fires when:** The object enters a room (carried by someone, transferred, etc.).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = carrier (if any), `$(here)` = destination room.
- **Example:**
```
obj echo {MA dark presence enters the room...{x
```

### preenter

- **Fires when:** Just before the object enters a room. Can **prevent** entry.
- **Phrase:** Direction name ("north", "east", etc.) or "portal".
- **Bindings:** `$i` = object, `$n` = carrier.
- **Example:**
```
** Object cannot enter holy ground
obj echoat $n {RThe cursed artifact resists entering this place!{x
break
```

### greet

- **Fires when:** A character enters the room where the object is (character must be visible).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = arriving character, `$(here)` = room.
- **Example:**
```
obj echo {YThe crystal pulses as a new presence is detected.{x
```

### grall

- **Fires when:** A character enters the room (fires regardless of visibility).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = arriving character, `$(here)` = room.
- **Example:**
```
obj echo {xSomething stirs in the darkness...{x
```

### exit

- **Fires when:** A character leaves the room in a specific direction.
- **Phrase:** Direction number (0=north, 1=east, 2=south, 3=west, 4=up, 5=down).
- **Bindings:** `$i` = object, `$n` = departing character, `$(here)` = room.
- **Example:**
```
** trigger phrase: 0 (north)
obj echo The statue's eyes follow the departing figure.
```

### exall

- **Fires when:** A character attempts to leave in a direction (regardless of position or sight).
- **Phrase:** Direction number (0–5).
- **Bindings:** `$i` = object, `$n` = departing character.
- **Example:**
```
** trigger phrase: 4 (up)
obj echoat $n The ceiling is too low to go that way!
break
```

### board

- **Fires when:** Someone boards the object (vehicle, ship, mount).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character boarding.
- **Example:**
```
obj echoat $n You climb aboard the ship.
obj echo $N climbs aboard.
```

---

## Hiding

### hide

- **Fires when:** The object is hidden.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character hiding it.
- **Example:**
```
obj echoat $n You tuck the dagger out of sight.
```

### prehide

- **Fires when:** Just before the object is hidden. Can **prevent** hiding.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to hide it.
- **Example:**
```
** Glowing objects cannot be hidden
obj echoat $n {xThe object glows too brightly to conceal.{x
break
```

### bury

- **Fires when:** The object is buried.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character burying it.
- **Example:**
```
obj echoat $n {xYou bury the bones. May they rest in peace.{x
```

---

## Timed and Periodic

### tick

- **Fires when:** Every game tick (periodic heartbeat).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$(here)` = room.
- **Example:**
```
** 10% chance per tick to emit a sound
obj echo {xThe clock chimes softly.{x
```

### random

- **Fires when:** On periodic random checks (similar to tick).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$(here)` = room.
- **Example:**
```
** 5% chance of ambient effect
obj echo {cThe enchanted pool ripples on its own.{x
```

### delay

- **Fires when:** A previously set delay timer completes (set with `obj delay`).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object.
- **Example:**
```
** This runs after obj delay was called in another trigger
obj echo {RThe bomb explodes!{x
obj damage $n 100
obj selfdestruct
```

### regen

- **Fires when:** During regeneration processing for the object or its holder.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = holder (if held/worn).
- **Example:**
```
obj echoat $n {GThe ring pulses gently, mending your wounds.{x
```

### regen_hp

- **Fires when:** HP regeneration is calculated for the holder.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = holder, `$register1` = regen amount.
- **Example:**
```
** Boost HP regeneration by showing a message
obj echoat $n {GThe healing amulet glows as your wounds close.{x
```

### regen_mana

- **Fires when:** Mana regeneration is calculated for the holder.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = holder, `$register1` = regen amount.
- **Example:**
```
obj echoat $n {BThe staff channels magical energy into you.{x
```

### regen_move

- **Fires when:** Movement regeneration is calculated for the holder.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = holder, `$register1` = regen amount.
- **Example:**
```
obj echoat $n {GThe boots invigorate your tired legs.{x
```

### grow

- **Fires when:** A seed or plant object grows.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object.
- **Example:**
```
obj echo {GThe seedling sprouts and grows into a small bush!{x
```

---

## Lifecycle

### repop

- **Fires when:** The object is reset/repopulated by the area reset system.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$(here)` = room.
- **Example:**
```
obj echo {xThe treasure chest refills with gold.{x
```

### extract

- **Fires when:** The object is being extracted (destroyed/removed from game).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object.
- **Example:**
```
obj echo {MA flash of light marks the artifact's destruction.{x
```

### save

- **Fires when:** The object is saved (usually when the player saves).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object.
- **Example:**
```
** Update a timestamp variable on save
obj varset last_saved $T
```

### login

- **Fires when:** A player logs in while carrying this object.
- **Phrase:** None (always fires).
- **Bindings:** `$i` = object, `$n` = player logging in.
- **Example:**
```
obj echoat $n {CYour enchanted compass spins to life as you enter the world.{x
```

### preanimate

- **Fires when:** Just before the object is animated (turned into a mobile).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object.
- **Example:**
```
obj echo {xThe golem's eyes begin to flicker with dim light.{x
```

### preresurrect

- **Fires when:** Just before the object is resurrected or reactivated.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object.
- **Example:**
```
obj echo {WDivine power courses through the relic.{x
```

---

## Spell-Related

### spellcast

- **Fires when:** A spell is cast near or on the object.
- **Phrase:** Exact spell name (e.g., `fireball`).
- **Bindings:** `$i` = object, `$n` = caster, `$(victim)` = spell target.
- **Example:**
```
** trigger phrase: identify
obj echoat $n {YThe artifact resists your identify spell and shows false information!{x
break
```

### spell_dispel

- **Fires when:** A dispel spell targets the object.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = caster, `$(victim)` = target.
- **Example:**
```
obj echo {MThe enchantments on the artifact waver but hold firm.{x
break
```

---

## Quest-Related

### quest_complete

- **Fires when:** A quest is completed while this object is held. Fires before rewards.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = quest completer.
- **Example:**
```
obj echoat $n {YThe quest token transforms into a golden badge!{x
obj stringobj $i short a golden badge of valor
```

### quest_cancel

- **Fires when:** A quest is cancelled while this object is held.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character cancelling quest.
- **Example:**
```
obj echoat $n {xThe quest scroll crumbles to dust.{x
obj selfdestruct
```

### quest_incomplete

- **Fires when:** A quest is turned in incomplete while this object is held.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character.
- **Example:**
```
obj echoat $n {xThe token dims — the quest is not yet finished.{x
```

---

## Catalyst and Alchemy

### catalyst

- **Fires when:** The object is used as an alchemy catalyst.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = alchemist.
- **Example:**
```
obj echoat $n {MThe crystal resonates with alchemical energy.{x
```

### catalystfull

- **Fires when:** The catalyst container is fully charged.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = alchemist.
- **Example:**
```
obj echoat $n {YThe catalyst vessel overflows with power!{x
```

### catalystsrc

- **Fires when:** The object is used as a catalyst source.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = alchemist.
- **Example:**
```
obj echoat $n {xThe source crystal pulses and transfers its energy.{x
```

---

## Identification and Lore

### identify

- **Fires when:** The object is identified (via spell or skill).
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character identifying.
- **Example:**
```
obj echoat $n {CYou sense something hidden about this artifact...{x
```

### lore

- **Fires when:** Lore information is gathered about the object.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character using lore.
- **Example:**
```
obj echoat $n {YAncient texts speak of this blade — it was forged in dragonfire.{x
```

### lorex

- **Fires when:** Extended lore is obtained about the object.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character using lore.
- **Example:**
```
obj echoat $n {YThe blade's true name is Dawnrender. It hungers for undead.{x
```

---

## Experience and Reckoning

### xpgain

- **Fires when:** The object's holder gains experience points.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character gaining XP.
- **Example:**
```
obj echoat $n {YThe learning gem absorbs some of your experience.{x
```

### reckoning

- **Fires when:** A reckoning event occurs.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$(here)` = room.
- **Example:**
```
obj echo {RThe relic shudders as the reckoning wave passes.{x
```

### prereckoning

- **Fires when:** Just before reckoning processing. Can **prevent** reckoning effects.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$(here)` = room.
- **Example:**
```
** Object protects the room from reckoning
obj echo {WThe ward flares, shielding this place from the reckoning!{x
break
```

---

## Miscellaneous

### prebite

- **Fires when:** Just before biting or attacking the object. Can **prevent** it.
- **Phrase:** Percent chance (1–100).
- **Bindings:** `$i` = object, `$n` = character attempting to bite.
- **Example:**
```
obj echoat $n {xThe surface is too hard to bite!{x
break
```

---

## Trigger Quick Reference

Alphabetical list of all 100 object program triggers:

| Trigger | Phrase Type | Category |
|:--------|:-----------|:---------|
| `act` | Keyword | Speech |
| `afterkill` | Percent | Combat |
| `blow` | Percent | Consumption |
| `board` | Percent | Movement |
| `brandish` | Percent | Consumption |
| `bury` | Percent | Hiding |
| `catalyst` | Percent | Alchemy |
| `catalystfull` | Percent | Alchemy |
| `catalystsrc` | Percent | Alchemy |
| `check_damage` | Percent | Combat |
| `close` | Percent / Direction | Container |
| `damage` | Percent | Combat |
| `delay` | Percent | Timed |
| `drink` | Percent | Consumption |
| `drop` | Percent / Vnum | Inventory |
| `eat` | Percent | Consumption |
| `entry` | Percent | Movement |
| `exall` | Direction | Movement |
| `examine` | Percent | Interaction |
| `exit` | Direction | Movement |
| `extract` | Percent | Lifecycle |
| `fight` | Percent | Combat |
| `flee` | Percent | Combat |
| `get` | Percent / Vnum | Inventory |
| `grall` | Percent | Movement |
| `greet` | Percent | Movement |
| `grow` | Percent | Timed |
| `hide` | Percent | Hiding |
| `identify` | Percent | Lore |
| `inspect` | Percent | Interaction |
| `kill` | Percent | Combat |
| `login` | None | Lifecycle |
| `lore` | Percent | Lore |
| `lorex` | Percent | Lore |
| `open` | Percent / Direction | Container |
| `preanimate` | Percent | Lifecycle |
| `prebite` | Percent | Misc |
| `predrink` | Percent | Consumption |
| `predrop` | Percent | Inventory |
| `preeat` | Percent | Consumption |
| `preenter` | Direction name | Movement |
| `preget` | Percent | Inventory |
| `prehide` | Percent | Hiding |
| `prehidein` | Percent | Container |
| `preput` | Percent | Container |
| `prerecite` | Percent | Consumption |
| `prereckoning` | Percent | Reckoning |
| `preremove` | Percent | Equipment |
| `prerest` | Percent | Furniture |
| `preresurrect` | Percent | Lifecycle |
| `presit` | Percent | Furniture |
| `presleep` | Percent | Furniture |
| `prestand` | Percent | Furniture |
| `prewake` | Percent | Furniture |
| `prewear` | Percent | Equipment |
| `prewimpy` | Percent | Combat |
| `pull` | Percent | Interaction |
| `pullon` | Keyword | Interaction |
| `push` | Percent | Interaction |
| `pushon` | Keyword | Interaction |
| `put` | Percent | Container |
| `quest_cancel` | Percent | Quest |
| `quest_complete` | Percent | Quest |
| `quest_incomplete` | Percent | Quest |
| `random` | Percent | Timed |
| `recite` | Percent | Consumption |
| `reckoning` | Percent | Reckoning |
| `regen` | Percent | Timed |
| `regen_hp` | Percent | Timed |
| `regen_mana` | Percent | Timed |
| `regen_move` | Percent | Timed |
| `remove` | Percent | Equipment |
| `repop` | Percent | Lifecycle |
| `rest` | Percent | Furniture |
| `save` | Percent | Lifecycle |
| `sayto` | Keyword | Speech |
| `showcommands` | None | Interaction |
| `sit` | Percent | Furniture |
| `sleep` | Percent | Furniture |
| `speech` | Keyword | Speech |
| `spell_dispel` | Percent | Spell |
| `spellcast` | Spell name | Spell |
| `stand` | Percent | Furniture |
| `startcombat` | Percent | Combat |
| `throw` | Vnum / Keyword | Inventory |
| `tick` | Percent | Timed |
| `touch` | Percent | Interaction |
| `turn` | Percent | Interaction |
| `turnon` | Keyword | Interaction |
| `use` | Percent | Interaction |
| `usewith` | Percent | Interaction |
| `verb` | Keyword | Interaction |
| `wake` | Percent | Furniture |
| `weapon_blocked` | Percent | Combat |
| `weapon_caught` | Percent | Combat |
| `weapon_parried` | Percent | Combat |
| `wear` | Percent | Equipment |
| `wimpy` | Percent | Combat |
| `xpgain` | Percent | Experience |
| `zap` | Percent | Consumption |