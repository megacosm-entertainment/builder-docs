---
layout: default
title: Entity Reference
nav_order: 5
parent: Scripting
---

# Entity Reference
{: .no_toc }

Scripts operate on **entities** — characters (PCs and NPCs), objects, rooms, tokens, and areas. This page describes how entities are referenced in scripts, how to use entity-targeting commands, and how to navigate entity relationships.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Entity Types

| Type | Used As | Description |
|:-----|:--------|:------------|
| Character | `mob`, `ch` | A living being — player (`ispc`) or NPC (`isnpc`) |
| Object | `obj` | An item in a room, inventory, or equipped |
| Room | `room` | A location in the world |
| Token | `token` | A non-physical token carried by a character |
| Area | `area` | A collection of rooms |

---

## Trigger Entity Bindings

Each trigger type binds a different set of entities to the quick-code variables ($n, $t, etc.). The most common bindings are:

| Trigger | `$n` (Actor) | `$t` (Target) | `$p` (Object) |
|:--------|:-------------|:--------------|:--------------|
| `greet` | Character entering the room | — | — |
| `speech` | Character who spoke | — | — |
| `sayto` | Character who spoke | — | — |
| `give` | Character giving | Character receiving | Object given |
| `get` | Character picking up | — | Object picked up |
| `drop` | Character dropping | — | Object dropped |
| `kill` | Character who killed | — | — |
| `death` | Character who died | Killer | — |
| `fight` | Character being attacked | — | — |
| `wear` | Character wearing | — | Object worn |
| `remove` | Character removing | — | Object removed |
| `buy` | Character buying | — | Object bought |
| `sell` | Character selling | — | Object sold |
| `entry` | Character entering | — | — |
| `exit` | Character leaving | — | — |
| `random` | Will be a local character | — | — |
| `act` | Variable | — | — |
| `damage` | Attacker | Defender | — |
| `startcombat` | Character entering combat | Target | — |

---

## Targeting Commands

Many commands require specifying a target entity. Targets can be specified as:

| Format | Meaning |
|:-------|:--------|
| `$n` | The actor from the trigger |
| `$i` | The entity the script is on (self) |
| `$t` | The secondary target from the trigger |
| `all` | All entities matching criteria |
| A keyword | Match by keyword (e.g. `guard`, `sword`) |

### Examples

```
// Force the actor to stand
force $n stand

// Transfer actor to room 3001
transfer $n 3001

// Give an object to the actor
give sword $n

// Apply affect to target
addaffect $t blind 10 0

// Purge an object by keyword in current room
purge rusted_sword
```

---

## Navigating Entity Relationships

### Character in a group
```
// Iterate over group members (use grpsize ifcheck)
if grpsize $n > 1
  gecho $n's group has $[grpsize $n] members.
endif
```

### Room and its contents
```
// Check if an NPC exists in the room
if mobhere guard
  echo The guard is present.
endif

// Check if an object is in the room
if objhere key
  echo The key is here.
endif
```

### Object carrier
```
// Test (in an oprog) if item is carried vs worn
if carriedby $n
  echo You're holding $p.
endif

if wornby $n
  echo You're wearing $p.
endif
```

---

## Entity IFChecks Reference

For script conditionals operating on entities, see the [IFChecks Reference](ifchecks-reference) which contains the full list of available checks such as:

- `ispc $n` / `isnpc $n` — player or NPC
- `isimmort $n` — is an immortal
- `level $n` — numeric level
- `hpcnt $n` — HP percentage (0–100)
- `hastoken $n vnum` — carries a specific token
- `carries $n keyword` — carries an object
- `has $n keyword` — has in inventory or equipped
- `room $n == 3001` — in a specific room
- `vnum $n == 500` — the entity is a specific mob vnum
- `act $n sentinel` — has a specific act flag
- `affected $n blind` — has a specific affect

---

## Working with Objects in Scripts

### Loading an object
```
// Load object by widevnum into room
oload 2#800

// Load into actor's inventory
oload 800
give sword $n
```

### Checking object state
```
if objhere rusted_key
  say The key is on the floor.
endif

if has $n leather_armor
  say You're equipped for the journey.
endif
```

### Transferring objects
```
// Move all objects from actor to self (mob picks up everything)
otransfer all.item $i
```

---

## Working with Rooms in Scripts

### Sending a message to another room
```
at 3200 echo A distant howl echoes through the corridor.
```

### Moving an entity to another room
```
transfer $n 3100
```

### Checking room conditions
```
if roomflag $r safe
  say You are in a safe area.
endif

if sector $r forest
  say These woods feel ancient.
endif
```
