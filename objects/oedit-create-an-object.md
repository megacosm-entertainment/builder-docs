---
layout: default
title: Creating an Object
nav_order: 1
parent: Object Editor
---

# Creating an Object
{: .no_toc }

This guide walks through creating several common object types using the Object Editor (`oedit`): a weapon, a piece of armor, and a container.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## The Basics

Open the object editor with a widevnum (format: `area_uid#vnum`):

```
oedit 5#800
```

If the object doesn't exist, you'll be asked to confirm. Type `yes`.

---

## Creating a Weapon

### Step 1: Name and descriptions

```
oedit 5#800
name iron longsword sword
short an iron longsword
long An iron longsword lies here.
description
A well-balanced longsword forged from cold iron. The blade bears faint
runes near the hilt, though their meaning is long forgotten.
@
```

### Step 2: Set the object type

```
type weapon
```

### Step 3: Set weapon values

```
v0 sword           // weapon type (see Object Types reference)
v1 2               // number of damage dice
v2 8               // sides per die  → 2d8
v3 4               // bonus damage   → 2d8+4
v4 slashing        // damage type
```

### Step 4: Set wear location

```
wear take wield
```

The `take` flag must always be set for objects players can pick up. `wield` allows it to be wielded.

### Step 5: Set extra flags

```
extra glow
```

### Step 6: Add stat modifiers (optional)

```
addaffect str 2 0
addaffect hitroll 3 0
```

Format: `addaffect <stat> <modifier> <random_bonus>`

### Step 7: Set weapon-specific flags

```
v5 sharp one-handed
```

### Step 8: Set level and material

```
level 15
material iron
weight 30
cost 5000
```

### Step 9: Review and save

```
show
done
```

#### Complete weapon example

```
oedit 5#800
name iron longsword sword
short an iron longsword
long An iron longsword lies here.
description
A well-balanced longsword forged from cold iron.
@
type weapon
v0 sword
v1 2
v2 8
v3 4
v4 slashing
v5 sharp one-handed
wear take wield
level 15
material iron
weight 30
cost 5000
addaffect str 2 0
show
done
```

---

## Creating Armor

### Step 1: Basic info

```
oedit 5#801
name leather vest armor
short a leather vest
long A leather vest lies crumpled on the floor.
description
A sturdy leather vest reinforced with iron studs.
@
```

### Step 2: Set type and wear

```
type armor
wear take torso
```

### Step 3: Set armor class

```
v0 10   // armor class value
```

### Step 4: Set level, material, weight

```
level 10
material leather
weight 20
cost 2000
```

### Step 5: Add stat modifier

```
addaffect con 1 0
```

---

## Creating a Container

```
oedit 5#802
name leather sack bag container
short a leather sack
long A leather sack sits on the ground.
description
A large leather sack, practical and unadorned.
@
type container
v0 100    // max weight capacity
v1 closeable    // container flags (see types reference)
v2 0      // key vnum (0 = no lock)
v3 80     // current capacity used (usually leave at 0)
wear take
weight 5
cost 50
show
done
```

---

## Object Value Fields by Type

The `v0`–`v7` fields have different meanings for each type. See the [Object Types Reference](oedit-object-types-reference) for a full table. Most important types:

| Type | v0 | v1 | v2 | v3 | v4 | v5 |
|:-----|:---|:---|:---|:---|:---|:---|
| `weapon` | weapon type | dice count | dice sides | bonus | damage type | weapon flags |
| `armor` | AC value | — | — | — | — | — |
| `container` | max weight | flags | key vnum | — | — | — |
| `potion` | spell level | spell 1 | spell 2 | spell 3 | — | — |
| `scroll` | spell level | spell 1 | spell 2 | spell 3 | — | — |
| `wand` | spell level | max charges | current charges | spell name | — | — |
| `staff` | spell level | max charges | current charges | spell name | — | — |
| `food` | hunger value | — | — | — | — | — |
| `drink` | capacity | current volume | liquid type | — | — | — |
| `key` | — | — | — | — | — | — |
| `portal` | destination vnum | — | — | — | — | — |
| `furniture` | max people | max weight | heal bonus | mana bonus | — | — |

---

## Next Steps

- See [OEdit Command Reference](oedit-command-reference) for all commands
- See [OEdit Flags Reference](oedit-flags-reference) for all flags, materials, damage types, and liquids
- Add the object to a [room reset](../rooms/redit-command-reference#oreset)
