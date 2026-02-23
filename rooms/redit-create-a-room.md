---
layout: default
title: Creating a Room
nav_order: 1
parent: Room Editor
---

# Creating a Room
{: .no_toc }

This guide walks through creating your first room using the in-game Room Editor (`redit`). By the end you'll have a fully described room with exits, flags, and a reset mob.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Prerequisites

- You must have building access and be in the area you want to build in (use `goto <area_vnum>` or `aedit <area_name>` then `goto`).
- Reserve a vnum range for your area first (see the [Area Editor guide](../areas/aedit-create-an-area)).

---

## Step 1: Enter the Room Editor

Open a room for editing with a **widevnum**. If your area has UID `5` and you want to create room `100`:

```
redit 5#100
```

Or, if you are already inside your area's vnum range, you can use just the vnum:

```
redit 100
```

If the room doesn't exist yet, `redit` will ask you to confirm creation. Type `yes`.

---

## Step 2: Set the Room Name

```
name The Thornwood Hollow
```

Room names appear in the header when players look at a room. Keep them evocative and location-appropriate.

---

## Step 3: Write a Description

```
description
```

This opens the string editor. Type your description and end with `@` on a blank line:

```
A hollow in the heart of the Thornwood, where ancient oaks twist their
roots around mossy stones. Pale shafts of moonlight filter through the
canopy above, casting silver patterns on the leaf-strewn floor. To the
north, the trees thin and a worn path leads deeper into the forest.
@
```

{: .notice }
Good room descriptions are 2–5 sentences. Describe what the player can **see**, not abstract game information.

---

## Step 4: Set the Sector

The sector type determines movement cost, map appearance, and some gameplay mechanics:

```
sector forest
```

See [Sector Types](redit-flags-reference#sector-types) for all options.

---

## Step 5: Add Room Flags

Flags control gameplay behavior. Add them with the `room` command:

```
room indoors
```

For multiple flags at once:

```
room safe no_mob
```

Some flags require room2:

```
room2 vis_on_map
```

See the [Flags Reference](redit-flags-reference) for all available flags and their effects.

---

## Step 6: Create Exits

Exits link rooms together. Use the cardinal directions as commands:

```
north 5#101
south 5#099
```

This creates double-linked exits — room `5#101` will automatically show a matching south exit back to this room.

To create a door on an exit, add door flags after the destination:

```
north 5#101 door
```

To add a lockable door:

```
north 5#101 door locked
```

The full syntax for a complex exit:

```
north <destination_vnum> [exit_flags] [key_vnum] [description]
```

---

## Step 7: Add Extra Descriptions

Extra descriptions are shown when a player `look`s at specific objects or features in the room (not objects in their inventory — this is room-level scenery):

```
ed mossy stones
These ancient stones are covered in soft green moss, worn smooth by
centuries of rain and wind.
@
```

Players can then type `look mossy stones` or `look stones` to see this description.

---

## Step 8: Set Regen Rates (Optional)

Control how fast players regenerate in this room:

```
heal 150
mana 150
move 100
```

Default is 100 (100%). Higher values increase regen speed.

---

## Step 9: Add Mob Resets

Mob resets cause a mob to respawn here after an area reset:

```
mreset 5#200 1 1
```

Format: `mreset <mob_widevnum> <max_in_room> <min_to_load>`

- **max_in_room** — Maximum number of this mob that can be in the room at once.
- **min_to_load** — Won't load if fewer than this many still exist. Usually `1`.

---

## Step 10: Check Your Work

Type `show` (or press Enter) to see the full room state:

```
show
```

You'll see:
- Name and description
- Room and room2 flags
- Sector type
- Exits and their flags
- Mob and object resets
- Attached room progs

---

## Step 11: Save and Exit

```
done
```

Rooms save automatically when you use `done`. You can also use `asave area` to save everything in the area at once.

---

## Complete Example

```
redit 5#100
name The Thornwood Hollow
description
A hollow in the heart of the Thornwood, where ancient oaks twist their
roots around mossy stones. Pale shafts of moonlight filter through the
canopy above, casting silver patterns on the leaf-strewn floor. To the
north, the trees thin and a worn path leads deeper into the forest.
@
sector forest
heal 120
north 5#101
south 5#099
ed mossy stones
These ancient stones are covered in soft green moss, worn smooth by
centuries of rain and wind.
@
mreset 5#200 2 1
show
done
```

---

## Next Steps

- Link your room to an existing area exit
- Add conditional descriptions with `addcdesc`
- Attach room scripts with `addrprog`
- See the full [Command Reference](redit-command-reference)
