---
layout: default
title: Triggers
nav_order: 1
parent: Token Programs
grand_parent: Scripting
---

# Token Program Triggers
{: .no_toc }

Tokens have **206 triggers** — the largest set of any entity type. This means token scripts can react to nearly every event in the game: combat, movement, speech, spells, death, quests, login/logout, and more.

Triggers marked with {: .label .label-purple } **Token-exclusive** are only available on tokens — no other entity type can use them.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Phrase Matching

Each trigger has a **phrase** that determines when it fires. The phrase type depends on the trigger:

| Phrase Type | How It Works | Example |
|:------------|:-------------|:--------|
| **Percent chance** | Number 1–100. Fires with that probability each time the event occurs. | `50` = 50% chance |
| **Keyword substring** | Text matched as substring in the triggering message. `*` matches all. | `hello` matches "say hello there" |
| **Exact name match** | Must exactly match the triggering name (case-insensitive). | `smile` matches only the "smile" emote |
| **Keyword match** | Matched against the custom verb keyword. | `meditate` |
| **Vnum/name match** | Matches object vnum or keyword. `all` or `*` matches any. | `3001` or `sword` |
| **Gold amount** | Fires when bribe amount ≥ the threshold value. | `100` fires for 100+ gold |
| **HP% threshold** | Fires when character's HP% drops below the value. | `25` fires at < 25% HP |
| **Direction** | Direction number (0=north, 1=east, 2=south, 3=west, 4=up, 5=down). | `0` for north |
| **Direct execution** | Always fires when the event occurs (no phrase needed). | — |

---

## Entity Bindings

When a token script runs, these context variables are available:

| Variable | Aliases | Type | Description |
|:---------|:--------|:-----|:------------|
| `$self` | `$this`, `$i` | token | The token running the script |
| `$enactor` | `$n` | mobile | The character who triggered the event |
| `$victim` | `$t` | mobile | The victim/target of the action |
| `$random` | `$r` | mobile | A random character in the room |
| `$obj1` | `$p` | object | Primary object involved |
| `$obj2` | — | object | Secondary object involved |
| `$token` | `$q` | token | Token parameter (if applicable) |
| `$here` | — | room | The current room |
| `$phrase` | — | string | The matched phrase text |
| `$trigger` | — | string | The trigger phrase pattern |
| `$register1`–`$register5` | — | number | Numeric data registers |

---

## Token-Exclusive Triggers

These 20 triggers are **only available on tokens**. They are the key reason tokens are so powerful — no other entity type can react to these events.

| Trigger | Category | Description |
|:--------|:---------|:------------|
| `afterdeath` | Lifecycle | Fires just after the token owner dies |
| `caninterrupt` | Interrupt | Check if the token can interrupt an action |
| `combatstyle` | Combat Style | Select combat style for the token |
| `emoteself` | Action | Self-targeted emote trigger |
| `expire` | Lifecycle | Fires when the token's timer reaches zero |
| `interrupt` | Interrupt | Handle action interruption |
| `practicetoken` | General | Fires when practicing with a token-defined skill |
| `precast` | General | Pre-casting hook before spell begins |
| `prepracticetoken` | General | Pre-practice check for token-defined skills |
| `prespell` | Spell | Pre-spell check before token spell fires |
| `pretraintoken` | General | Pre-training check for token-defined skills |
| `quit` | General | Fires when the token owner disconnects |
| `spell` | Spell | Token spell casting execution |
| `spellbeat` | Spell | Fires each round during token spell casting |
| `spellinter` | Spell | Token spell interruption handling |
| `spellpenetrate` | Spell | Token spell penetration check |
| `stripaffect` | Lifecycle | Fires when affects are stripped from the token |
| `token_given` | Lifecycle | Fires when this token is given to an entity |
| `token_removed` | Lifecycle | Fires when this token is removed from an entity |
| `verbself` | Verb | Self-targeted custom verb trigger |

---

## General Triggers (Slot 0)

General triggers cover a wide range of game actions — from basic interactions to quest events, position changes, and skill use.

### act

- **Phrase:** keyword substring
- **Fires:** When a message matching the phrase appears in the token owner's room.
- **Bindings:** `$enactor` = actor, `$phrase` = matched text
- **Also on:** mob, obj, room

### board

- **Phrase:** percent chance
- **Fires:** When a character boards a ship or transport.
- **Bindings:** `$enactor` = boarding character
- **Also on:** mob, obj, room

### brandish

- **Phrase:** percent chance
- **Fires:** When a character brandishes a staff or wand.
- **Bindings:** `$enactor` = character, `$obj1` = brandished item
- **Also on:** obj

### bribe

- **Phrase:** gold amount ≥ threshold
- **Fires:** When someone gives gold to the token owner (mob only) and the amount meets the threshold.
- **Bindings:** `$enactor` = giver
- **Also on:** mob

### bury

- **Phrase:** percent chance
- **Fires:** When an object is buried.
- **Bindings:** `$enactor` = character, `$obj1` = buried object
- **Also on:** obj, room

### buy

