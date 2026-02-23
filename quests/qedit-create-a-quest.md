---
layout: default
title: Creating a Quest
nav_order: 1
parent: Quests
---

# Creating a Quest

This guide walks through building a complete quest from scratch using the in-game `qedit` command. By the end you'll have a working multi-stage quest with objectives, rewards, and a quest-giver NPC hook.

{: .notice}
**Prerequisites:** You must be an immortal (level 91+) with builder privileges in the area. You need an area already created (see the [Area Editor guide](../areas/)) and at least one mobile to act as the quest-giver.

---

## Overview of Quest Structure

A quest in Sentience looks like this:

```
Quest (qedit)
├── Metadata: name, summary, description, class, type, category, scope
├── Settings: repeat, allowance, cost, entry stage, seed policy
├── Stages (1..N)
│   ├── Stage settings: source, complete mode, next stage
│   └── Objectives (1..M per stage)
│       ├── Type: kill, collect, talk, travel, locate, rescue, escort, custom
│       ├── Target binding (exact widevnum, pool, or ref)
│       └── Optional token attachment
└── Rewards (1..R)
    └── Type: points, currency, reputation, token, item, script
```

Stages are linked together with the `next` pointer. The quest begins at the `entry` stage. Each stage completes when its objectives are satisfied (according to its `complete` mode).

---

## Step 1 — Enter the Editor

Create a new quest using a widevnum in your area:

```
qedit 5#1000 create
```

Replace `5` with your area's UID and `1000` with the vnum you want. If you are already standing in your area you can use the shorthand:

```
qedit #1000 create
```

If the quest was created successfully you'll see:

```
QEdit: quest 5#1000 created.
```

Now open the editor for that quest reference:

```
qedit 5#1000
```

{: .info}
**Tip:** Type `show` at any time inside the editor to see the current state of the quest.

---

## Step 2 — Set the Name

```
qedit 5#1000 name Kill the Rats
```

The name is the short, display title of the quest that players will see in quest logs.

---

## Step 3 — Write a Summary

The summary is a single-line teaser shown in the quest journal and NPC dialogue:

```
qedit 5#1000 summary Rid the storage cellar of its rat infestation.
```

---

## Step 4 — Write the Description

The description is a full prose narrative shown when the player reads the quest. It opens the string editor:

```
qedit 5#1000 description
```

Type your description, end with `~` on its own line:

```
The storage cellar beneath the inn has become overrun with rats.
The innkeeper is desperate for help — supplies are being contaminated
and guests are complaining. Clear them out before the situation
gets any worse.
~
```

---

## Step 5 — Set Quest Class

The **class** determines the broad kind of quest:

| Value | Description |
|-------|-------------|
| `narrative` | Story-driven quest; often part of a chain |
| `mission` | Task or job quest; standalone |

```
qedit 5#1000 class mission
```

---

## Step 6 — Set Quest Type

The **type** is a categorisation used for filtering and logic:

| Value | Description |
|-------|-------------|
| `main` | Core story content |
| `side` | Optional world content |
| `unlock` | Unlocks further content on completion |
| `class` | Class-specific quest |
| `event` | Temporary seasonal or server event |
| `other` | Uncategorised |

```
qedit 5#1000 type side
```

---

## Step 7 — Set Quest Category

The **category** is used for the journal log grouping:

```
qedit 5#1000 category regional
```

Valid values: `none`, `regional`, `class`, `story`, `church`, `dungeon`, `crafting`, `event`, `other`

---

## Step 8 — Set Scope

The **scope** determines who participates in the quest:

| Value | Description |
|-------|-------------|
| `character` | Quest is individual; each player gets their own instance |
| `group` | Quest tracks progress across a party group |
| `church` | Quest is shared by an entire church/guild |

```
qedit 5#1000 scope character
```

---

## Step 9 — Set Repeat Policy

```
qedit 5#1000 repeat once
```

Use `repeatable` if this quest can be done multiple times (e.g. a daily task).

---

## Step 10 — Add Stages

Every quest needs at least one stage. Add a stage with:

```
qedit 5#1000 stage add 1
```

Then configure it:

```
qedit 5#1000 stage 1 summary Exterminate the rats
qedit 5#1000 stage 1 description
Kill the rats in the storage cellar beneath the inn.
~
qedit 5#1000 stage 1 complete all
qedit 5#1000 stage 1 source static
```

For a single-stage quest, leave `next` unset (or set it to `none`). To chain stages:

```
qedit 5#1000 stage 1 next 2
qedit 5#1000 stage add 2
qedit 5#1000 stage 2 summary Report back to the innkeeper
```

---

## Step 11 — Set the Entry Stage

Tell the quest where to start:

```
qedit 5#1000 entry 1
```

---

