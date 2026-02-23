---
layout: default
title: Creating a Reputation
nav_order: 1
parent: Reputation
---

# Creating a Reputation Faction

This guide walks through creating a reputation faction from scratch using `repedit`. By the end you'll have a working reputation system with ranks, descriptions, and a linked token.

{: .notice}
**Prerequisites:** Immortal (level 91+) with access to your area. Have your area's UID handy.

---

## Reputation Overview

A reputation consists of:

- **Metadata:** name, description, flags
- **Initial rank and points:** where a brand-new player starts
- **Ranks:** named tiers representing different levels of standing (hostile → neutral → friendly → revered, etc.)
- **Token:** optionally attached to ranks so players carry a "faction badge"

Players start at the initial rank/point value. Spending quest rewards or being affected by scripts changes their point total, and the rank updates automatically.

---

## Step 1 — Create the Reputation

```
repedit create 5#500
```

Replace `5#500` with your area UID and your chosen vnum. If successful you'll see:

```
Repedit: Reputation 5#500 created.
```

{: .info}
You can also use `#<vnum>` (current area shorthand) or a bare vnum if you are standing in your area.

---

## Step 2 — Open the Editor

```
repedit 5#500
```

Type `show` at any time to see the current state of the faction.

---

## Step 3 — Set the Name

```
name The Thornwood Wardens
```

The name is the display title of the faction shown to players in the reputation panel.

---

## Step 4 — Write the Description

```
description
```

This opens the string editor. Write a brief explanation of the faction for players:

```
The Thornwood Wardens are an order of rangers and druids dedicated to
protecting the ancient Thornwood Forest from encroaching lumber barons
and creatures of chaos. Earning their trust is no easy task.
~
```

---

## Step 5 — Set Flags

Flags modify how the reputation behaves. Flags are toggled on/off:

```
flags hidden
```

Removes the reputation from the player's panel until they discover it (recommended for secret factions).

---

## Step 6 — Add Ranks

Add at least two or three ranks to define the range of standing. Rank order is by insertion.

```
rank add Hostile
rank add Unfriendly
rank add Neutral
rank add Friendly
rank add Honored
rank add Revered
rank add Exalted
```

Each rank gets a sequential number (1, 2, 3...) automatically. Use `show` to see them.

---

## Step 7 — Configure Each Rank

Set properties for each rank using its index number:

```
rank 3 name Neutral
rank 3 capacity 500
rank 3 color yellow
rank 3 description Standing in Neutral — the Wardens acknowledge your existence.
```

| Sub-command | Description |
|-------------|-------------|
| `rank <#> name <text>` | Display name for the rank |
| `rank <#> capacity <points>` | Points required to reach this rank from the previous |
| `rank <#> color <color>` | Terminal color displayed in reputation listings |
| `rank <#> flags <flag>` | Toggle rank-level flags (see flags reference) |
| `rank <#> description [text]` | Description shown at this rank level |
| `rank <#> comments [text]` | Internal notes (not shown to players) |

**Available colors:** `blue`, `cyan`, `dark`, `green`, `magenta`, `red`, `white`, `yellow`

{: .notice}
**Tip:** `capacity` means "points needed to move from the previous rank to this rank". Rank 1 (typically Hostile) usually has capacity 0 (starting point).

---

## Step 8 — Set Rank Flags

Rank flags affect behavior at that standing level:

```
rank 1 flags hostile
rank 2 flags hostile
```

This marks ranks 1 and 2 as hostile — NPCs tagged with this reputation faction will attack players at those ranks.

```
rank 6 flags paragon
```

Marks rank 6 as the "paragon" rank — required for certain rewards.

---

## Step 9 — Set Initial Rank and Points

Tell the system where new players start:

```
initial 3 0
```

Format: `initial <rank#> <starting_points>`. This places new players at rank 3 (Neutral) with 0 points into that rank.

---

## Step 10 — Attach a Token (Optional)

You can attach a token to the reputation — players who carry this token are considered members:

```
token 5#600
```

Replace `5#600` with the widevnum of the token definition. Use `token none` to remove.

---

## Complete Example

```
repedit create 5#500
repedit 5#500

name The Thornwood Wardens

description
The Thornwood Wardens are an order of rangers and druids dedicated to
protecting the ancient Thornwood Forest from encroaching lumber barons
and creatures of chaos. Earning their trust is no easy task.
~

rank add Hostile
rank add Unfriendly
rank add Neutral
rank add Friendly
rank add Honored
rank add Revered
rank add Exalted

rank 1 capacity 0
rank 1 color red
rank 1 flags hostile

rank 2 capacity 500
rank 2 color magenta

rank 3 capacity 500
rank 3 color yellow

rank 4 capacity 1000
rank 4 color green

rank 5 capacity 2000
rank 5 color cyan

rank 6 capacity 3000
rank 6 color white
rank 6 flags paragon

rank 7 capacity 5000
rank 7 color white
rank 7 flags paragon

initial 3 0

show
done
```

---

## Giving Reputation to Players

Reputation is typically awarded via:

1. **Quest rewards** — use `reward add reputation` in `qedit` and set `reward <index> target 5#500`
2. **Scripts** — use the reputation-related script commands (see [Script Commands](../scripting/mobile-programs/mprog-script-commands.html))
3. **Kill events** — if the reputation has `on_encounter` flag set, proximity or kills may trigger it

---

## Next Steps

- [Reputation Command Reference](repedit-command-reference.html)
- [Reputation Flags Reference](repedit-flags-reference.html)
