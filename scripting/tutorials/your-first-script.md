---
layout: default
title: Your First Script
nav_order: 2
parent: Tutorials
grand_parent: Scripting
---

# Your First Script
{: .no_toc }

Build a town guard who greets players, reacts to their alignment, and responds when
spoken to — all from scratch.
{: .fs-6 .fw-300 }

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## What You'll Build

By the end of this tutorial you will have a **Town Guard** mob with two scripts:

1. A **greet** script that welcomes players when they walk into the room and
   adjusts its message based on alignment (good, evil, or neutral).
2. A **speech** script that responds when a player says "hello."

Along the way you'll learn comments, quick codes, ifchecks, and how to attach
triggers — everything you need to start writing scripts of your own.

### Prerequisites

- You have completed the [Getting Started](getting-started.html) tutorial and
  know how to open the script editor.
- You have builder access to an area where you can create mobs and scripts.
- You know basic OLC commands (`medit`, `mpedit`, `done`).

> Throughout this tutorial we use area **10** with widevnum prefix `10#`.
> Substitute your own area number wherever you see it.
{: .note }

---

## Step 1 — Create the Mob

Before we can script anything, we need a mob to attach scripts to. Open the
mobile editor and create a simple town guard:

```
medit 10#100 create
```

Give the guard a name and description, then type `done` to save.  The exact
appearance doesn't matter for this tutorial — we only need a mob that exists in
a room so we can walk in and see it react.

Place the guard in a room with `reset` or `load` so it's there when you test:

```
load mob 10#100
```

---

## Step 2 — Write the Greet Script

Now create the script that will fire when a player enters the guard's room.

### Open the script editor

```
mpedit 10#200 create
mpedit 10#200
```

### Set the trigger type and phrase

```
trigger greet_prog
phrase 100
```

- **`greet_prog`** — fires once when a character the mob can see enters the
  room.
- **`100`** — the percentage chance the script fires.  `100` means *every
  time*.  If you set it to `50` the guard would only greet players half the
  time.

### Write the script body

Type (or paste) the following into the editor:

```
** Town Guard — greet script
** Fires when a player enters the room.
if ispc $n
  mob say Welcome to town, $n. Stay out of trouble.
endif
```

### Save

```
done
```

That's it — five lines of script.  Let's walk through each one.

---

## Step 3 — Line-by-Line Walkthrough

Here is the complete script again with numbered annotations:

```
** Town Guard — greet script                         ← 1
** Fires when a player enters the room.              ← 2
if ispc $n                                           ← 3
  mob say Welcome to town, $n. Stay out of trouble.  ← 4
endif                                                ← 5
```

### Line 1 — Comment

```
** Town Guard — greet script
```

Lines that start with `**` are **comments**.  The game ignores them completely
at runtime.  Use comments to label what a script does and who it belongs to.
Future-you (and other builders) will thank you.

### Line 2 — Another comment

```
** Fires when a player enters the room.
```

A second comment describing *when* this script fires.  Good practice is to note
the trigger type and any important context at the top of every script.

### Line 3 — Ifcheck

```
if ispc $n
```

This is a **conditional**.  It asks: *"Is the entity that triggered this script
a player character?"*

| Part | Meaning |
|:-----|:--------|
| `if` | Start a conditional block |
| `ispc` | An [ifcheck](../ifchecks-reference.html) that returns true when the target is a player character (not an NPC) |
| `$n` | A [quick code](../quick-codes.html) that refers to the **actor** — the character who triggered the script |

Why check `ispc`?  Without it the guard would also greet *other mobs* that
wander into the room.  We only want to greet real players.

### Line 4 — Command

```
  mob say Welcome to town, $n. Stay out of trouble.
```

This is the action the guard performs when the condition is true.

| Part | Meaning |
|:-----|:--------|
| `mob` | The **entity prefix** — required on every script command for mob scripts |
| `say` | The command — makes the mob speak out loud |
| `$n` | Replaced at runtime with the player's name |

If a player named *Kael* enters the room, the guard says:

```
The Town Guard says 'Welcome to town, Kael. Stay out of trouble.'
```

> Every command in a mob script must start with the `mob` prefix.
> Object scripts use `obj`, room scripts use `room`, and so on.  See
> [Scripting Basics](../scripting-basics.html) for details.
{: .info }

### Line 5 — End of conditional

```
endif
```

Closes the `if` block opened on line 3.  Every `if` must have a matching
`endif`.

---

## Step 4 — Attach the Script and Test

### Attach to the mob

