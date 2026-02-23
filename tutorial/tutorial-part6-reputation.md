---
layout: default
title: "Part 6: The Reputation Faction"
nav_order: 6
parent: Tutorial
---

# Part 6: The Reputation Faction

In this part we create the Thornwood Wardens reputation faction referenced in the quest rewards.

---

## Step 1 — Create the Reputation

```
repedit create 5#600
repedit 5#600
```

---

## Step 2 — Basic Info

```
name The Thornwood Wardens
description
The Thornwood Wardens are an ancient order of rangers and druids
who protect the Thornwood Forest against those who would defile it.
Earning their trust is difficult, but their allies want for nothing.
~
```

---

## Step 3 — Add Ranks

```
rank add Outsider
rank add Recognized
rank add Ally
rank add Trusted
rank add Honored Warden
```

---

## Step 4 — Configure Ranks

```
rank 1 capacity 0
rank 1 color red
rank 1 description You are an unknown stranger to the Thornwood Wardens.

rank 2 capacity 100
rank 2 color yellow
rank 2 description The Wardens have heard of your deeds and acknowledge your existence.

rank 3 capacity 500
rank 3 color green
rank 3 description You are considered a friend and ally of the Thornwood Wardens.

rank 4 capacity 1000
rank 4 color cyan
rank 4 description The Wardens trust you with sensitive tasks and reward your loyalty.

rank 5 capacity 3000
rank 5 color white
rank 5 flags paragon
rank 5 description You are a fully recognized Honored Warden — the highest standing.
```

---

## Step 5 — Set Initial Standing

New players start as Outsiders with 0 points:

```
initial 1 0
```

---

## Step 6 — Confirm

```
show
done
```

---

## Step 7 — Verify Quest Reward Link

Open the quest and verify the reputation reward target is set to `5#600`:

```
qedit 5#500 reward show 4
```

If it shows `target: 5#600`, you're good. If not:

```
qedit 5#500 reward 4 target 5#600
```

---

## Congratulations!

The Thornwood Hollow is now a complete, functional mini-area with:

- [x] 6 rooms with descriptions and exits
- [x] 4 NPCs (quest-giver, shopkeeper, trash mob, boss)
- [x] 3 objects (weapon reward, collectible, loot container)
- [x] A 3-stage quest with multi-objective stages and 4 reward types
- [x] Greeting and completion scripts on the quest-giver NPC
- [x] A death script on the boss
- [x] A reputation faction with 5 ranks

---

## What to Try Next

Now that you've built the basics, expand your skills:

- **Add more quest variety** — branching stages, pool objectives
- **Make mobs more interesting** — fear scripts, callouts when low on health
- **Add a shop** — stock the Thornwood Trader with weapons and potions using `shedit` or shop commands in `medit`
- **Create tokens** — unique buffs or markers for Warden members
- **Build a dungeon** — sections + blueprint + dngedit for instanced runs
- **Explore area scripts** — `aredit` can attach progs to the area itself for timed events

---

## Full Builder Reference

| Editor | Guide |
|--------|-------|
| Rooms | [redit-create-a-room](../rooms/redit-create-a-room.html) |
| Mobiles | [medit-create-a-mobile](../mobiles/medit-create-a-mobile.html) |
| Objects | [oedit-create-an-object](../objects/oedit-create-an-object.html) |
| Quests | [qedit-create-a-quest](../quests/qedit-create-a-quest.html) |
| Scripts | [scripting-basics](../scripting/scripting-basics.html) |
| Reputation | [repedit-create-a-reputation](../reputation/repedit-create-a-reputation.html) |
