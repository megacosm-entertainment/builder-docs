---
layout: default
title: Ifchecks Reference
nav_order: 10
parent: Scripting
---

# Ifchecks Reference
{: .no_toc }

Complete reference for all **482 ifchecks** available in Sentience scripts. Ifchecks are
conditional functions used in `if` statements to test conditions about the game world —
character stats, inventory, room state, flags, and more.

Entries marked **New** were recently added and were not in previous documentation.

**Syntax:** `if <check> [target] [args...] [operator value]`

```
if ispc $n
  mob echo $n is a player!
endif

if level $n > 10
  mob echo $n is experienced.
endif

if carries $n sword
  mob echo $n has a sword.
endif
```

**Return types:**
- **T/F** — Boolean: tests as true/false. Use without an operator: `if ispc $n`
- **NUM** — Numeric: returns a number. Compare with `==`, `!=`, `<`, `>`, `<=`, `>=`: `if level $n > 10`

**Entity type support:** Most ifchecks work in all 9 program types (mob, object, room,
token, area, instance, dungeon, quest, event). A few are restricted to specific types as noted.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Quick Reference

| Category | Count | Description |
|:---------|------:|:------------|
| [Identity and Type](#identity-and-type) | 17 (2 new) | Entity identification and type checks |
| [Alignment and Grouping](#alignment-and-grouping) | 16 (3 new) | Alignment, church, PK status |
| [Level, Stats, and Resources](#level-stats-and-resources) | 31 | Level, attributes, currency, XP |
| [Combat State](#combat-state) | 31 | Fighting, PK stats, combat values |
| [Conditions and Status](#conditions-and-status) | 37 | Affects, spells, position, hunger/thirst |
| [Act and Offensive Flags](#act-and-offensive-flags) | 22 | NPC act flags, immunities, resistances |
| [Inventory and Equipment](#inventory-and-equipment) | 16 | Carrying, wearing, weight |
| [Object-Specific Checks](#object-specific-checks) | 60 (23 new) | Object properties, values, types |
| [Room-Specific Checks](#room-specific-checks) | 28 | Room properties, exits, mob presence |
| [Group and Following](#group-and-following) | 21 | Group membership, stats, following |
| [Tokens](#tokens) | 5 | Token presence, values, timers |
| [Variables](#variables) | 10 (5 new) | Script variables and temp storage |
| [Map and Wilderness](#map-and-wilderness) | 14 | Map coordinates, area geography |
| [Time](#time) | 9 | In-game and real-world time |
| [Random and Math](#random-and-math) | 12 | Random chances, dice, math functions |
| [Quests and Progress](#quests-and-progress) | 11 (8 new) | Quest state, completion, stages |
| [Events](#events) | 10 (10 new) | Server event system checks |
| [Classes and Skills](#classes-and-skills) | 14 (14 new) | Class, skill, song, spell availability |
| [Missions and PK Totals](#missions-and-pk-totals) | 8 (8 new) | Mission tracking, lifetime PK stats |
| [Traits](#traits) | 4 (4 new) | Trait system checks |
| [Flag Lookups](#flag-lookups) | 12 (12 new) | Numeric flag value lookups |
| [Miscellaneous](#miscellaneous) | 94 (18 new) | Affects, combat detail, regen, pathfinding, and more |

**Total: 482 ifchecks** (107 new)

---

## Identity and Type

*17 ifchecks (2 new)*

### id

**Syntax:** `id $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the unique ID of the entity.

```
if id $n > 0
  mob echo Check passed.
endif
```

---

### id2

**Syntax:** `id2 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the secondary ID.

```
if id2 $n > 0
  mob echo Check passed.
endif
```

---

### identical

**Syntax:** `identical $n $p`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if two entities are identical (same object instance).

```
if identical $n $p
  mob echo Same entity!
endif
```

---

### isangel

**Syntax:** `isangel $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is an angel.

```
if isangel $n
  mob echo You are angelic.
endif
```

---

### isdemon

**Syntax:** `isdemon $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is a demon.

```
if isdemon $n
  mob echo You are demonic.
endif
```

---

### isimmort

**Syntax:** `isimmort $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is an immortal player.

```
if isimmort $n
  mob echo Greetings, Immortal.
endif
```

---

### ismobile

**Syntax:** `ismobile $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

Alias for `isnpc`.

```
if ismobile $n
  mob echo Check passed.
endif
```

---

### ismystic

**Syntax:** `ismystic $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is a mystic.

```
if ismystic $n
  mob echo You are a mystic.
endif
```

---

### isnpc

**Syntax:** `isnpc $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is an NPC/mobile.

```
if isnpc $n
  mob echo $n is an NPC.
endif
```

---

### isobject

**Syntax:** `isobject $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is an object.

```
if isobject $n
  mob echo Check passed.
endif
```

---

### ispc

**Syntax:** `ispc $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is a player character.

```
if ispc $n
  mob echo $n is a player!
endif
```

---

### isroom

**Syntax:** `isroom $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is a room.

```
if isroom $n
  mob echo Check passed.
endif
```

---

### istoken

**Syntax:** `istoken $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is a token.

```
if istoken $n
  mob echo Check passed.
endif
```

---

### name

**Syntax:** `name $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity's name matches the argument.

```
if name $n Gandalf
  mob echo Hello, wizard.
endif
```

---

### racepath **New**{: .label .label-green }

**Syntax:** `racepath $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity's race has a path/evolution.

```
if racepath $n
  mob echo Check passed.
endif
```

---

### raceremort **New**{: .label .label-green }

**Syntax:** `raceremort $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity's race is a remort race.

```
if raceremort $n
  mob echo Check passed.
endif
```

---

### vnum

**Syntax:** `vnum $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the widevnum of the entity.

```
if vnum $n == 3001
  mob echo That is mob 3001.
endif
```

---

## Alignment and Grouping

*16 ifchecks (3 new)*

### align

**Syntax:** `align $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric alignment value (-1000 to 1000).

```
if align $n > 500
  mob echo You are very good.
endif
```

---

### church

**Syntax:** `church $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is a member of the named church.

```
if church $n templar
  mob echo Hail, Templar.
endif
```

---

### churchhasrelic

**Syntax:** `churchhasrelic $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character's church currently holds a relic.

```
if churchhasrelic $n templar
  mob echo Check passed.
endif
```

---

### churchonline

**Syntax:** `churchonline $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns number of church members currently online.

```
if churchonline $n > 0
  mob echo Check passed.
endif
```

---

### churchrank

**Syntax:** `churchrank $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the character's church rank.

```
if churchrank $n > 0
  mob echo Check passed.
endif
```

---

### churchsize

**Syntax:** `churchsize $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the size of the character's church.

```
if churchsize $n > 0
  mob echo Check passed.
endif
```

---

### clan **New**{: .label .label-green }

**Syntax:** `clan $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** Special (internal)

True if the entity belongs to the named clan.

```
if clan $n raiders
  mob echo Check passed.
endif
```

---

### haschurch **New**{: .label .label-green }

**Syntax:** `haschurch $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity belongs to the named church.

```
if haschurch $n templar
  mob echo Check passed.
endif
```

---

### inchurch **New**{: .label .label-green }

**Syntax:** `inchurch $n $p`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if both entities are in the same church.

```
if inchurch $n $p
  mob echo Check passed.
endif
```

---

### ischurchexcom

**Syntax:** `ischurchexcom $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character has been excommunicated.

```
if ischurchexcom $n
  mob echo Check passed.
endif
```

---

### ischurchpk

**Syntax:** `ischurchpk $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is in a church PK state.

```
if ischurchpk $n
  mob echo Check passed.
endif
```

---

### iscpkproof

**Syntax:** `iscpkproof $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is CPK-proof.

```
if iscpkproof $n
  mob echo Check passed.
endif
```

---

### isevil

**Syntax:** `isevil $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has evil alignment.

```
if isevil $n
  mob echo Begone, evil one!
endif
```

---

### isgood

**Syntax:** `isgood $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has good alignment.

```
if isgood $n
  mob echo Welcome, noble soul.
endif
```

---

### isneutral

**Syntax:** `isneutral $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has neutral alignment.

```
if isneutral $n
  mob echo Check passed.
endif
```

---

### ispk

**Syntax:** `ispk $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is PK-flagged.

```
if ispk $n
  mob echo You are a player killer.
endif
```

---

## Level, Stats, and Resources

*31 ifchecks*

### age

**Syntax:** `age $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Character age in game years.

```
if age $n > 0
  mob echo Check passed.
endif
```

---

### bankbalance

**Syntax:** `bankbalance` or `bankbalance <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Bank account balance.

```
if bankbalance > 0
  mob echo Check passed.
endif
```

---

### curhit

**Syntax:** `curhit $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current HP.

```
if curhit $n < 50
  mob echo You look wounded.
endif
```

---

### curmana

**Syntax:** `curmana $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current mana.

```
if curmana $n > 0
  mob echo Check passed.
endif
```

---

### curmove

**Syntax:** `curmove $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current movement.

```
if curmove $n > 0
  mob echo Check passed.
endif
```

---

### deathcount

**Syntax:** `deathcount $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of times the character has died.

```
if deathcount $n > 0
  mob echo Check passed.
endif
```

---

### gold

**Syntax:** `gold $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Gold in inventory.

```
if gold $n > 100
  mob echo You look wealthy.
endif
```

---

### hpcnt

**Syntax:** `hpcnt $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

HP as a percentage (0-100).

```
if hpcnt $n < 25
  mob echo You are nearly dead!
endif
```

---

### isremort

**Syntax:** `isremort $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character has remorted.

```
if isremort $n
  mob echo You have remorted!
endif
```

---

### level

**Syntax:** `level $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

The character's level.

```
if level $n > 20
  mob echo You are powerful!
endif
```

---

### maxhit

**Syntax:** `maxhit $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Maximum HP.

```
if maxhit $n > 0
  mob echo Check passed.
endif
```

---

### maxmana

**Syntax:** `maxmana $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Maximum mana.

```
if maxmana $n > 0
  mob echo Check passed.
endif
```

---

### maxmove

**Syntax:** `maxmove $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Maximum movement.

```
if maxmove $n > 0
  mob echo Check passed.
endif
```

---

### maxxp

**Syntax:** `maxxp $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

XP needed for next level.

```
if maxxp $n > 0
  mob echo Check passed.
endif
```

---

### money

**Syntax:** `money $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Total coin value (normalized).

```
if money $n > 0
  mob echo Check passed.
endif
```

---

### permcon

**Syntax:** `permcon $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Permanent CON.

```
if permcon $n > 0
  mob echo Check passed.
endif
```

---

### permdex

**Syntax:** `permdex $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Permanent DEX.

```
if permdex $n > 0
  mob echo Check passed.
endif
```

---

### permint

**Syntax:** `permint $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Permanent INT.

```
if permint $n > 0
  mob echo Check passed.
endif
```

---

### permstr

**Syntax:** `permstr $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Permanent STR (unmodified by equipment).

```
if permstr $n > 0
  mob echo Check passed.
endif
```

---

### permwis

**Syntax:** `permwis $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Permanent WIS.

```
if permwis $n > 0
  mob echo Check passed.
endif
```

---

### pneuma

**Syntax:** `pneuma $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Pneuma (soul energy) level.

```
if pneuma $n > 0
  mob echo Check passed.
endif
```

---

### practices

**Syntax:** `practices $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of practice sessions available.

```
if practices $n > 0
  mob echo Check passed.
endif
```

---

### silver

**Syntax:** `silver $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Silver in inventory.

```
if silver $n > 0
  mob echo Check passed.
endif
```

---

### statcon

**Syntax:** `statcon $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current CON stat.

```
if statcon $n > 0
  mob echo Check passed.
endif
```

---

### statdex

**Syntax:** `statdex $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current DEX stat.

```
if statdex $n > 0
  mob echo Check passed.
endif
```

---

### statint

**Syntax:** `statint $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current INT stat.

```
if statint $n > 0
  mob echo Check passed.
endif
```

---

### statstr

**Syntax:** `statstr $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current STR stat.

```
if statstr $n > 0
  mob echo Check passed.
endif
```

---

### statwis

**Syntax:** `statwis $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current WIS stat.

```
if statwis $n > 0
  mob echo Check passed.
endif
```

---

### sublevel

**Syntax:** `sublevel $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

The character's sub-level.

```
if sublevel $n > 0
  mob echo Check passed.
endif
```

---

### trains

**Syntax:** `trains $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of train sessions available.

```
if trains $n > 0
  mob echo Check passed.
endif
```

---

### xp

**Syntax:** `xp` or `xp <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current experience points.

```
if xp > 0
  mob echo Check passed.
endif
```

---

## Combat State

*31 ifchecks*

### arenafights

**Syntax:** `arenafights $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Arena fights.

```
if arenafights $n > 0
  mob echo Check passed.
endif
```

---

### arenaloss

**Syntax:** `arenaloss $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Arena losses.

```
if arenaloss $n > 0
  mob echo Check passed.
endif
```

---

### arenaratio

**Syntax:** `arenaratio $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Arena win ratio.

```
if arenaratio $n > 0
  mob echo Check passed.
endif
```

---

### arenawins

**Syntax:** `arenawins $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Arena wins.

```
if arenawins $n > 0
  mob echo Check passed.
endif
```

---

### cpkfights

**Syntax:** `cpkfights $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

CPK fights.

```
if cpkfights $n > 0
  mob echo Check passed.
endif
```

---

### cpkloss

**Syntax:** `cpkloss $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

CPK losses.

```
if cpkloss $n > 0
  mob echo Check passed.
endif
```

---

### cpkratio

**Syntax:** `cpkratio $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

CPK win ratio.

```
if cpkratio $n > 0
  mob echo Check passed.
endif
```

---

### cpkwins

**Syntax:** `cpkwins $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

CPK wins.

```
if cpkwins $n > 0
  mob echo Check passed.
endif
```

---

### danger

**Syntax:** `danger $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Calculated danger level of the character's situation.

```
if danger $n > 0
  mob echo Check passed.
endif
```

---

### hastarget

**Syntax:** `hastarget $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character has a combat target.

```
if hastarget $n
  mob echo Check passed.
endif
```

---

### isfighting

**Syntax:** `isfighting $n [$p]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is currently in combat.

```
if isfighting $n
  mob echo $n is in combat!
endif
```

---

### isflying

**Syntax:** `isflying $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is flying.

```
if isflying $n
  mob echo Check passed.
endif
```

---

### isridden

**Syntax:** `isridden $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity (mount) is being ridden.

```
if isridden $n
  mob echo Check passed.
endif
```

---

### isrider

**Syntax:** `isrider $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is being ridden.

```
if isrider $n
  mob echo Check passed.
endif
```

---

### isriding

**Syntax:** `isriding $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is riding a mount.

```
if isriding $n
  mob echo Check passed.
endif
```

---

### istarget

**Syntax:** `istarget $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is someone's combat target.

```
if istarget $n
  mob echo Check passed.
endif
```

---

### monkills

**Syntax:** `monkills $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Monster kills count.

```
if monkills $n > 0
  mob echo Check passed.
endif
```

---

### pkfights

**Syntax:** `pkfights $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Total PK fights.

```
if pkfights $n > 0
  mob echo Check passed.
endif
```

---

### pkloss

**Syntax:** `pkloss $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

PK losses count.

```
if pkloss $n > 0
  mob echo Check passed.
endif
```

---

### pkratio

**Syntax:** `pkratio $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

PK win ratio (win percentage).

```
if pkratio $n > 0
  mob echo Check passed.
endif
```

---

### pkwins

**Syntax:** `pkwins $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

PK wins count.

```
if pkwins $n > 0
  mob echo Check passed.
endif
```

---

### reckoning

**Syntax:** `reckoning` or `reckoning <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current reckoning value.

```
if reckoning > 0
  mob echo Check passed.
endif
```

---

### reckoningchance

**Syntax:** `reckoningchance` or `reckoningchance <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Reckoning proc chance.

```
if reckoningchance > 0
  mob echo Check passed.
endif
```

---

### reckoningcooldown

**Syntax:** `reckoningcooldown` or `reckoningcooldown <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Reckoning cooldown remaining.

```
if reckoningcooldown > 0
  mob echo Check passed.
endif
```

---

### reckoningduration

**Syntax:** `reckoningduration` or `reckoningduration <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Reckoning duration.

```
if reckoningduration > 0
  mob echo Check passed.
endif
```

---

### reckoningintensity

**Syntax:** `reckoningintensity` or `reckoningintensity <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Reckoning intensity.

```
if reckoningintensity > 0
  mob echo Check passed.
endif
```

---

### totalfights

**Syntax:** `totalfights $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Total fights.

```
if totalfights $n > 0
  mob echo Check passed.
endif
```

---

### totalloss

**Syntax:** `totalloss $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Total combat losses.

```
if totalloss $n > 0
  mob echo Check passed.
endif
```

---

### totalratio

**Syntax:** `totalratio $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Overall win ratio.

```
if totalratio $n > 0
  mob echo Check passed.
endif
```

---

### totalwins

**Syntax:** `totalwins $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Total combat wins.

```
if totalwins $n > 0
  mob echo Check passed.
endif
```

---

### wimpy

**Syntax:** `wimpy $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

The character's wimpy (flee) threshold.

```
if wimpy $n > 0
  mob echo Check passed.
endif
```

---

## Conditions and Status

*37 ifchecks*

### affected

**Syntax:** `affected $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if affected by the named condition/spell.

```
if affected $n poison
  mob echo You look ill.
endif
```

---

### affected2

**Syntax:** `affected2 $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if affected by the named affect2 flag.

```
if affected2 $n flag_name
  mob echo Check passed.
endif
```

---

### boost

**Syntax:** `boost` or `boost <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Boost value.

```
if boost > 0
  mob echo Check passed.
endif
```

---

### boosttimer

**Syntax:** `boosttimer` or `boosttimer <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Remaining boost duration.

```
if boosttimer > 0
  mob echo Check passed.
endif
```

---

### drunk

**Syntax:** `drunk $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Alcohol intoxication level.

```
if drunk $n > 0
  mob echo Check passed.
endif
```

---

### fullness

**Syntax:** `fullness $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Fullness level.

```
if fullness $n > 0
  mob echo Check passed.
endif
```

---

### hasspell

**Syntax:** `hasspell $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character has a specific spell granted.

```
if hasspell $n fireball
  mob echo Check passed.
endif
```

---

### hunger

**Syntax:** `hunger $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Hunger value (positive = hungry).

```
if hunger $n > 0
  mob echo Check passed.
endif
```

---

### isactive

**Syntax:** `isactive $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is active/awake.

```
if isactive $n
  mob echo Check passed.
endif
```

---

### isaffectcustom

**Syntax:** `isaffectcustom $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if affected by a custom-named affect.

```
if isaffectcustom $n
  mob echo Check passed.
endif
```

---

### isaffectgroup

**Syntax:** `isaffectgroup $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if affected by a group-type affect.

```
if isaffectgroup $n
  mob echo Check passed.
endif
```

---

### isaffectskill

**Syntax:** `isaffectskill $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if affected by a specific skill affect.

```
if isaffectskill $n
  mob echo Check passed.
endif
```

---

### isaffectwhere

**Syntax:** `isaffectwhere $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if affected at a specific wear location.

```
if isaffectwhere $n
  mob echo Check passed.
endif
```

---

### isbrewing

**Syntax:** `isbrewing $n [<argument>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character is brewing.

```
if isbrewing $n sentinel
  mob echo Check passed.
endif
```

---

### isbusy

**Syntax:** `isbusy $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character is currently busy (crafting, etc.).

```
if isbusy $n
  mob echo $n is busy right now.
endif
```

---

### iscastfailure

**Syntax:** `iscastfailure $n [<argument>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the last cast failed.

```
if iscastfailure $n example
  mob echo Check passed.
endif
```

---

### iscasting

**Syntax:** `iscasting $n [<argument>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character is casting a spell.

```
if iscasting $n fireball
  mob echo Check passed.
endif
```

---

### iscastrecovered

**Syntax:** `iscastrecovered $n [<argument>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if recovered from casting.

```
if iscastrecovered $n example
  mob echo Check passed.
endif
```

---

### iscastroomblocked

**Syntax:** `iscastroomblocked $n [<argument>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the room blocks casting.

```
if iscastroomblocked $n safe
  mob echo Check passed.
endif
```

---

### iscastsuccess

**Syntax:** `iscastsuccess $n [<argument>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the last cast succeeded.

```
if iscastsuccess $n example
  mob echo Check passed.
endif
```

---

### isdead

**Syntax:** `isdead $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is dead.

```
if isdead $n
  mob echo $n is dead.
endif
```

---

### isfading

**Syntax:** `isfading $n [<argument>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character is fading (invisible).

```
if isfading $n sentinel
  mob echo Check passed.
endif
```

---

### ismorphed

**Syntax:** `ismorphed $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character has morphed form.

```
if ismorphed $n
  mob echo Check passed.
endif
```

---

### ison

**Syntax:** `ison $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if sitting/resting on furniture.

```
if ison $n
  mob echo Check passed.
endif
```

---

### isscribing

**Syntax:** `isscribing $n [<argument>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character is scribing.

```
if isscribing $n sentinel
  mob echo Check passed.
endif
```

---

### isshifted

**Syntax:** `isshifted $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character has shifted.

```
if isshifted $n
  mob echo Check passed.
endif
```

---

### isshooting

**Syntax:** `isshooting $n [<argument>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character is shooting a ranged weapon.

```
if isshooting $n sword
  mob echo Check passed.
endif
```

---

### issustained

**Syntax:** `issustained $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character has a life-sustaining effect.

```
if issustained $n
  mob echo Check passed.
endif
```

---

### istattooing

**Syntax:** `istattooing $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character is getting tattooed.

```
if istattooing $n
  mob echo Check passed.
endif
```

---

### isvisible

**Syntax:** `isvisible $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is visible.

```
if isvisible $n
  mob echo Check passed.
endif
```

---

### isvisibleto

**Syntax:** `isvisibleto $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is visible to the specified observer.

```
if isvisibleto $n
  mob echo Check passed.
endif
```

---

### lostparts

**Syntax:** `lostparts $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character has lost body parts.

```
if lostparts $n sentinel
  mob echo Check passed.
endif
```

---

### pos

**Syntax:** `pos $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character is in the named position (standing, sitting, etc.).

```
if pos $n sleeping
  mob echo $n is asleep.
endif
```

---

### skill

**Syntax:** `skill $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the skill percentage for the named skill.

```
if skill $n dodge > 75
  mob echo You are skilled at dodging.
endif
```

---

### stoned

**Syntax:** `stoned $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Stoned level (toxic high).

```
if stoned $n > 0
  mob echo Check passed.
endif
```

---

### thirst

**Syntax:** `thirst $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Thirst value (positive = thirsty).

```
if thirst $n > 0
  mob echo Check passed.
endif
```

---

### toxin

**Syntax:** `toxin $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Toxin level.

```
if toxin $n example > 0
  mob echo Check passed.
endif
```

---

## Act and Offensive Flags

*22 ifchecks*

### act

**Syntax:** `act $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the NPC has the specified act flag (e.g. `sentinel`, `aggressive`).

```
if act $n sentinel
  mob echo This mob won't move.
endif
```

---

### act2

**Syntax:** `act2 $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the NPC has the specified act2 flag.

```
if act2 $n flag_name
  mob echo Check passed.
endif
```

---

### comm

**Syntax:** `comm $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character has the named comm flag.

```
if comm $n flag_name
  mob echo Check passed.
endif
```

---

### flagact

**Syntax:** `flagact` or `flagact <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of the act flags bitmask.

```
if flagact > 0
  mob echo Check passed.
endif
```

---

### flagact2

**Syntax:** `flagact2` or `flagact2 <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of act2 flags.

```
if flagact2 > 0
  mob echo Check passed.
endif
```

---

### flagaffect

**Syntax:** `flagaffect` or `flagaffect <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of affect flags.

```
if flagaffect > 0
  mob echo Check passed.
endif
```

---

### flagaffect2

**Syntax:** `flagaffect2` or `flagaffect2 <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of affect2 flags.

```
if flagaffect2 > 0
  mob echo Check passed.
endif
```

---

### flagcomm

**Syntax:** `flagcomm` or `flagcomm <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of comm flags.

```
if flagcomm > 0
  mob echo Check passed.
endif
```

---

### flagform

**Syntax:** `flagform` or `flagform <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of form flags.

```
if flagform > 0
  mob echo Check passed.
endif
```

---

### flagimm

**Syntax:** `flagimm` or `flagimm <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of immunity flags.

```
if flagimm > 0
  mob echo Check passed.
endif
```

---

### flagoff

**Syntax:** `flagoff` or `flagoff <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of offensive flags.

```
if flagoff > 0
  mob echo Check passed.
endif
```

---

### flagpart

**Syntax:** `flagpart` or `flagpart <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of body part flags.

```
if flagpart > 0
  mob echo Check passed.
endif
```

---

### flagres

**Syntax:** `flagres` or `flagres <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of resistance flags.

```
if flagres > 0
  mob echo Check passed.
endif
```

---

### flagvuln

**Syntax:** `flagvuln` or `flagvuln <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of vulnerability flags.

```
if flagvuln > 0
  mob echo Check passed.
endif
```

---

### flagweapon

**Syntax:** `flagweapon` or `flagweapon <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of weapon flags.

```
if flagweapon > 0
  mob echo Check passed.
endif
```

---

### flagwear

**Syntax:** `flagwear` or `flagwear <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Numeric value of wear flags.

```
if flagwear > 0
  mob echo Check passed.
endif
```

---

### imm

**Syntax:** `imm $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if immune to the damage/affect type.

```
if imm $n fire
  mob echo $n is immune to fire.
endif
```

---

### off

**Syntax:** `off $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the NPC/character has the specified offensive flag.

```
if off $n flag_name
  mob echo Check passed.
endif
```

---

### parts

**Syntax:** `parts $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character has the named body part.

```
if parts $n keyword
  mob echo Check passed.
endif
```

---

### res

**Syntax:** `res $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if resistant to the damage/affect type.

```
if res $n cold
  mob echo $n resists cold.
endif
```

---

### vuln

**Syntax:** `vuln $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if vulnerable to the damage/affect type.

```
if vuln $n lightning
  mob echo $n is vulnerable to lightning!
endif
```

---

### weapon

**Syntax:** `weapon $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character uses the named weapon type.

```
if weapon $n sword
  mob echo Check passed.
endif
```

---

## Inventory and Equipment

*16 ifchecks*

### candrop

**Syntax:** `candrop $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity can be dropped.

```
if candrop $n
  mob echo Check passed.
endif
```

---

### canget

**Syntax:** `canget $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity can be picked up.

```
if canget $n
  mob echo Check passed.
endif
```

---

### canpull

**Syntax:** `canpull $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity can be pulled.

```
if canpull $n
  mob echo Check passed.
endif
```

---

### canput

**Syntax:** `canput $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity can be placed in a container.

```
if canput $n
  mob echo Check passed.
endif
```

---

### carries

**Syntax:** `carries $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character carries an item matching the keyword.

```
if carries $n sword
  mob echo You have a sword.
endif
```

---

### carryleft

**Syntax:** `carryleft $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of items the character can still carry.

```
if carryleft $n > 0
  mob echo Check passed.
endif
```

---

### handsfull

**Syntax:** `handsfull $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if character has no free hands.

```
if handsfull $n
  mob echo Check passed.
endif
```

---

### has

**Syntax:** `has $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character has (carries or wears) a matching item.

```
if has $n shield
  mob echo You have a shield.
endif
```

---

### ispulling

**Syntax:** `ispulling $n [$p]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is pulling something.

```
if ispulling $n $p
  mob echo Check passed.
endif
```

---

### ispullingrelic

**Syntax:** `ispullingrelic $n [<argument>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if pulling a church relic.

```
if ispullingrelic $n templar
  mob echo Check passed.
endif
```

---

### maxcarry

**Syntax:** `maxcarry $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Maximum items the character can carry.

```
if maxcarry $n > 0
  mob echo Check passed.
endif
```

---

### maxweight

**Syntax:** `maxweight $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Maximum weight the character can carry.

```
if maxweight $n > 0
  mob echo Check passed.
endif
```

---

### wears

**Syntax:** `wears $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is wearing a matching item.

```
if wears $n helmet
  mob echo Nice helmet!
endif
```

---

### wearused

**Syntax:** `wearused $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the specified wear location is occupied.

```
if wearused $n head
  mob echo Check passed.
endif
```

---

### weight

**Syntax:** `weight $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Total weight carried.

```
if weight $n > 200
  mob echo You carry a heavy load.
endif
```

---

### weightleft

**Syntax:** `weightleft $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Remaining weight capacity.

```
if weightleft $n > 0
  mob echo Check passed.
endif
```

---

## Object-Specific Checks

*60 ifchecks (23 new)*

### carriedby

**Syntax:** `carriedby $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** Object programs only

True if the object is in the specified character's inventory.

```
if carriedby $n
  mob echo Check passed.
endif
```

---

### container

**Syntax:** `container $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is a container.

```
if container $n bag
  mob echo Check passed.
endif
```

---

### damtype

**Syntax:** `damtype $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the weapon deals the named damage type.

```
if damtype $p fire
  mob echo This weapon deals fire damage.
endif
```

---

### furniture

**Syntax:** `furniture $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is furniture.

```
if furniture $n example
  mob echo Check passed.
endif
```

---

### groundweight

**Syntax:** `groundweight $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Weight when on the ground.

```
if groundweight $n > 0
  mob echo Check passed.
endif
```

---

### isbook **New**{: .label .label-green }

**Syntax:** `isbook $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is a book.

```
if isbook $n
  mob echo Check passed.
endif
```

---

### iscontainer **New**{: .label .label-green }

**Syntax:** `iscontainer $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is a container.

```
if iscontainer $n
  mob echo Check passed.
endif
```

---

### isfluidcontainer **New**{: .label .label-green }

**Syntax:** `isfluidcontainer $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is a fluid container (drink container).

```
if isfluidcontainer $n
  mob echo Check passed.
endif
```

---

### isfood **New**{: .label .label-green }

**Syntax:** `isfood $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is food.

```
if isfood $n
  mob echo Check passed.
endif
```

---

### isfurniture **New**{: .label .label-green }

**Syntax:** `isfurniture $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is furniture.

```
if isfurniture $n
  mob echo Check passed.
endif
```

---

### iskey

**Syntax:** `iskey $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is a key.

```
if iskey $p
  mob echo That is a key.
endif
```

---

### islight **New**{: .label .label-green }

**Syntax:** `islight $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is a light source.

```
if islight $n
  mob echo Check passed.
endif
```

---

### ismoney **New**{: .label .label-green }

**Syntax:** `ismoney $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is money/coins.

```
if ismoney $n
  mob echo Check passed.
endif
```

---

### ispage **New**{: .label .label-green }

**Syntax:** `ispage $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is a page.

```
if ispage $n
  mob echo Check passed.
endif
```

---

### isportal **New**{: .label .label-green }

**Syntax:** `isportal $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is a portal.

```
if isportal $n
  mob echo Check passed.
endif
```

---

### isrepairable

**Syntax:** `isrepairable $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object can be repaired.

```
if isrepairable $n
  mob echo Check passed.
endif
```

---

### isrestrung

**Syntax:** `isrestrung $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object has been restrung.

```
if isrestrung $n
  mob echo Check passed.
endif
```

---

### isworn

**Syntax:** `isworn $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is currently worn.

```
if isworn $n
  mob echo Check passed.
endif
```

---

### liquid

**Syntax:** `liquid $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object contains the named liquid.

```
if liquid $p water
  mob echo It contains water.
endif
```

---

### material

**Syntax:** `material $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is made of the named material.

```
if material $p iron
  mob echo It is made of iron.
endif
```

---

### numenchants

**Syntax:** `numenchants $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of enchantments on the object.

```
if numenchants $n example > 0
  mob echo Check passed.
endif
```

---

### objclones

**Syntax:** `objclones $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of copies of this object in the world.

```
if objclones $n > 0
  mob echo Check passed.
endif
```

---

### objcond

**Syntax:** `objcond $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Object condition (0-100).

```
if objcond $p < 20
  mob echo That item is badly damaged.
endif
```

---

### objcorpse

**Syntax:** `objcorpse $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is a corpse.

```
if objcorpse $n npc
  mob echo Check passed.
endif
```

---

### objcost

**Syntax:** `objcost $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Object base cost.

```
if objcost $n > 0
  mob echo Check passed.
endif
```

---

### objexists

**Syntax:** `objexists <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if an object with the given vnum exists in the world.

```
if objexists example
  mob echo Check passed.
endif
```

---

### objextra

**Syntax:** `objextra $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object has the named extra flag.

```
if objextra $p glow
  mob echo It glows softly.
endif
```

---

### objextra2

**Syntax:** `objextra2 $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object has the named extra2 flag.

```
if objextra2 $n flag_name
  mob echo Check passed.
endif
```

---

### objextra3

**Syntax:** `objextra3 $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object has the named extra3 flag.

```
if objextra3 $n flag_name
  mob echo Check passed.
endif
```

---

### objextra4 **New**{: .label .label-green }

**Syntax:** `objextra4 $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object has the named extra4 flag.

```
if objextra4 $n flag_name
  mob echo Check passed.
endif
```

---

### objfrag

**Syntax:** `objfrag $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object can be fragmented.

```
if objfrag $n
  mob echo Check passed.
endif
```

---

### objhere

**Syntax:** `objhere $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if an object matching the keyword is in the current room.

```
if objhere $n safe
  mob echo Check passed.
endif
```

---

### objmaxrepairs **New**{: .label .label-green }

**Syntax:** `objmaxrepairs $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the maximum repairs allowed for the object.

```
if objmaxrepairs $n > 0
  mob echo Check passed.
endif
```

---

### objmaxweight

**Syntax:** `objmaxweight $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Maximum weight a container can hold.

```
if objmaxweight $n > 0
  mob echo Check passed.
endif
```

---

### objranged

**Syntax:** `objranged $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is a ranged weapon.

```
if objranged $n sword
  mob echo Check passed.
endif
```

---

### objrepairs **New**{: .label .label-green }

**Syntax:** `objrepairs $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the current repair count of the object.

```
if objrepairs $n > 0
  mob echo Check passed.
endif
```

---

### objtimer

**Syntax:** `objtimer $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Object decay timer.

```
if objtimer $n > 0
  mob echo Check passed.
endif
```

---

### objtype

**Syntax:** `objtype $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is of the named type (weapon, armor, food, etc.).

```
if objtype $p weapon
  mob echo That is a weapon.
endif
```

---

### objval0 **New**{: .label .label-green }

**Syntax:** `objval0 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns object value 0 (v0).

```
if objval0 $p > 10
  mob echo Object value 0 is high.
endif
```

---

### objval1 **New**{: .label .label-green }

**Syntax:** `objval1 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns object value 1 (v1).

```
if objval1 $n > 0
  mob echo Check passed.
endif
```

---

### objval2 **New**{: .label .label-green }

**Syntax:** `objval2 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns object value 2 (v2).

```
if objval2 $n > 0
  mob echo Check passed.
endif
```

---

### objval3 **New**{: .label .label-green }

**Syntax:** `objval3 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns object value 3 (v3).

```
if objval3 $n > 0
  mob echo Check passed.
endif
```

---

### objval4 **New**{: .label .label-green }

**Syntax:** `objval4 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns object value 4 (v4).

```
if objval4 $n > 0
  mob echo Check passed.
endif
```

---

### objval5 **New**{: .label .label-green }

**Syntax:** `objval5 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns object value 5 (v5).

```
if objval5 $n > 0
  mob echo Check passed.
endif
```

---

### objval6 **New**{: .label .label-green }

**Syntax:** `objval6 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns object value 6 (v6).

```
if objval6 $n > 0
  mob echo Check passed.
endif
```

---

### objval7 **New**{: .label .label-green }

**Syntax:** `objval7 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns object value 7 (v7).

```
if objval7 $n > 0
  mob echo Check passed.
endif
```

---

### objval8 **New**{: .label .label-green }

**Syntax:** `objval8 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns object value 8 (v8).

```
if objval8 $n > 0
  mob echo Check passed.
endif
```

---

### objval9 **New**{: .label .label-green }

**Syntax:** `objval9 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns object value 9 (v9).

```
if objval9 $n > 0
  mob echo Check passed.
endif
```

---

### objweapon

**Syntax:** `objweapon $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is a weapon of the named type.

```
if objweapon $n sword
  mob echo Check passed.
endif
```

---

### objweaponstat

**Syntax:** `objweaponstat $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the weapon has the named weapon stat.

```
if objweaponstat $n sword
  mob echo Check passed.
endif
```

---

### objwear

**Syntax:** `objwear $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object can be worn at the named location.

```
if objwear $n keyword
  mob echo Check passed.
endif
```

---

### objwearloc

**Syntax:** `objwearloc $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is currently at the named wear location.

```
if objwearloc $n keyword
  mob echo Check passed.
endif
```

---

### objweight

**Syntax:** `objweight $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Object weight.

```
if objweight $n > 0
  mob echo Check passed.
endif
```

---

### objweightleft

**Syntax:** `objweightleft $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Remaining capacity of a container.

```
if objweightleft $n > 0
  mob echo Check passed.
endif
```

---

### order

**Syntax:** `order $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** Mob and Object programs only

The order of the item in a list.

```
if order $n > 0
  mob echo Check passed.
endif
```

---

### portal

**Syntax:** `portal $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object is a portal.

```
if portal $n normal
  mob echo Check passed.
endif
```

---

### portalexit

**Syntax:** `portalexit $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the portal has a valid exit.

```
if portalexit $n normal
  mob echo Check passed.
endif
```

---

### uses

**Syntax:** `uses $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object has remaining uses.

```
if uses $n example
  mob echo Check passed.
endif
```

---

### valueportaltype **New**{: .label .label-green }

**Syntax:** `valueportaltype $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric portal type value for the named type.

```
if valueportaltype $n example > 0
  mob echo Check passed.
endif
```

---

### wornby

**Syntax:** `wornby $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** Object programs only

True if the object is worn by the specified character.

```
if wornby $n
  mob echo Check passed.
endif
```

---

## Room-Specific Checks

*28 ifchecks*

### clones

**Syntax:** `clones $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** Mob programs only

Number of clones of the current mob.

```
if clones $n > 0
  mob echo Check passed.
endif
```

---

### dungeonflag

**Syntax:** `dungeonflag $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the dungeon has the named flag.

```
if dungeonflag $n
  mob echo Check passed.
endif
```

---

### exitexists

**Syntax:** `exitexists [$n] <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if an exit exists in the specified direction.

```
if exitexists north
  mob echo There is an exit north.
endif
```

---

### exitflag

**Syntax:** `exitflag $n <argument> <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if an exit has the named flag.

```
if exitflag $n flag_name
  mob echo Check passed.
endif
```

---

### innature

**Syntax:** `innature $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the room is a nature sector.

```
if innature $n
  mob echo Nature surrounds you.
endif
```

---

### inwilds

**Syntax:** `inwilds`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is in the wilderness.

```
if inwilds
  mob echo You are in the wilderness.
endif
```

---

### isareaunlocked

**Syntax:** `isareaunlocked $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the area is unlocked for the character.

```
if isareaunlocked $n
  mob echo Check passed.
endif
```

---

### iscloneroom

**Syntax:** `iscloneroom $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the room is a cloned instance room.

```
if iscloneroom $n
  mob echo Check passed.
endif
```

---

### isroomdark

**Syntax:** `isroomdark $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the room is dark.

```
if isroomdark $n
  mob echo It is very dark.
endif
```

---

### issafe

**Syntax:** `issafe $n [<argument>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the room is a safe room.

```
if issafe $n
  mob echo No fighting here.
endif
```

---

### istreasureroom

**Syntax:** `istreasureroom $n $p`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the room is a treasure room.

```
if istreasureroom $n $p
  mob echo Check passed.
endif
```

---

### mobclones

**Syntax:** `mobclones $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of copies of a mob in the world.

```
if mobclones $n > 0
  mob echo Check passed.
endif
```

---

### mobexists

**Syntax:** `mobexists <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if a mob with the given vnum exists in the world.

```
if mobexists 3001
  mob echo The guardian exists.
endif
```

---

### mobhere

**Syntax:** `mobhere <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if a mob with the given vnum is in the current room.

```
if mobhere 3001
  mob echo The guardian is here.
endif
```

---

### mobs

**Syntax:** `mobs $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of NPCs currently in the room.

```
if mobs $n > 3
  mob echo Many creatures lurk here.
endif
```

---

### people

**Syntax:** `people $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of characters in the entity's current room.

```
if people $n > 5
  mob echo This room is crowded.
endif
```

---

### players

**Syntax:** `players $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of players in the room.

```
if players $n == 0
  mob echo No players here.
endif
```

---

### room

**Syntax:** `room $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the vnum of the room the entity is in.

```
if room $n == 3001
  mob echo Welcome to Midgaard!
endif
```

---

### roomflag

**Syntax:** `roomflag $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the room has the named room flag.

```
if roomflag $n safe
  mob echo This is a safe room.
endif
```

---

### roomflag2

**Syntax:** `roomflag2 $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the room has the named room2 flag.

```
if roomflag2 $n flag_name
  mob echo Check passed.
endif
```

---

### roomviewwilds

**Syntax:** `roomviewwilds $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

View range in wilderness.

```
if roomviewwilds $n > 0
  mob echo Check passed.
endif
```

---

### roomweight

**Syntax:** `roomweight $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Total weight of objects in the room.

```
if roomweight $n > 0
  mob echo Check passed.
endif
```

---

### roomwilds

**Syntax:** `roomwilds $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Wilds map ID.

```
if roomwilds $n > 0
  mob echo Check passed.
endif
```

---

### roomx

**Syntax:** `roomx $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Wilderness X coordinate of the room.

```
if roomx $n > 0
  mob echo Check passed.
endif
```

---

### roomy

**Syntax:** `roomy $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Wilderness Y coordinate of the room.

```
if roomy $n > 0
  mob echo Check passed.
endif
```

---

### roomz

**Syntax:** `roomz $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Wilderness Z (floor) coordinate.

```
if roomz $n > 0
  mob echo Check passed.
endif
```

---

### sectionflag

**Syntax:** `sectionflag $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the room's blueprint section has the named flag.

```
if sectionflag $n
  mob echo Check passed.
endif
```

---

### sector

**Syntax:** `sector $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the room's sector type matches.

```
if sector $n forest
  mob echo The trees surround you.
endif
```

---

## Group and Following

*21 ifchecks*

### groupcon

**Syntax:** `groupcon $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Highest CON in group.

```
if groupcon $n > 0
  mob echo Check passed.
endif
```

---

### groupdex

**Syntax:** `groupdex $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Highest DEX in group.

```
if groupdex $n > 0
  mob echo Check passed.
endif
```

---

### grouphit

**Syntax:** `grouphit $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Total HP of all group members.

```
if grouphit $n > 0
  mob echo Check passed.
endif
```

---

### groupint

**Syntax:** `groupint $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Highest INT in group.

```
if groupint $n > 0
  mob echo Check passed.
endif
```

---

### groupmana

**Syntax:** `groupmana $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Total mana of group.

```
if groupmana $n > 0
  mob echo Check passed.
endif
```

---

### groupmaxhit

**Syntax:** `groupmaxhit $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Max HP total of group.

```
if groupmaxhit $n > 0
  mob echo Check passed.
endif
```

---

### groupmaxmana

**Syntax:** `groupmaxmana $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Max mana total of group.

```
if groupmaxmana $n > 0
  mob echo Check passed.
endif
```

---

### groupmaxmove

**Syntax:** `groupmaxmove $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Max moves total of group.

```
if groupmaxmove $n > 0
  mob echo Check passed.
endif
```

---

### groupmove

**Syntax:** `groupmove $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Total moves of group.

```
if groupmove $n > 0
  mob echo Check passed.
endif
```

---

### groupstr

**Syntax:** `groupstr $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Highest STR in group.

```
if groupstr $n > 0
  mob echo Check passed.
endif
```

---

### groupwis

**Syntax:** `groupwis $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Highest WIS in group.

```
if groupwis $n > 0
  mob echo Check passed.
endif
```

---

### grpsize

**Syntax:** `grpsize $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of members in the character's group.

```
if grpsize $n > 3
  mob echo You have a large group.
endif
```

---

### ischarm

**Syntax:** `ischarm $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is charmed.

```
if ischarm $n
  mob echo Check passed.
endif
```

---

### isfollow

**Syntax:** `isfollow $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is following someone.

```
if isfollow $n
  mob echo Check passed.
endif
```

---

### isleader

**Syntax:** `isleader $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is the group leader.

```
if isleader $n
  mob echo Check passed.
endif
```

---

### pgroupcon

**Syntax:** `pgroupcon $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Player-only group CON aggregate.

```
if pgroupcon $n > 0
  mob echo Check passed.
endif
```

---

### pgroupdex

**Syntax:** `pgroupdex $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Player-only group DEX aggregate.

```
if pgroupdex $n > 0
  mob echo Check passed.
endif
```

---

### pgroupint

**Syntax:** `pgroupint $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Player-only group INT aggregate.

```
if pgroupint $n > 0
  mob echo Check passed.
endif
```

---

### pgroupstr

**Syntax:** `pgroupstr $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Player-only group STR aggregate.

```
if pgroupstr $n > 0
  mob echo Check passed.
endif
```

---

### pgroupwis

**Syntax:** `pgroupwis $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Player-only group WIS aggregate.

```
if pgroupwis $n > 0
  mob echo Check passed.
endif
```

---

### samegroup

**Syntax:** `samegroup $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if two characters are in the same group.

```
if samegroup $n sentinel
  mob echo Check passed.
endif
```

---

## Tokens

*5 ifchecks*

### hastoken

**Syntax:** `hastoken $n <value>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character has a token with the given widevnum.

```
if hastoken $n 500
  mob echo You have the quest token.
endif
```

---

### tokencount

**Syntax:** `tokencount $n [<value>] <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of tokens the character carries.

```
if tokencount $n > 5
  mob echo You have many tokens.
endif
```

---

### tokenexists

**Syntax:** `tokenexists <value>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if a token with the given vnum exists in the world.

```
if tokenexists 50
  mob echo Check passed.
endif
```

---

### tokentimer

**Syntax:** `tokentimer $n <value> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Remaining timer on the specified token.

```
if tokentimer $n 100 > 0
  mob echo Check passed.
endif
```

---

### tokenvalue

**Syntax:** `tokenvalue $n <value> <value> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Value field of the specified token.

```
if tokenvalue $n 100 > 0
  mob echo Check passed.
endif
```

---

## Variables

*10 ifchecks (5 new)*

### tempstore1 **New**{: .label .label-green }

**Syntax:** `tempstore1 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns temporary storage value 1 for the entity.

```
if tempstore1 $n > 0
  mob echo Check passed.
endif
```

---

### tempstore2 **New**{: .label .label-green }

**Syntax:** `tempstore2 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns temporary storage value 2 for the entity.

```
if tempstore2 $n > 0
  mob echo Check passed.
endif
```

---

### tempstore3 **New**{: .label .label-green }

**Syntax:** `tempstore3 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns temporary storage value 3 for the entity.

```
if tempstore3 $n > 0
  mob echo Check passed.
endif
```

---

### tempstore4 **New**{: .label .label-green }

**Syntax:** `tempstore4 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns temporary storage value 4 for the entity.

```
if tempstore4 $n > 0
  mob echo Check passed.
endif
```

---

### tempstore5 **New**{: .label .label-green }

**Syntax:** `tempstore5 $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns temporary storage value 5 for the entity.

```
if tempstore5 $n > 0
  mob echo Check passed.
endif
```

---

### varbool

**Syntax:** `varbool <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the variable has a truthy value.

```
if varbool example
  mob echo Check passed.
endif
```

---

### vardefined

**Syntax:** `vardefined <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if a named variable is set on the entity.

```
if vardefined quest_started
  mob echo Quest is in progress.
endif
```

---

### varexit

**Syntax:** `varexit <argument> <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the variable references a valid exit room.

```
if varexit example
  mob echo Check passed.
endif
```

---

### varnumber

**Syntax:** `varnumber <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named variable.

```
if varnumber quest_stage > 3
  mob echo You are past stage 3.
endif
```

---

### varstring

**Syntax:** `varstring <argument> <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the variable equals the comparison string.

```
if varstring quest_status complete
  mob echo Quest complete!
endif
```

---

## Map and Wilderness

*14 ifchecks*

### angle

**Syntax:** `angle $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Direction angle (for wilderness movement).

```
if angle $n > 0
  mob echo Check passed.
endif
```

---

### areahasland

**Syntax:** `areahasland`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the area has a wilderness land allocation.

```
if areahasland
  mob echo Check passed.
endif
```

---

### areaid

**Syntax:** `areaid` or `areaid <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the area UID.

```
if areaid > 0
  mob echo Check passed.
endif
```

---

### arealandx

**Syntax:** `arealandx` or `arealandx <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

X coordinate of the area's land on the wilderness map.

```
if arealandx > 0
  mob echo Check passed.
endif
```

---

### arealandy

**Syntax:** `arealandy` or `arealandy <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Y coordinate of the area's land.

```
if arealandy > 0
  mob echo Check passed.
endif
```

---

### areax

**Syntax:** `areax` or `areax <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

X position of the entity in the area's local map.

```
if areax > 0
  mob echo Check passed.
endif
```

---

### areay

**Syntax:** `areay` or `areay <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Y position in local map.

```
if areay > 0
  mob echo Check passed.
endif
```

---

### maparea

**Syntax:** `maparea` or `maparea <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Map area ID.

```
if maparea > 0
  mob echo Check passed.
endif
```

---

### mapheight

**Syntax:** `mapheight` or `mapheight <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Height of the map.

```
if mapheight > 0
  mob echo Check passed.
endif
```

---

### mapid

**Syntax:** `mapid` or `mapid <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current map ID.

```
if mapid > 0
  mob echo Check passed.
endif
```

---

### mapvalid

**Syntax:** `mapvalid`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character has a valid map position.

```
if mapvalid
  mob echo Check passed.
endif
```

---

### mapwidth

**Syntax:** `mapwidth` or `mapwidth <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Width of the map.

```
if mapwidth > 0
  mob echo Check passed.
endif
```

---

### mapx

**Syntax:** `mapx` or `mapx <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Map X position.

```
if mapx > 0
  mob echo Check passed.
endif
```

---

### mapy

**Syntax:** `mapy` or `mapy <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Map Y position.

```
if mapy > 0
  mob echo Check passed.
endif
```

---

## Time

*9 ifchecks*

### day

**Syntax:** `day` or `day <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current in-game day.

```
if day > 0
  mob echo Check passed.
endif
```

---

### hour

**Syntax:** `hour $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current in-game hour (0-23).

```
if hour $n > 18
  mob echo Evening is upon us.
endif
```

---

### ismoonup

**Syntax:** `ismoonup`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the moon is up.

```
if ismoonup
  mob echo Check passed.
endif
```

---

### month

**Syntax:** `month` or `month <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current in-game month.

```
if month > 0
  mob echo Check passed.
endif
```

---

### moonphase

**Syntax:** `moonphase` or `moonphase <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current moon phase (0-7).

```
if moonphase > 0
  mob echo Check passed.
endif
```

---

### sunlight

**Syntax:** `sunlight $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns 0=dark, 1=sunrise, 2=daytime, 3=sunset.

```
if sunlight $n > 0
  mob echo Check passed.
endif
```

---

### systemtime

**Syntax:** `systemtime` or `systemtime <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns real-world Unix timestamp.

```
if systemtime > 0
  mob echo Check passed.
endif
```

---

### timeofday

**Syntax:** `timeofday <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if current time matches `day`, `dusk`, `night`, or `dawn`.

```
if timeofday night
  mob echo The night is dark.
endif
```

---

### year

**Syntax:** `year` or `year <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Current in-game year.

```
if year > 0
  mob echo Check passed.
endif
```

---

## Random and Math

*12 ifchecks*

### abs

**Syntax:** `abs <value>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

Returns the absolute value.

```
if abs 50
  mob echo Check passed.
endif
```

---

### bit

**Syntax:** `bit`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

Tests a specific bit in a bitmask.

```
if bit
  mob echo Check passed.
endif
```

---

### cos

**Syntax:** `cos <value> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Cosine of angle.

```
if cos 10 > 0
  mob echo Check passed.
endif
```

---

### dice

**Syntax:** `dice <value> <value> [<value>] <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Rolls NdS dice and returns the sum.

```
if dice 2 6 > 8
  mob echo You rolled well!
endif
```

---

### max

**Syntax:** `max $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the larger of two values.

```
if max $n > 0
  mob echo Check passed.
endif
```

---

### min

**Syntax:** `min $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the smaller of two values.

```
if min $n > 0
  mob echo Check passed.
endif
```

---

### rand

**Syntax:** `rand <value> [<value>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True randomly, with the given percent probability.

```
if rand 25
  mob echo Lucky you! (25% chance)
endif
```

---

### randpoint

**Syntax:** `randpoint <value> [<value>]`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

Used for random waypoint checks.

```
if randpoint 50
  mob echo Check passed.
endif
```

---

### register

**Syntax:** `register <value> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns a temporary register value.

```
if register 10 > 0
  mob echo Check passed.
endif
```

---

### roll

**Syntax:** `roll $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

Rolls a die; true if roll > half of sides.

```
if roll $n
  mob echo Check passed.
endif
```

---

### sign

**Syntax:** `sign <value> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns -1, 0, or 1 indicating the sign.

```
if sign 10 > 0
  mob echo Check passed.
endif
```

---

### sin

**Syntax:** `sin <value> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Sine of angle (integer degrees).

```
if sin 10 > 0
  mob echo Check passed.
endif
```

---

## Quests and Progress

*11 ifchecks (8 new)*

### hasquest **New**{: .label .label-green }

**Syntax:** `hasquest $n <value>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has the specified quest.

```
if hasquest $n 100
  mob echo Check passed.
endif
```

---

### isquesting

**Syntax:** `isquesting $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character is on an active quest.

```
if isquesting $n
  mob echo You are on a quest.
endif
```

---

### questabandoned **New**{: .label .label-green }

**Syntax:** `questabandoned $n <value>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has abandoned the specified quest.

```
if questabandoned $n 100
  mob echo Check passed.
endif
```

---

### questactive **New**{: .label .label-green }

**Syntax:** `questactive $n <value>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has the specified quest active.

```
if questactive $n 100
  mob echo Your quest is active.
endif
```

---

### questcomplete **New**{: .label .label-green }

**Syntax:** `questcomplete $n <value>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has completed the specified quest.

```
if questcomplete $n 100
  mob echo You have completed this quest.
endif
```

---

### questcompletions **New**{: .label .label-green }

**Syntax:** `questcompletions $n <value> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the number of times the entity completed the quest.

```
if questcompletions $n 100 > 0
  mob echo Check passed.
endif
```

---

### questfailed **New**{: .label .label-green }

**Syntax:** `questfailed $n <value>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has failed the specified quest.

```
if questfailed $n 100
  mob echo Check passed.
endif
```

---

### questfailures **New**{: .label .label-green }

**Syntax:** `questfailures $n <value> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the number of times the entity failed the quest.

```
if questfailures $n 100 > 0
  mob echo Check passed.
endif
```

---

### questpoint

**Syntax:** `questpoint $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Quest points held.

```
if questpoint $n > 50
  mob echo You have many quest points.
endif
```

---

### queststage **New**{: .label .label-green }

**Syntax:** `queststage $n <value> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the current stage of the specified quest.

```
if queststage $n 100 > 2
  mob echo You are past stage 2.
endif
```

---

### totalquests

**Syntax:** `totalquests $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Total quests completed.

```
if totalquests $n > 10
  mob echo An experienced quester!
endif
```

---

## Events

*10 ifchecks (10 new)*

### eventactive **New**{: .label .label-green }

**Syntax:** `eventactive <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the named event is currently active.

```
if eventactive invasion
  mob echo The invasion event is active!
endif
```

---

### eventbracket **New**{: .label .label-green }

**Syntax:** `eventbracket $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the bracket level of the current event.

```
if eventbracket $n > 0
  mob echo Check passed.
endif
```

---

### eventgoal **New**{: .label .label-green }

**Syntax:** `eventgoal <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the goal value for the named event.

```
if eventgoal example > 0
  mob echo Check passed.
endif
```

---

### eventitems **New**{: .label .label-green }

**Syntax:** `eventitems <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the item count for the named event.

```
if eventitems example > 0
  mob echo Check passed.
endif
```

---

### eventkills **New**{: .label .label-green }

**Syntax:** `eventkills <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the kill count for the named event.

```
if eventkills example > 0
  mob echo Check passed.
endif
```

---

### eventphase **New**{: .label .label-green }

**Syntax:** `eventphase <argument> <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the named event is in the specified phase.

```
if eventphase example
  mob echo Check passed.
endif
```

---

### eventsourcebracket **New**{: .label .label-green }

**Syntax:** `eventsourcebracket $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the bracket of the event source entity.

```
if eventsourcebracket $n > 0
  mob echo Check passed.
endif
```

---

### eventsourceinstance **New**{: .label .label-green }

**Syntax:** `eventsourceinstance $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the instance of the event source entity.

```
if eventsourceinstance $n > 0
  mob echo Check passed.
endif
```

---

### eventsourceuid **New**{: .label .label-green }

**Syntax:** `eventsourceuid $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the UID of the event source entity.

```
if eventsourceuid $n > 0
  mob echo Check passed.
endif
```

---

### haseventsource **New**{: .label .label-green }

**Syntax:** `haseventsource $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has an event source.

```
if haseventsource $n
  mob echo Check passed.
endif
```

---

## Classes and Skills

*14 ifchecks (14 new)*

### availskill **New**{: .label .label-green }

**Syntax:** `availskill $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the named skill is available to the entity.

```
if availskill $n dodge
  mob echo Check passed.
endif
```

---

### availsong **New**{: .label .label-green }

**Syntax:** `availsong $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the named song is available to the entity.

```
if availsong $n battle_hymn
  mob echo Check passed.
endif
```

---

### availspell **New**{: .label .label-green }

**Syntax:** `availspell $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the named spell is available to the entity.

```
if availspell $n fireball
  mob echo Check passed.
endif
```

---

### availtrait **New**{: .label .label-green }

**Syntax:** `availtrait $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the named trait is available to the entity.

```
if availtrait $n night_vision
  mob echo Check passed.
endif
```

---

### class **New**{: .label .label-green }

**Syntax:** `class $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity matches the named class.

```
if class $n warrior
  mob echo You are a warrior.
endif
```

---

### classcaster **New**{: .label .label-green }

**Syntax:** `classcaster $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity's class is a caster class.

```
if classcaster $n
  mob echo Check passed.
endif
```

---

### classcombat **New**{: .label .label-green }

**Syntax:** `classcombat $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity's class is a combat class.

```
if classcombat $n
  mob echo Check passed.
endif
```

---

### classcount **New**{: .label .label-green }

**Syntax:** `classcount $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the number of classes the entity has.

```
if classcount $n > 0
  mob echo Check passed.
endif
```

---

### classlevel **New**{: .label .label-green }

**Syntax:** `classlevel $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the entity's level in the specified class.

```
if classlevel $n warrior > 10
  mob echo You are an experienced warrior.
endif
```

---

### hasclass **New**{: .label .label-green }

**Syntax:** `hasclass $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has the named class.

```
if hasclass $n mage
  mob echo You have the mage class.
endif
```

---

### hasskill **New**{: .label .label-green }

**Syntax:** `hasskill $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has the named skill.

```
if hasskill $n parry
  mob echo You know how to parry.
endif
```

---

### hassong **New**{: .label .label-green }

**Syntax:** `hassong $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has the named song.

```
if hassong $n battle_hymn
  mob echo Check passed.
endif
```

---

### isclass **New**{: .label .label-green }

**Syntax:** `isclass $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity's primary class matches the argument.

```
if isclass $n warrior
  mob echo Check passed.
endif
```

---

### isspell **New**{: .label .label-green }

**Syntax:** `isspell $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the object/entity is spell-related.

```
if isspell $n
  mob echo Check passed.
endif
```

---

## Missions and PK Totals

*8 ifchecks (8 new)*

### missionpoint **New**{: .label .label-green }

**Syntax:** `missionpoint $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the entity's mission points.

```
if missionpoint $n > 0
  mob echo Check passed.
endif
```

---

### onmission **New**{: .label .label-green }

**Syntax:** `onmission $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is currently on a mission.

```
if onmission $n
  mob echo You are on a mission.
endif
```

---

### savage **New**{: .label .label-green }

**Syntax:** `savage $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the entity's savage/ferocity value.

```
if savage $n > 0
  mob echo Check passed.
endif
```

---

### totalmissions **New**{: .label .label-green }

**Syntax:** `totalmissions $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns total missions completed by the entity.

```
if totalmissions $n > 0
  mob echo Check passed.
endif
```

---

### totalpkfights **New**{: .label .label-green }

**Syntax:** `totalpkfights $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns total PK fights for the entity.

```
if totalpkfights $n > 0
  mob echo Check passed.
endif
```

---

### totalpkloss **New**{: .label .label-green }

**Syntax:** `totalpkloss $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns total PK losses for the entity.

```
if totalpkloss $n > 0
  mob echo Check passed.
endif
```

---

### totalpkratio **New**{: .label .label-green }

**Syntax:** `totalpkratio $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns PK win/loss ratio for the entity.

```
if totalpkratio $n > 0
  mob echo Check passed.
endif
```

---

### totalpkwins **New**{: .label .label-green }

**Syntax:** `totalpkwins $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns total PK wins for the entity.

```
if totalpkwins $n > 0
  mob echo Check passed.
endif
```

---

## Traits

*4 ifchecks (4 new)*

### hastrait **New**{: .label .label-green }

**Syntax:** `hastrait $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has the named trait.

```
if hastrait $n night_vision
  mob echo You can see in the dark.
endif
```

---

### traitbool **New**{: .label .label-green }

**Syntax:** `traitbool $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

Returns boolean value of the named trait.

```
if traitbool $n can_fly
  mob echo You can fly!
endif
```

---

### traitint **New**{: .label .label-green }

**Syntax:** `traitint $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns integer value of the named trait.

```
if traitint $n strength_bonus > 5
  mob echo Very strong trait!
endif
```

---

### traitstring **New**{: .label .label-green }

**Syntax:** `traitstring $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity's trait string matches the argument.

```
if traitstring $n night_vision
  mob echo Check passed.
endif
```

---

## Flag Lookups

*12 ifchecks (12 new)*

### flagcontainer **New**{: .label .label-green }

**Syntax:** `flagcontainer` or `flagcontainer <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named container flag.

```
if flagcontainer > 0
  mob echo Check passed.
endif
```

---

### flagcorpse **New**{: .label .label-green }

**Syntax:** `flagcorpse` or `flagcorpse <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named corpse flag.

```
if flagcorpse > 0
  mob echo Check passed.
endif
```

---

### flagexit **New**{: .label .label-green }

**Syntax:** `flagexit` or `flagexit <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named exit flag.

```
if flagexit > 0
  mob echo Check passed.
endif
```

---

### flagextra **New**{: .label .label-green }

**Syntax:** `flagextra` or `flagextra <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named extra flag.

```
if flagextra > 0
  mob echo Check passed.
endif
```

---

### flagextra2 **New**{: .label .label-green }

**Syntax:** `flagextra2` or `flagextra2 <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named extra2 flag.

```
if flagextra2 > 0
  mob echo Check passed.
endif
```

---

### flagextra3 **New**{: .label .label-green }

**Syntax:** `flagextra3` or `flagextra3 <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named extra3 flag.

```
if flagextra3 > 0
  mob echo Check passed.
endif
```

---

### flagextra4 **New**{: .label .label-green }

**Syntax:** `flagextra4` or `flagextra4 <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named extra4 flag.

```
if flagextra4 > 0
  mob echo Check passed.
endif
```

---

### flagfurniture **New**{: .label .label-green }

**Syntax:** `flagfurniture` or `flagfurniture <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named furniture flag.

```
if flagfurniture > 0
  mob echo Check passed.
endif
```

---

### flaginterrupt **New**{: .label .label-green }

**Syntax:** `flaginterrupt` or `flaginterrupt <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named interrupt flag.

```
if flaginterrupt > 0
  mob echo Check passed.
endif
```

---

### flagportal **New**{: .label .label-green }

**Syntax:** `flagportal` or `flagportal <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named portal flag.

```
if flagportal > 0
  mob echo Check passed.
endif
```

---

### flagroom **New**{: .label .label-green }

**Syntax:** `flagroom` or `flagroom <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named room flag.

```
if flagroom > 0
  mob echo Check passed.
endif
```

---

### flagroom2 **New**{: .label .label-green }

**Syntax:** `flagroom2` or `flagroom2 <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value of the named room2 flag.

```
if flagroom2 > 0
  mob echo Check passed.
endif
```

---

## Miscellaneous

*94 ifchecks (18 new)*

### affectbit

**Syntax:** `affectbit $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

True if the specific affect bitmask bit is set.

```
if affectbit $n example > 0
  mob echo Check passed.
endif
```

---

### affectbit2

**Syntax:** `affectbit2 $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

True if the specific affect2 bitmask bit is set.

```
if affectbit2 $n example > 0
  mob echo Check passed.
endif
```

---

### affectedname

**Syntax:** `affectedname $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if affected by the named custom affect.

```
if affectedname $n keyword
  mob echo Check passed.
endif
```

---

### affectedspell

**Syntax:** `affectedspell $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if affected by the named spell.

```
if affectedspell $n fireball
  mob echo Check passed.
endif
```

---

### affectgroup

**Syntax:** `affectgroup $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the strength of an affect group.

```
if affectgroup $n example > 0
  mob echo Check passed.
endif
```

---

### affectlocation

**Syntax:** `affectlocation $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the modifier for an affect at the given location.

```
if affectlocation $n example > 0
  mob echo Check passed.
endif
```

---

### affectmodifier

**Syntax:** `affectmodifier $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the stat modifier for an affect.

```
if affectmodifier $n example > 0
  mob echo Check passed.
endif
```

---

### affectskill

**Syntax:** `affectskill $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the skill modifier from an affect.

```
if affectskill $n example > 0
  mob echo Check passed.
endif
```

---

### affecttimer

**Syntax:** `affecttimer $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns remaining duration of an affect.

```
if affecttimer $n example > 0
  mob echo Check passed.
endif
```

---

### aura **New**{: .label .label-green }

**Syntax:** `aura $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has the named aura.

```
if aura $n glow
  mob echo Check passed.
endif
```

---

### canhunt

**Syntax:** `canhunt $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity can be hunted.

```
if canhunt $n
  mob echo Check passed.
endif
```

---

### canpractice

**Syntax:** `canpractice $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character can practice skills.

```
if canpractice $n dodge
  mob echo Check passed.
endif
```

---

### canscare

**Syntax:** `canscare $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity can be scared.

```
if canscare $n
  mob echo Check passed.
endif
```

---

### death **New**{: .label .label-green }

**Syntax:** `death $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has the named death flag/type.

```
if death $n flag_name
  mob echo Check passed.
endif
```

---

### deitypoint

**Syntax:** `deitypoint $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Deity point total.

```
if deitypoint $n > 0
  mob echo Check passed.
endif
```

---

### exists **New**{: .label .label-green }

**Syntax:** `exists <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** Special (internal)

True if the named entity/variable exists.

```
if exists example
  mob echo Check passed.
endif
```

---

### findpath

**Syntax:** `findpath` or `findpath <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns path cost to the target vnum.

```
if findpath > 0
  mob echo Check passed.
endif
```

---

### gc **New**{: .label .label-green }

**Syntax:** `gc $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is garbage collected / invalid.

```
if gc $n
  mob echo Check passed.
endif
```

---

### hasaura **New**{: .label .label-green }

**Syntax:** `hasaura $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has the named aura effect.

```
if hasaura $n glow
  mob echo Check passed.
endif
```

---

### hascatalyst

**Syntax:** `hascatalyst $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns catalyst count for the given ID.

```
if hascatalyst $n example > 0
  mob echo Check passed.
endif
```

---

### hascheckpoint

**Syntax:** `hascheckpoint $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns checkpoint value for the given ID.

```
if hascheckpoint $n example > 0
  mob echo Check passed.
endif
```

---

### hasenviroment

**Syntax:** `hasenviroment $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns environment value for the given ID.

```
if hasenviroment $n example > 0
  mob echo Check passed.
endif
```

---

### hasfaction **New**{: .label .label-green }

**Syntax:** `hasfaction $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has a faction.

```
if hasfaction $n
  mob echo Check passed.
endif
```

---

### hasprompt

**Syntax:** `hasprompt $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character has a custom prompt set.

```
if hasprompt $n
  mob echo Check passed.
endif
```

---

### hasqueue

**Syntax:** `hasqueue $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has a queued command.

```
if hasqueue $n
  mob echo Check passed.
endif
```

---

### hasreputation **New**{: .label .label-green }

**Syntax:** `hasreputation $n <value>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has reputation with the specified faction.

```
if hasreputation $n 100
  mob echo Check passed.
endif
```

---

### hasship **New**{: .label .label-green }

**Syntax:** `hasship $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** Special (internal)

True if the entity has a ship.

```
if hasship $n
  mob echo Check passed.
endif
```

---

### hassubclass

**Syntax:** `hassubclass $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character has a subclass.

```
if hassubclass $n warrior
  mob echo Check passed.
endif
```

---

### hasvlink

**Syntax:** `hasvlink` or `hasvlink <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns number of vlinks on the entity.

```
if hasvlink > 0
  mob echo Check passed.
endif
```

---

### healregen

**Syntax:** `healregen $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

HP regeneration rate.

```
if healregen $n > 0
  mob echo Check passed.
endif
```

---

### hired

**Syntax:** `hired $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of players currently hiring this NPC.

```
if hired $n > 0
  mob echo Check passed.
endif
```

---

### hitdamage

**Syntax:** `hitdamage $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the hit damage value.

```
if hitdamage $n > 0
  mob echo Check passed.
endif
```

---

### hitdamclass

**Syntax:** `hitdamclass $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the hit deals the given damage class.

```
if hitdamclass $n
  mob echo Check passed.
endif
```

---

### hitdamtype

**Syntax:** `hitdamtype $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the hit uses the given damage type.

```
if hitdamtype $n
  mob echo Check passed.
endif
```

---

### hitdicebonus

**Syntax:** `hitdicebonus $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Bonus to hit dice roll.

```
if hitdicebonus $n > 0
  mob echo Check passed.
endif
```

---

### hitdicenumber

**Syntax:** `hitdicenumber $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Number of hit dice.

```
if hitdicenumber $n > 0
  mob echo Check passed.
endif
```

---

### hitdicetype

**Syntax:** `hitdicetype $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Die type for hit dice.

```
if hitdicetype $n > 0
  mob echo Check passed.
endif
```

---

### hitskilltype

**Syntax:** `hitskilltype $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if hit uses the given skill type.

```
if hitskilltype $n
  mob echo Check passed.
endif
```

---

### inputwait

**Syntax:** `inputwait $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is waiting for input.

```
if inputwait $n
  mob echo Check passed.
endif
```

---

### isboss

**Syntax:** `isboss $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the mob is a boss.

```
if isboss $n
  mob echo Check passed.
endif
```

---

### iscrosszone

**Syntax:** `iscrosszone $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the mob has crossed zone boundaries.

```
if iscrosszone $n
  mob echo Check passed.
endif
```

---

### isdelay

**Syntax:** `isdelay $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has a pending delayed script.

```
if isdelay $n
  mob echo Check passed.
endif
```

---

### isdungeonunlocked **New**{: .label .label-green }

**Syntax:** `isdungeonunlocked $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the dungeon is unlocked for the entity.

```
if isdungeonunlocked $n
  mob echo Check passed.
endif
```

---

### isexitvisible **New**{: .label .label-green }

**Syntax:** `isexitvisible $n $p <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the exit is visible to the entity in the specified direction.

```
if isexitvisible $n $p
  mob echo Check passed.
endif
```

---

### ishired

**Syntax:** `ishired $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the NPC is hired.

```
if ishired $n
  mob echo Check passed.
endif
```

---

### ishunting

**Syntax:** `ishunting $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the mob is actively hunting.

```
if ishunting $n
  mob echo Check passed.
endif
```

---

### isowner

**Syntax:** `isowner $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity is the owner of something.

```
if isowner $n
  mob echo Check passed.
endif
```

---

### ispersist

**Syntax:** `ispersist $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the mob is marked as persistent.

```
if ispersist $n
  mob echo Check passed.
endif
```

---

### isprey

**Syntax:** `isprey $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the mob is flagged as prey.

```
if isprey $n
  mob echo Check passed.
endif
```

---

### isprog **New**{: .label .label-green }

**Syntax:** `isprog $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has the named program attached.

```
if isprog $n my_prog
  mob echo Check passed.
endif
```

---

### isshopkeeper **New**{: .label .label-green }

**Syntax:** `isshopkeeper $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the mobile is a shopkeeper.

```
if isshopkeeper $n
  mob echo Check passed.
endif
```

---

### issubclass

**Syntax:** `issubclass $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character has a subclass.

```
if issubclass $n warrior
  mob echo Check passed.
endif
```

---

### isvalid **New**{: .label .label-green }

**Syntax:** `isvalid $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity reference is valid (non-null).

```
if isvalid $n
  mob echo Entity is valid.
endif
```

---

### isvaliditem **New**{: .label .label-green }

**Syntax:** `isvaliditem $n $p`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the second entity is a valid item for the first.

```
if isvaliditem $n $p
  mob echo Check passed.
endif
```

---

### iswnum **New**{: .label .label-green }

**Syntax:** `iswnum $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity has a valid widevnum.

```
if iswnum $n
  mob echo Check passed.
endif
```

---

### lastreturn

**Syntax:** `lastreturn` or `lastreturn <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Return value of the last `call`/`xcall`.

```
if lastreturn > 0
  mob echo Check passed.
endif
```

---

### listcontains

**Syntax:** `listcontains $n $p`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the list variable contains the element.

```
if listcontains $n $p
  mob echo Check passed.
endif
```

---

### loaded

**Syntax:** `loaded` or `loaded <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the load counter for this mob/object instance.

```
if loaded > 0
  mob echo Check passed.
endif
```

---

### manaregen

**Syntax:** `manaregen $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Mana regeneration rate.

```
if manaregen $n > 0
  mob echo Check passed.
endif
```

---

### manastore

**Syntax:** `manastore $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Stored mana value.

```
if manastore $n > 0
  mob echo Check passed.
endif
```

---

### mobsize

**Syntax:** `mobsize $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Size of the mob.

```
if mobsize $n > 0
  mob echo Check passed.
endif
```

---

### moveregen

**Syntax:** `moveregen $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Move regeneration rate.

```
if moveregen $n > 0
  mob echo Check passed.
endif
```

---

### number **New**{: .label .label-green }

**Syntax:** `number $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the numeric value associated with the named parameter.

```
if number $n example > 0
  mob echo Check passed.
endif
```

---

### playerexists

**Syntax:** `playerexists <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns 1 if a player with the given name exists.

```
if playerexists example > 0
  mob echo Check passed.
endif
```

---

### protocol

**Syntax:** `protocol $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the character's connection uses the named protocol.

```
if protocol $n keyword
  mob echo Check passed.
endif
```

---

### race

**Syntax:** `race $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity's race matches the named race.

```
if race $n elf
  mob echo Greetings, elf.
endif
```

---

### scriptsecurity

**Syntax:** `scriptsecurity` or `scriptsecurity <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

The security level of the last script that ran.

```
if scriptsecurity > 0
  mob echo Check passed.
endif
```

---

### sex

**Syntax:** `sex $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns 0=none, 1=male, 2=female, 3=neutral.

```
if sex $n == 1
  mob echo Hello, sir.
endif
```

---

### shiptype

**Syntax:** `shiptype $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the ship is of the named type.

```
if shiptype $n keyword
  mob echo Check passed.
endif
```

---

### skeyword

**Syntax:** `skeyword $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if text matches a keyword list.

```
if skeyword $n example
  mob echo Check passed.
endif
```

---

### strlen

**Syntax:** `strlen $n <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Length of a string.

```
if strlen $n > 0
  mob echo Check passed.
endif
```

---

### strprefix

**Syntax:** `strprefix $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if string starts with prefix.

```
if strprefix $n
  mob echo Check passed.
endif
```

---

### tempstring

**Syntax:** `tempstring $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the temp string equals the argument.

```
if tempstring $n example
  mob echo Check passed.
endif
```

---

### testhardmagic

**Syntax:** `testhardmagic $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if hard magic conditions apply.

```
if testhardmagic $n example
  mob echo Check passed.
endif
```

---

### testskill

**Syntax:** `testskill $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the skill check passes.

```
if testskill $n dodge
  mob echo Check passed.
endif
```

---

### testslowmagic

**Syntax:** `testslowmagic $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if slow magic conditions apply.

```
if testslowmagic $n example
  mob echo Check passed.
endif
```

---

### testtokenspell

**Syntax:** `testtokenspell $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if a token's spell test passes.

```
if testtokenspell $n fireball
  mob echo Check passed.
endif
```

---

### timer **New**{: .label .label-green }

**Syntax:** `timer` or `timer <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Returns the current script timer value.

```
if timer > 0
  mob echo Check passed.
endif
```

---

### valueac

**Syntax:** `valueac` or `valueac <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Armor class value.

```
if valueac > 0
  mob echo Check passed.
endif
```

---

### valueacstr

**Syntax:** `valueacstr` or `valueacstr <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

AC strength value.

```
if valueacstr > 0
  mob echo Check passed.
endif
```

---

### valuedamage

**Syntax:** `valuedamage` or `valuedamage <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Damage value.

```
if valuedamage > 0
  mob echo Check passed.
endif
```

---

### valueposition

**Syntax:** `valueposition` or `valueposition <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Required position value.

```
if valueposition > 0
  mob echo Check passed.
endif
```

---

### valueranged

**Syntax:** `valueranged` or `valueranged <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Ranged combat value.

```
if valueranged > 0
  mob echo Check passed.
endif
```

---

### valuerelic

**Syntax:** `valuerelic` or `valuerelic <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Relic value.

```
if valuerelic > 0
  mob echo Check passed.
endif
```

---

### valuesector

**Syntax:** `valuesector` or `valuesector <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Sector type value.

```
if valuesector > 0
  mob echo Check passed.
endif
```

---

### valuesize

**Syntax:** `valuesize` or `valuesize <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Size value.

```
if valuesize > 0
  mob echo Check passed.
endif
```

---

### valuetoxin

**Syntax:** `valuetoxin` or `valuetoxin <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Toxin value.

```
if valuetoxin > 0
  mob echo Check passed.
endif
```

---

### valuetype

**Syntax:** `valuetype` or `valuetype <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Type field value.

```
if valuetype > 0
  mob echo Check passed.
endif
```

---

### valueweapon

**Syntax:** `valueweapon` or `valueweapon <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Weapon type value.

```
if valueweapon > 0
  mob echo Check passed.
endif
```

---

### valuewear

**Syntax:** `valuewear` or `valuewear <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Wear location value.

```
if valuewear > 0
  mob echo Check passed.
endif
```

---

### weaponskill

**Syntax:** `weaponskill $n <argument> <operator> <value>`\
**Returns:** Numeric\
**Entity Types:** All

Value of the current weapon skill.

```
if weaponskill $n example > 0
  mob echo Check passed.
endif
```

---

### weapontype

**Syntax:** `weapontype $n <argument>`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity uses the named weapon type.

```
if weapontype $n sword
  mob echo Check passed.
endif
```

---

### wnumvalid **New**{: .label .label-green }

**Syntax:** `wnumvalid $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the entity's widevnum is valid.

```
if wnumvalid $n
  mob echo Check passed.
endif
```

---

### word

**Syntax:** `word $n`\
**Returns:** Boolean (true/false)\
**Entity Types:** All

True if the nth word of the string matches.

```
if word $n
  mob echo Check passed.
endif
```

---

