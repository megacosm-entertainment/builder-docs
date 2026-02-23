---
layout: default
title: Scripting IFChecks Reference
nav_order: 11
parent: Scripting
---

# Scripting IFChecks Reference
{: .no_toc }

IFChecks are predicates and value-getters used in `if` conditions within scripts. Most checks work on any entity type (mob, obj, room, token, area), though a few are entity-specific.

**Syntax:** `if <check> <entity> [args...] [operator value]`

Entity args use `$` quick codes or `$(entity.field)` expressions. The new direct-comparison syntax is also supported:

```
if ispc $n
  say Hello, player!
endif

if level $n >= 20
  say You're quite experienced.
endif

if hastoken $n 500
  say You already have the quest token.
endif

// Direct entity field comparison (equivalent to "if race $n human")
if $(enactor.race) == human
  say Welcome, human!
endif
```

**Return types:**
- **T/F** — tests as true/false; can be used without an operator
- **NUM** — returns a number; compare with `==`, `!=`, `<`, `>`, `<=`, `>=`

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Identity and Type

| Check | Return | Description |
|:------|:-------|:------------|
| `ispc $n` | T/F | True if the entity is a player character |
| `isnpc $n` | T/F | True if the entity is an NPC/mobile |
| `ismobile $n` | T/F | Alias for `isnpc` |
| `isobject $n` | T/F | True if the entity is an object |
| `isroom $n` | T/F | True if the entity is a room |
| `istoken $n` | T/F | True if the entity is a token |
| `isimmort $n` | T/F | True if the entity is an immortal player |
| `isangel $n` | T/F | True if the character is an angel |
| `isdemon $n` | T/F | True if the character is a demon |
| `ismystic $n` | T/F | True if the character is a mystic |
| `vnum $n` | NUM | Returns the widevnum of the entity |
| `id $n` | NUM | Returns the unique ID of the entity |
| `id2 $n` | NUM | Returns the secondary ID |
| `name $n` | T/F | True if the entity's name matches the argument |
| `identical $n` | T/F | True if two entities are identical (same object instance) |

---

## Alignment and Grouping

| Check | Return | Description |
|:------|:-------|:------------|
| `isgood $n` | T/F | True if the entity has good alignment |
| `isevil $n` | T/F | True if the entity has evil alignment |
| `isneutral $n` | T/F | True if the entity has neutral alignment |
| `align $n` | NUM | Returns the numeric alignment value (-1000 to 1000) |
| `ispk $n` | T/F | True if the character is PK-flagged |
| `iscpkproof $n` | T/F | True if the character is CPK-proof |
| `church $n` | T/F | True if the character is a member of the named church |
| `churchrank $n` | NUM | Returns the character's church rank |
| `churchsize $n` | NUM | Returns the size of the character's church |
| `churchonline $n` | NUM | Returns number of church members currently online |
| `churchhasrelic $n` | T/F | True if the character's church currently holds a relic |
| `ischurchpk $n` | T/F | True if the character is in a church PK state |
| `ischurchexcom $n` | T/F | True if the character has been excommunicated |

---

## Level, Stats, and Resources

| Check | Return | Description |
|:------|:-------|:------------|
| `level $n` | NUM | The character's level |
| `sublevel $n` | NUM | The character's sub-level |
| `isremort $n` | T/F | True if the character has remorte'd |
| `curhit $n` | NUM | Current HP |
| `curmana $n` | NUM | Current mana |
| `curmove $n` | NUM | Current movement |
| `maxhit $n` | NUM | Maximum HP |
| `maxmana $n` | NUM | Maximum mana |
| `maxmove $n` | NUM | Maximum movement |
| `hpcnt $n` | NUM | HP as a percentage (0-100) |
| `statstr $n` | NUM | Current STR stat |
| `statint $n` | NUM | Current INT stat |
| `statwis $n` | NUM | Current WIS stat |
| `statdex $n` | NUM | Current DEX stat |
| `statcon $n` | NUM | Current CON stat |
| `permstr $n` | NUM | Permanent STR (unmodified by equipment) |
| `permint $n` | NUM | Permanent INT |
| `permwis $n` | NUM | Permanent WIS |
| `permdex $n` | NUM | Permanent DEX |
| `permcon $n` | NUM | Permanent CON |
| `age $n` | NUM | Character age in game years |
| `xp $n` | NUM | Current experience points |
| `maxxp $n` | NUM | XP needed for next level |
| `gold $n` | NUM | Gold in inventory |
| `silver $n` | NUM | Silver in inventory |
| `money $n` | NUM | Total coin value (normalized) |
| `bankbalance $n` | NUM | Bank account balance |
| `practices $n` | NUM | Number of practice sessions available |
| `trains $n` | NUM | Number of train sessions available |
| `questpoint $n` | NUM | Quest points held |
| `totalquests $n` | NUM | Total quests completed |
| `deathcount $n` | NUM | Number of times the character has died |
| `pneuma $n` | NUM | Pneuma (soul energy) level |

---

## Combat State

