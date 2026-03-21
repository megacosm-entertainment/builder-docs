---
layout: default
title: Entity Reference
nav_order: 5
parent: Scripting
---

# Entity Reference
{: .no_toc }

Scripts operate on **entities** — the building blocks of the game world. Every character, object, room, token, area, instance, dungeon, quest, and event is an entity with properties you can read and relationships you can navigate.

This page is the data-model reference. It tells you what entities exist, how to access them from scripts, and what fields each one exposes.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Entity Types Overview

Sentience has nine scriptable entity types. Each type can carry scripts (programs) that respond to triggers.

| Type | Script Type | Description |
|:-----|:------------|:------------|
| **Character** | [Mobile programs](mobile-programs/) | A living being — player (PC) or NPC (mob). The most commonly scripted entity. |
| **Object** | [Object programs](object-programs/) | An item that can exist in a room, inventory, equipment slot, or container. |
| **Room** | [Room programs](room-programs/) | A location in the world. Can react to player movement, speech, and actions. |
| **Token** | [Token programs](token-programs/) | An invisible marker attached to a character, object, or room. Used for buffs, quest flags, cooldowns, and custom state. |
| **Area** | [Area programs](area-programs/) | A collection of rooms grouped under one builder. Scripts here react to area-wide events. |
| **Instance** | [Instance programs](instance-programs/) | A single floor of a procedurally generated dungeon. Created from a blueprint at runtime. |
| **Dungeon** | [Dungeon programs](dungeon-programs/) | A multi-floor procedural dungeon containing one or more instances. |
| **Quest** | — | An active quest on a player. Accessed through character fields; quest programs run via token triggers. |
| **Event** | — | A world event (e.g. invasion, seasonal activity). Entities participating in events expose event-related fields. |

### When to script each type

- **Mobile programs** — NPC dialogue, shops, combat behaviour, quest givers, patrols.
- **Object programs** — Items that react when picked up, worn, dropped, eaten, or used.
- **Room programs** — Environmental effects, traps, puzzle rooms, area transitions.
- **Token programs** — The most versatile type. Tokens inherit *all* triggers from their host entity type, so a token on a mob can respond to any mob trigger. Use for reusable behaviours, timed effects, quest tracking, and custom abilities.
- **Area programs** — Reset-time scripts, area-wide announcements, repop customisation.
- **Instance / Dungeon programs** — Procedural content lifecycle: generation, population, completion, failure.

---

## Trigger Entity Bindings

When a trigger fires, the scripting engine binds specific game entities to named references your script can use. **These bindings change depending on the trigger type** — always check which entity is which for the trigger you are writing.

### Named Entity References

These are the named references available inside any script:

| Reference | Type | Description |
|:----------|:-----|:------------|
| `$self` / `$this` | varies | The entity the script is attached to (mob, obj, room, token, area, instance, or dungeon) |
| `$enactor` | character | The character who caused the trigger to fire (the "actor") |
| `$victim` | character | The secondary character target of the action |
| `$victim2` | character | A third character involved (rare — e.g. some combat triggers) |
| `$random` | character | A random character in the room |
| `$target` | character | The current script target (set by `settarget`) |
| `$obj1` | object | The primary object involved in the trigger |
| `$obj2` | object | A secondary object (e.g. container in a put-in action) |
| `$token` | token | The token parameter (when a token trigger fires) |
| `$here` | room | The current room of the scripted entity |
| `$phrase` | string | The text/argument that matched the trigger phrase |
| `$trigger` | string | The trigger phrase pattern that matched |
| `$register1`–`$register5` | number | Numeric registers for passing data between scripts |

### Legacy Quick-Code Equivalents

The named references above correspond to [quick codes](quick-codes.md):

| Named Reference | Quick Code |
|:----------------|:-----------|
| `$self` / `$this` | `$i` |
| `$enactor` | `$n` |
| `$victim` | `$t` |
| `$obj1` | `$p` |
| `$token` | `$q` |

See [Quick Codes Reference](quick-codes.md) for the full expansion list.

### Common Trigger Binding Examples

The meaning of `$enactor` (`$n`), `$victim` (`$t`), and `$obj1` (`$p`) changes with each trigger. Here are the most common ones:

