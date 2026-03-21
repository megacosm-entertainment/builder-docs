---
layout: default
title: Working with Variables
nav_order: 3
parent: Tutorials
grand_parent: Scripting
---

# Working with Variables
{: .no_toc }

So far you have written scripts that react to what a player does — greeting them when they enter a room, responding when they say something. But those scripts have no memory. Every time they fire, they start fresh with no idea what happened last time. Variables change that. They let your scripts remember things between trigger fires, so an NPC can count how many times a player has visited, a quest can track its stage, and a room can remember that a lever has been pulled.

This tutorial builds on what you learned in [Scripting Basics](../scripting-basics.md). If you have not written a script and attached a trigger yet, start there first.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## What Are Variables?

A variable is a named piece of data stored on an entity — a mob, a player, an object, a room, or a token. You give the variable a name and a value, and the game remembers that value for as long as the entity exists (or longer, if you mark it for saving).

Here is the key idea: **variables persist between trigger fires**. A normal script line runs once and is gone. A variable stays on the entity, ready for the next time a script checks it.

Think of it like an NPC keeping a notebook. Each variable is one line in that notebook — a name and a value. Other scripts can read the notebook, add lines, or change what is written.

---

## Variable Types

Every variable has a **type** that tells the game what kind of value it holds. The three types you will use most are:

### Integer (Number)

Integers store whole numbers — positive, negative, or zero. Use them for counters, quest stages, scores, and amounts.

```
mob varset kill_count integer 0
mob varset quest_stage integer 1
mob varset gold_reward integer 500
```

### String (Text)

Strings store text. Everything after the keyword `string` to the end of the line becomes the value.

```
mob varset greeting string Hello, traveller!
mob varset quest_note string Find the lost amulet in the caves.
```

### Boolean (True/False)

Booleans store a simple yes-or-no. The value is either `true` (1) or `false` (0).

```
mob varset is_hostile bool false
mob varset door_unlocked bool true
```