- **Phrase:** percent chance
- **Fires:** After a purchase is completed at a shop.
- **Bindings:** `$enactor` = buyer, `$obj1` = purchased item
- **Also on:** mob

### check_buyer

- **Phrase:** percent chance
- **Fires:** Before a purchase — allows the script to approve or deny the sale.
- **Bindings:** `$enactor` = potential buyer, `$obj1` = item being purchased
- **Also on:** mob

### clone_extract

- **Phrase:** percent chance
- **Fires:** When a cloned room created by this token is being extracted/destroyed.
- **Bindings:** `$here` = room being extracted
- **Also on:** room

### close

- **Phrase:** direction number or percent
- **Fires:** When a door or container is closed.
- **Bindings:** `$enactor` = character, direction or `$obj1` = container
- **Also on:** obj, room

### contract_complete

- **Phrase:** percent chance
- **Fires:** When a contract is completed.
- **Bindings:** `$enactor` = character completing the contract
- **Also on:** mob

### custom_price

- **Phrase:** percent chance
- **Fires:** Allows the script to set a custom price for an item in a shop.
- **Bindings:** `$enactor` = shopper, `$obj1` = item being priced
- **Also on:** mob

### delay

- **Phrase:** percent chance
- **Fires:** After a previously set delay timer expires (set with the `delay` command on mob/obj/room).
- **Bindings:** `$enactor` = entity that set the delay
- **Also on:** mob, obj, room

### drink

- **Phrase:** percent chance
- **Fires:** When a character drinks from a fountain or container.
- **Bindings:** `$enactor` = drinker, `$obj1` = drink source
- **Also on:** mob, obj, room

### drop

- **Phrase:** vnum/name match or percent
- **Fires:** When an object is dropped.
- **Bindings:** `$enactor` = character, `$obj1` = dropped object
- **Also on:** mob, obj, room

### eat

- **Phrase:** percent chance
- **Fires:** When a character eats food.
- **Bindings:** `$enactor` = eater, `$obj1` = food item
- **Also on:** mob, obj, room

### examine

- **Phrase:** percent chance
- **Fires:** When a character examines an object.
- **Bindings:** `$enactor` = examiner, `$obj1` = examined object
- **Also on:** mob, obj

### flee

- **Phrase:** percent chance
- **Fires:** When a character flees from combat.
- **Bindings:** `$enactor` = fleeing character
- **Also on:** mob, obj, room

### forcedismount

- **Phrase:** percent chance
- **Fires:** When a character is forcibly dismounted.
- **Bindings:** `$enactor` = dismounted character
- **Also on:** mob

### get

- **Phrase:** vnum/name match or percent
- **Fires:** When an object is picked up.
- **Bindings:** `$enactor` = character, `$obj1` = retrieved object
- **Also on:** mob, obj, room

### give

- **Phrase:** vnum/name match
- **Fires:** When an object is given to someone.
- **Bindings:** `$enactor` = giver, `$victim` = receiver, `$obj1` = given object
- **Also on:** mob

### grouped

- **Phrase:** percent chance
- **Fires:** When a character joins a group.
- **Bindings:** `$enactor` = grouped character
- **Also on:** mob

### hidden

- **Phrase:** percent chance
- **Fires:** When a hidden character is detected/revealed.
- **Bindings:** `$enactor` = revealed character
- **Also on:** mob

### hide

- **Phrase:** percent chance
- **Fires:** When a character attempts to hide.
- **Bindings:** `$enactor` = hiding character
- **Also on:** mob, obj

### hpcnt

- **Phrase:** HP% threshold (fires when below)
- **Fires:** During combat when the token owner's HP% drops below the threshold.
- **Bindings:** `$enactor` = fighting character
- **Also on:** mob

### identify

- **Phrase:** percent chance
- **Fires:** When an identify spell is cast on the token's host object.
- **Bindings:** `$enactor` = caster, `$obj1` = identified object
- **Also on:** obj

### inspect

- **Phrase:** percent chance
- **Fires:** When a character inspects an entity.
- **Bindings:** `$enactor` = inspector
- **Also on:** mob, obj

### inspect_custom

- **Phrase:** exact keyword match
- **Fires:** When a character uses a custom inspect keyword.
- **Bindings:** `$enactor` = inspector, `$phrase` = keyword
- **Also on:** mob

### kill

- **Phrase:** percent chance
- **Fires:** When a character initiates an attack.
- **Bindings:** `$enactor` = attacker, `$victim` = target
- **Also on:** mob, obj, room

### knock / knocking

- **Phrase:** direction number
- **Fires:** When a character knocks on a door. `knock` fires on the knocker's side; `knocking` fires on the other side.
- **Bindings:** `$enactor` = knocker
- **Also on:** room

### level

- **Phrase:** percent chance
- **Fires:** When a character gains a level.
- **Bindings:** `$enactor` = leveled character
- **Also on:** mob

### login

- **Phrase:** direct execution
- **Fires:** When the token owner logs into the game. Always fires (no phrase match needed).
- **Bindings:** `$enactor` = logging-in character
- **Also on:** mob, obj, room

### lore / lorex