| Trigger | `$enactor` / `$n` | `$victim` / `$t` | `$obj1` / `$p` |
|:--------|:-------------------|:------------------|:----------------|
| `greet` | Character arriving in the room | — | — |
| `speech` | Character who spoke | — | — |
| `sayto` | Character who spoke | — | — |
| `give` | Character giving the item | Character receiving | Object given |
| `get` | Character picking up | — | Object picked up |
| `drop` | Character dropping | — | Object dropped |
| `wear` | Character wearing | — | Object worn |
| `remove` | Character removing | — | Object removed |
| `buy` | Character buying | — | Object bought |
| `sell` | Character selling | — | Object sold |
| `eat` | Character eating | — | Object eaten |
| `drink` | Character drinking | — | Object drunk from |
| `kill` | Character who killed | — | — |
| `death` | Character who died | Killer | — |
| `fight` | Character in combat | — | — |
| `damage` | Attacker | Defender | — |
| `startcombat` | Character entering combat | Opponent | — |
| `entry` | Character entering room | — | — |
| `exit` | Character leaving room | — | — |
| `random` | A random character in the room | — | — |
| `delay` | Varies (preserved from when `delay` was set) | — | — |

{: .note }
> Always check the specific trigger page under [Mobile](mobile-programs/), [Object](object-programs/), [Room](room-programs/), or [Token](token-programs/) programs for exact binding details — some triggers have unique bindings not shown here.

---

## Entity Field Access

You can read properties of any entity using the **field access syntax**:

```
$(entity.field)
```

Where `entity` is a named reference and `field` is a property name from the tables below.

### Basic Syntax

| Syntax | Meaning | Example |
|:-------|:--------|:--------|
| `$(self.field)` | Field of the scripted entity | `$(self.name)` |
| `$(enactor.field)` | Field of the actor | `$(enactor.level)` |
| `$(victim.field)` | Field of the victim | `$(victim.race)` |
| `$(obj1.field)` | Field of the primary object | `$(obj1.short)` |
| `$(token.field)` | Field of the trigger token | `$(token.val0)` |
| `$(here.field)` | Field of the current room | `$(here.name)` |
| `$(random.field)` | Field of a random room character | `$(random.name)` |

### Chained Field Access

Fields that return another entity can be chained to navigate relationships:

```
$(enactor.room.area.name)
```

This reads the enactor's room, then that room's area, then the area's name. You can chain as deep as you need:

```
$(self.room.instance.dungeon.name)
```

### Using Field Values

Field values expand inline wherever `$()` appears — in `say`, `echo`, `if` checks, and script commands:

```
say Welcome, $(enactor.name)! You are level $(enactor.level).
say This room belongs to the $(here.area.name) area.

if $(enactor.level) >= 50
  say You are powerful enough to enter.
endif
```

---

## Character Fields

Character entities represent both players and NPCs. Access these fields on any character reference (`$self`, `$enactor`, `$victim`, `$random`, `$target`, etc.).

### Identity and Display

| Field | Type | Description |
|:------|:-----|:------------|
| `name` | string | Keyword string (what you target the character by) |
| `short` | string | Short description (display name, e.g. "a burly guard") |
| `long` | string | Long description (seen when looking in a room) |
| `fulldesc` | string | Full description (seen with `look` at character) |
| `level` | number | Effective character level |
| `race` | string | Current race name |
| `originalrace` | string | Original race name (before polymorph, etc.) |
| `class` | class | Class data entity |
| `sex` / `gender` | string | Body type name (e.g. "male", "female", "nonbinary") |

### Pronouns

| Field | Type | Description | Example |
|:------|:-----|:------------|:--------|
| `he` | string | Subjective pronoun | he, she, they, ze |
| `him` | string | Objective pronoun | him, her, them, zir |
| `his` | string | Possessive adjective | his, her, their, zis |
| `hisobj` | string | Possessive pronoun | his, hers, theirs, zirs |
| `himself` | string | Reflexive pronoun | himself, herself, themself, zirself |

### Location and Navigation

| Field | Type | Description |
|:------|:-----|:------------|
| `room` | room | The room the character is currently in |
| `area` | area | The area the character is in (shortcut for `room.area`) |
| `home` / `house` | room | The character's house room |
| `recall` | room | The character's recall point |
| `checkpoint` | room | The character's last checkpoint |

### Relationships

