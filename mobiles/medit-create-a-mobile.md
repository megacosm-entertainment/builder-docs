---
layout: default
title: Creating a Mobile
nav_order: 1
parent: Mobile Editor
---

# Creating a Mobile
{: .no_toc }

This guide walks through creating a complete NPC from scratch using the Mobile Editor (`medit`). We'll build a guard NPC with a shop and a greeting script.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Prerequisites

- Building access with an assigned widevnum range in your area.
- Know your area's UID (use `aedit <area_name>` and check the UID field, or use `areas`).

---

## Step 1: Open the Mobile Editor

Use a **widevnum** in the format `area_uid#vnum`:

```
medit 5#200
```

If vnum `5#200` doesn't exist, you'll be asked to confirm creation. Type `yes`.

---

## Step 2: Set Name and Keywords

Keywords are what players use to interact with the mob (`kill guard`, `look guard`):

```
name thornwood guard
```

Multiple keywords are separated by spaces.

---

## Step 3: Set Short Description

The short description is shown inline in room descriptions and combat messages:

```
short a Thornwood guard
```

Example output: `A Thornwood guard stands at attention here.`

---

## Step 4: Set Long Description

Shown when the mob is standing in a room (the room description line):

```
long A Thornwood guard stands at attention here.
```

---

## Step 5: Write Examine Description

Shown when a player types `look guard`:

```
description
The guard is a tall human dressed in polished chainmail bearing the
Thornwood insignia — a silver oak leaf on a field of green. $e carries
a longsword at $s hip and watches every visitor carefully.
@
```

{: .notice }
You can use `$e`, `$s`, `$m` pronouns in descriptions (he/she/it, his/her/its, him/her/it) — they adapt to the mob's sex setting.

---

## Step 6: Set Race and Sex

```
race human
sex male
```

Race affects body parts, size, and some defaults. See the [Flags Reference](medit-flags-reference) for all available races.

---

## Step 7: Set Level

```
level 15
```

Setting level automatically calculates default hit dice, mana dice, and movement.

---

## Step 8: Set Alignment

```
alignment 500
```

Ranges from -1000 (pure evil) to +1000 (pure good). `0` is neutral.

---

## Step 9: Set Act Flags

Act flags control NPC behavior:

```
act sentinel
act protected
```

- `sentinel` — NPC won't wander from its starting room.
- `protected` — Cannot be stolen from by players.

See [Act Flags](medit-flags-reference#act-flags) for all options.

---

## Step 10: Set Combat Properties

```
off kick parry
```

Offensive flags determine what combat techniques the NPC uses. See [Offensive Flags](medit-flags-reference#offenses).

For resistances and immunities:

```
immune charm
res weapon
```

---

## Step 11: Set Damage Dice

If not using a weapon object, set raw damage dice:

```
damdice 2 8 4
```

Format: `damdice <number> <sides> <bonus>` = `2d8+4` damage per hit.

---

## Step 12: Set Position

```
position start standing
position default standing
```

Start position is where the mob begins. Default is where it returns to after combat.

---

## Step 13: Add a Script

Attach a greeting mob prog (created separately with `mpedit`):

```
addmprog 5#300 greet 100
```

Format: `addmprog <script_widevnum> <trigger> <percentage_or_phrase>`

See [Scripting Basics](../scripting/scripting-basics) for creating scripts.

---

## Step 14: Review

```
show
```

Review all fields. Press Enter to see the show display without typing `show`.

---

## Step 15: Save

```
done
```

---

## Complete Example

```
medit 5#200
name thornwood guard
short a Thornwood guard
long A Thornwood guard stands at attention here.
description
The guard is a tall human dressed in polished chainmail bearing the
Thornwood insignia — a silver oak leaf on a field of green. $e carries
a longsword at $s hip and watches every visitor carefully.
@
race human
sex male
level 15
alignment 500
act sentinel protected
off kick parry
immune charm
res weapon
damdice 2 8 4
position start standing
position default standing
addmprog 5#300 greet 100
show
done
```

---

## Adding a Shop

To make the NPC a shopkeeper, use the `shop` sub-command:

```
medit 5#200
act banker
shop open 6
shop close 20
shop profit 115 80
shop type weapon armor
shop list
done
```

See [Shop Data](medit-additional-data#shop) for all shop commands and options.

---

## Adding Quest Master Functionality

```
medit 5#200
act questor
questor min 5
questor max 15
questor cooldown 30
done
```

See [Questor Data](medit-additional-data#questor) for full details.

---

## Next Steps

- [Command Reference](medit-command-reference) — all medit commands
- [Flags Reference](medit-flags-reference) — all act, act2, offensive, immune/res/vuln flags
- [Reset the mob in a room](../rooms/redit-command-reference#mreset) using `mreset`