- **Phrase:** percent chance
- **Fires:** When a lore check is performed on an item. `lorex` provides extended lore information.
- **Bindings:** `$enactor` = character, `$obj1` = lored item
- **Also on:** mob, obj

### moon

- **Phrase:** percent chance
- **Fires:** On moon phase changes.
- **Bindings:** (none specific)
- **Token-exclusive context** — only tokens respond to moon phases by default.

### mount

- **Phrase:** percent chance
- **Fires:** When a character mounts a creature.
- **Bindings:** `$enactor` = rider, `$victim` = mount
- **Also on:** mob

### multiclass

- **Phrase:** percent chance
- **Fires:** When a character multiclasses.
- **Bindings:** `$enactor` = multiclassing character
- **Also on:** mob

### open

- **Phrase:** direction number or percent
- **Fires:** When a door or container is opened.
- **Bindings:** `$enactor` = character, direction or `$obj1` = container
- **Also on:** obj, room

### postquest

- **Phrase:** percent chance
- **Fires:** After a quest is completed (post-processing).
- **Bindings:** `$enactor` = quest completer
- **Also on:** mob

### practice

- **Phrase:** percent chance
- **Fires:** When a character practices a skill.
- **Bindings:** `$enactor` = practicing character
- **Also on:** mob

### practicetoken
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** When a character practices a token-defined skill.
- **Bindings:** `$enactor` = practicing character

### precast
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** Before a token spell begins casting. Returning failure cancels the cast.
- **Bindings:** `$enactor` = caster

### prebite

- **Phrase:** percent chance
- **Fires:** Before a vampire bite attack. Returning failure prevents the bite.
- **Bindings:** `$enactor` = biter, `$victim` = target
- **Also on:** mob, obj, room

### prebuy / prebuy_obj

- **Phrase:** percent chance
- **Fires:** Before a purchase. `prebuy` checks the buyer; `prebuy_obj` checks the object. Returning failure cancels the purchase.
- **Bindings:** `$enactor` = buyer, `$obj1` = item
- **Also on:** mob

### predeath

- **Phrase:** percent chance
- **Fires:** Before death processing. Can prevent death by returning failure.
- **Bindings:** `$enactor` = dying character
- **Also on:** mob

### predismount

- **Phrase:** percent chance
- **Fires:** Before dismounting. Returning failure prevents the dismount.
- **Bindings:** `$enactor` = rider
- **Also on:** mob

### predrink

- **Phrase:** percent chance
- **Fires:** Before drinking. Returning failure prevents the drink.
- **Bindings:** `$enactor` = drinker, `$obj1` = drink source
- **Also on:** mob, obj, room

### predrop

- **Phrase:** percent chance
- **Fires:** Before dropping an object. Returning failure prevents the drop.
- **Bindings:** `$enactor` = character, `$obj1` = object
- **Also on:** obj

### preeat

- **Phrase:** percent chance
- **Fires:** Before eating. Returning failure prevents the eat.
- **Bindings:** `$enactor` = eater, `$obj1` = food item
- **Also on:** mob, obj, room

### preflee

- **Phrase:** percent chance
- **Fires:** Before fleeing combat. Returning failure prevents the flee.
- **Bindings:** `$enactor` = fleeing character
- **Also on:** mob

### preget

- **Phrase:** percent chance
- **Fires:** Before picking up an object. Returning failure prevents the get.
- **Bindings:** `$enactor` = character, `$obj1` = object
- **Also on:** obj

### prehide / prehidein

- **Phrase:** percent chance
- **Fires:** Before hiding. `prehidein` fires when hiding inside a container. Returning failure prevents the action.
- **Bindings:** `$enactor` = hiding character
- **Also on:** mob, obj

### prekill

- **Phrase:** percent chance
- **Fires:** Before an attack is initiated. Returning failure prevents the attack.
- **Bindings:** `$enactor` = attacker, `$victim` = target
- **Also on:** mob

### premount

- **Phrase:** percent chance
- **Fires:** Before mounting. Returning failure prevents the mount.
- **Bindings:** `$enactor` = rider, `$victim` = mount
- **Also on:** mob

### prepractice / prepracticeother / prepracticethat

- **Phrase:** percent chance
- **Fires:** Before practicing a skill. `prepracticeother` fires on the trainer's token; `prepracticethat` fires when practicing a specific skill. Returning failure prevents the practice.
- **Bindings:** `$enactor` = practicing character
- **Also on:** mob

### prepracticetoken
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** Before practicing a token-defined skill. Returning failure prevents the practice.
- **Bindings:** `$enactor` = practicing character

### preput

- **Phrase:** percent chance
- **Fires:** Before putting an object into a container. Returning failure prevents the put.
- **Bindings:** `$enactor` = character, `$obj1` = object, `$obj2` = container
- **Also on:** obj

### prequest

- **Phrase:** percent chance
- **Fires:** Before accepting a quest. Returning failure prevents the quest from starting.
- **Bindings:** `$enactor` = character
- **Also on:** mob

### prerecall

- **Phrase:** percent chance
- **Fires:** Before recalling. Returning failure prevents the recall.
- **Bindings:** `$enactor` = recalling character
- **Also on:** room

### prerecite