| Field | Type | Description |
|:------|:-----|:------------|
| `master` | character | Who this character follows |
| `leader` | character | Group leader |
| `owner` | character | The mob's owner (for charmed/summoned mobs) |
| `mount` | character | The mount this character is riding |
| `rider` | character | The character riding this mount |
| `opponent` | character | Current combat opponent |
| `hunting` / `prey` | character | The mob this character is hunting |
| `target` | character | The script target (set via `settarget`) |
| `next` | character | Next character in the room list |

### Inventory and Equipment

| Field | Type | Description |
|:------|:-----|:------------|
| `carrying` / `inv` | list | List of objects in inventory |
| `worn` | list | List of equipped objects |
| `eq` | equipment | Equipment slots accessor |
| `cart` | object | The cart the character is pulling |
| `on` / `bedroll` | object | Furniture the character is sitting/sleeping on |
| `instrument` | object | Currently held instrument |
| `stache` | list | Hidden stash objects |

### Tokens and Variables

| Field | Type | Description |
|:------|:-----|:------------|
| `tokens` | list | List of tokens on this character |
| `vars` | list | Script variables attached to this character |
| `affects` | list | Active spell/affect list |
| `auras` | list | Active aura effects |

### Metadata and Flags

| Field | Type | Description |
|:------|:-----|:------------|
| `act` | flags | ACT flags (sentinel, aggressive, etc.) |
| `affected` | flags | AFF flags (blind, invisible, etc.) |
| `offense` | flags | Offensive behaviour flags |
| `immune` | flags | Damage immunities |
| `resist` | flags | Damage resistances |
| `vuln` | flags | Damage vulnerabilities |
| `index` | mob index | The mob's prototype (template) data |
| `tempstring` | string | Temporary string storage |

### Casting and Music

| Field | Type | Description |
|:------|:-----|:------------|
| `castspell` | skill | Spell currently being cast |
| `casttoken` | token | Token associated with current cast |
| `casttarget` | string | Target of current cast |
| `song` | song | Song currently being performed |
| `songtoken` | token | Token associated with current song |
| `songtarget` | string | Target of current song |

### Group

| Field | Type | Description |
|:------|:-----|:------------|
| `group` | group | The character's group |
| `numgrouped` | number | Number of characters in the group |

### Race and Class

| Field | Type | Description |
|:------|:-----|:------------|
| `racedata` | race | Race entity (for accessing race fields) |
| `originalracedata` | race | Original race entity (pre-polymorph) |
| `classlevel` | classlevel | Class level data |
| `trait` | trait | Access to character traits |

### Connection and Time

| Field | Type | Description |
|:------|:-----|:------------|
| `connection` | connection | The player's network connection |
| `church` | church | The player's church/guild |
| `played` | number | Total time played (seconds) |
| `session_time` | number | Current session time (seconds) |
| `last_login` | number | Timestamp of last login |
| `last_logoff` | number | Timestamp of last logoff |
| `created` | number | Timestamp of character creation |
| `last_login_human` | string | Human-readable last login time |
| `last_logoff_human` | string | Human-readable last logoff time |
| `created_human` | string | Human-readable creation time |
| `last_login_delta` | number | Seconds since last login |
| `last_logoff_delta` | number | Seconds since last logoff |
| `created_delta` | number | Seconds since creation |

### Quest and Reputation

| Field | Type | Description |
|:------|:-----|:------------|
| `questpoint` / `missionpoint` | number | Quest/mission points |
| `totalquests` / `totalmissions` | number | Total quests completed |
| `isquesting` / `onmission` | boolean | Whether on an active mission |
| `quests` | list | Active quest list |
| `questhistory` | list | Completed quest history |
| `reputations` | list | All reputation standings |
| `reputation` | reputation | Access specific reputation |
| `factions` | list | Faction index list |

### Event

| Field | Type | Description |
|:------|:-----|:------------|
| `event` | event | Current active event |
| `events` | list | All event associations |
| `event_active` | number | Whether event is active |
| `event_kills` | number | Event kill count |
| `event_items` | number | Event item count |
| `event_goal` | number | Event goal target |
| `event_phase` | string | Current event phase name |
| `event_stage` | string | Current event stage name |
| `event_stage_index` | number | Current stage number |
| `event_stage_count` | number | Total stages |
| `event_objectives_met` | number | Objectives completed |
| `event_objectives_total` | number | Total objectives |
| `event_completion` | number | Completion percentage |

### Example: Character Fields in Practice

```
// Greet trigger on a guard mob
say Halt, $(enactor.name)! State your business in $(here.area.name).
if $(enactor.level) < 10
  say You seem too inexperienced. Come back when you're stronger.
  transfer $n $(self.recall)
endif
```