Open the guard in the mobile editor and add the script:

```
medit 10#100
addmprog 10#200 greet_prog 100
done
```

This tells the game: *"When trigger `greet_prog` fires (100% chance), run
script `10#200`."*

### Test it

Walk out of the room the guard is in, then walk back in:

```
south
north
```

### What you should see

```
The Town Guard says 'Welcome to town, Kael. Stay out of trouble.'
```

If you see the message — congratulations, your first script works!

If nothing happens, double-check:
- The guard is loaded in the room (`look` to confirm).
- You typed `done` after adding the script in `medit`.
- The trigger is `greet_prog` and the phrase is `100`.

> `greet_prog` fires **once** when a player enters and the mob can see them.
> If you want the script to fire every single time regardless of visibility,
> use the `grall` trigger instead.
{: .tip }

---

## Step 5 — Quick Codes

Before we extend the script, let's look at the **quick codes** you'll use most
often.  Quick codes are placeholders that the game replaces with real values at
runtime.

### The Four You Need Right Now

| Code | Refers To | Expands To | Example |
|:-----|:----------|:-----------|:--------|
| `$n` | **Actor** (the player who triggered the script) | Actor's keyword name | `Kael` |
| `$i` | **Self** (the mob running the script) | Self's keyword name | `guard` |
| `$e` | **Actor** | Subjective pronoun | `he`, `she`, or `it` |
| `$s` | **Actor** | Possessive pronoun | `his`, `her`, or `its` |

### Quick example

```
mob say Greetings, $n! I am $i, and I see $e travels light today.
```

If a female player named *Lyra* triggers this, the guard says:

```
The Town Guard says 'Greetings, Lyra! I am guard, and I see she travels light today.'
```

### More quick codes

There are many more quick codes for victims, objects, and self-pronouns.  See
the full [Quick Codes Reference](../quick-codes.html) when you're ready to
explore them all.

---

## Step 6 — Conditional Behavior with Alignment

Let's make the guard react differently depending on the player's alignment.  We'll
use three ifchecks:

| Ifcheck | True when… |
|:--------|:-----------|
| `isgood` | The character has good alignment |
| `isevil` | The character has evil alignment |
| `isneutral` | The character has neutral alignment |

### Update the script

Open the script editor:

```
mpedit 10#200
```

Replace the body with:

```
** Town Guard — greet script (alignment-aware)
** Fires when a player enters the room.
if ispc $n
  if isgood $n
    mob say Hail, $n! The light of virtue shines upon you.
    mob emote salutes $n respectfully.
  elseif isevil $n
    mob say I've got my eye on you, $n. Don't start any trouble.
    mob emote rests $s hand on $s sword hilt.
  else
    mob say Welcome to town, $n. Stay out of trouble.
  endif
endif
```

Save with `done`.

### Line-by-line

```
** Town Guard — greet script (alignment-aware)         ← comment
** Fires when a player enters the room.                ← comment
if ispc $n                                             ← only greet players
  if isgood $n                                         ← check good alignment
    mob say Hail, $n! The light of virtue shines upon you.
    mob emote salutes $n respectfully.                  ← visual flair
  elseif isevil $n                                     ← check evil alignment
    mob say I've got my eye on you, $n. Don't start any trouble.
    mob emote rests $s hand on $s sword hilt.           ← $s = "his"/"her"/"its"
  else                                                 ← must be neutral
    mob say Welcome to town, $n. Stay out of trouble.
  endif                                                ← closes inner if
endif                                                  ← closes outer if
```

Key points:

- **Nested ifs** — the alignment check is *inside* the `ispc` check. The
  guard first confirms the arrival is a player, then checks alignment.
- **`elseif`** — chains conditions without extra nesting. Only the first
  matching branch executes.
- **`$s`** on the `emote` line expands to the actor's possessive pronoun:
  *"rests his hand"* or *"rests her hand"* depending on the player's sex.

### Test it

If your test character is good-aligned, you should see:

```
The Town Guard says 'Hail, Kael! The light of virtue shines upon you.'
The Town Guard salutes Kael respectfully.
```

If evil-aligned:

```
The Town Guard says 'I've got my eye on you, Kael. Don't start any trouble.'
The Town Guard rests his hand on his sword hilt.
```

If neutral:

```
The Town Guard says 'Welcome to town, Kael. Stay out of trouble.'
```

> You can change your test character's alignment with the immortal `set`
> command to try each branch: `set char Kael alignment 1000` (good),
> `set char Kael alignment -1000` (evil), `set char Kael alignment 0`
> (neutral).
{: .tip }