| Check | Return | Description |
|:------|:-------|:------------|
| `isfighting $n` | T/F | True if the character is currently in combat |
| `istarget $n` | T/F | True if the character is someone's combat target |
| `hastarget $n` | T/F | True if the character has a combat target |
| `isflying $n` | T/F | True if the character is flying |
| `isriding $n` | T/F | True if the character is riding a mount |
| `isrider $n` | T/F | True if the character is being ridden |
| `isridden $n` | T/F | True if the entity (mount) is being ridden |
| `wimpy $n` | NUM | The character's wimpy (flee) threshold |
| `danger $n` | NUM | Calculated danger level of the character's situation |
| `monkills $n` | NUM | Monster kills count |
| `pkwins $n` | NUM | PK wins count |
| `pkloss $n` | NUM | PK losses count |
| `pkfights $n` | NUM | Total PK fights |
| `pkratio $n` | NUM | PK win ratio (win percentage) |
| `cpkwins $n` | NUM | CPK wins |
| `cpkloss $n` | NUM | CPK losses |
| `cpkfights $n` | NUM | CPK fights |
| `cpkratio $n` | NUM | CPK win ratio |
| `totalwins $n` | NUM | Total combat wins |
| `totalloss $n` | NUM | Total combat losses |
| `totalfights $n` | NUM | Total fights |
| `totalratio $n` | NUM | Overall win ratio |
| `arenawins $n` | NUM | Arena wins |
| `arenaloss $n` | NUM | Arena losses |
| `arenafights $n` | NUM | Arena fights |
| `arenaratio $n` | NUM | Arena win ratio |
| `reckoning $n` | NUM | Current reckoning value |
| `reckoningchance $n` | NUM | Reckoning proc chance |
| `reckoningcooldown $n` | NUM | Reckoning cooldown remaining |
| `reckoningduration $n` | NUM | Reckoning duration |
| `reckoningintensity $n` | NUM | Reckoning intensity |

---

## Conditions and Status

| Check | Return | Description |
|:------|:-------|:------------|
| `isaffectskill $n` | T/F | True if affected by a specific skill affect |
| `isaffectgroup $n` | T/F | True if affected by a group-type affect |
| `isaffectcustom $n` | T/F | True if affected by a custom-named affect |
| `isaffectwhere $n` | T/F | True if affected at a specific wear location |
| `affected $n` | T/F | True if affected by the named condition/spell |
| `affected2 $n` | T/F | True if affected by the named affect2 flag |
| `hasspell $n` | T/F | True if the character has a specific spell granted |
| `skill $n skill_name` | NUM | Returns the skill percentage for the named skill |
| `isbusy $n` | T/F | True if character is currently busy (crafting, etc.) |
| `isbrewing $n` | T/F | True if character is brewing |
| `isscribing $n` | T/F | True if character is scribing |
| `istattooing $n` | T/F | True if character is getting tattooed |
| `iscasting $n` | T/F | True if character is casting a spell |
| `iscastfailure $n` | T/F | True if the last cast failed |
| `iscastsuccess $n` | T/F | True if the last cast succeeded |
| `iscastrecovered $n` | T/F | True if recovered from casting |
| `iscastroomblocked $n` | T/F | True if the room blocks casting |
| `isshooting $n` | T/F | True if character is shooting a ranged weapon |
| `ismorphed $n` | T/F | True if character has morphed form |
| `isshifted $n` | T/F | True if character has shifted |
| `issustained $n` | T/F | True if character has a life-sustaining effect |
| `isfading $n` | T/F | True if character is fading (invisible) |
| `isvisible $n` | T/F | True if the entity is visible |
| `isvisibleto $n` | T/F | True if the entity is visible to the specified observer |
| `isactive $n` | T/F | True if the entity is active/awake |
| `isdead $n` | T/F | True if the character is dead |
| `ison $n` | T/F | True if sitting/resting on furniture |
| `pos $n` | T/F | True if character is in the named position (standing, sitting, etc.) |
| `lostparts $n` | T/F | True if character has lost body parts |
| `hunger $n` | NUM | Hunger value (positive = hungry) |
| `thirst $n` | NUM | Thirst value (positive = thirsty) |
| `drunk $n` | NUM | Drunk level |
| `fullness $n` | NUM | Fullness level |
| `stoned $n` | NUM | Stoned level (toxic high) |
| `toxin $n` | NUM | Toxin level |
| `drunk $n` | NUM | Alcohol intoxication level |
| `boost $n` | NUM | Boost value |
| `boosttimer $n` | NUM | Remaining boost duration |

---

## Act and Offensive Flags

| Check | Return | Description |
|:------|:-------|:------------|
| `act $n flag` | T/F | True if the NPC has the specified act flag (e.g. `sentinel`, `aggressive`) |
| `act2 $n flag` | T/F | True if the NPC has the specified act2 flag |
| `off $n flag` | T/F | True if the NPC/character has the specified offensive flag |
| `imm $n type` | T/F | True if immune to the damage/affect type |
| `res $n type` | T/F | True if resistant to the damage/affect type |
| `vuln $n type` | T/F | True if vulnerable to the damage/affect type |
| `parts $n part` | T/F | True if the character has the named body part |
| `comm $n flag` | T/F | True if the character has the named comm flag |
| `weapon $n type` | T/F | True if the character uses the named weapon type |
| `flagact $n` | NUM | Numeric value of the act flags bitmask |
| `flagact2 $n` | NUM | Numeric value of act2 flags |
| `flagoff $n` | NUM | Numeric value of offensive flags |
| `flagimm $n` | NUM | Numeric value of immunity flags |
| `flagres $n` | NUM | Numeric value of resistance flags |
| `flagvuln $n` | NUM | Numeric value of vulnerability flags |
| `flagaffect $n` | NUM | Numeric value of affect flags |
| `flagaffect2 $n` | NUM | Numeric value of affect2 flags |
| `flagcomm $n` | NUM | Numeric value of comm flags |
| `flagweapon $n` | NUM | Numeric value of weapon flags |
| `flagwear $n` | NUM | Numeric value of wear flags |
| `flagpart $n` | NUM | Numeric value of body part flags |
| `flagform $n` | NUM | Numeric value of form flags |

---

## Inventory and Equipment

