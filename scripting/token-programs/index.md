---
layout: default
title: Token Programs
nav_order: 4
parent: Scripting
has_children: true
---

# Token Programs
{: .no_toc }

Tokens are **the most versatile scripting entity** in Sentience. With 206 triggers — more than any other entity type — token programs can react to nearly every event in the game. Tokens are invisible, attachable script containers that carry their own variables, values, and timers.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## What Are Tokens?

A token is an invisible game entity that attaches to a **character**, **object**, or **room**. Unlike objects, tokens have no physical presence — players cannot see, pick up, or interact with them directly. They exist purely as carriers of scripts, data, and effects.

Think of tokens as programmable sticky notes. You attach one to a character and it quietly runs scripts in response to what that character does — taking damage, casting spells, entering rooms, logging in, or any of 206 different events.

### How Tokens Differ from Objects

| Feature | Objects | Tokens |
|:--------|:--------|:-------|
| Visible to players | Yes | No |
| Can be picked up / dropped | Yes | No |
| Attachable to characters | Carried / equipped | Directly attached |
| Attachable to objects | Inside containers | Directly attached |
| Attachable to rooms | On the floor | Directly attached |
| Has inventory / contents | Yes | No |
| Has script variables | Yes | Yes |
| Has value slots | Type-dependent | 8 general-purpose slots |
| Has a countdown timer | No | Yes (built-in) |
| Number of triggers | 100 | **206** |
| Survives logout (optional) | Depends on save rules | Configurable via flags |

### When to Use Tokens

Tokens excel at these common patterns:

- **Buffs and debuffs** — Attach a token with a timer. It adds affects, fires `expire` when time runs out, and cleans up after itself.
- **Quest tracking** — Use token values and variables to track quest progress. The `quest_complete`, `quest_part`, and related triggers let the token react to quest events.
- **Temporary abilities** — Grant a character a skill or spell via a token. The `combatstyle`, `spell`, and `spellbeat` triggers let the token define how the ability works.
- **Cooldowns** — Set a timer on the token. Check for the token's existence before allowing an action. When the timer expires, remove the token.
- **Persistent effects** — Use the persist flag and `save` trigger to make effects survive logout, reboot, or even death (configurable via token flags).
- **Combat modifiers** — React to `fight`, `hit`, `damage`, `attack_*`, and `defense` triggers to modify combat behavior.
- **Login / logout hooks** — The `login` and `quit` triggers fire when the token's owner connects or disconnects.

---

## Token Types

Each token has a type that determines its role:

| Type | Value | Purpose |
|:-----|:------|:--------|
| `GENERAL` | 0 | General-purpose token (default) |
| `QUEST` | 1 | Quest tracking token |
| `AFFECT` | 2 | Creates character affects (buffs/debuffs) |
| `SKILL` | 3 | Defines a custom skill |
| `SPELL` | 4 | Defines a custom spell |
| `SONG` | 5 | Defines a custom bard song |

---

## Token Flags

Flags control token lifecycle behavior:

| Flag | Behavior |
|:-----|:---------|
| `PURGE_DEATH` | Remove token when the owner dies |
| `PURGE_IDLE` | Remove token on idle timeout |
| `PURGE_QUIT` | Remove token when the owner quits |
| `PURGE_REBOOT` | Remove token on server reboot |
| `PURGE_RIFT` | Remove token on rift travel |
| `REVERSETIMER` | Timer counts up instead of down |
| `CASTING` | Protected during spell casting |
| `NOSKILLTEST` | Skip skill test on action completion |
| `SINGULAR` | Only one copy of this token allowed per owner |
| `SEE_ALL` | Token ignores line-of-sight rules |
| `SPELLBEATS` | Fires `spellbeat` triggers during casting |
| `PERMANENT` | Cannot be removed unless the source entity is extracted |

---

## Token Data Slots

Every token has **8 integer value slots** (`value[0]` through `value[7]`) plus a built-in **timer**. These provide lightweight data storage without needing variables.

For `SPELL`-type tokens, the value slots have predefined meanings:

| Slot | Purpose |
|:-----|:--------|
| `value[0]` | Spell rating |
| `value[1]` | Casting difficulty |
| `value[2]` | Target type |
| `value[3]` | Required position |
| `value[4]` | Mana cost |
| `value[5]` | Learn rate |
| `value[6]`–`value[7]` | Available for custom use |

