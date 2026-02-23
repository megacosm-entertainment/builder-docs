---
layout: default
title: Ships
nav_order: 4
parent: Advanced Concepts
---

# Ships (shedit)

Ships are player-controllable or NPC vessels — sailing boats or airships — that can traverse the world. A ship definition specifies its combat stats, crew requirements, movement properties, blueprint (interior rooms), and the object used to represent it in the game world.

{: .warning}
**Ships are an advanced feature.** The ship must have a blueprint (interior rooms), a helm room, and an object to represent it before it will function correctly.

---

## Requirements

Before creating a ship you will need:

1. A **blueprint** (`bpedit`) for the ship's interior rooms, with at least one room flagged `helm`.
2. An **object** (`oedit`) to represent the physical ship in the world.
3. Optionally: key objects for locking the ship.

---

## Entering the Editor

```
shedit create <widevnum>
shedit <widevnum>
```

**Example:**
```
shedit create 5#950
shedit 5#950
```

---

## Commands

### name

```
name <text>
```

Sets the ship's name shown to players.

---

### class

```
class <sailboat|airship>
```

| Value | Description |
|-------|-------------|
| `sailboat` | A water-bound sailing vessel |
| `airship` | An airborne vessel traveling above land and sea |

---

### flags

```
flags <flag>
```

Toggle ship flags:

| Flag | Description |
|------|-------------|
| `protected` | The ship cannot be attacked or damaged by players or mobs |

---

### blueprint

```
blueprint <widevnum>
```

Links this ship definition to a blueprint (the interior room layout). The blueprint must have at least one entry point (for boarding) and at least one room with the `helm` flag set.

**Example:** `blueprint 5#800`

---

### object

```
object <widevnum>
```

Sets the object definition that represents this ship in the game world (what players see and target when the ship is docked or sailing).

**Example:** `object 5#501`

---

### hit

```
hit <number>
```

Sets the ship's hit point total — how much damage it can take before being destroyed.

---

### armor

```
armor <number>
```

Base armor class of the ship. Higher values reduce damage taken.

---

### guns

```
guns <number>
```

Maximum number of cannons/weapons the ship can mount.

---

### oars

```
oars <number>
```

Number of oars. Affects movement in calm or no-wind conditions.

---

### crew

```
crew <min> <max>
```

Minimum and maximum crew members. Ships with fewer than `min` crew may suffer movement penalties.

**Example:** `crew 2 8`

---

### move

```
move <delay> <steps>
```

Controls movement:
- `delay` — ticks between movement pulses
- `steps` — rooms moved per pulse

**Example:** `move 3 1`

---

### turning

```
turning <value>
```

Maximum turning rate. Controls how quickly the ship can change direction.

---

### weight

```
weight <number>
```

Maximum weight the ship can carry (cargo + crew + equipment).

---

### capacity

```
capacity <number>
```

Maximum number of entities (passengers, cargo items) the ship can hold.

---

### desc

```
desc
```

Opens the string editor for the ship's full description. End with `~`.

---

### keys

```
keys list
keys add <widevnum>
keys remove <#>
```

Manages key objects for this ship. Players must have one of these key objects to dock/undock or board the ship when locked.

---

### list

```
shedit list
```

Lists all ships defined in the current area.

---

## Example

```
shedit create 5#950
shedit 5#950

name The Thornwood Sloop
class sailboat
blueprint 5#800
object 5#501
flags protected

hit 500
armor 100
guns 4
oars 2
crew 2 8
move 3 1
turning 4
weight 10000
capacity 20

desc
A weathered sloop of Thornwood oak, scarred from years on the southern sea.
Her sails bear the warden's leaf emblem.
~

keys add 5#502
show
done
```
