---
layout: default
title: Sections
nav_order: 1
parent: Advanced Concepts
---

# Blueprint Sections (bsedit)

Blueprint sections are the building blocks of instanced content. A **section** defines a set of rooms that can be placed as a unit inside a blueprint. Sections can be static (a hand-crafted room layout) or procedurally generated mazes.

{: .warning}
**Instancing is an advanced feature.** Sections, blueprints, and dungeons work together to create instanced content. Make sure you understand all three editors before building instanced areas.

---

## Concepts

| Term | Description |
|------|-------------|
| **Section** | A defined set of rooms with connection points (links) |
| **Link** | An exit point on a section that can connect to another section |
| **Blueprint** | An assembly of sections into a complete instance layout |
| **Dungeon** | A definition for how a blueprint is presented to players |

A section's rooms must already exist in `redit` before being added to a section. The section then references that room range.

---

## Entering the Editor

```
bsedit create <widevnum>
bsedit <widevnum>
```

**Example:**
```
bsedit create 5#700
bsedit 5#700
```

Type `show` at any time to see the current state.

---

## Commands

### name

```
name <text>
```

Sets the section name used for identification in the blueprint editor.

---

### description

```
description
```

Opens the string editor for a description of this section. End input with `~`.

---

### comments

```
comments
```

Internal builder notes. Not visible to players.

---

### type

```
type <static|maze>
```

| Value | Description |
|-------|-------------|
| `static` | A hand-crafted set of rooms. Room layout is fixed. |
| `maze` | The section generates a random maze when instantiated. Configure with `maze`. |

---

### flags

```
flags <flag>
```

Toggle section flags. Currently available: `no_rotate`

| Flag | Description |
|------|-------------|
| `no_rotate` | Prevents the section from being rotated when placed in a blueprint. By default sections may be rotated to fit. |

---

### rooms

```
rooms <lower_widevnum> <upper_widevnum>
```

Defines the room range this section uses. Both vnums must be in the same area.

**Example:** `rooms 5#100 5#120`

{: .notice}
**Tip:** Rooms must be created in `redit` first. The section references them by vnum range — all rooms in that range belong to the section.

---

### recall

```
recall <widevnum>
```

Sets the designated recall/safe room within this section. Used when players are sent to the safe point of the instance.

---

### link

Links define the exit points where this section connects to other sections in a blueprint.

#### link list

```
link list
```

Lists all defined links.

#### link add

```
link add
```

Adds a new link. A link index is assigned automatically.

#### link # name

```
link <#> name <text>
```

Sets a name for this link point (for identification in the blueprint editor).

#### link # room

```
link <#> room <widevnum>
```

Sets which room in the section is the connection point for this link.

#### link # exit

```
link <#> exit <north|south|east|west|ne|nw|se|sw|up|down>
```

Sets which direction the link exits from its room. The exit must be an ENVIRONMENT flagged exit in `redit`.

#### link # delete

```
link <#> delete
```

Removes a link.

---

### maze

For `type maze` sections. Controls how the maze is generated.

#### maze size

```
maze size <width> <height>
```

Sets the grid dimensions of the maze. Both must be between 1 and 100. **Example:** `maze size 5 5`

#### maze templates add

```
maze templates add <weight> <room_widevnum> [exit_count]
```

Adds a room template to the maze generator. Optional `exit_count`: `0`=any, `1`=dead end, `2`=tunnel, `3`=T-junction, `4`=crossroads.

#### maze templates remove

```
maze templates remove <#>
```

#### maze templates exit

Configure exit template properties:

```
maze templates exit <#> flags <exit_flags>
maze templates exit <#> keyword <word>
maze templates exit <#> strength <value>
maze templates exit <#> material <name>
maze templates exit <#> key <obj_widevnum>
maze templates exit <#> pick <chance 0-100>
maze templates exit <#> lockflags <flags>
maze templates exit <#> clear
```

---

## Example: Static Section

```
bsedit create 5#700
bsedit 5#700

name Cellar West Wing
type static
rooms 5#100 5#110

link add
link 1 name West Entry
link 1 room 5#100
link 1 exit north

link add
link 2 name East Exit
link 2 room 5#110
link 2 exit east

recall 5#105
show
done
```