---

## Object Fields

Object entities represent items. Access these on `$self` (in object programs), `$obj1`, `$obj2`, or any field that returns an object.

### Core Properties

| Field | Type | Description |
|:------|:-----|:------------|
| `name` | string | Keyword string (what players type to interact) |
| `short` | string | Short description ("a rusty sword") |
| `long` | string | Long description (seen on the ground) |
| `fulldesc` | string | Full description (seen with `look` at object) |
| `level` | number | Object level |

### Location and Ownership

| Field | Type | Description |
|:------|:-----|:------------|
| `carrier` / `user` / `wearer` | character | The character carrying or wearing this object |
| `room` | room | The room the object is in (if on the ground) |
| `container` / `in` | object | The container this object is inside |
| `owner` | string | Owner name string |
| `area` | area | The area this object belongs to |

### Contents and Relations

| Field | Type | Description |
|:------|:-----|:------------|
| `contents` / `inv` / `items` | list | Objects inside this container |
| `on` | object | Furniture reference |
| `tokens` | list | Tokens attached to this object |
| `target` | character | The object's current target |
| `next` | object | Next object in the list |
| `ed` | extra desc | Extra description data |

### Metadata and Flags

| Field | Type | Description |
|:------|:-----|:------------|
| `extra` | flags | Extra flags (glow, hum, nodrop, etc.) |
| `wear` | flags | Wear location flags |
| `affects` | list | Magical affects on the object |
| `vars` | list | Script variables |
| `index` | obj index | The object's prototype (template) |
| `ship` | ship | Ship this object represents |
| `stache` | list | Stashed objects |
| `isstached` | boolean | Whether this object is stashed |

### Typed Data Sub-Entities

Objects have type-specific data accessed through sub-entity fields. The available sub-entity depends on the object's item type:

| Field | Object Type | Description |
|:------|:------------|:------------|
| `weapon` | Weapon | Weapon class, damage type, dice, flags |
| `armor` / `armour` | Armor | AC values per damage type |
| `container_data` | Container | Capacity, flags, key vnum |
| `fluid` / `drink` | Drink container | Liquid type, capacity, current amount |
| `food` | Food | Nutrition, poison, buff data |
| `furniture` | Furniture | Capacity, heal/mana bonus |
| `portal` | Portal | Destination, flags |
| `light` | Light | Duration remaining |
| `money` | Money | Gold and silver amounts |
| `wand` / `staff` | Wand/Staff | Spell, charges, level |
| `corpse` | Corpse | Original mob data |
| `instrument` | Instrument | Instrument type, quality |
| `scroll` | Scroll | Spells and level |
| `book` | Book | Pages, content |
| `seed` | Seed | Growth data |
| `cart` | Cart | Capacity, contents |
| `jewelry` | Jewelry | Gem slots, enchantments |
| `tattoo` | Tattoo | Design, effects |
| `ink` | Ink | Color, quality |
| `herb` | Herb | Herb type, potency |
| `tool` | Tool | Tool type, durability |
| `compass` | Compass | Calibration, target |
| `telescope` | Telescope | Range, zoom |
| `sextant` | Sextant | Navigation data |
| `trade` | Trade good | Trade value, origin |
| `mist` | Mist | Effect area, density |
| `map` | Map | Region, detail level |
| `page` | Page | Text, page number |
| `body_part` | Body part | Part type, condition |
| `weapon_con` | Weapon container | Ammo type, capacity |
| `item_ship` | Ship item | Ship-related properties |

### Event Fields (on Objects)

Objects participating in events expose the same event fields as characters (`event_active`, `event_kills`, etc.).

### Example: Object Fields in Practice

```
// Object program on a magical sword — react to being worn
say $(enactor.name) wields $(self.short)!
if $(enactor.level) < $(self.level)
  echo The sword is too powerful for you and slips from your grasp!
  force $n remove $(self.name)
endif
```

---

## Room Fields

Room entities represent locations. Access these on `$here`, `$self` (in room programs), or any field that returns a room.

### Core Properties

| Field | Type | Description |
|:------|:-----|:------------|
| `name` | string | Room title |
| `desc` | string | Room description text |
| `sector` | sector | Sector type entity (forest, city, water, etc.) |
| `sectorflags` | flags | Sector-related flags |
| `flags` | flags | Room flags (safe, dark, indoors, etc.) |