## Step 12 — Add Objectives

Objectives define what the player must do to complete a stage.

### Kill objective

```
qedit 5#1000 objective add 1 1
qedit 5#1000 objective 1 1 type kill
qedit 5#1000 objective 1 1 required 5
qedit 5#1000 objective 1 1 target wnum 5#201
qedit 5#1000 objective 1 1 summary Sewer Rat
```

This objective requires killing 5 of the mob at widevnum `5#201`.

### Collect objective

```
qedit 5#1000 objective add 1 2
qedit 5#1000 objective 1 2 type collect
qedit 5#1000 objective 1 2 required 3
qedit 5#1000 objective 1 2 target wnum 5#301
qedit 5#1000 objective 1 2 quantity 1
qedit 5#1000 objective 1 2 summary Rat Tail
```

This requires turning in 3 of item `5#301`.

### Talk objective

```
qedit 5#1000 objective add 2 1
qedit 5#1000 objective 2 1 type talk
qedit 5#1000 objective 2 1 target wnum 5#101
qedit 5#1000 objective 2 1 summary Innkeeper Aldric
```

Player must speak to the mob at `5#101` to complete stage 2.

### Travel / Locate objectives

Use `travel` for "go to this room" and `locate` for "find this entity anywhere":

```
qedit 5#1000 objective 1 1 type travel
qedit 5#1000 objective 1 1 destination wnum 5#110
```

---

## Step 13 — Add Rewards

Add a quest point reward:

```
qedit 5#1000 reward add points
qedit 5#1000 reward 1 type points
qedit 5#1000 reward 1 amount 10
```

Add a gold reward:

```
qedit 5#1000 reward add currency
qedit 5#1000 reward 2 type currency
qedit 5#1000 reward 2 amount 500
qedit 5#1000 reward 2 currency gold
```

Add a token item reward:

```
qedit 5#1000 reward add token
qedit 5#1000 reward 3 type token
qedit 5#1000 reward 3 target 5#401
qedit 5#1000 reward 3 amount 1
```

Optionally, override the default delivery message for any reward:

```
qedit 5#1000 reward 1 display Aldric thanks you warmly and hands you a pouch of coins.
```

If not set, the system generates a default message based on the reward type and amount.

---

## Step 14 — Enable the Quest

Quests are disabled by default. Enable it when you're ready:

```
qedit 5#1000 enabled on
```

---

## Step 15 — Add a Quest Prog (NPC Hook)

To make an NPC give and receive this quest, attach a quest program to the NPC using `medit`:

```
medit 5#101
addqprog 5#1000 greet_prog
done
```

See [Mobile Programs](../scripting/mobile-programs/) for how to write the script logic.

---

## Complete Example

Here is the complete sequence for the "Kill the Rats" quest:

```
qedit 5#1000 create
qedit 5#1000 name Kill the Rats
qedit 5#1000 summary Rid the storage cellar of its rat infestation.
qedit 5#1000 description
The storage cellar beneath the inn has become overrun with rats.
The innkeeper is desperate for help - supplies are being contaminated
and guests are complaining. Clear them out before the situation
gets any worse.
~
qedit 5#1000 class mission
qedit 5#1000 type side
qedit 5#1000 category regional
qedit 5#1000 scope character
qedit 5#1000 repeat once

qedit 5#1000 stage add 1
qedit 5#1000 stage 1 summary Exterminate the rats
qedit 5#1000 stage 1 complete all
qedit 5#1000 stage 1 source static
qedit 5#1000 stage 1 next 2

qedit 5#1000 objective add 1 1
qedit 5#1000 objective 1 1 type kill
qedit 5#1000 objective 1 1 required 5
qedit 5#1000 objective 1 1 target wnum 5#201
qedit 5#1000 objective 1 1 summary Sewer Rat

qedit 5#1000 stage add 2
qedit 5#1000 stage 2 summary Report back to Aldric
qedit 5#1000 stage 2 complete all
qedit 5#1000 stage 2 source static

qedit 5#1000 objective add 2 1
qedit 5#1000 objective 2 1 type talk
qedit 5#1000 objective 2 1 target wnum 5#101
qedit 5#1000 objective 2 1 summary Innkeeper Aldric

qedit 5#1000 reward add points
qedit 5#1000 reward 1 amount 10

qedit 5#1000 reward add currency
qedit 5#1000 reward 2 currency gold
qedit 5#1000 reward 2 amount 500

qedit 5#1000 entry 1
qedit 5#1000 enabled on
```

---

## Next Steps

- [Quest Command Reference](qedit-command-reference.html) — full syntax for every command
- [Quest Flags & Types](qedit-flags-reference.html) — all valid values
- [Scripting Guide](../scripting/scripting-basics.html) — write quest programs