{: .note }
> There are also advanced variable types for storing references to rooms, mobiles, objects, and more. See [Variables & Tokens](../variables-and-tokens.md#entity-reference-types) when you are ready for those.

---

## Setting Variables

The `varset` command creates a variable on the entity that owns the script (the "self"). The syntax is:

```
mob varset <name> <type> <value>
```

- **name** — the variable name. Use lowercase with underscores: `visit_count`, `quest_stage`, `greeting`.
- **type** — one of `integer`, `string`, or `bool` (plus the advanced types).
- **value** — the starting value.

Let's set a few variables on an NPC:

```
** Set up initial state for this merchant
mob varset shop_open bool true
mob varset customers_today integer 0
mob varset motto string The finest goods in all the land!
```

Lines starting with `**` are comments — the game ignores them. They are just notes for you.

{: .note }
> `varset` creates the variable if it does not exist, or overwrites it if it does. You do not need to check first.

---

## Reading Variables

Once a variable is set, you read its value with the `$<name>` syntax. Put the variable name between `$<` and `>` and the game replaces it with the current value.

### In Echo and Say Commands

```
mob varset customers_today integer 3
mob say I have served $<customers_today> customers today!
```

The player sees:

> The merchant says 'I have served 3 customers today!'

You can use `$<name>` anywhere you would use normal text:

```
mob varset player_title string the Brave
mob say Welcome back, $n $<player_title>!
```

### In Arithmetic

Use `$<name>` inside arithmetic expressions (`$[...]`) to do maths:

```
mob varset count integer 5
mob varset count integer $[$<count> + 1]
mob echo Count is now $<count>.
```

The player sees:

> Count is now 6.

The expression `$[$<count> + 1]` reads the current value of `count` (5), adds 1, and the result (6) becomes the new value.

---

## Using Variables in Conditions

Variables become truly powerful when you use them in `if` checks. You can test a variable's value to make your script behave differently depending on state.

### Numeric Comparisons

```
if $<quest_stage> == 1
  mob say Have you found the amulet yet?
endif

if $<kill_count> >= 10
  mob say Impressive! You have slain enough beasts.
endif

if $<gold_owed> > 0
  mob say You still owe me $<gold_owed> gold.
endif
```

All the standard comparison operators work: `==`, `!=`, `>`, `<`, `>=`, `<=`.

### Boolean Checks

For booleans, you can test them directly — no comparison needed:

```
if $<is_hostile>
  mob say You dare show your face here?!
endif

if !$<door_unlocked>
  mob say The door is locked. Find the key.
endif
```

The `!` means "not" — it flips the check.

### String Comparisons

```
if $<faction> == shadow
  mob say A shadow agent... I have a job for you.
endif
```

### Combining Conditions

Use `and` and `or` to combine multiple checks:

```
if $<quest_stage> == 2 and $<kill_count> >= 5
  mob say Well done! The quest is complete.
  mob varseton $n quest_stage integer 3
endif
```

### A Complete Example

Here is a script that tracks a simple two-stage quest. Attach it to an NPC with a `speech` trigger matching `quest`:

```
** Quest giver — speech trigger on "quest"
if $<quest_stage> == 0 or $<quest_stage> == null
  ** Player has not started the quest yet
  mob say I need someone to clear the wolves from the north road.
  mob say Slay 5 of them and report back.
  mob varseton $n quest_stage integer 1
  mob varseton $n wolf_kills integer 0
elseif $<quest_stage> == 1
  ** Quest is in progress
  if $<wolf_kills> >= 5
    mob say Excellent work! The road is safe again.
    mob varseton $n quest_stage integer 2
    mob award $n qp 5
  else
    mob say You have slain $<wolf_kills> of 5 wolves. Keep at it!
  endif
elseif $<quest_stage> == 2
  mob say Thank you again for clearing those wolves.
endif
```

{: .note }
> When a variable has never been set, reading it returns `null`. The check `$<quest_stage> == null` catches the first-ever visit.

---

## Variables on Other Entities

So far, every `varset` has stored a variable on the NPC that owns the script (on "self"). But you often need to store variables on the **player** instead — so the data follows them wherever they go.

### Setting Variables on a Target

Use `varseton` to set a variable on another entity:

```
mob varseton <target> <name> <type> <value>
```

The target is usually a quick code like `$n` (the player who triggered the script):

```
** Store quest progress on the PLAYER, not on this NPC
mob varseton $n quest_stage integer 1
mob varseton $n quest_name string The Wolf Hunt
```

### Why This Matters

Think about it: if you use `mob varset quest_stage integer 1`, the variable lives on the NPC. Every player who talks to that NPC would see the same `quest_stage` — the NPC only has one copy. That is rarely what you want.

By using `mob varseton $n quest_stage integer 1`, each player gets their own `quest_stage`. Player A can be on stage 1 while Player B is on stage 3. The variable travels with the player.

### Clearing Variables on a Target

To remove a variable from another entity:

```
mob varclearon $n quest_stage
```

This removes the variable entirely — it goes back to `null`.

---

## Persistence: Making Variables Survive

By default, variables are **temporary**. They disappear when:
- A player logs out and back in
- The server reboots
- An NPC dies and respawns

For things that need to last — quest progress, player preferences, long-term counters — you must explicitly mark them for saving with `varsave`.

### Saving Variables

```
mob varseton $n quest_stage integer 1
mob varsaveon $n quest_stage on
```

Now `quest_stage` is saved with the player's character file. It will survive logouts, reboots, and anything short of a full character wipe.

For variables on self (the NPC), use `varsave`:

```
mob varset total_sales integer 0
mob varsave total_sales on
```

{: .warning }
> `varsave` only works on player characters and rooms. NPC variables are lost when the NPC dies or the area resets, even with `varsave on`, because the NPC instance itself is destroyed. If you need data to survive NPC death, store it on the **player** or on a **token** the player carries.

### What Happens Without `varsave`?

The variable works perfectly — until the player logs out or the server reboots. Then it is gone. The script behaves as if the variable was never set.

This is fine for temporary things: combat flags, conversation state during an active dialogue, counters that should reset. But for quest progress, achievements, or preferences — always save.

### Quick Checklist

| What You Want | Command |
|:--------------|:--------|
| Set on self | `mob varset name type value` |
| Set on player | `mob varseton $n name type value` |
| Save on self | `mob varsave name on` |
| Save on player | `mob varsaveon $n name on` |
| Remove from self | `mob varclear name` |
| Remove from player | `mob varclearon $n name` |

---

## Introduction to Tokens

Variables are great for storing simple values, but sometimes you need more than a single number or string. You need a timer that counts down automatically. Or you need an invisible marker on a player that carries its own variables and scripts. That is what **tokens** are.

### What Is a Token?

A token is an invisible object that you attach to a character, object, or room. Players cannot see tokens in their inventory — tokens exist only for scripts to work with. Think of them as invisible tags that the game can detect and respond to.

Every token has:

- **A vnum** — a unique template number that identifies what kind of token it is
- **A timer** — an optional countdown (in ticks, roughly 4 seconds each) that automatically removes the token when it reaches zero
- **Value slots (val0–val7)** — eight integer slots for storing data
- **Variables** — tokens can hold their own variables, just like mobs or players
- **Scripts** — tokens can have their own programs with their own triggers

You create token templates in the token editor (`tpedit`), then give instances to players at runtime.

### Giving a Token

```
mob givetoken <target> <vnum>
```

For example, to give token 12345 to the player who triggered the script:

```
mob givetoken $n 12345
mob say You have been marked!
```

### Checking for a Token

The `hastoken` ifcheck tests whether a character carries a specific token:

```
if hastoken $n 12345
  mob say I see you carry the mark.
endif

if !hastoken $n 12345
  mob say You do not have the mark yet.
endif
```

### Removing a Token

```
mob taketoken $n 12345
```

### Token Timers

One of the most useful features of tokens is the built-in timer. Set the timer on the token template, and when you give that token to a player, the countdown begins. When the timer reaches zero, the token is automatically removed and fires a `timer` trigger (if the token has a script for one).

This is perfect for:
- **Buffs** that last a set duration
- **Cooldowns** that prevent an action for a time
- **Timed quests** with a deadline

### Token Values

Tokens have eight integer value slots (`val0` through `val7`) set in the token template. Scripts can read these:

```
** Inside a token program (tp script)
if $(self.val0) >= 10
  tp echoat $(self.owner) The power surges through you!
endif
```

### Tokens with Variables

The real power of tokens comes from combining them with variables. A token can carry its own variables, giving each player their own copy of the data automatically — because each player gets their own token instance.

```
** Give the quest token
mob givetoken $n 50001

** Set a variable ON the token the player just received
mob varseton $n.token.50001 kills integer 0
```

### When to Use a Token vs a Variable

| You Need | Use |
|:---------|:----|
| A simple counter or flag | Variable on the player |
| A timer that auto-expires | Token |
| A cooldown or buff | Token (timer handles removal) |
| A package of related data with its own scripts | Token with variables |
| A quick boolean check | Variable (simpler) |

For a complete reference, see [Variables & Tokens](../variables-and-tokens.md).

---

## Practical Walkthrough: Visit Counter NPC

Let's put everything together and build something real. We will create an NPC that:

1. Tracks how many times each player has visited
2. Greets them differently based on the count
3. Gives a reward when they reach 10 visits
4. Remembers the count between sessions (persistence)

We will use a **token** on the player to carry the visit count. This way every player gets their own counter, and the token can survive reboots.

### Step 1: Create the Token Template

Open the token editor and create a new token. For this example, assume it gets vnum **80001**.

```
tpedit create 80001
```

Set it up:
- **Name:** visit_tracker
- **Type:** General
- **Timer:** 0 (no timer — we want this to last forever)
- **Flags:** none (no `purge_death`, no `purge_quit` — we want it to survive everything)

This token will carry one variable: `visits`, which tracks the count.

### Step 2: Write the Script

Create a mob program on the NPC. We will use a `greet` trigger so the script fires whenever a player enters the room.

Open the mob program editor:

```
mpedit create 1
```

Name it and write the code:

```
name visit counter
code
```

Here is the complete script:

```
** Visit counter — greet trigger
** Fires when a player walks into the room

** Ignore NPCs
if isnpc $n
  break
endif

** Check if the player already has our tracker token
if hastoken $n 80001
  ** They have visited before — increment the count
  mob varseton $n.token.80001 visits integer $[$<visits> + 1]

  ** Check the milestone
  if $<visits> == 10
    ** 10th visit! Give a reward
    mob say $n! This is your 10th visit — I have a gift for you!
    mob echo {YThe shopkeeper rummages under the counter and pulls out a gleaming pendant.{x
    mob oload 80050
    mob give pendant $n
    mob say Come back any time, friend!
  elseif $<visits> > 10
    ** Regular greeting for frequent visitors
    mob say Welcome back, $n! Always good to see a loyal customer.
    mob say That makes $<visits> visits now!
  else
    ** Visits 2 through 9
    mob say Ah, $n! Good to see you again.
    mob say You have visited me $<visits> times now.

    if $<visits> == 5
      mob say Come back 5 more times and I will have something special for you!
    endif
  endif
else
  ** First visit — give the tracker token and set the count to 1
  mob givetoken $n 80001
  mob varseton $n.token.80001 visits integer 1
  mob say Welcome, $n! I do not think we have met before.
  mob say I am the shopkeeper. Come visit me often — I reward loyal customers!
endif
```

Compile it:

```
compile
```

### Step 3: Attach the Trigger

Now attach this script to the NPC with a `greet` trigger. In the mob editor for your shopkeeper NPC:

```
addmprog greet 100 1
```

This means: on the `greet` trigger, with a 100% chance, run mob program #1.

### Step 4: Test It

Walk into the room with the shopkeeper. You should see the first-visit greeting. Leave and come back — you should see "visited 2 times." Keep going until you hit 10 to see the reward.

### How It Works — Step by Step

Here is what happens each time a player enters the room:

1. The `greet` trigger fires the script.
2. The script checks `if isnpc $n` — if it is an NPC walking in (not a player), the script stops.
3. It checks `if hastoken $n 80001` — does the player carry our tracker token?
4. **First visit:** No token → give token 80001, set `visits` to 1, greet as new.
5. **Return visits:** Token exists → read `visits`, add 1, save the new value. Branch based on the count.
6. **10th visit:** Hit the milestone → give reward object 80050.

### Making It Persistent

There is one problem: if we want `visits` to survive a server reboot, we need to mark it for persistence. Add this after setting the variable:

```
mob varsaveon $n.token.80001 visits on
```

Update both places where we set `visits`. Here are the two lines to add:

After the first visit block:
```
mob givetoken $n 80001
mob varseton $n.token.80001 visits integer 1
mob varsaveon $n.token.80001 visits on
```

After the increment:
```
mob varseton $n.token.80001 visits integer $[$<visits> + 1]
mob varsaveon $n.token.80001 visits on
```

{: .note }
> You only need to call `varsaveon` once per variable name — after that the persistence flag stays set. But calling it each time is harmless and makes the script easier to understand.

### The Complete Final Script

Here is the full script with persistence and comments:

```
** Visit counter NPC — greet trigger
** Tracks player visits using a token, rewards at 10 visits.

** Ignore NPCs — only track real players
if isnpc $n
  break
endif

if hastoken $n 80001
  ** Return visit — increment the counter
  mob varseton $n.token.80001 visits integer $[$<visits> + 1]
  mob varsaveon $n.token.80001 visits on

  if $<visits> == 10
    ** Milestone reward
    mob say $n! This is your 10th visit — I have a gift for you!
    mob echo {YThe shopkeeper rummages under the counter and pulls out a gleaming pendant.{x
    mob oload 80050
    mob give pendant $n
    mob say Come back any time, friend!
  elseif $<visits> > 10
    mob say Welcome back, $n! Always good to see a loyal customer.
    mob say That makes $<visits> visits now!
  else
    ** Visits 2–9
    mob say Ah, $n! Good to see you again.
    mob say You have visited me $<visits> times now.
    if $<visits> == 5
      mob say Come back 5 more times and I will have something special for you!
    endif
  endif
else
  ** First visit ever
  mob givetoken $n 80001
  mob varseton $n.token.80001 visits integer 1
  mob varsaveon $n.token.80001 visits on
  mob say Welcome, $n! I do not think we have met before.
  mob say I am the shopkeeper. Come visit me often — I reward loyal customers!
endif
```

---

## What You Learned

In this tutorial you covered:

- **Variables** store named values on entities — they persist between trigger fires
- **Three basic types:** `integer` for numbers, `string` for text, `bool` for true/false
- **`varset`** creates or overwrites a variable on the script's own entity
- **`$<name>`** reads a variable's value in echo commands, conditions, and arithmetic
- **`if $<name> > 5`** uses variables in conditions with standard comparison operators
- **`varseton`** sets a variable on another entity (usually the player with `$n`)
- **`varsave` / `varsaveon`** marks a variable for persistence across logouts and reboots — without it, the variable is lost
- **Tokens** are invisible objects with timers, value slots, variables, and scripts
- **`givetoken`**, **`hastoken`**, and **`taketoken`** manage tokens on characters
- How to combine tokens and variables to build a practical visit counter

---

## Next Steps

- Read the full [Variables & Tokens](../variables-and-tokens.md) reference for entity-reference types, index variables, and all the advanced features
- Review [Scripting Basics](../scripting-basics.md) if you need a refresher on control flow, quick codes, or the editor workflow
- Explore the [Practical Patterns](../variables-and-tokens.md#practical-patterns) section for ready-made recipes: quest tracking, buffs, cooldowns, kill counters, and state machines
- Try modifying the visit counter — add a second milestone at 25 visits, or have the NPC remember the player's name with a string variable
