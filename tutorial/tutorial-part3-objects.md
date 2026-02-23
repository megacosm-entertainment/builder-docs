---
layout: default
title: "Part 3: Creating Objects"
nav_order: 3
parent: Tutorial
---

# Part 3: Creating Objects

In this part we create the items players find in the area: a weapon reward, a rat tail collectible, a pouch container, and consumables sold at the inn. We also set up the Trader's shop so players can buy goods.

---

## Object Plan

| Object | Widevnum | Type | Purpose |
|--------|----------|------|---------|
| Warden's Short Sword | 5#300 | Weapon | Quest reward |
| Rat Tail | 5#301 | Misc | Quest collectible (collect objective) |
| Leather Pouch | 5#302 | Container | Loot container for boss mob || Hearty Stew | 5#303 | Food | Sold by Trader at the inn |
| Mug of Ale | 5#304 | Drink Container | Sold by Trader at the inn |
---

## Step 1 — Create the Warden's Short Sword

This is a quest reward weapon given by Aldric on completion.

```
oedit 5#300 create
oedit 5#300

name wardens warden short sword broadblade
short a warden's short sword
long A warden's short sword has been left here.
description
A short sword of polished Thornwood steel, bearing the leaf insignia
of the Thornwood Wardens on its crossguard. Well-balanced and reliable.
type weapon
v0 sword
v1 2
v2 8
v3 slash
wear take wield
extra magic
material steel
weight 30
cost 500
done
```

The weapon values mean:
- `v0 sword` — weapon type
- `v1 2` `v2 8` — 2d8 damage dice
- `v3 slash` — slash damage type

---

## Step 2 — Create the Rat Tail

A collectible item rats drop that players bring back to Aldric.

```
oedit 5#301 create
oedit 5#301

name rat tail
short a rat tail
long A severed rat tail lies here.
description
A grey, hairless tail from one of the sewer rats. It smells faintly of
wet fur and garbage. Someone might want proof of the cull.
type trash
wear take
weight 1
cost 5
done
```

**Add the rat tail to the rat mob's drop table:**

```
medit 5#202
addoprog 5#301 random_prog 10
done
```

{: .notice}
The `10` is the drop chance percentage. Adjust as needed.

---

## Step 3 — Create the Leather Pouch

A container dropped by Blagtooth containing bonus loot.

```
oedit 5#302 create
oedit 5#302

name leather pouch bag
short a leather pouch
long A small leather pouch lies here.
description
A small pouch crafted from rough leather, its cord still tied. Something
bulges inside.
type container
v0 5
v1 closeable closed
v2 0
v3 5
wear take
weight 5
cost 50
done
```

Container values:
- `v0 5` — capacity (holds 5 items)
- `v1 closeable closed` — can be opened/closed, starts closed
- `v2 0` — no key required
- `v3 5` — max weight inside

---

## Step 4 — Place Objects

Add gold and the pouch to Blagtooth's inventory:

```
medit 5#203
addoprog 5#302 death_prog ~
done
```

{: .info}
In practice the pouch would be added as an `oreset` or through death script. See [Part 5: Scripts](tutorial-part5-scripts.html) for how to do this properly via a death program.

---

## Step 5 — Create the Hearty Stew

A filling meal sold by the Trader at the inn.

```
oedit 5#303 create
oedit 5#303

name stew hearty bowl
short a hearty stew
long A bowl of thick, steaming stew sits here.
description
A generous bowl of stew, thick with root vegetables and chunks of
tender meat. The warm broth smells hearty and filling.
type food
food hunger 4
food full 2
wear take
weight 1
cost 20
done
```

Food values:
- `food hunger 4` — reduces hunger by 4 hours
- `food full 2` — reduces fullness (eating too much) by 2

---

## Step 6 — Create the Mug of Ale

A drink sold by the Trader. Drink containers hold a liquid that can be consumed with the `drink` command.

```
oedit 5#304 create
oedit 5#304

name mug ale
short a mug of ale
long A frothing mug of ale sits here.
description
A sturdy ceramic mug filled with amber ale. Foam clings to the inside
of the rim as the liquid settles.
type drink container
drink liquid ale
drink capacity 8
drink amount 8
wear take
weight 2
cost 12
done
```

Drink container values:
- `drink liquid ale` — sets the liquid type (any name from `liqedit list`)
- `drink capacity 8` — total sips available when full
- `drink amount 8` — starts completely full

---

## Step 7 — Set Up the Trader's Shop

Now that the shop items exist, configure the Trader (`5#201`) with shop stock. The `shopkeeper` act flag was already set in Part 2; this adds the inventory she sells.

```
medit 5#201
shop assign
shop hours 7 22
shop profit 120 80
shop stock add object 5#300
shop stock add object 5#303
shop stock add object 5#304
shop stock 1 quantity 1 0
shop stock 2 quantity 10 2
shop stock 3 quantity 5 2
done
```

| Command | Effect |
|---------|--------|
| `shop assign` | Creates the shop data on the mobile |
| `shop hours 7 22` | Open 7am–10pm (24-hour clock) |
| `shop profit 120 80` | Charges 120% of item value; pays 80% when buying from players |
| `shop stock add object 5#300` | Stock slot 1: Warden's Short Sword |
| `shop stock add object 5#303` | Stock slot 2: Hearty Stew |
| `shop stock add object 5#304` | Stock slot 3: Mug of Ale |
| `shop stock 1 quantity 1 0` | Sword: 1 available, never restocks |
| `shop stock 2 quantity 10 2` | Stew: 10 available, restocks 2 per cycle |
| `shop stock 3 quantity 5 2` | Ale: 5 available, restocks 2 per cycle |

Players browse with `list` and purchase with `buy <item>`. Use `shop restock <minutes>` to set how often stock refreshes.

---

## Summary

| Object | Widevnum | Where Used |
|--------|----------|-----------|
| Warden's Short Sword | 5#300 | Quest reward (given by Aldric) |
| Rat Tail | 5#301 | Drops from Sewer Rat; collect objective |
| Leather Pouch | 5#302 | Drops from Blagtooth |
| Hearty Stew | 5#303 | Sold by Thornwood Trader |
| Mug of Ale | 5#304 | Sold by Thornwood Trader |

---

**Next:** [Part 4: Setting Up the Quest](tutorial-part4-quest.html)
