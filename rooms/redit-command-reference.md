---
layout: default
title: REdit Command Reference
nav_order: 2
parent: Room Editor
---

# Room Editor Command Reference
{: .no_toc }

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Entering the Editor

```
redit <widevnum>
redit <uid>#<vnum>
```

If the room doesn't exist, you will be prompted to create it. Examples:

```
redit 5#100          // open/create room vnum 100 in area UID 5
redit 100            // shorthand if you're within area UID 5's range
```

---

## Command List

```
?              addcdesc       addrprog       commands       comments       
coords         create         delcdesc       delrprog       description    
dislink        down           east           ed             editcdesc      
heal           locale         mana           move           mreset         
name           north          northeast      northwest      oreset         
owner          persist        room           room2          sector         
show           south          southeast      southwest      up             
west           varset         varclear       
```

---

## Room Configuration

### name `<string>`

Sets the room's display name. Shown as the room header when players look around.

```
name The Thornwood Hollow
```

---

### description

Opens the string editor. Type the room description, end with `@` on a blank line.

```
description
The hollow is bathed in pale moonlight...
@
```

---

### sector `<type>`

Sets the terrain sector. Affects movement cost, map appearance, and environmental effects.

```
sector forest
sector city
sector underwater
```

See [Sector Types](redit-flags-reference#sector-types) for all options.

---

### room `<flag> [flag] ...`

Toggles one or more [Room Flags](redit-flags-reference#room-flags). Multiple flags can be set at once.

```
room dark
room safe no_mob
room indoors
```

---

### room2 `<flag> [flag] ...`

Toggles one or more [Room2 Flags](redit-flags-reference#room2-flags).

```
room2 vis_on_map
room2 no_quit underground
```

---

### region `[#|name|default]`

With no arguments, lists all regions defined for the area and marks the current room's region with `*`.

With an argument, assigns the room to the specified region. Regions are sub-divisions of an area that can override `areawho`, `placetype`, `topic`, and flags for their rooms. Regions are defined and managed in the area editor with `aedit regions`.

- `region` — lists all regions, shows which one this room is in
- `region default` — returns the room to the area's default region
- `region <#>` — assigns the room by number (from `region` or `aedit regions list`)
- `region <name>` — assigns the room by region name (prefix match)

```
region
region default
region 2
region forest
```

---

### heal `<number>`

Sets the HP regeneration rate multiplier for the room. `100` = normal rate. Higher values speed regen.

```
heal 150
```

---

### mana `<number>`

Sets the mana regeneration multiplier.

```
mana 120
```

---

### move `<number>`

Sets the movement regeneration multiplier.

```
move 80
```

---

### locale `<number>`

Sets a locale ID for the room. Mobs with the `stay_locale` act flag will only wander within rooms sharing the same locale.

```
locale 5
```

---

### owner `<player_name>`

Assigns an owner to the room. The owner bypasses the `private` flag. Used for player housing.

```
owner Brandis
```

---

### persist `<yes|no>`

When `yes`, the room persists in memory and doesn't get cleaned up during inactive instance garbage collection.

```
persist yes
```

---

### coords `<x> <y> <z> [map_uid]`

Sets the room's wilderness coordinates. Used with the `view_wilds` room flag to anchor a room to a map position.

```
coords 42 17 0 1
```

---

## Exit Commands

Create exits with direction keywords. Syntax:

```
<direction> <destination_widevnum> [exit_flags...] [key_vnum]
```

### north / south / east / west / northeast / northwest / southeast / southwest / up / down

Creates an exit in the named direction.

```
north 5#101
north 5#101 door
north 5#101 door locked 800
```

Exit flags go after the destination. Include a key widevnum after the flags if the door requires a key.

{: .notice }
Exits are automatically double-linked — a corresponding reverse exit is created in the destination room.

### dislink

Removes the link between this room's exits and their destinations without deleting the exits themselves. Used when you want to change destinations without breaking the exit.

```
dislink north
```

---

## Resets

### mreset `<mob_widevnum> <max_in_room> <min_to_load>`

Adds a mob reset. The mob respawns here when the area resets, as long as fewer than `max_in_room` of that mob are already present (across all locations), and at least `min_to_load` were present before the reset.

```
mreset 5#200 2 1
mreset 2#500 1 1
```

---

### oreset

Adds an object reset for the room — an object that respawns on the floor here.

```
oreset 5#800 1
```

---

## Extra Descriptions

### ed `<keywords>`

Opens the string editor to write an extra description. Players can `look <keyword>` to read it.

```
ed mossy stones
These ancient stones are covered in soft green moss...
@
```

---

## Conditional Descriptions

### addcdesc `<type> <phrase>`

Adds a conditional description shown based on player state or world conditions. The description shows in addition to the main room description when conditions are met.

```
addcdesc time night
Shadows lengthen as darkness falls across the hollow.
@
```

---

### editcdesc `<number>`

Opens an existing conditional description (by index) for editing.

```
editcdesc 1
```

---

### delcdesc `<number>`

Deletes a conditional description by its index number.

```
delcdesc 1
```

---

## Scripts

### addrprog `<script_widevnum> <trigger> <phrase>`

Attaches a room script. The `<trigger>` is the event name and `<phrase>` is the match string or percentage.

```
addrprog 5#300 greet 100
addrprog 5#301 entry 50
addrprog 5#302 speech hello
```

See [Room Script Triggers](../scripting/room-programs/rprog-triggers) for all trigger types.

---

### delrprog `<number>`

Removes an attached room script by its index.

```
delrprog 1
```

---

## Variables

### varset `<name> <type> <yes|no> <value>`

Sets a persistent variable on the room. Types: `number`, `string`, `room`, `rsg`.

| Type | Example |
|------|--------|
| `number` | `varset door_state number no 0` |
| `string` | `varset label string no open` |
| `room` | `varset hometown room yes 5#100` |
| `rsg` | `varset room_name rsg yes region_names:pattern:full_name` |

The `yes/no` flag controls whether the variable is saved to disk. Use `rsg` (or the alias `generator`) to generate a random string from an RSG spec — `<rsg_name>:<pattern|class>:<entry_name>`. The variable is re-evaluated each time it is expanded.

{: .info}
**Variable expansion in string fields:** Variables can be used inside a room's `name`, `title`, and `description` fields using `$<var_name>` syntax. This allows dynamic text based on stored or generated values.

---

### varclear `<name>`

Removes a variable from the room.

```
varclear door_state
```

---

## Utility

### show

Displays the current room's full state including all settings, exits, resets, and attached scripts. Also triggered by pressing Enter while in the editor.

---

### commands

Lists all available commands in the room editor.

---

### comments

Opens the string editor for builder-only notes. Players never see this. Use it to document plans, related widevnums, scripting notes, etc.

```
comments
This room connects to the dungeon entrance at 5#200.
Shop NPC is at vnum 5#210.
@
```

---

### ?

Displays available flag banks. You can query specific flag types:

```
? room
? room2
? sector
? exit
```

---

### create `[widevnum]`

Creates a new room at the specified widevnum, or the next available vnum if none specified.

```
create
create 5#105
```

Note: It is usually easier to open the target widevnum directly with `redit 5#105` and confirm creation.

This is used to access the list of available flag banks, but is not limited to room flags. You can do things like `? token` to get a list of token types that can be set, for example.

### addcdesc `<type> <phrase>`
Used to add conditional descriptions to the room, based on specific criteria.

### addrprog `<vnum> <trigger> <phrase>`

### commands
Shows the list of commands available in the room editor

### comments
Opens the string editor to write builder comments. This field is like writing any description, but is only visible to builders. Leave notes here about plans, things you need help with, the purpose of the room, related vnums, etc.

### coords `<x> <y> <z> [uid]`
Sets the room's coordinates on a wilderness map. Meant for use with the show_wilds room2 flag.

### create `[vnum]`
Creates a room using the next available vnum in the zone. If `[vnum]` is specified, it will create a room at that vnum instead.

### delcdesc

### delrprog `<number>`
Removes the selected room prog from the room.

### description
Enters the string editor to set a description for the room.

### dislink

### down

### east

### ed

### editcdesc

### heal `<number>`
Sets the health regen rate of the room.

### locale `<number>`
Sets a locale for the room. Locales are used for the `stay_locale` mob flag.

### mana `<number>`
Sets the mana regen rate of the room.

### move `<number>`
Sets the movement regen rate of the room.

### mreset `<vnum> <maximum number> <minimum number>`
Sets a mob reset for the room.

### name `<string>`
Sets the name of the room.

### north

### northeast

### northwest

### oreset

### owner `<string>`
Sets an owner for the room. Owners are able to ignore the 'private' room flag.

### persist `<yes|no>`

### room `<flag1> [flag2] [flag3] ...`
Sets one or more [`room` flags](redit-flags-reference#room-flags)

### region `[#|name|default]`
Lists regions (no args) or assigns the room to a region by number, name prefix, or `default`. See [`regions`](../areas/aedit-command-reference#regions-subcommand-args) in the area editor.

### room2 `<flag1> [flag2] [flag3] ...`

### sector `<sector>`
Sets the [sector](redit-flags-reference#sector-types) of the room. Used for movement loss, among other things.

### show
Shows the current state of the room being edited. Can also be accessed by pressing enter.

### south

### southeast

### southwest

### up

### west

### varset `<name> <number|string|room|rsg> <yes|no> <value>`
Sets the given value on the room index. Use `rsg` for random-string-generator variables.

### varclear