| Check | Return | Description |
|:------|:-------|:------------|
| `carries $n keyword` | T/F | True if the character carries an item matching the keyword |
| `has $n keyword` | T/F | True if the character has (carries or wears) a matching item |
| `wears $n keyword` | T/F | True if the character is wearing a matching item |
| `wearused $n location` | T/F | True if the specified wear location is occupied |
| `carryleft $n` | NUM | Number of items the character can still carry |
| `maxcarry $n` | NUM | Maximum items the character can carry |
| `weight $n` | NUM | Total weight carried |
| `maxweight $n` | NUM | Maximum weight the character can carry |
| `weightleft $n` | NUM | Remaining weight capacity |
| `handsfull $n` | T/F | True if character has no free hands |
| `candrop $n` | T/F | True if the entity can be dropped |
| `canget $n` | T/F | True if the entity can be picked up |
| `canput $n` | T/F | True if the entity can be placed in a container |
| `canpull $n` | T/F | True if the entity can be pulled |
| `ispulling $n` | T/F | True if the character is pulling something |
| `ispullingrelic $n` | T/F | True if pulling a church relic |

---

## Object-Specific Checks

| Check | Return | Description |
|:------|:-------|:------------|
| `objtype $n type` | T/F | True if the object is of the named type (weapon, armor, food, etc.) |
| `objextra $n flag` | T/F | True if the object has the named extra flag |
| `objextra2 $n flag` | T/F | True if the object has the named extra2 flag |
| `objextra3($n, flag)` | T/F | True if the object has the named extra3 flag |
| `objwear $n location` | T/F | True if the object can be worn at the named location |
| `objwearloc $n` | T/F | True if the object is currently at the named wear location |
| `objweapon $n type` | T/F | True if the object is a weapon of the named type |
| `objweaponstat $n stat` | T/F | True if the weapon has the named weapon stat |
| `objranged $n` | T/F | True if the object is a ranged weapon |
| `objfrag $n` | T/F | True if the object can be fragmented |
| `objcorpse $n` | T/F | True if the object is a corpse |
| `objcond $n` | NUM | Object condition (0-100) |
| `objcost $n` | NUM | Object base cost |
| `objweight $n` | NUM | Object weight |
| `objmaxweight $n` | NUM | Maximum weight a container can hold |
| `objweightleft $n` | NUM | Remaining capacity of a container |
| `objtimer $n` | NUM | Object decay timer |
| `objval0($n)` ... `objval7($n)` | NUM | Object value fields 0–7 |
| `objclones $n` | NUM | Number of copies of this object in the world |
| `objexists $n vnum` | T/F | True if an object with the given vnum exists in the world |
| `objhere $n keyword` | T/F | True if an object matching the keyword is in the current room |
| `numenchants $n` | NUM | Number of enchantments on the object |
| `isrestrung $n` | T/F | True if the object has been restrung |
| `isrepairable $n` | T/F | True if the object can be repaired |
| `isworn $n` | T/F | True if the object is currently worn |
| `carriedby $n` | T/F | True if the object is in the specified character's inventory |
| `wornby $n` | T/F | True if the object is worn by the specified character |
| `iskey $n` | T/F | True if the object is a key |
| `container $n` | T/F | True if the object is a container |
| `portal $n` | T/F | True if the object is a portal |
| `portalexit $n` | T/F | True if the portal has a valid exit |
| `furniture $n` | T/F | True if the object is furniture |
| `liquid $n type` | T/F | True if the object contains the named liquid |
| `uses $n` | T/F | True if the object has remaining uses |
| `damtype $n type` | T/F | True if the weapon deals the named damage type |
| `material $n name` | T/F | True if the object is made of the named material |
| `groundweight $n` | NUM | Weight when on the ground |
| `order $n` | NUM | The order of the item in a list |

---

## Room-Specific Checks

| Check | Return | Description |
|:------|:-------|:------------|
| `room $n` | NUM | Returns the vnum of the room the entity is in |
| `roomflag $n flag` | T/F | True if the room has the named room flag |
| `roomflag2 $n flag` | T/F | True if the room has the named room2 flag |
| `sector $n type` | T/F | True if the room's sector type matches |
| `isroomdark $n` | T/F | True if the room is dark |
| `issafe $n` | T/F | True if the room is a safe room |
| `istreasureroom $n` | T/F | True if the room is a treasure room |
| `iscloneroom $n` | T/F | True if the room is a cloned instance room |
| `sectionflag $n flag` | T/F | True if the room's blueprint section has the named flag |
| `people $n` | NUM | Number of characters in the entity's current room |
| `mobs $n` | NUM | Number of NPCs in the room |
| `players $n` | NUM | Number of players in the room |
| `exitexists $n dir` | T/F | True if an exit exists in the specified direction |
| `exitflag $n dir, flag` | T/F | True if an exit has the named flag |
| `roomweight $n` | NUM | Total weight of objects in the room |
| `mobexists $n vnum` | T/F | True if a mob with the given vnum exists in the world |
| `mobhere $n vnum` | T/F | True if a mob with the given vnum is in the current room |
| `mobclones $n vnum` | NUM | Number of copies of a mob in the world |
| `clones $n` | NUM | Number of clones of the current mob |
| `mobs $n` | NUM | Number of NPCs currently in the room |
| `roomx $n` | NUM | Wilderness X coordinate of the room |
| `roomy $n` | NUM | Wilderness Y coordinate of the room |
| `roomz $n` | NUM | Wilderness Z (floor) coordinate |
| `roomwilds $n` | NUM | Wilds map ID |
| `roomviewwilds $n` | NUM | View range in wilderness |
| `innature $n` | T/F | True if the room is a nature sector |
| `inwilds $n` | T/F | True if the entity is in the wilderness |
| `isareaunlocked $n` | T/F | True if the area is unlocked for the character |
| `dungeonflag $n flag` | T/F | True if the dungeon has the named flag |

---

## Group and Following

