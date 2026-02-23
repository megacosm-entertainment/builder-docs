---
layout: default
title: "Part 2: Creating Mobiles"
nav_order: 2
parent: Tutorial
---

# Part 2: Creating Mobiles

In this part we create three NPCs: the innkeeper/quest-giver Aldric, the shopkeeper, and the boss rat.

---

## NPC Plan

| Mobile | Widevnum | Role |
|--------|----------|------|
| Innkeeper Aldric | 5#200 | Quest-giver NPC |
| Thornwood Trader | 5#201 | Shop NPC |
| Giant Sewer Rat | 5#202 | Standard rat (spam mob) |
| Blagtooth, the Rat King | 5#203 | Boss mob |

---

## Step 1 — Create Innkeeper Aldric

```
medit 5#200 create
medit 5#200
```

Configure:

```
name aldric innkeeper
short Innkeeper Aldric
long Innkeeper Aldric stands behind the bar, polishing a mug.
description
A broad-shouldered man with a salt-and-pepper beard and a wary eye.
Aldric has run this inn since inheriting it from his father, and the
growing rat problem in the cellar has him deeply worried.
race human
bodytype humanoid
sex male
pronounss he
pronounos him
pronounpas his
pronounpps his
pronounrs himself
level 10
align 500
act sentinel questor
position standing
done
```

{: .info}
**Body type and pronouns:** `bodytype` sets the mob's anatomy (used for hit-location and equipment). `pronounss/os/pas/pps/rs` set the five pronoun forms — subjective (*he*), objective (*him*), possessive adjective (*his*), possessive pronoun (*his*), and reflexive (*himself*). The game uses these when constructing messages about the mob.

{: .info}
The `questor` act flag marks this NPC as a quest-giver. The `sentinel` flag keeps him in place.

**Place Aldric in the inn:**

```
redit 5#111
mreset 5#200 1
done
```

---

## Step 2 — Create the Shopkeeper

```
medit 5#201 create
medit 5#201

name trader merchant thornwood
short The Thornwood Trader
long A stout trader stands beside a table piled with gear.
description
A road-worn merchant who sets up shop wherever coin is to be made.
She eyes your purse with professional interest.
race human
bodytype humanoid
sex female
pronounss she
pronounos her
pronounpas her
pronounpps hers
pronounrs herself
level 8
act sentinel shopkeeper
position standing
done
```

**Place the trader in the inn:**

```
redit 5#111
mreset 5#201 1
done
```

---

## Step 3 — Create the Sewer Rat

This is the regular rat mob players will kill for the quest objective.

```
medit 5#202 create
medit 5#202

name rat sewer
short a sewer rat
long A sewer rat skitters through the shadows.
description
A large rat, nearly the size of a small cat. Its fur is matted and its
teeth are yellowed and sharp. It watches you with small, cunning eyes.
race rat
bodytype quadruped
sex neutral
pronounss it
pronounos it
pronounpas its
pronounpps its
pronounrs itself
level 3
align -100
act aggressive
off bash
done
```

**Place rats in the cellar passages:**

```
redit 5#112
mreset 5#202 3
done

redit 5#113
mreset 5#202 4
done

redit 5#114
mreset 5#202 3
done
```

---

## Step 4 — Create Blagtooth, the Rat King

The boss mob — a named, powerful version of the giant rat.

```
medit 5#203 create
medit 5#203

name blagtooth rat king boss
short Blagtooth, the Rat King
long Blagtooth, the Rat King, crouches in the center of the brood chamber.
description
An immense rat the size of a large dog. Old scars cover its muzzle and
flanks. A crude crown of twisted wire sits between its ears. Its eyes
burn with an intelligence that has no business being there.
race rat
bodytype quadruped
sex male
pronounss he
pronounos him
pronounpas his
pronounpps his
pronounrs himself
level 15
align -500
act aggressive
off bash dodge disarm
done
```

**Place Blagtooth in the boss chamber:**

```
redit 5#115
mreset 5#203 1
done
```

---

## Step 5 — Verify

Walk to the inn and confirm Aldric and the trader are present:

```
goto 5#111
look
```

Walk to the cellar and confirm rats are present:

```
down
look
```

Walk to the boss chamber:

```
north
back
east
east
look
```

---

## Summary

| Mobile | Widevnum | Location |
|--------|----------|----------|
| Innkeeper Aldric | 5#200 | The Thornwood Inn (5#111) |
| Thornwood Trader | 5#201 | The Thornwood Inn (5#111) |
| Giant Sewer Rat | 5#202 | Cellar rooms (5#112, 5#113, 5#114) |
| Blagtooth, the Rat King | 5#203 | The Brood Chamber (5#115) |

---

**Next:** [Part 3: Creating Objects](tutorial-part3-objects.html)