For all other token types, all 8 value slots are available for any purpose.

---

## Creating and Editing Tokens with tpedit

Use `tpedit` (the token editor) to create and modify tokens and their scripts.

### Creating a New Token

```
tpedit create <vnum>
```

This opens the token editor for a new token at the specified vnum. Set the token's properties:

```
name <keyword list>
desc <short description>
type <general|quest|affect|skill|spell|song>
flags <flag list>
timer <ticks>
value <slot#> <value>
```

### Adding a Script

From within `tpedit`, add a script program:

```
addprog <trigger> <phrase>
```

This opens the line editor. Write your token script using `token` as the command prefix:

```
token echo {YA warm glow surrounds $n.{x
token echoaround $n {Y$N is surrounded by a warm glow.{x
```

### Example: Creating a Simple Buff Token

```
tpedit create 5001
name strength_buff power_surge
desc a surge of strength
type affect
flags purge_death purge_quit
timer 60
addprog expire 100
```

In the script editor:
```
token echoat $n {xThe surge of strength fades from your body.{x
token echoaround $n {x$N looks a little less imposing.{x
token stripaffect $n 'giant strength'
token junk self
```

Type `@` to finish editing. The token will fire the `expire` script when its 60-tick timer reaches zero, remove the affect, and clean itself up.

---

## TokenOther Commands

Token programs have access to **3 special commands** that operate on the token's host entity or on other tokens. These are unique to the token scripting context and give tokens the ability to manipulate themselves and other tokens directly.

### adjust

```
token adjust <target> <token_vnum> <slot|timer|tempstore#> <operator> <value>
token adjust <token_ref> <slot|timer|tempstore#> <operator> <value>
```

Adjusts a numeric property on a token. The target can be a character, object, or room that carries the token, or a direct token reference. The slot is a value index (0–7), `timer`, or `tempstore1`–`tempstore4`.

**Operators:** `+` (add), `-` (subtract), `*` (multiply), `/` (divide), `%` (modulo), `=` (set), `&` (bitwise AND), `|` (bitwise OR), `!` (clear bits), `^` (XOR).

**Examples:**
```
** Increment value[0] on this token by 1
token adjust $i 0 + 1

** Set value[2] on token vnum 5001 carried by $n to 100
token adjust $n 5001 2 = 100

** Subtract 10 from the token's timer
token adjust $i timer - 10
```

### give

```
token give <target> <token_vnum> [variable_name]
```

Creates a new instance of the specified token and attaches it to the target (character, object, or room). If a variable name is provided, stores a reference to the newly created token in that variable.

Respects the `SINGULAR` flag — if the token is singular and the target already has one, the command fails silently.

Fires the `token_given` trigger on the new token after attachment.

**Examples:**
```
** Give token 5001 to the triggering player
token give $n 5001

** Give token 5002 to the current room, store reference
token give $here 5002 new_token
```

### junk

```
token junk <target> <token_vnum>
token junk <token_ref>
token junk self
```

Silently destroys a token. Can target a specific token on a character, object, or room by vnum, or destroy a token by direct reference. Using `self` causes the token running the script to destroy itself — this should be the **last command** in the script since execution stops.

Fires the `token_removed` trigger before extraction.

**Examples:**
```
** Remove token 5001 from $n
token junk $n 5001

** Self-destruct
token junk self
```

---

## Entity Prefix

All token script commands use the `token` prefix:

```
token echo A mysterious force stirs.
token echoat $n You feel something change within you.
token damage $n 50 fire
token varset quest_step integer 3
```

---

## Script Comments

Comments in token scripts (and all Sentience scripts) use the `**` prefix:

```
** This is a comment — ignored by the script engine
token echo This line runs.
** token echo This line does NOT run.
```

---

## Quick Reference

| Resource | Content |
|:---------|:--------|
| [Triggers](tprog-triggers.html) | All 206 token triggers with descriptions and phrase formats |
| [Commands](tprog-commands.html) | Complete command reference (162 commands + 3 TokenOther) |
| [Examples](tprog-examples.html) | Worked examples: buffs, quests, cooldowns, combat, curses |
| [Shared Commands](../shared-commands.html) | Detailed documentation for commands shared across entity types |
| [Scripting Basics](../scripting-basics.html) | Fundamentals: prefixes, variables, if-checks, flow control |
| [Variables and Tokens](../variables-and-tokens.html) | Variable system, quick codes, and data types |