| Check | Return | Description |
|:------|:-------|:------------|
| `grpsize $n` | NUM | Number of members in the character's group |
| `samegroup $n` | T/F | True if two characters are in the same group |
| `isleader $n` | T/F | True if the character is the group leader |
| `isfollow $n` | T/F | True if the character is following someone |
| `ischarm $n` | T/F | True if the character is charmed |
| `grouphit $n` | NUM | Total HP of all group members |
| `groupmana $n` | NUM | Total mana of group |
| `groupmove $n` | NUM | Total moves of group |
| `groupmaxhit $n` | NUM | Max HP total of group |
| `groupmaxmana $n` | NUM | Max mana total of group |
| `groupmaxmove $n` | NUM | Max moves total of group |
| `groupstr $n` | NUM | Highest STR in group |
| `groupint $n` | NUM | Highest INT in group |
| `groupwis $n` | NUM | Highest WIS in group |
| `groupdex $n` | NUM | Highest DEX in group |
| `groupcon $n` | NUM | Highest CON in group |
| `pgroupstr $n` | NUM | Player-only group STR aggregate |
| `pgroupint $n` | NUM | Player-only group INT aggregate |
| `pgroupwis $n` | NUM | Player-only group WIS aggregate |
| `pgroupdex $n` | NUM | Player-only group DEX aggregate |
| `pgroupcon $n` | NUM | Player-only group CON aggregate |

---

## Tokens

| Check | Return | Description |
|:------|:-------|:------------|
| `hastoken $n vnum` | T/F | True if the character has a token with the given widevnum |
| `tokencount $n` | NUM | Number of tokens the character carries |
| `tokenexists $n vnum` | T/F | True if a token with the given vnum exists in the world |
| `tokentimer $n vnum` | NUM | Remaining timer on the specified token |
| `tokenvalue $n vnum` | NUM | Value field of the specified token |

---

## Variables

| Check | Return | Description |
|:------|:-------|:------------|
| `vardefined $n name` | T/F | True if a named variable is set on the entity |
| `varnumber $n name` | NUM | Returns the numeric value of the named variable |
| `varstring $n name` | T/F | True if the variable equals the comparison string |
| `varbool $n name` | T/F | True if the variable has a truthy value |
| `varexit $n name` | T/F | True if the variable references a valid exit room |

---

## Map and Wilderness

| Check | Return | Description |
|:------|:-------|:------------|
| `areahasland $n` | T/F | True if the area has a wilderness land allocation |
| `areaid $n` | NUM | Returns the area UID |
| `arealandx $n` | NUM | X coordinate of the area's land on the wilderness map |
| `arealandy $n` | NUM | Y coordinate of the area's land |
| `areax $n` | NUM | X position of the entity in the area's local map |
| `areay $n` | NUM | Y position in local map |
| `maparea $n` | NUM | Map area ID |
| `mapheight $n` | NUM | Height of the map |
| `mapwidth $n` | NUM | Width of the map |
| `mapid $n` | NUM | Current map ID |
| `mapvalid $n` | T/F | True if the character has a valid map position |
| `mapx $n` | NUM | Map X position |
| `mapy $n` | NUM | Map Y position |
| `angle $n` | NUM | Direction angle (for wilderness movement) |

---

## Time

| Check | Return | Description |
|:------|:-------|:------------|
| `hour $n` | NUM | Current in-game hour (0–23) |
| `day $n` | NUM | Current in-game day |
| `month $n` | NUM | Current in-game month |
| `year $n` | NUM | Current in-game year |
| `timeofday $n time` | T/F | True if current time matches `day`, `dusk`, `night`, or `dawn` |
| `sunlight $n` | NUM | Returns 0=dark, 1=sunrise, 2=daytime, 3=sunset |
| `ismoonup $n` | T/F | True if the moon is up |
| `moonphase $n` | NUM | Current moon phase (0–7) |
| `systemtime $n` | NUM | Returns real-world Unix timestamp |

---

## Random and Math

| Check | Return | Description |
|:------|:-------|:------------|
| `rand $n pct` | T/F | True randomly, with `pct`% probability |
| `randpoint $n` | T/F | Used for random waypoint checks |
| `roll $n sides` | T/F | Rolls a die; true if roll > half of sides |
| `dice $n number, sides` | NUM | Rolls NdS dice and returns the sum |
| `max $n a, b` | NUM | Returns the larger of two values |
| `min $n a, b` | NUM | Returns the smaller of two values |
| `abs $n value` | NUM | Returns the absolute value |
| `sign $n value` | NUM | Returns -1, 0, or 1 indicating the sign |
| `sin $n angle` | NUM | Sine of angle (integer degrees) |
| `cos $n angle` | NUM | Cosine of angle |
| `bit $n value, bit` | NUM | Tests a specific bit in a bitmask |
| `register $n` | NUM | Returns a temporary register value |

---

## Quests and Progress

| Check | Return | Description |
|:------|:-------|:------------|
| `isquesting $n` | T/F | True if the character is on an active quest |
| `totalquests $n` | NUM | Total quests completed |
| `questpoint $n` | NUM | Quest points held |

---

## Miscellaneous

