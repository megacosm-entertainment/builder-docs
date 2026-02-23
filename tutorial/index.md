---
layout: default
title: Tutorial
nav_order: 12
has_children: true
---

# Builder Tutorial: The Thornwood Hollow

This tutorial walks through building a complete, functional mini-area from scratch. You will:

- Create an area with its own rooms and exits
- Build and configure NPCs (a quest-giver, a shopkeeper, a boss mob)
- Create objects (weapons, items, a container)
- Set up a multi-stage quest with objectives and rewards
- Write basic mob scripts
- Create a reputation faction
- Wire everything together

By the end you will have a small but fully playable area called **The Thornwood Hollow** — a woodland clearing with an inn, a cellar dungeon, and a ranger wardens questline.

---

## Prerequisites

- Immortal character (level 91+) with access to OLC
- Basic familiarity with MUD commands (movement, look, etc.)
- You should have read at least the [Creating a Room](../rooms/redit-create-a-room.html) and [Creating a Mobile](../mobiles/medit-create-a-mobile.html) guides

---

## Tutorial Parts

| Part | Topic |
|------|-------|
| [Part 1: Creating the Area](tutorial-part1-area.html) | Area setup, rooms, and exits |
| [Part 2: Creating Mobiles](tutorial-part2-mobiles.html) | Quest-giver, shopkeeper, and boss mob |
| [Part 3: Creating Objects](tutorial-part3-objects.html) | Weapons, items, and a container |
| [Part 4: Setting Up the Quest](tutorial-part4-quest.html) | Quest definition with stages and objectives |
| [Part 5: Mobile Scripts](tutorial-part5-scripts.html) | Quest-giver dialogue and scripted boss |
| [Part 6: The Reputation Faction](tutorial-part6-reputation.html) | Thornwood Wardens reputation system |

---

## Area Overview

**Area:** The Thornwood Hollow (`5#`)

| Type | Name | Widevnum Range |
|------|------|----------------|
| Rooms | The area | 5#100 – 5#199 |
| Mobiles | The area | 5#200 – 5#299 |
| Objects | The area | 5#300 – 5#399 |
| Scripts | The area | 5#400 – 5#499 |
| Quests | The area | 5#500 – 5#599 |
| Reputations | The area | 5#600 – 5#699 |

{: .notice}
**Note:** Replace `5` with your actual area UID throughout this tutorial.

{: .info}
**Widevnum namespaces are independent per entity type.** Room `5#100`, mob `5#100`, and object `5#100` can all coexist without conflict — each entity type maintains its own completely separate vnum space. The ranges above are chosen for tutorial organisation only; you are not required to follow them.