- **Phrase:** percent chance
- **Fires:** Before reciting a scroll. Returning failure prevents the recite.
- **Bindings:** `$enactor` = character, `$obj1` = scroll
- **Also on:** obj, room

### prerehearse

- **Phrase:** percent chance
- **Fires:** Before rehearsing a bard song. Returning failure prevents the rehearsal.
- **Bindings:** `$enactor` = bard
- **Also on:** mob

### preremove

- **Phrase:** percent chance
- **Fires:** Before removing (unequipping) an item. Returning failure prevents the remove.
- **Bindings:** `$enactor` = character, `$obj1` = item
- **Also on:** obj

### prerenew

- **Phrase:** percent chance
- **Fires:** Before renewing a subscription or service. Returning failure prevents the renewal.
- **Bindings:** `$enactor` = character
- **Also on:** mob

### prerest

- **Phrase:** percent chance
- **Fires:** Before resting. Returning failure prevents the rest.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### presell

- **Phrase:** percent chance
- **Fires:** Before selling an item. Returning failure prevents the sale.
- **Bindings:** `$enactor` = seller, `$obj1` = item
- **Also on:** mob

### presit

- **Phrase:** percent chance
- **Fires:** Before sitting. Returning failure prevents the sit.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### presleep

- **Phrase:** percent chance
- **Fires:** Before sleeping. Returning failure prevents sleep.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### prestand

- **Phrase:** percent chance
- **Fires:** Before standing. Returning failure prevents the stand.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### pretrain

- **Phrase:** percent chance
- **Fires:** Before training a stat. Returning failure prevents the training.
- **Bindings:** `$enactor` = character
- **Also on:** mob

### pretraintoken
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** Before training a token-defined skill. Returning failure prevents the training.
- **Bindings:** `$enactor` = character

### prewake

- **Phrase:** percent chance
- **Fires:** Before waking up. Returning failure prevents waking.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### prewear

- **Phrase:** percent chance
- **Fires:** Before wearing/equipping an item. Returning failure prevents equipping.
- **Bindings:** `$enactor` = character, `$obj1` = item
- **Also on:** obj

### pull / pullon

- **Phrase:** percent (pull) or keyword substring (pullon)
- **Fires:** When a character pulls an object or lever. `pullon` matches against the target keyword.
- **Bindings:** `$enactor` = character, `$obj1` = pulled object
- **Also on:** mob, obj

### push / pushon

- **Phrase:** percent (push) or keyword substring (pushon)
- **Fires:** When a character pushes an object. `pushon` matches against the target keyword.
- **Bindings:** `$enactor` = character, `$obj1` = pushed object
- **Also on:** obj

### put

- **Phrase:** percent chance
- **Fires:** When an object is placed into a container.
- **Bindings:** `$enactor` = character, `$obj1` = object, `$obj2` = container
- **Also on:** obj

### quest_cancel / quest_complete / quest_incomplete / quest_part

- **Phrase:** percent chance
- **Fires:** On quest lifecycle events: cancellation, completion, failure, or partial progress.
- **Bindings:** `$enactor` = character involved in the quest
- **Also on:** mob, obj, room (except quest_part: mob only)

### quit
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** When the token owner logs out or disconnects. Useful for cleanup, save-state, or goodbye messages.
- **Bindings:** `$enactor` = quitting character

### recall

- **Phrase:** percent chance
- **Fires:** When a character recalls to their home room.
- **Bindings:** `$enactor` = recalling character
- **Also on:** room

### recite

- **Phrase:** percent chance
- **Fires:** When a character recites a scroll.
- **Bindings:** `$enactor` = character, `$obj1` = scroll
- **Also on:** obj

### remort

- **Phrase:** percent chance
- **Fires:** When a character remorts (rebirths at a higher tier).
- **Bindings:** `$enactor` = remorting character
- **Also on:** mob

### remove

- **Phrase:** percent chance
- **Fires:** When an item is unequipped.
- **Bindings:** `$enactor` = character, `$obj1` = removed item
- **Also on:** obj

### renew / renew_list

- **Phrase:** percent chance
- **Fires:** When a shop's stock is renewed. `renew_list` fires for the full restock list.
- **Bindings:** `$enactor` = shopkeeper
- **Also on:** mob

### rest

- **Phrase:** percent chance
- **Fires:** When a character rests.
- **Bindings:** `$enactor` = resting character
- **Also on:** mob, obj, room

### restocked

- **Phrase:** percent chance
- **Fires:** When a shop is restocked.
- **Bindings:** `$enactor` = shopkeeper
- **Also on:** mob

### sit

- **Phrase:** percent chance
- **Fires:** When a character sits down.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### skill_berserk / skill_sneak / skill_warcry

- **Phrase:** percent chance
- **Fires:** When the respective skill is used. Each reacts to a specific combat skill activation.
- **Bindings:** `$enactor` = skill user
- **Also on:** mob

### sleep

- **Phrase:** percent chance
- **Fires:** When a character falls asleep.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### stand

- **Phrase:** percent chance
- **Fires:** When a character stands up.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### throw