| Check | Return | Description |
|:------|:-------|:------------|
| `race $n name` | T/F | True if the entity's race matches the named race |
| `sex $n` | NUM | Returns 0=none, 1=male, 2=female, 3=neutral |
| `loaded $n` | NUM | Returns the load counter for this mob/object instance |
| `playerexists $n name` | NUM | Returns 1 if a player with the given name exists |
| `listcontains $n list, element` | T/F | True if the list variable contains the element |
| `strlen $n string` | NUM | Length of a string |
| `strprefix $n string, prefix` | T/F | True if string starts with prefix |
| `skeyword $n text` | T/F | True if text matches a keyword list |
| `word $n index, string` | T/F | True if the nth word of `string` matches |
| `tempstring $n str` | T/F | True if the temp string equals `str` |
| `tempstore1($n)` ... `tempstore4($n)` | NUM | Temporary storage registers |
| `hasvlink $n` | NUM | Returns number of vlinks on the entity |
| `lastreturn $n` | NUM | Return value of the last `call`/`xcall` |
| `scriptsecurity $n` | NUM | The security level of the last script that ran |
| `protocol $n name` | T/F | True if the character's connection uses the named protocol |
| `hasprompt $n` | T/F | True if the character has a custom prompt set |
| `ishired $n` | T/F | True if the NPC is hired |
| `hired $n` | NUM | Number of players currently hiring this NPC |
| `isowner $n` | T/F | True if the entity is the owner of something |
| `isprey $n` | T/F | True if the mob is flagged as prey |
| `isboss $n` | T/F | True if the mob is a boss |
| `mobsize $n` | NUM | Size of the mob |
| `shiptype $n type` | T/F | True if the ship is of the named type (`sailboat`/`airship`) |
| `issubclass $n` | T/F | True if the character has a subclass |
| `hassubclass $n` | T/F | True if the character has a subclass (alias) |
| `findpath $n vnum` | NUM | Returns path cost to the target vnum |
| `canhunt $n` | T/F | True if the entity can be hunted |
| `canscare $n` | T/F | True if the entity can be scared |
| `canpractice $n` | T/F | True if the character can practice skills |
| `ishunting $n` | T/F | True if the mob is actively hunting |
| `iscrosszone $n` | T/F | True if the mob has crossed zone boundaries |
| `isdelay $n` | T/F | True if the entity has a pending delayed script |
| `hasqueue $n` | T/F | True if the entity has a queued command |
| `inputwait $n` | T/F | True if the entity is waiting for input |
| `ispersist $n` | T/F | True if the mob is marked as persistent |
| `testhardmagic $n` | T/F | True if hard magic conditions apply |
| `testslowmagic $n` | T/F | True if slow magic conditions apply |
| `testskill $n skill` | T/F | True if the skill check passes |
| `testtokenspell $n token` | T/F | True if a token's spell test passes |
| `hascheckpoint $n id` | NUM | Returns checkpoint value for the given ID |
| `hasenviroment $n id` | NUM | Returns environment value for the given ID |
| `hascatalyst $n id` | NUM | Returns catalyst count for the given ID |
| `affectbit $n flag` | T/F | True if the specific affect bitmask bit is set |
| `affectbit2 $n flag` | T/F | True if the specific affect2 bitmask bit is set |
| `affectgroup $n group` | NUM | Returns the strength of an affect group |
| `affectlocation $n loc` | NUM | Returns the modifier for an affect at the given location |
| `affectmodifier $n loc` | NUM | Returns the stat modifier for an affect |
| `affectskill $n skill` | NUM | Returns the skill modifier from an affect |
| `affecttimer $n affect` | NUM | Returns remaining duration of an affect |
| `affectedname $n name` | T/F | True if affected by the named custom affect |
| `affectedspell $n spell` | T/F | True if affected by the named spell |
| `hitdamage $n` | NUM | Returns the hit damage value |
| `hitdamclass $n class` | T/F | True if the hit deals the given damage class |
| `hitdamtype $n type` | T/F | True if the hit uses the given damage type |
| `hitdicebonus $n` | NUM | Bonus to hit dice roll |
| `hitdicenumber $n` | NUM | Number of hit dice |
| `hitdicetype $n` | NUM | Die type for hit dice |
| `hitskilltype $n type` | T/F | True if hit uses the given skill type |
| `valueac $n` | NUM | Armor class value |
| `valueacstr $n` | NUM | AC strength value |
| `valuedamage $n` | NUM | Damage value |
| `valueposition $n` | NUM | Required position value |
| `valueranged $n` | NUM | Ranged combat value |
| `valuerelic $n` | NUM | Relic value |
| `valuesector $n` | NUM | Sector type value |
| `valuesize $n` | NUM | Size value |
| `valuetoxin $n` | NUM | Toxin value |
| `valuetype $n` | NUM | Type field value |
| `valueweapon $n` | NUM | Weapon type value |
| `valuewear $n` | NUM | Wear location value |
| `healregen $n` | NUM | HP regeneration rate |
| `manaregen $n` | NUM | Mana regeneration rate |
| `moveregen $n` | NUM | Move regeneration rate |
| `manastore $n` | NUM | Stored mana value |
| `deitypoint $n` | NUM | Deity point total |
| `weapontype $n type` | T/F | True if the entity uses the named weapon type |
| `weaponskill $n` | NUM | Value of the current weapon skill |


