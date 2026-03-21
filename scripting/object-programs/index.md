---
layout: default
title: Object Programs
nav_order: 2
parent: Scripting
has_children: true
---

# Object Programs
{: .no_toc }

Object programs (oprogs) bring items to life. They let weapons react during combat, potions produce custom effects when consumed, chests spring traps, furniture trigger events when sat on, and puzzle items respond to speech or actions. Any object in the game can have one or more programs attached to it.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## When to Use Object Programs

Use an object program whenever you need an item to **do something** beyond its basic stats. Common use cases:

| Use Case | Typical Triggers | Example |
|:---------|:-----------------|:--------|
| Equipment effects | `wear`, `remove`, `fight` | A ring that grants fire resistance when worn |
| Consumables | `use`, `eat`, `drink`, `recite` | A potion that teleports the drinker |
| Interactive objects | `push`, `pull`, `turn`, `touch` | A lever that opens a secret door |
| Puzzle items | `speech`, `sayto`, `verb` | A crystal that responds to a password |
| Trapped containers | `open`, `get`, `preget` | A chest that explodes when opened |
| Furniture | `sit`, `rest`, `sleep`, `stand` | A throne that grants a buff while seated |
| Growing/evolving items | `fight`, `afterkill`, `tick` | A weapon that gains power with each kill |
| Quest items | `quest_complete`, `quest_cancel` | A token that transforms on quest completion |

If the behavior you want is purely stat-based (a sword that does fire damage, armor with a spell affect), you may not need a program at all — the object's built-in values handle that. Programs are for **custom logic** that the engine does not already provide.

---

## How Object Programs Work

An object program has two parts:

1. **Trigger** — tells the game *when* to run the program (for example, "when someone wears this item" or "25% of the time each combat round").
2. **Script body** — the list of commands that run when the trigger fires.

Every command in the script body starts with the `obj` prefix:

```
obj echo The sword hums with power.
obj echoat $n You feel the weapon pulse in your hand.
obj damage $n 50
```

For a refresher on prefixes, comments, variables, and control flow, see [Scripting Basics](../scripting-basics.md).

---

## Creating an Object Program with opedit

You attach programs to objects through the object editor (`oedit`). Here is a complete walkthrough.

### Step 1 — Open the Object

Enter the object editor for the item you want to script:

```
oedit <vnum>
```

For example, `oedit 3001` opens object 3001 for editing.

### Step 2 — Open the Program Editor

From inside `oedit`, type:

```
opedit
```

This opens the object's program list. If the object has no programs yet, the list is empty.

### Step 3 — Create a New Program

```
create
```

This creates a new, blank program and assigns it the next available slot number.

### Step 4 — Set the Trigger

```
trigger <trigger_name>
```

For example:

```
trigger wear
```

This tells the game to run this program when the object is worn. See [Triggers](oprog-triggers.md) for the complete trigger list.

### Step 5 — Set the Phrase

```
phrase <value>
```

The phrase meaning depends on the trigger type:

- **Percent triggers** (most triggers): `phrase 100` means "fire every time," `phrase 25` means "fire 25% of the time."
- **Keyword triggers** (`speech`, `act`, `sayto`, `verb`): `phrase open sesame` means "fire when someone says 'open sesame'."
- **Direction triggers** (`exit`, `exall`): `phrase 0` means north, `1` east, `2` south, `3` west, `4` up, `5` down.

### Step 6 — Write the Script

```
code
```

This opens the line editor. Type your script, one command per line:

```
obj echoat $n {CThe amulet glows brightly as you put it on.{x
obj echoaround $n $N's amulet begins to glow with a soft blue light.
obj addaffectname $n amulet_protection 200
```

Type `.` on a blank line to finish editing.

### Step 7 — Save and Exit

```
done
```

Returns to `oedit`. Then type `done` again to save the object. The program is now live — any instance of that object will run the program when the trigger fires.

---

## Quick Reference

| Topic | Link |
|:------|:-----|
| All object triggers | [Triggers](oprog-triggers.md) |
| All object commands | [Commands](oprog-commands.md) |
| Worked examples | [Examples](oprog-examples.md) |
| Commands shared across all entity types | [Shared Commands Reference](../shared-commands.md) |
| Variables, quick codes, and color codes | [Variables and Tokens](../variables-and-tokens.md) |
| Control flow (`if`, `or`, `endif`, `switch`) | [Scripting Basics](../scripting-basics.md) |

---

## Tips

- **Comments** start with `**` (two asterisks). Use them generously — future you will thank present you.
- **`lastreturn`** is set by many commands to indicate success (`1`) or failure (`0`). Check it with `if lastreturn == 1` right after the command.
- **`selfdestruct`** causes the object to destroy itself. Useful for one-use consumables.
- **`pre*` triggers** (like `prewear`, `preget`, `preput`) fire *before* the action and can **prevent** it. Use them for restrictions ("you are not worthy to wield this blade").
- **Variables** persist across trigger firings. Use `obj varset` and `obj varclear` to track state — for example, counting how many times a weapon has been used in combat.