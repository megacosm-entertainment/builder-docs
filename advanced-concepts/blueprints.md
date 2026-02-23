---
layout: default
title: Blueprints
nav_order: 2
parent: Advanced Concepts
---

# Blueprints (bpedit)

A **blueprint** is an assembly of blueprint sections wired together to form a complete instance layout. When a dungeon spawns an instance, it uses a blueprint as its template — the blueprint determines which sections appear and how they connect.

{: .warning}
**Instancing is an advanced feature.** Create your sections in `bsedit` first, then assemble them here.

---

## Concepts

- A blueprint's **mode** (static or procedural) controls whether sections are hand-placed or randomly assembled.
- **Static** blueprints have a fixed layout: you specify exactly which sections go where and how their links connect.
- **Procedural** blueprints have a section pool; the engine picks and assembles sections randomly at instance creation time.
- Blueprints have **entry** and **exit** points used to place and remove players.

---

## Entering the Editor

```
bpedit create <widevnum>
bpedit <widevnum>
```

**Example:**
```
bpedit create 5#800
bpedit 5#800
```

---

## Commands

### name

```
name <text>
```

Sets the blueprint's display name.

---

### description

```
description
```

Opens the string editor for a description. End with `~`.

---

### comments

```
comments
```

Internal builder notes.

---

### mode

```
mode <static|procedural>
```

| Value | Description |
|-------|-------------|
| `static` | The layout is fixed. Sections and their links are set manually with `static`. |
| `procedural` | The engine picks sections from the pool and assembles them randomly. |

---

### flags

```
flags <flag>
```

Toggle blueprint instance flags:

| Flag | Description |
|------|-------------|
| `idle_on_complete` | Instance remains available (idle) after being completed rather than immediately destroying itself. |
| `isolated` | Instance is isolated from the outside world (no tells, no summons, etc.). |
| `no_save` | Instance state is not saved. On shutdown/crash, instance is lost. |

---

### repop

```
repop <seconds>
```

Sets the repop interval for the instance in seconds. Controls how often mob resets trigger inside.

---

### areawho

```
areawho <value>
```

Sets the area title shown in the "who" list for players inside this instance.

---

### section

Manages the section pool (used in procedural mode).

```
section add <widevnum>
section delete <#>
section list
```

Adds/removes sections from the procedural pool.

---

### channel

```
channel add <channel_id>
channel list
channel del <#>
```

Associates communication channels with this blueprint instance.

---

### static

The `static` sub-command configures the fixed layout for `mode static` blueprints.

#### static link add

```
static link add <section1#> <link1#> <section2#> <link2#>
```

Connects link `<link1#>` on section `<section1#>` to link `<link2#>` on section `<section2#>`. Creates the passage between sections.

#### static special add

```
static special add <section#> <room_widevnum> <name>
```

Marks a room within a section as a "special" named location (e.g. boss room, treasure room). These can be referenced by scripts.

#### static recall

```
static recall <section#>
```

Sets which section is the recall/safe destination for the blueprint.

#### static entry add / remove

```
static entry add <section#> <link#>
static entry remove <#>
```

Adds/removes player entry points. Players appear at these link/room positions when they enter the instance.

#### static exit add / remove

```
static exit add <section#> <link#>
static exit remove <#>
```

Adds/removes exit points. Players at these positions leave the instance.

---

### addiprog / deliprog

```
addiprog <widevnum> <trigger> <phrase>
deliprog <group#>
```

Attaches/removes instance programs (scripts fired on instance events).

---

### varset / varclear

```
varset <name> <number|string|room|rsg> <yes|no> <value>
varclear <name>
```

Defines or removes blueprint-level variables used by scripts. Use `rsg` (alias `generator`) for random-string-generator variables with spec `<rsg_name>:<pattern|class>:<entry_name>`.

---

### list

```
bpedit list
```

Lists all blueprints in the current area.

---

## Example: Static Blueprint

```
bpedit create 5#800
bpedit 5#800

name Thornwood Cellar
mode static
repop 300
areawho Thornwood Cellar

static special add 1 5#100 Entry Hall
static special add 2 5#115 Boss Room

static link add 1 1 2 1

static recall 1
static entry add 1 1
static exit add 2 2

show
done
```