| Name | Types | Value |
|:-----|:------|:------|
| abs  | mob obj room token area | T/F |
| act  | mob obj room token area | T/F |
| act2 | mob obj room token area | T/F |
| affectbit | mob obj room token area | NUM |
| affectbit2 | mob obj room token area | NUM |
| affected | mob obj room token area | T/F |
| affected2 | mob obj room token area | T/F |
| affectedname | mob obj room token area | T/F |
| affectedspell | mob obj room token area | T/F |
| affectgroup | mob obj room token area | NUM |
| affectlocation | mob obj room token area | NUM |
| affectmodifier | mob obj room token area | NUM |
| affectskill | mob obj room token area | NUM |
| affecttimer | mob obj room token area | NUM |
| age | mob obj room token area | NUM |
| align | mob obj room token area | NUM |
| angle | mob obj room token area | NUM |
| areahasland | mob obj room token area | T/F |
| areaid | mob obj room token area | NUM |
| arealandx | mob obj room token area | NUM |
| arealandy | mob obj room token area | NUM |
| arenafights | mob obj room token area | NUM |
| arenaloss | mob obj room token area | NUM |
| arenaratio | mob obj room token area | NUM |
| arenawins | mob obj room token area | NUM |
| areax | mob obj room token area | NUM |
| areay | mob obj room token area | NUM |
| bankbalance | mob obj room token area | NUM |
| bit | mob obj room token area | NUM |
| boost | mob obj room token area | NUM |
| boosttimer | mob obj room token area | NUM |
| candrop | mob obj room token area | T/F |
| canget | mob obj room token area | T/F |
| canhunt | mob obj room token area | T/F |
| canpractice | mob obj room token area | T/F |
| canpull | mob obj room token area | T/F |
| canput | mob obj room token area | T/F |
| canscare | mob obj room token area | T/F |
| carriedby | obj | T/F |
| carries | mob obj room token area | T/F |
| carryleft | mob obj room token area | NUM |
| church | mob obj room token area | T/F |
| churchhasrelic | mob obj room token area | T/F |
| churchonline | mob obj room token area | NUM |
| churchrank | mob obj room token area | NUM |
| churchsize | mob obj room token area | NUM |
| clones | mob | NUM |
| comm | mob obj room token area | T/F |
| container | mob obj room token area | T/F |
| cos | mob obj room token area | NUM |
| cpkfights | mob obj room token area | NUM |
| cpkloss | mob obj room token area | NUM |
| cpkratio | mob obj room token area | NUM |
| cpkwins | mob obj room token area | NUM |
| curhit | mob obj room token area | NUM |
| curmana | mob obj room token area | NUM |
| curmove | mob obj room token area | NUM |
| damtype | mob obj room token area | T/F |
| danger | mob obj room token area | NUM |
| day | mob obj room token area | NUM |
| death | mob obj room token area | T/F |
| deathcount | mob obj room token area | NUM |
| deitypoint | mob obj room token area | NUM |
| dice | mob obj room token area | NUM |
| drunk | mob obj room token area | NUM |
| dungeonflag | mob obj room token area | T/F |
| exitexists | mob obj room token area | T/F |
| exitflag | mob obj room token area | T/F |
| findpath | mob obj room token area | NUM |
| flagact | mob obj room token area | NUM |
| flagact2 | mob obj room token area | NUM |
| flagaffect | mob obj room token area | NUM |
| flagaffect2 | mob obj room token area | NUM |
| flagcomm | mob obj room token area | NUM |
| flagcontainer | mob obj room token area | NUM |
| flagcorpse | mob obj room token area | NUM |
| flagexit | mob obj room token area | NUM |
| flagextra | mob obj room token area | NUM |
| flagextra2 | mob obj room token area | NUM |
| flagextra3 | mob obj room token area | NUM |
| flagextra4 | mob obj room token area | NUM |
| flagform | mob obj room token area | NUM |
| flagfurniture | mob obj room token area | NUM |
| flagimm | mob obj room token area | NUM |
| flaginterrupt | mob obj room token area | NUM |
| flagoff | mob obj room token area | NUM |
| flagpart | mob obj room token area | NUM |
| flagportal | mob obj room token area | NUM |
| flagres | mob obj room token area | NUM |
| flagroom | mob obj room token area | NUM |
| flagroom2 | mob obj room token area | NUM |
| flagvuln | mob obj room token area | NUM |
| flagweapon | mob obj room token area | NUM |
| flagwear | mob obj room token area | NUM |
| fullness | mob obj room token area | NUM |
| furniture | mob obj room token area | T/F |
| gold | mob obj room token area | NUM |
| groundweight | mob obj room token area | NUM |
| groupcon | mob obj room token area | NUM |
| groupdex | mob obj room token area | NUM |
| grouphit | mob obj room token area | NUM |
| groupint | mob obj room token area | NUM |
| groupmana | mob obj room token area | NUM |
| groupmaxhit | mob obj room token area | NUM |
| groupmaxmana | mob obj room token area | NUM |
| groupmaxmove | mob obj room token area | NUM |
| groupmove | mob obj room token area | NUM |
| groupstr | mob obj room token area | NUM |
| groupwis | mob obj room token area | NUM |
| grpsize | mob obj room token area | NUM |
| handsfull | mob obj room token area | T/F |
| has | mob obj room token area | T/F |
| hascatalyst | mob obj room token area | NUM |
| hascheckpoint | mob obj room token area | NUM |
| hasenviroment | mob obj room token area | NUM |
| hasprompt | mob obj room token area | T/F |
| hasqueue | mob obj room token area | T/F |
| hasspell | mob obj room token area | T/F |
| hassubclass | mob obj room token area | T/F |
| hastarget | mob obj room token area | T/F |
| hastoken | mob obj room token area | T/F |
| hasvlink | mob obj room token area | NUM |
| healregen | mob obj room token area | NUM |
| hired | mob obj room token area | NUM |
| hitdamage | mob obj room token area | NUM |
| hitdamclass | mob obj room token area | T/F |
| hitdamtype | mob obj room token area | T/F |
| hitdicebonus | mob obj room token area | NUM |
| hitdicenumber | mob obj room token area | NUM |
| hitdicetype | mob obj room token area | NUM |
| hitskilltype | mob obj room token area | T/F |
| hour | mob obj room token area | NUM |
| hpcnt | mob obj room token area | NUM |
| hunger | mob obj room token area | NUM |
| id | mob obj room token area | NUM |
| id2 | mob obj room token area | NUM |
| identical | mob obj room token area | T/F |
| imm | mob obj room token area | T/F |
| innature | mob obj room token area | T/F |
| inputwait | mob obj room token area | T/F |
| inwilds | mob obj room token area | T/F |
| isactive | mob obj room token area | T/F |
| isaffectcustom | mob obj room token area | T/F |
| isaffectgroup | mob obj room token area | T/F |
| isaffectskill | mob obj room token area | T/F |
| isaffectwhere | mob obj room token area | T/F |
| isangel | mob obj room token area | T/F |
| isareaunlocked | mob obj room token area | T/F |
| isboss | mob obj room token area | T/F |
| isbrewing | mob obj room token area | T/F |
| isbusy | mob obj room token area | T/F |
| iscastfailure | mob obj room token area | T/F |
| iscasting | mob obj room token area | T/F |
| iscastrecovered | mob obj room token area | T/F |
| iscastroomblocked | mob obj room token area | T/F |
| iscastsuccess | mob obj room token area | T/F |
| ischarm | mob obj room token area | T/F |
| ischurchexcom | mob obj room token area | T/F |
| ischurchpk | mob obj room token area | T/F |
| iscloneroom | mob obj room token area | T/F |
| iscpkproof | mob obj room token area | T/F |
| iscrosszone | mob obj room token area | T/F |
| isdead | mob obj room token area | T/F |
| isdelay | mob obj room token area | T/F |
| isdemon | mob obj room token area | T/F |
| isevil | mob obj room token area | T/F |
| isfading | mob obj room token area | T/F |
| isfighting | mob obj room token area | T/F |
| isflying | mob obj room token area | T/F |
| isfollow | mob obj room token area | T/F |
| isgood | mob obj room token area | T/F |
| ishired | mob obj room token area | T/F |
| ishunting | mob obj room token area | T/F |
| isimmort | mob obj room token area | T/F |
| iskey | mob obj room token area | T/F |
| isleader | mob obj room token area | T/F |
| ismobile | mob obj room token area | T/F |
| ismoonup | mob obj room token area | T/F |
| ismorphed | mob obj room token area | T/F |
| ismystic | mob obj room token area | T/F |
| isneutral | mob obj room token area | T/F |
| isnpc | mob obj room token area | T/F |
| isobject | mob obj room token area | T/F |
| ison | mob obj room token area | T/F |
| isowner | mob obj room token area | T/F |
| ispc | mob obj room token area | T/F |
| ispersist | mob obj room token area | T/F |
| ispk | mob obj room token area | T/F |
| isprey | mob obj room token area | T/F |
| ispulling | mob obj room token area | T/F |
| ispullingrelic | mob obj room token area | T/F |
| isquesting | mob obj room token area | T/F |
| isremort | mob obj room token area | T/F |
| isrepairable | mob obj room token area | T/F |
| isrestrung | mob obj room token area | T/F |
| isridden | mob obj room token area | T/F |
| isrider | mob obj room token area | T/F |
| isriding | mob obj room token area | T/F |
| isroom | mob obj room token area | T/F |
| isroomdark | mob obj room token area | T/F |
| issafe | mob obj room token area | T/F |
| isscribing | mob obj room token area | T/F |
| isshifted | mob obj room token area | T/F |
| isshooting | mob obj room token area | T/F |
| isshopkeeper | mob obj room token area | T/F |
| isspell | mob obj room token area | T/F |
| issubclass | mob obj room token area | T/F |
| issustained | mob obj room token area | T/F |
| istarget | mob obj room token area | T/F |
| istattooing | mob obj room token area | T/F |
| istoken | mob obj room token area | T/F |
| istreasureroom | mob obj room token area | T/F |
| isvisible | mob obj room token area | T/F |
| isvisibleto | mob obj room token area | T/F |
| isworn | mob obj room token area | T/F |
| lastreturn | mob obj room token area | NUM |
| level | mob obj room token area | NUM |
| liquid | mob obj room token area | T/F |
| listcontains | mob obj room token area | T/F |
| loaded | mob obj room token area | NUM |
| lostparts | mob obj room token area | T/F |
| manaregen | mob obj room token area | NUM |
| manastore | mob obj room token area | NUM |
| maparea | mob obj room token area | NUM |
| mapheight | mob obj room token area | NUM |
| mapid | mob obj room token area | NUM |
| mapvalid | mob obj room token area | T/F |
| mapwidth | mob obj room token area | NUM |
| mapx | mob obj room token area | NUM |
| mapy | mob obj room token area | NUM |
| material | mob obj room token area | T/F |
| max | mob obj room token area | NUM |
| maxcarry | mob obj room token area | NUM |
| maxhit | mob obj room token area | NUM |
| maxmana | mob obj room token area | NUM |
| maxmove | mob obj room token area | NUM |
| maxweight | mob obj room token area | NUM |
| maxxp | mob obj room token area | NUM |
| min | mob obj room token area | NUM |
| mobclones | mob obj room token area | NUM |
| mobexists | mob obj room token area | T/F |
| mobhere | mob obj room token area | T/F |
| mobs | mob obj room token area | NUM |
| mobsize | mob obj room token area | NUM |
| money | mob obj room token area | NUM |
| monkills | mob obj room token area | NUM |
| month | mob obj room token area | NUM |
| moonphase | mob obj room token area | NUM |
| moveregen | mob obj room token area | NUM |
| name | mob obj room token area | T/F |
| numenchants | mob obj room token area | NUM |
| objclones | mob obj room token area | NUM |
| objcond | mob obj room token area | NUM |
| objcorpse | mob obj room token area | T/F |
| objcost | mob obj room token area | NUM |
| objexists | mob obj room token area | T/F |
| objextra | mob obj room token area | T/F |
| objextra2 | mob obj room token area | T/F |
| objextra3 | mob obj room token area | T/F |
| objextra4 | mob obj room token area | T/F |
| objfrag | mob obj room token area | T/F |
| objhere | mob obj room token area | T/F |
| objmaxweight | mob obj room token area | NUM |
| objranged | mob obj room token area | T/F |
| objtimer | mob obj room token area | NUM |
| objtype | mob obj room token area | T/F |
| objval0 | mob obj room token area | NUM |
| objval1 | mob obj room token area | NUM |
| objval2 | mob obj room token area | NUM |
| objval3 | mob obj room token area | NUM |
| objval4 | mob obj room token area | NUM |
| objval5 | mob obj room token area | NUM |
| objval6 | mob obj room token area | NUM |
| objval7 | mob obj room token area | NUM |
| objweapon | mob obj room token area | T/F |
| objweaponstat | mob obj room token area | T/F |
| objwear | mob obj room token area | T/F |
| objwearloc | mob obj room token area | T/F |
| objweight | mob obj room token area | NUM |
| objweightleft | mob obj room token area | NUM |
| off | mob obj room token area | T/F |
| order | mob obj | NUM |
| parts | mob obj room token area | T/F |
| people | mob obj room token area | NUM |
| permcon | mob obj room token area | NUM |
| permdex | mob obj room token area | NUM |
| permint | mob obj room token area | NUM |
| permstr | mob obj room token area | NUM |
| permwis | mob obj room token area | NUM |
| pgroupcon | mob obj room token area | NUM |
| pgroupdex | mob obj room token area | NUM |
| pgroupint | mob obj room token area | NUM |
| pgroupstr | mob obj room token area | NUM |
| pgroupwis | mob obj room token area | NUM |
| pkfights | mob obj room token area | NUM |
| pkloss | mob obj room token area | NUM |
| pkratio | mob obj room token area | NUM |
| pkwins | mob obj room token area | NUM |
| playerexists | mob obj room token area | NUM |
| players | mob obj room token area | NUM |
| pneuma | mob obj room token area | NUM |
| portal | mob obj room token area | T/F |
| portalexit | mob obj room token area | T/F |
| pos | mob obj room token area | T/F |
| practices | mob obj room token area | NUM |
| protocol | mob obj room token area | T/F |
| questpoint | mob obj room token area | NUM |
| race | mob obj room token area | T/F |
| rand | mob obj room token area | T/F |
| randpoint | mob obj room token area | T/F |
| reckoning | mob obj room token area | NUM |
| reckoningchance | mob obj room token area | NUM |
| reckoningcooldown | mob obj room token area | NUM |
| reckoningduration | mob obj room token area | NUM |
| reckoningintensity | mob obj room token area | NUM |
| register | mob obj room token area | NUM |
| res | mob obj room token area | T/F |
| roll | mob obj room token area | T/F |
| room | mob obj room token area | NUM |
| roomflag | mob obj room token area | T/F |
| roomflag2 | mob obj room token area | T/F |
| roomviewwilds | mob obj room token area | NUM |
| roomweight | mob obj room token area | NUM |
| roomwilds | mob obj room token area | NUM |
| roomx | mob obj room token area | NUM |
| roomy | mob obj room token area | NUM |
| roomz | mob obj room token area | NUM |
| samegroup | mob obj room token area | T/F |
| scriptsecurity | mob obj room token area | NUM |
| sectionflag | mob obj room token area | T/F |
| sector | mob obj room token area | T/F |
| sex | mob obj room token area | NUM |
| shiptype | mob obj room token area | T/F |
| sign | mob obj room token area | NUM |
| silver | mob obj room token area | NUM |
| sin | mob obj room token area | NUM |
| skeyword | mob obj room token area | T/F |
| skill | mob obj room token area | NUM |
| statcon | mob obj room token area | NUM |
| statdex | mob obj room token area | NUM |
| statint | mob obj room token area | NUM |
| statstr | mob obj room token area | NUM |
| statwis | mob obj room token area | NUM |
| stoned | mob obj room token area | NUM |
| strlen | mob obj room token area | NUM |
| strprefix | mob obj room token area | T/F |
| sublevel | mob obj room token area | NUM |
| sunlight | mob obj room token area | NUM |
| systemtime | mob obj room token area | NUM |
| tempstore1 | mob obj room token area | NUM |
| tempstore2 | mob obj room token area | NUM |
| tempstore3 | mob obj room token area | NUM |
| tempstore4 | mob obj room token area | NUM |
| tempstring | mob obj room token area | T/F |
| testhardmagic | mob obj room token area | T/F |
| testskill | mob obj room token area | T/F |
| testslowmagic | mob obj room token area | T/F |
| testtokenspell | mob obj room token area | T/F |
| thirst | mob obj room token area | NUM |
| timeofday | mob obj room token area | T/F |
| timer | mob obj room token area | NUM |
| tokencount | mob obj room token area | NUM |
| tokenexists | mob obj room token area | T/F |
| tokentimer | mob obj room token area | NUM |
| tokenvalue | mob obj room token area | NUM |
| totalfights | mob obj room token area | NUM |
| totalloss | mob obj room token area | NUM |
| totalpkfights | mob obj room token area | NUM |
| totalpkloss | mob obj room token area | NUM |
| totalpkratio | mob obj room token area | NUM |
| totalpkwins | mob obj room token area | NUM |
| totalquests | mob obj room token area | NUM |
| totalratio | mob obj room token area | NUM |
| totalwins | mob obj room token area | NUM |
| toxin | mob obj room token area | NUM |
| trains | mob obj room token area | NUM |
| uses | mob obj room token area | T/F |
| valueac | mob obj room token area | NUM |
| valueacstr | mob obj room token area | NUM |
| valuedamage | mob obj room token area | NUM |
| valueposition | mob obj room token area | NUM |
| valueranged | mob obj room token area | NUM |
| valuerelic | mob obj room token area | NUM |
| valuesector | mob obj room token area | NUM |
| valuesize | mob obj room token area | NUM |
| valuetoxin | mob obj room token area | NUM |
| valuetype | mob obj room token area | NUM |
| valueweapon | mob obj room token area | NUM |
| valuewear | mob obj room token area | NUM |
| varbool | mob obj room token area | T/F |
| vardefined | mob obj room token area | T/F |
| varexit | mob obj room token area | T/F |
| varnumber | mob obj room token area | NUM |
| varstring | mob obj room token area | T/F |
| vnum | mob obj room token area | NUM |
| vuln | mob obj room token area | T/F |
| weapon | mob obj room token area | T/F |
| weapontype | mob obj room token area | T/F |
| weaponskill | mob obj room token area | NUM |
| wears | mob obj room token area | T/F |
| wearused | mob obj room token area | T/F |
| weight | mob obj room token area | NUM |
| weightleft | mob obj room token area | NUM |
| wimpy | mob obj room token area | NUM |
| word | mob obj room token area | T/F |
| wornby | obj | T/F |
| xp | mob obj room token area | NUM |
| year | mob obj room token area | NUM |