For the complete list of available ifchecks, see the
[Ifchecks Reference](../ifchecks-reference.html).

---

## Step 7 — Add a Speech Trigger

Let's give the guard a second script so it responds when a player says "hello."

### Create the script

```
mpedit 10#201 create
mpedit 10#201
```

### Set the trigger

```
trigger speech_prog
phrase hello
```

- **`speech_prog`** — fires when a character in the room says something that
  matches the phrase.
- **`hello`** — the keyword to match. If a player types `say hello` or
  `say hello there`, this trigger fires.

### Write the script body

```
** Town Guard — speech script
** Responds when a player says "hello."
if ispc $n
  mob say Hello, $n. Staying safe, I hope?
  mob emote nods at $n.
endif
```

### Save

```
done
```

### Attach to the mob

```
medit 10#100
addmprog 10#201 speech_prog hello
done
```

### Test it

Stand in the same room as the guard and type:

```
say hello
```

### What you should see

```
You say 'hello'
The Town Guard says 'Hello, Kael. Staying safe, I hope?'
The Town Guard nods at Kael.
```

Try variations:

```
say hello there
say why hello good sir
```

Both should trigger the script because the word "hello" appears in what you said.

> Speech triggers match on **individual keywords**, not exact phrases.  If the
> phrase is `hello`, the trigger fires whenever the player's spoken text
> contains the word "hello" anywhere in it.
{: .info }

---

## The Complete Scripts

Here are both finished scripts together for reference.

### Script `10#200` — Greet (alignment-aware)

**Trigger:** `greet_prog 100`

```
** Town Guard — greet script (alignment-aware)
** Fires when a player enters the room.
if ispc $n
  if isgood $n
    mob say Hail, $n! The light of virtue shines upon you.
    mob emote salutes $n respectfully.
  elseif isevil $n
    mob say I've got my eye on you, $n. Don't start any trouble.
    mob emote rests $s hand on $s sword hilt.
  else
    mob say Welcome to town, $n. Stay out of trouble.
  endif
endif
```

### Script `10#201` — Speech (responds to "hello")

**Trigger:** `speech_prog hello`

```
** Town Guard — speech script
** Responds when a player says "hello."
if ispc $n
  mob say Hello, $n. Staying safe, I hope?
  mob emote nods at $n.
endif
```

### Mob `10#100` — Script attachments

```
addmprog 10#200 greet_prog 100
addmprog 10#201 speech_prog hello
```

---

## Quick Reference

A cheat sheet of everything introduced in this tutorial.

### Comments

```
** This line is ignored by the game.
```

### Entity prefix

Every command in a mob script starts with `mob`:

```
mob say ...
mob emote ...
mob echo ...
```

### Conditionals

```
if <ifcheck> <target>
  ** true branch
elseif <ifcheck> <target>
  ** alternate branch
else
  ** fallback branch
endif
```

### Quick codes used

| Code | Meaning |
|:-----|:--------|
| `$n` | Actor's keyword name (the player) |
| `$i` | Self's keyword name (the mob) |
| `$e` | Actor's subjective pronoun (he/she/it) |
| `$s` | Actor's possessive pronoun (his/her/its) |

### Ifchecks used

| Ifcheck | What it tests |
|:--------|:--------------|
| `ispc $n` | Is the actor a player character? |
| `isgood $n` | Does the actor have good alignment? |
| `isevil $n` | Does the actor have evil alignment? |
| `isneutral $n` | Does the actor have neutral alignment? |

### Trigger types used

| Trigger | Phrase | Fires when… |
|:--------|:-------|:------------|
| `greet_prog` | `100` (percent) | A player the mob can see enters the room |
| `speech_prog` | `hello` (keyword) | A player says something containing "hello" |

---

## What's Next?

You've written real scripts, used conditionals, and attached two different
trigger types.  Here are some ideas to practice:

- **Change the percentage** — set the greet phrase to `50` so the guard only
  reacts half the time.
- **Add more speech keywords** — create a script on `speech_prog` with phrase
  `help` that gives directions.
- **Try `echoat` and `echoaround`** — send different messages to the actor and
  everyone else in the room (see [Scripting Basics](../scripting-basics.html)).
- **Explore more ifchecks** — check the player's level, race, or whether they
  carry a specific item.  The full list is in the
  [Ifchecks Reference](../ifchecks-reference.html).

When you're ready, continue to the next tutorial to learn about variables,
loops, and more advanced trigger types.