- **Phrase:** vnum/name match
- **Fires:** When an object is thrown.
- **Bindings:** `$enactor` = thrower, `$obj1` = thrown object, `$victim` = target
- **Also on:** mob, obj, room

### touch

- **Phrase:** percent chance
- **Fires:** When a character touches an object.
- **Bindings:** `$enactor` = character, `$obj1` = touched object
- **Also on:** obj

### turn / turnon

- **Phrase:** percent (turn) or keyword substring (turnon)
- **Fires:** When a character turns an object (like a dial or valve).
- **Bindings:** `$enactor` = character, `$obj1` = turned object
- **Also on:** obj

### ungrouped

- **Phrase:** percent chance
- **Fires:** When a character leaves a group.
- **Bindings:** `$enactor` = ungrouped character
- **Also on:** mob

### use / usewith

- **Phrase:** percent chance
- **Fires:** When an object is used. `usewith` fires when used in conjunction with another object.
- **Bindings:** `$enactor` = character, `$obj1` = used object, `$obj2` = target (usewith)
- **Also on:** obj

### wake

- **Phrase:** percent chance
- **Fires:** When a character wakes up.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### wear

- **Phrase:** percent chance
- **Fires:** When an item is equipped.
- **Bindings:** `$enactor` = character, `$obj1` = worn item
- **Also on:** mob, obj, room

### xpgain

- **Phrase:** percent chance
- **Fires:** When a character gains experience points.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### zap

- **Phrase:** percent chance
- **Fires:** When a character zaps a wand.
- **Bindings:** `$enactor` = character, `$obj1` = wand
- **Also on:** obj

---

## Speech Triggers (Slot 1)

Speech triggers react to characters speaking, saying things to the token owner, or whispering.

### sayto

- **Phrase:** keyword substring
- **Fires:** When someone says something directly to the token's owner using `say to <name> <message>`.
- **Bindings:** `$enactor` = speaker, `$phrase` = spoken text
- **Also on:** mob, obj

### speech

- **Phrase:** keyword substring
- **Fires:** When someone speaks in the room and the phrase matches. Use `*` to match all speech.
- **Bindings:** `$enactor` = speaker, `$phrase` = spoken text
- **Also on:** mob, obj, room

### whisper

- **Phrase:** keyword substring
- **Fires:** When someone whispers and the phrase matches.
- **Bindings:** `$enactor` = whisperer, `$victim` = target, `$phrase` = whispered text
- **Also on:** mob

---

## Random / Timed Triggers (Slot 2)

These triggers fire on periodic game ticks or regeneration cycles.

### grow

- **Phrase:** percent chance
- **Fires:** On object growth cycles (e.g., plants growing).
- **Bindings:** `$obj1` = growing object
- **Also on:** obj

### hitgain / managain / movegain

- **Phrase:** percent chance
- **Fires:** During HP/mana/movement regeneration for the token owner.
- **Bindings:** `$enactor` = regenerating character
- **Also on:** mob

### prereckoning

- **Phrase:** percent chance
- **Fires:** Before a reckoning world event begins. Returning failure can prevent it.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### pulse

- **Phrase:** percent chance
- **Fires:** Every game pulse (fast tick, ~4 seconds). Use sparingly — fires very frequently.
- **Bindings:** `$enactor` = token owner
- **Also on:** mob

### random

- **Phrase:** percent chance
- **Fires:** Every game tick with the specified probability. The classic periodic check trigger.
- **Bindings:** `$enactor` = token owner
- **Also on:** mob, obj, room

### reckoning

- **Phrase:** percent chance
- **Fires:** When a reckoning world event is active.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### regen / regen_hp / regen_mana / regen_move

- **Phrase:** percent chance
- **Fires:** During regeneration. `regen` is general; the specific variants fire for HP, mana, or movement only.
- **Bindings:** `$enactor` = regenerating character
- **Also on:** mob, obj, room

### tick

- **Phrase:** percent chance
- **Fires:** Every game tick (slower than pulse, typically ~30 seconds).
- **Bindings:** `$enactor` = token owner
- **Also on:** mob, obj, room

### toxingain

- **Phrase:** percent chance
- **Fires:** During toxin/poison tick processing for the token owner.
- **Bindings:** `$enactor` = poisoned character
- **Also on:** mob

---

## Movement Triggers (Slot 3)

These triggers react to characters entering, leaving, or moving through rooms.

### entry

