---
layout: default
title: "Part 1: Creating the Area"
nav_order: 1
parent: Tutorial
---

# Part 1: Creating the Area

In this part we create the area, build the core rooms, and connect them with exits.

---

## Step 1 — Create the Area

If you are starting fresh you need an area definition first. Use `aedit` to create one:

```
aedit create Thornwood Hollow
```

Note your area UID (shown in `aedit show`). We'll use `5` as the example UID throughout this tutorial.

{: .info}
If you already have an area, skip to Step 2.

---

## Step 2 — Plan the Room Layout

Our area has these rooms:

```
[5#110] The Thornwood Clearing (outdoor start)
    |
   [S]
    |
[5#111] The Thornwood Inn (indoor)
    |
   [D] (down)
    |
[5#112] The Inn Cellar (dungeon entrance)
    |
   [N/S/E/W] (cellar rooms)
[5#113] Cellar North Passage
[5#114] Cellar East Chamber
[5#115] Boss Chamber
```

---

## Step 3 — Create the Start Room

```
redit 5#110 create
```

Now open and configure it:

```
redit 5#110
name The Thornwood Clearing
sector forest
room outdoors noteleport
description
A wide clearing in the ancient Thornwood Forest. Towering oaks ring
the grassy open space, their gnarled branches forming a natural canopy
overhead. A well-worn path leads south toward a warm, firelit inn.
~
done
```

---

## Step 4 — Create the Inn

```
redit 5#111 create
redit 5#111
name The Thornwood Inn
sector inside
room indoors notrack
description
The common room of a well-worn woodland inn. Hunting trophies line
the timber walls. A bar runs along the north end, and a trapdoor in
the corner leads down to the cellar. The innkeeper wipes down the bar,
watching you carefully.
~
done
```

---

## Step 5 — Create the Cellar Entrance

```
redit 5#112 create
redit 5#112
name The Inn Cellar
sector inside
room dark nodrop
description
A damp stone cellar beneath the inn. Barrels and crates line the walls,
many gnawed by teeth. The smell of rats is unmistakable. Passages
lead north and east through the gloom.
~
done
```

---

## Step 6 — Create the Cellar Passages

```
redit 5#113 create
redit 5#113
name Cellar North Passage
sector inside
room dark
description
A narrow passage carved from the earth and stone. Water drips from the
ceiling. Something rustles in the darkness ahead.
~
done

redit 5#114 create
redit 5#114
name Cellar East Chamber
sector inside
room dark
description
A wider chamber used for dry storage. Most of the old crates have been
shredded. Rat droppings cover the floor. A heavier door leads further east.
~
done
```

---

## Step 7 — Create the Boss Chamber

```
redit 5#115 create
redit 5#115
name The Brood Chamber
sector inside
room dark nosupplicate
description
A foul-smelling cavern scraped beneath the cellar. Bones and scraps of
cloth litter the floor. In the center, an enormous rat sits amid a nest
of its offspring, red eyes gleaming.
~
done
```

---

## Step 8 — Link the Rooms with Exits

Connect all the rooms:

```
redit 5#110
south 5#111
done

redit 5#111
north 5#110
down 5#112
done

redit 5#112
up 5#111
north 5#113
east 5#114
done

redit 5#113
south 5#112
done

redit 5#114
west 5#112
east 5#115
done

redit 5#115
west 5#114
done
```

---

## Step 9 — Verify

Walk the area to confirm all exits work:

```
goto 5#110
look
south
look
down
look
north
look
back
east
look
east
look
```

All rooms should be reachable and described correctly.

---

## Summary

You now have a 6-room mini-area:

| Room | Widevnum | Description |
|------|----------|-------------|
| The Thornwood Clearing | 5#110 | Outdoor entry to the area |
| The Thornwood Inn | 5#111 | Interior hub with quest-giver |
| The Inn Cellar | 5#112 | Dungeon entrance |
| Cellar North Passage | 5#113 | Rat patrol area |
| Cellar East Chamber | 5#114 | Rat gathering area |
| The Brood Chamber | 5#115 | Boss fight room |

---

**Next:** [Part 2: Creating Mobiles](tutorial-part2-mobiles.html)
