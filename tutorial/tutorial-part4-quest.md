---
layout: default
title: "Part 4: Setting Up the Quest"
nav_order: 4
parent: Tutorial
---

# Part 4: Setting Up the Quest

In this part we create the "Kill the Rats" quest with two stages: clear the cellar and deal with Blagtooth.

---

## Quest Design

**Quest:** Thornwood Cellar Clearance  
**Widevnum:** `5#500`

### Stage 1 — Clear the Cellar
- Objective 1: Kill 10 sewer rats (mob `5#202`)
- Objective 2: Collect 3 rat tails (item `5#301`)

### Stage 2 — Deal with Blagtooth
- Objective 1: Kill Blagtooth, the Rat King (mob `5#203`)

### Stage 3 — Report Back
- Objective 1: Talk to Innkeeper Aldric (mob `5#200`)

### Rewards
- 50 quest points
- 1000 gold
- 1x Warden's Short Sword (`5#300`)
- +100 Thornwood Wardens reputation

---

## Step 1 — Create the Quest

```
qedit 5#500 create
```

---

## Step 2 — Set Basic Info

```
qedit 5#500 name Thornwood Cellar Clearance
qedit 5#500 summary Clear the rats from the inn cellar and deal with their leader.
qedit 5#500 description
Innkeeper Aldric is desperate. The cellar beneath his inn has become
overrun with vicious sewer rats, and their leader — a bloated monster
called Blagtooth — has turned the lower chambers into a brood warren.
Supplies are ruined and guests refuse to stay. Aldric needs someone
capable to clean out the infestation once and for all.
~
qedit 5#500 class mission
qedit 5#500 type side
qedit 5#500 category regional
qedit 5#500 scope character
qedit 5#500 repeat once
```

---

## Step 3 — Create Stage 1: Clear the Cellar

```
qedit 5#500 stage add 1
qedit 5#500 stage 1 summary Clear out the sewer rats
qedit 5#500 stage 1 description
Kill at least ten sewer rats and gather some tails as proof. The
cellar is accessible through the trapdoor in the inn.
~
qedit 5#500 stage 1 complete all
qedit 5#500 stage 1 source static
qedit 5#500 stage 1 next 2
```

**Objective 1: Kill 10 rats**

```
qedit 5#500 objective add 1 1
qedit 5#500 objective 1 1 type kill
qedit 5#500 objective 1 1 required 10
qedit 5#500 objective 1 1 target wnum 5#202
qedit 5#500 objective 1 1 summary Sewer Rat
```

**Objective 2: Collect 3 rat tails**

```
qedit 5#500 objective add 1 2
qedit 5#500 objective 1 2 type collect
qedit 5#500 objective 1 2 required 3
qedit 5#500 objective 1 2 quantity 1
qedit 5#500 objective 1 2 target wnum 5#301
qedit 5#500 objective 1 2 summary Rat Tail
```

---

## Step 4 — Create Stage 2: Kill Blagtooth

```
qedit 5#500 stage add 2
qedit 5#500 stage 2 summary Defeat Blagtooth, the Rat King
qedit 5#500 stage 2 description
Track down Blagtooth in the brood chamber deep in the cellar.
He will not go quietly.
~
qedit 5#500 stage 2 complete all
qedit 5#500 stage 2 source static
qedit 5#500 stage 2 next 3
```

**Objective: Kill Blagtooth**

```
qedit 5#500 objective add 2 1
qedit 5#500 objective 2 1 type kill
qedit 5#500 objective 2 1 required 1
qedit 5#500 objective 2 1 target wnum 5#203
qedit 5#500 objective 2 1 summary Blagtooth, the Rat King
```

---

## Step 5 — Create Stage 3: Report Back

```
qedit 5#500 stage add 3
qedit 5#500 stage 3 summary Report back to Innkeeper Aldric
qedit 5#500 stage 3 description
Return to Innkeeper Aldric in the inn above and tell him the good news.
~
qedit 5#500 stage 3 complete all
qedit 5#500 stage 3 source static
```

**Objective: Talk to Aldric**

```
qedit 5#500 objective add 3 1
qedit 5#500 objective 3 1 type talk
qedit 5#500 objective 3 1 target wnum 5#200
qedit 5#500 objective 3 1 summary Innkeeper Aldric
```

---

## Step 6 — Set Entry Stage

```
qedit 5#500 entry 1
```

---

## Step 7 — Set up Rewards

**Quest points:**

```
qedit 5#500 reward add points
qedit 5#500 reward 1 amount 50
```

**Gold:**

```
qedit 5#500 reward add currency
qedit 5#500 reward 2 currency gold
qedit 5#500 reward 2 amount 1000
```

**Warden's Short Sword:**

```
qedit 5#500 reward add item
qedit 5#500 reward 3 target 5#300
```

**Reputation:**

```
qedit 5#500 reward add reputation
qedit 5#500 reward 4 target 5#600
qedit 5#500 reward 4 amount 100
```

_(We'll create the 5#600 reputation faction in Part 6.)_

---

## Step 8 — Enable the Quest

```
qedit 5#500 enabled on
```

---

## Step 9 — Verify

```
qedit 5#500 show all
```

Check that:
- All 3 stages are listed
- Each stage has the correct objectives
- 4 rewards are defined
- Entry stage is 1
- Quest is enabled

---

## Summary

The quest is now defined. In the next part we'll write the scripts to make Aldric actually offer and complete the quest.

---

**Next:** [Part 5: Mobile Scripts](tutorial-part5-scripts.html)