### Contents

| Field | Type | Description |
|:------|:-----|:------------|
| `mobiles` | list | Characters in the room |
| `objects` | list | Objects on the ground in the room |
| `tokens` | list | Tokens attached to the room |
| `vars` | list | Script variables on the room |
| `ed` | extra desc | Extra description data |

### Exits (Directions)

| Field | Type | Description |
|:------|:-----|:------------|
| `north` | exit | North exit |
| `east` | exit | East exit |
| `south` | exit | South exit |
| `west` | exit | West exit |
| `up` | exit | Up exit |
| `down` | exit | Down exit |
| `northeast` | exit | Northeast exit |
| `northwest` | exit | Northwest exit |
| `southeast` | exit | Southeast exit |
| `southwest` | exit | Southwest exit |

Each exit is an entity with its own fields (see [Exit Fields](#exit-fields) below).

### Hierarchy and Context

| Field | Type | Description |
|:------|:-----|:------------|
| `area` | area | The area this room belongs to |
| `region` | area region | The area region |
| `environ` / `environment` / `outside` / `extern` | room | The environ (parent) room |
| `env_mob` | character | The environ's mob |
| `env_obj` | object | The environ's object |
| `env_room` | room | The environ's room |
| `env_token` | token | The environ's token |
| `wilds` | wilderness | Wilderness data (if a wilderness room) |
| `section` | section | Instance section this room belongs to |
| `instance` | instance | Instance this room belongs to |
| `dungeon` | dungeon | Dungeon this room belongs to |
| `ship` | ship | Ship this room is part of |
| `target` | character | The room's script target |
| `clonerooms` / `clones` | list | Cloned rooms |
| `event` | event | Active event in this room |
| `events` | list | All events |

### Example: Room Fields in Practice

```
// Room program — describe the environment based on sector
if $(here.sector.name) == forest
  echo The ancient trees creak softly in the wind.
endif

// Check if there's a way north
if $(here.north) != ""
  say You can go north from here.
endif
```

---

## Exit Fields

Exits connect rooms. Access them through room direction fields (e.g. `$(here.north.dest)`).

| Field | Type | Description |
|:------|:-----|:------------|
| `name` | string | Exit keyword (door name) |
| `door` | number | Door number/direction index |
| `src` / `here` / `source` | room | The source room |
| `dest` / `remote` / `destination` | room | The destination room |
| `state` | string | Door state (open, closed, locked) |
| `mate` | exit | The corresponding exit on the other side |
| `next` | exit | Next exit in list |
| `north`–`southwest` | exit | Neighboring exits from the source room |

### Example: Exit Navigation

```
// Check where north leads
say The path north leads to $(here.north.dest.name).
```

---

## Token Fields

Tokens are invisible markers attached to characters, objects, or rooms. They are the primary tool for tracking custom state. Access these on `$self` (in token programs), `$token`, or any field that returns a token.

| Field | Type | Description |
|:------|:-----|:------------|
| `name` | string | Token keyword string |
| `owner` | character | The character carrying this token |
| `object` | object | The object this token is on (if on an object) |
| `room` | room | The room this token is in (if on a room) |
| `timer` | number | Countdown timer (in ticks). Token is removed when it reaches 0. |
| `val0`–`val7` | number | Eight general-purpose numeric values for storing custom data |
| `next` | token | Next token in the list |
| `vars` | list | Script variables on this token |

### Example: Token Fields in Practice

```
// Token program — a poison debuff that ticks down
// val0 = damage per tick, val1 = remaining ticks
damage $n $(self.val0) none
if $(self.val1) <= 0
  echo The poison fades from $(enactor.name)'s body.
  tokenrem self
else
  mathset self val1 $(self.val1) - 1
endif
```

---

## Area Fields

Areas group rooms under a single builder. Access these through `$(here.area)`, `$(self)` in area programs, or any field that returns an area.

| Field | Type | Description |
|:------|:-----|:------------|
| `name` | string | Area name |
| `recall` | room | Default recall room for the area |
| `post` | room | Post office room |
| `lower` | number | Lowest vnum in the area's range |
| `upper` | number | Highest vnum in the area's range |
| `minlevel` | number | Recommended minimum level |
| `maxlevel` | number | Recommended maximum level |
| `region` | area region | The area's region data |
| `rooms` | list | All rooms in the area |
| `event` | event | Active event in this area |
| `events` | list | All events |

### Example: Area Fields in Practice

```
// Check if the player is in the right level range for this area
if $(enactor.level) < $(here.area.minlevel)
  say This area is designed for level $(here.area.minlevel)+ adventurers.
endif
```

---

## Instance Fields

Instances are single floors of procedurally generated dungeons, created from blueprints at runtime.

| Field | Type | Description |
|:------|:-----|:------------|
| `name` | string | Instance name |
| `sections` | list | Blueprint sections used |
| `owners` | list | Characters who own/created this instance |
| `object` | object | The object representing this instance |
| `dungeon` | dungeon | The parent dungeon |
| `ship` | ship | Associated ship (if ship-based) |
| `floor` | number | Floor number within the dungeon |
| `entry` | room | Entry room |
| `exit` | room | Exit room |
| `recall` | room | Recall room within the instance |
| `environ` | room | The environ (external) room |
| `rooms` | list | All rooms in this instance |
| `players` | list | Players currently inside |
| `mobiles` | list | All mobs in the instance |
| `objects` | list | All objects in the instance |
| `bosses` | list | Boss mobs |
| `specialrooms` | list | Special/named rooms |
| `event` | event | Active event |
| `events` | list | All events |

---

## Dungeon Fields

Dungeons are multi-floor procedural areas containing one or more instances.

| Field | Type | Description |
|:------|:-----|:------------|
| `name` | string | Dungeon name |
| `desc` | string | Dungeon description |
| `index` | dungeon index | The dungeon's prototype definition |
| `floors` | list | List of instances (floors) |
| `owners` | list | Characters who own this dungeon |
| `entry` | room | Entry room |
| `exit` | room | Exit room |
| `rooms` | list | All rooms across all floors |
| `players` | list | All players in the dungeon |
| `mobiles` | list | All mobs in the dungeon |
| `objects` | list | All objects in the dungeon |
| `bosses` | list | Boss mobs across all floors |
| `specialrooms` | list | Special/named rooms |
| `event` | event | Active event |
| `events` | list | All events |

---

## Quest Fields

Quests are accessed through character fields (`$(enactor.quests)`) or through quest-specific references.

| Field | Type | Description |
|:------|:-----|:------------|
| `id` / `runid` | number | Unique run ID for this quest instance |
| `name` | string | Quest name |
| `description` | string | Quest description text |
| `status` | number | Numeric status code |
| `active` | boolean | Whether the quest is currently active |
| `completed` | boolean | Whether the quest has been completed |
| `failed` | boolean | Whether the quest has failed |
| `abandoned` | boolean | Whether the quest was abandoned |
| `scope` | number | Quest scope (personal, group, etc.) |
| `index` / `wnum` | widevnum | The quest definition's widevnum |
| `stage` / `stageid` | number | Current stage ID |
| `current_stage` | quest stage | Current stage entity |
| `stages` | list | All stages |
| `stage_objectives` / `objectivelist` | list | Current stage objectives |
| `stage_commenced` | boolean | Whether current stage has started |
| `stagecomplete` | boolean | Whether current stage is complete |
| `objective_count` / `objectives` | number | Number of objectives |
| `part_count` / `parts` | number | Number of parts |
| `generating` | boolean | Whether quest is being generated |
| `scripted` | boolean | Whether quest is script-driven |

---

## Event Fields

Events represent world events (invasions, seasonal activities, etc.). Access through `$(self.event)` on any entity participating in an event.

| Field | Type | Description |
|:------|:-----|:------------|
| `uid` / `event_uid` | number | Unique event identifier |
| `source_uid` / `event_source_uid` | number | Source entity UID |
| `instance` / `event_instance` | number | Instance ID |
| `source_instance` / `event_source_instance` | number | Source instance ID |
| `bracket` / `event_bracket` | number | Event bracket/tier |
| `active` / `event_active` | number | Whether event is active |
| `kills` / `event_kills` | number | Kill count |
| `items` / `event_items` | number | Item count |
| `goal` / `event_goal` | number | Goal target |
| `phase` / `event_phase` | string | Current phase name |
| `stage` / `event_stage` | string | Current stage name |
| `stage_index` / `event_stage_index` | number | Stage number |
| `stage_count` / `event_stage_count` | number | Total stages |
| `objectives_met` / `event_objectives_met` | number | Objectives completed |
| `objectives_total` / `event_objectives_total` | number | Total objectives |
| `objective_progress` / `event_objective_progress` | number | Progress value |
| `completion` / `event_completion` | number | Completion percentage |

---

## Navigating Entity Hierarchies

Entities form a tree of relationships. Use chained field access to navigate up and down:

### Going Up (Child → Parent)

```
$(self.room)                    Character/Object → Room
$(self.room.area)               Character/Object → Room → Area
$(self.room.instance)           Room → Instance
$(self.room.instance.dungeon)   Room → Instance → Dungeon
$(self.room.area.region)        Room → Area → Region
$(obj1.carrier)                 Object → Carrying Character
$(token.owner)                  Token → Owning Character
$(self.container)               Object → Container Object
```

### Going Down (Parent → Children)

```
$(here.mobiles)                 Room → Characters in room
$(here.objects)                 Room → Objects in room
$(enactor.carrying)             Character → Inventory
$(enactor.worn)                 Character → Equipped items
$(enactor.tokens)               Character → Tokens
$(self.contents)                Container → Contents
$(self.dungeon.floors)          Dungeon → Instances
```

### Hierarchy Diagram

```
Area
 └── Room
      ├── Characters (mobiles)
      │    ├── Inventory (carrying)
      │    ├── Equipment (worn)
      │    ├── Tokens
      │    └── Variables
      ├── Objects
      │    ├── Contents (if container)
      │    ├── Tokens
      │    └── Variables
      ├── Tokens
      ├── Variables
      └── Exits → other Rooms

Dungeon
 └── Instance (floor)
      └── Rooms (same structure as above)
```

---

## Targeting Syntax

Many script commands require you to specify which entity to act on. There are several ways to target:

### By Quick Code

| Code | Target |
|:-----|:-------|
| `$n` | The enactor (actor) |
| `$i` | Self (the scripted entity) |
| `$t` | The victim/target |
| `$p` | The primary object |
| `$q` | The token |
| `$r` | Random character in room |

### By Keyword

Target entities by their keyword string:

```
force guard kneel
purge rusted_sword
give gold_key $n
```

### By Vnum

Target entities by their vnum (virtual number):

```
mload 3001
oload 500
```

### By Widevnum

Target entities across areas using the `auid#vnum` format:

```
mload 2#3001
oload 5#500
```

The number before `#` is the Area Unique ID (AUID), and the number after is the vnum within that area. This is the preferred way to reference entities in scripts, as it avoids vnum collisions across areas.

### Examples

```
// Transfer actor to a room by vnum
transfer $n 3001

// Load a mob by widevnum
mload 2#3001

// Check for a token by vnum
if hastoken $n 5#100
  say You already have the quest marker.
endif

// Check for an object by keyword
if has $n dragon_scale
  say You have the scale!
endif
```

---

## Common Patterns

### Checking Who Triggered the Script

```
// Only react to players, not NPCs
if ispc $n
  say Greetings, adventurer!
endif

// Only react to NPCs
if isnpc $n
  // Ignore NPC triggers
  break
endif
```

### Reading and Comparing Fields

```
// Check enactor's level
if $(enactor.level) >= 50
  say You are worthy.
endif

// Compare token values
if $(self.val0) > 0
  mathset self val0 $(self.val0) - 1
endif
```

### Navigating to Related Entities

```
// Send a message to the area
echo You are in $(here.area.name), levels $(here.area.minlevel)-$(here.area.maxlevel).

// Check if character is in a dungeon
if $(enactor.room.dungeon) != ""
  say You are inside $(enactor.room.dungeon.name)!
endif

// Check who is carrying an object (in an oprog)
say My carrier is $(self.carrier.name).
```

---

## See Also

- [Quick Codes Reference](quick-codes.md) — `$n`, `$t`, `$p` and all quick-code expansions
- [IFChecks Reference](ifchecks-reference.md) — conditional checks on entity properties
- [Scripting Basics](scripting-basics.md) — introduction to the scripting system
- [Mobile Programs](mobile-programs/) — triggers for NPCs
- [Object Programs](object-programs/) — triggers for items
- [Room Programs](room-programs/) — triggers for rooms
- [Token Programs](token-programs/) — triggers for tokens
- [Area Programs](area-programs/) — triggers for areas
- [Instance Programs](instance-programs/) — triggers for instances
- [Dungeon Programs](dungeon-programs/) — triggers for dungeons