- **Phrase:** percent chance
- **Fires:** When a character enters the room where the token is (or the token owner's room).
- **Bindings:** `$enactor` = entering character
- **Also on:** mob, obj, room

### exall

- **Phrase:** direction number (exact)
- **Fires:** When any character exits through a specific direction. Unlike `exit`, fires for everyone, not just the token owner.
- **Bindings:** `$enactor` = exiting character
- **Also on:** mob, obj, room

### exit

- **Phrase:** direction number (exact)
- **Fires:** When a character exits through a specific direction.
- **Bindings:** `$enactor` = exiting character
- **Also on:** mob, obj, room

### grall / greet

- **Phrase:** percent chance
- **Fires:** When a character enters the room. `grall` fires for all characters; `greet` only fires for visible characters.
- **Bindings:** `$enactor` = entering character
- **Also on:** mob, obj, room

### land

- **Phrase:** percent chance
- **Fires:** When a flying character lands.
- **Bindings:** `$enactor` = landing character
- **Also on:** mob

### move_char

- **Phrase:** direction name string
- **Fires:** When a character moves in a specific direction.
- **Bindings:** `$enactor` = moving character
- **Also on:** mob, room

### preenter

- **Phrase:** direction name or "portal"
- **Fires:** Before a character enters through a specific direction or portal. Returning failure prevents the entry.
- **Bindings:** `$enactor` = entering character
- **Also on:** obj, room

### takeoff

- **Phrase:** percent chance
- **Fires:** When a character takes off (starts flying).
- **Bindings:** `$enactor` = flying character
- **Also on:** mob

---

## Action Triggers (Slot 4)

Action triggers react to emotes and act messages.

### emote / emoteat

- **Phrase:** exact name match
- **Fires:** When a character uses a specific emote. `emoteat` fires for targeted emotes.
- **Bindings:** `$enactor` = emoter, `$victim` = target (emoteat)
- **Also on:** mob

### emoteself
{: .label .label-purple }Token-exclusive

- **Phrase:** exact name match
- **Fires:** When the token owner uses a self-targeted emote matching the phrase.
- **Bindings:** `$enactor` = emoter

---

## Fight Triggers (Slot 5)

Fight triggers react to combat events.

### afterkill

- **Phrase:** percent chance
- **Fires:** After the token owner kills an enemy.
- **Bindings:** `$enactor` = killer, `$victim` = killed target
- **Also on:** mob, obj, room

### assist

- **Phrase:** percent chance
- **Fires:** When the token owner assists another character in combat.
- **Bindings:** `$enactor` = assisting character, `$victim` = assisted character
- **Also on:** mob

### attack

- **Phrase:** percent chance
- **Fires:** When the token owner performs a basic attack.
- **Bindings:** `$enactor` = attacker, `$victim` = target
- **Also on:** mob

### fight

- **Phrase:** percent chance
- **Fires:** Each combat round while the token owner is fighting.
- **Bindings:** `$enactor` = fighter, `$victim` = opponent
- **Also on:** mob, obj, room

### preassist

- **Phrase:** percent chance
- **Fires:** Before assisting. Returning failure prevents the assist.
- **Bindings:** `$enactor` = assisting character
- **Also on:** mob

### preround

- **Phrase:** percent chance
- **Fires:** Before each combat round. Returning failure can modify round behavior.
- **Bindings:** `$enactor` = fighter, `$victim` = opponent
- **Also on:** mob

### prewimpy

- **Phrase:** percent chance
- **Fires:** Before a wimpy flee. Returning failure prevents the wimpy flee.
- **Bindings:** `$enactor` = wimpy character
- **Also on:** mob, obj, room

### startcombat

- **Phrase:** percent chance
- **Fires:** When combat begins.
- **Bindings:** `$enactor` = combatant, `$victim` = opponent
- **Also on:** mob, obj, room

### wimpy

- **Phrase:** percent chance
- **Fires:** When a character flees due to wimpy threshold.
- **Bindings:** `$enactor` = wimpy character
- **Also on:** mob, obj, room

---

## Lifecycle Triggers (Slot 6)

Lifecycle triggers handle death, respawning, expiration, and token management.

### afterdeath
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** Just after the token owner dies. Unlike `death`, this fires after death processing is complete — useful for post-death cleanup.
- **Bindings:** `$enactor` = dead character

### death

- **Phrase:** percent chance
- **Fires:** When the token owner dies. Fires during death processing.
- **Bindings:** `$enactor` = dying character
- **Also on:** mob, room

### death_protection

- **Phrase:** percent chance
- **Fires:** Allows the token to provide death protection (prevent death). Return failure to block death.
- **Bindings:** `$enactor` = dying character
- **Also on:** mob, room

### death_timer

- **Phrase:** percent chance
- **Fires:** When the death timer expires (ghost/corpse timer).
- **Bindings:** `$enactor` = dead character
- **Also on:** mob, room

### expire
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** When the token's countdown timer reaches zero. This is the primary mechanism for timed buffs, debuffs, and temporary effects. The token can clean up, display messages, or remove affects before being junked.
- **Bindings:** `$enactor` = token owner

### preanimate

- **Phrase:** percent chance
- **Fires:** Before an animate-dead spell succeeds. Returning failure prevents animation.
- **Bindings:** `$enactor` = caster
- **Also on:** mob, obj, room

### preresurrect

- **Phrase:** percent chance
- **Fires:** Before resurrection. Returning failure prevents resurrection.
- **Bindings:** `$enactor` = resurrecter, `$victim` = target
- **Also on:** mob, obj, room

### repop

- **Phrase:** percent chance
- **Fires:** When the token's area repops/resets.
- **Bindings:** `$enactor` = token owner
- **Also on:** mob, obj

### reset

- **Phrase:** percent chance
- **Fires:** On room reset.
- **Bindings:** `$here` = resetting room
- **Also on:** room

### restore

- **Phrase:** percent chance
- **Fires:** When the token owner is restored (HP/mana/move set to max).
- **Bindings:** `$enactor` = restored character
- **Also on:** mob

### resurrect

- **Phrase:** percent chance
- **Fires:** After successful resurrection.
- **Bindings:** `$enactor` = resurrecter, `$victim` = resurrected character
- **Also on:** mob

### save

- **Phrase:** percent chance
- **Fires:** When the token owner's data is saved to disk.
- **Bindings:** `$enactor` = saved character
- **Also on:** mob, obj

### stripaffect
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** When affects are being stripped from the token. Allows the token to react to or prevent affect removal.
- **Bindings:** `$enactor` = character

### token_given
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** Immediately after this token is created and attached to a new host via the `give` command. Useful for initialization logic.
- **Bindings:** `$enactor` = new host entity

### token_removed
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** When this token is about to be removed from its host. Fires before extraction — last chance for cleanup.
- **Bindings:** `$enactor` = host entity

---

## Verb Triggers (Slot 7)

Custom verb triggers let tokens define entirely new commands.

### showcommands

- **Phrase:** direct execution
- **Fires:** When the game builds the list of available commands for a character. Allows the token to inject custom commands into the command list.
- **Bindings:** `$enactor` = character
- **Also on:** mob, obj, room

### verb

- **Phrase:** keyword match
- **Fires:** When a character uses a custom verb that matches the phrase keyword.
- **Bindings:** `$enactor` = character, `$phrase` = verb arguments
- **Also on:** mob, obj, room

### verbself
{: .label .label-purple }Token-exclusive

- **Phrase:** keyword match
- **Fires:** When the token owner uses a custom self-targeted verb matching the phrase.
- **Bindings:** `$enactor` = character, `$phrase` = verb arguments

---

## Attack Triggers (Slot 8)

Attack triggers fire when specific combat techniques are used. Each corresponds to a named attack skill.

### attack_backstab / attack_bash / attack_behead / attack_bite

- **Phrase:** percent chance
- **Fires:** When the respective attack skill is used by or against the token owner.
- **Bindings:** `$enactor` = attacker, `$victim` = target
- **Also on:** mob

### attack_blackjack / attack_circle / attack_counter / attack_cripple

- **Phrase:** percent chance
- **Fires:** When the respective attack skill is used.
- **Bindings:** `$enactor` = attacker, `$victim` = target
- **Also on:** mob

### attack_dirtkick / attack_disarm / attack_intimidate / attack_kick

- **Phrase:** percent chance
- **Fires:** When the respective attack skill is used.
- **Bindings:** `$enactor` = attacker, `$victim` = target
- **Also on:** mob

### attack_rend / attack_slit / attack_smite / attack_tailkick

- **Phrase:** percent chance
- **Fires:** When the respective attack skill is used.
- **Bindings:** `$enactor` = attacker, `$victim` = target
- **Also on:** mob

### attack_trample / attack_turn

- **Phrase:** percent chance
- **Fires:** When the respective attack skill is used.
- **Bindings:** `$enactor` = attacker, `$victim` = target
- **Also on:** mob

### barrier

- **Phrase:** percent chance
- **Fires:** When a barrier blocks or absorbs an attack.
- **Bindings:** `$enactor` = attacker, `$victim` = defended character
- **Also on:** mob

### weapon_blocked / weapon_caught / weapon_parried

- **Phrase:** percent chance
- **Fires:** When a weapon attack is blocked, caught, or parried by or against the token owner.
- **Bindings:** `$enactor` = attacker, `$victim` = defender
- **Also on:** obj

---

## Hit Trigger (Slot 9)

### hit

- **Phrase:** percent chance
- **Fires:** Each time the token owner lands a hit in combat.
- **Bindings:** `$enactor` = attacker, `$victim` = target, `$register1` = damage amount
- **Also on:** mob

---

## Damage Triggers (Slot 10)

### check_damage

- **Phrase:** percent chance
- **Fires:** Before damage is applied. Allows the token to modify or cancel damage.
- **Bindings:** `$enactor` = attacker, `$victim` = target, `$register1` = damage amount
- **Also on:** mob, obj, room

### damage

- **Phrase:** percent chance
- **Fires:** When damage is dealt to or by the token owner.
- **Bindings:** `$enactor` = attacker, `$victim` = target, `$register1` = damage amount
- **Also on:** mob, obj, room

---

## Spell Triggers (Slot 11)

Spell triggers handle spell casting, effects, and interactions. Several are token-exclusive, making tokens the primary way to define custom spells.

### prespell
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** Before a token-defined spell resolves. Returning failure cancels the spell effect.
- **Bindings:** `$enactor` = caster, `$victim` = target

### spell
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** When a token-defined spell is cast. This is the main execution trigger for custom spells.
- **Bindings:** `$enactor` = caster, `$victim` = target

### spell_cure

- **Phrase:** percent chance
- **Fires:** When a cure spell is cast on the token owner.
- **Bindings:** `$enactor` = caster, `$victim` = healed character
- **Also on:** mob

### spell_dispel

- **Phrase:** percent chance
- **Fires:** When a dispel spell targets the token owner.
- **Bindings:** `$enactor` = caster, `$victim` = dispelled character
- **Also on:** mob, obj

### spellbeat
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** Each round during spell casting when the token has the `SPELLBEATS` flag. Allows multi-round spell effects (channeling, progressive damage, etc.).
- **Bindings:** `$enactor` = caster

### spellcast

- **Phrase:** exact name match
- **Fires:** When a spell with a matching name is cast. Matches against the spell name.
- **Bindings:** `$enactor` = caster, `$victim` = target
- **Also on:** mob, obj, room

### spellinter
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** When a token-defined spell is interrupted during casting.
- **Bindings:** `$enactor` = interrupted caster

### spellpenetrate
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** When checking if a token-defined spell penetrates defenses (magic resistance, saves, etc.).
- **Bindings:** `$enactor` = caster, `$victim` = target

### spellreflect

- **Phrase:** percent chance
- **Fires:** When a spell is reflected back at the caster.
- **Bindings:** `$enactor` = original caster, `$victim` = reflector
- **Also on:** mob

---

## Interrupt Triggers (Slot 12)

### caninterrupt
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** To check if the token can interrupt an ongoing action (casting, skill use, etc.). Returning success means the action can be interrupted.
- **Bindings:** `$enactor` = character performing action

### interrupt
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** When an action is actually interrupted. Handles the interruption logic.
- **Bindings:** `$enactor` = interrupted character

---

## Combat Style Triggers (Slot 13)

### combatstyle
{: .label .label-purple }Token-exclusive

- **Phrase:** percent chance
- **Fires:** When the combat system selects a fighting style for the token owner. Allows the token to define custom combat behavior.
- **Bindings:** `$enactor` = combatant, `$victim` = opponent

### defense

- **Phrase:** percent chance
- **Fires:** When the token owner's defense is calculated. Allows the token to modify defensive behavior.
- **Bindings:** `$enactor` = defended character, `$victim` = attacker
- **Also on:** mob

---

## Animate Trigger (Slot 14)

### animate

- **Phrase:** percent chance
- **Fires:** When an animate-dead spell successfully raises a corpse.
- **Bindings:** `$enactor` = caster, `$victim` = animated undead
- **Also on:** mob

---

## Summary Table

| Category | Triggers | Count |
|:---------|:---------|------:|
| General | act, board, brandish, bribe, bury, buy, check_buyer, clone_extract, close, contract_complete, custom_price, delay, drink, drop, eat, examine, flee, forcedismount, get, give, grouped, hidden, hide, hpcnt, identify, inspect, inspect_custom, kill, knock, knocking, level, login, lore, lorex, moon, mount, multiclass, open, postquest, practice, practicetoken★, precast★, prebite, prebuy, prebuy_obj, predeath, predismount, predrink, predrop, preeat, preflee, preget, prehide, prehidein, prekill, premount, prepractice, prepracticeother, prepracticethat, prepracticetoken★, preput, prequest, prerecall, prerecite, prerehearse, preremove, prerenew, prerest, presell, presit, presleep, prestand, pretrain, pretraintoken★, prewake, prewear, pull, pullon, push, pushon, put, quest_cancel, quest_complete, quest_incomplete, quest_part, quit★, recall, recite, remort, remove, renew, renew_list, rest, restocked, sit, skill_berserk, skill_sneak, skill_warcry, sleep, stand, throw, touch, turn, turnon, ungrouped, use, usewith, wake, wear, xpgain, zap | 110 |
| Speech | sayto, speech, whisper | 3 |
| Random/Timed | grow, hitgain, managain, movegain, prereckoning, pulse, random, reckoning, regen, regen_hp, regen_mana, regen_move, tick, toxingain | 14 |
| Movement | entry, exall, exit, grall, greet, land, move_char, preenter, takeoff | 9 |
| Action | act, emote, emoteat, emoteself★ | 4 |
| Fight | afterkill, assist, attack, fight, preassist, preround, prewimpy, startcombat, wimpy | 9 |
| Lifecycle | afterdeath★, death, death_protection, death_timer, expire★, preanimate, preresurrect, repop, reset, restore, resurrect, save, stripaffect★, token_given★, token_removed★ | 15 |
| Verb | showcommands, verb, verbself★ | 3 |
| Attacks | attack_backstab, attack_bash, attack_behead, attack_bite, attack_blackjack, attack_circle, attack_counter, attack_cripple, attack_dirtkick, attack_disarm, attack_intimidate, attack_kick, attack_rend, attack_slit, attack_smite, attack_tailkick, attack_trample, attack_turn, barrier, weapon_blocked, weapon_caught, weapon_parried | 22 |
| Hits | hit | 1 |
| Damage | check_damage, damage | 2 |
| Spell | prespell★, spell★, spell_cure, spell_dispel, spellbeat★, spellcast, spellinter★, spellpenetrate★, spellreflect | 9 |
| Interrupt | caninterrupt★, interrupt★ | 2 |
| Combat Style | combatstyle★, defense | 2 |
| Animate | animate | 1 |
| **Total** | | **206** |

★ = Token-exclusive trigger