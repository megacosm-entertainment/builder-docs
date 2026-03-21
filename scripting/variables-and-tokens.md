---
layout: default
title: Variables & Tokens
nav_order: 7
parent: Scripting
---

# Variables & Tokens
{: .no_toc }

Variables and tokens are the two core state-management systems in Sentience scripting. **Variables** store named values on entities — numbers, strings, booleans, and references to game objects. **Tokens** are invisible objects that attach to characters, objects, or rooms, carrying their own variables, value slots, timers, and even scripts.

Together they let you build quests, buffs, cooldowns, state machines, and anything else that needs to remember something between script runs.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Variable Types

Every variable has a **type** that determines what kind of value it holds. When you run a `varset` command you choose the type explicitly.

### Basic Types

These are the types you will use most often.

| Type Keyword | What It Stores | Example |
|:-------------|:---------------|:--------|
| `bool` | True or false | `varset quest_started bool true` |
| `integer` | A whole number (positive or negative) | `varset kill_count integer 0` |
| `string` | A line of text | `varset greeting string Hello there!` |

{: .note }
> `integer` is often shortened to `number` in casual conversation, but the command keyword is `integer`.

#### Boolean

A boolean is either **true** (1) or **false** (0). Any non-zero integer counts as true when tested in an `if`.

```
varset is_wanted bool true
if $<is_wanted>
  mob say I recognise you — guards!
endif
```

#### Integer (Number)

Integers store counts, scores, timers, and any other whole-number value. They are the most commonly used variable type.

```
varset stage integer 0
varset gold_owed integer 500
```

Use [arithmetic expansion](quick-codes.md) to do maths with integer variables:

```
varset stage integer $[$<stage> + 1]
mob echo You are now on stage $<stage>.
```

#### String

Strings store text. A string value is everything after the keyword `string` to the end of the line.

```
varset player_title string the Brave
varset quest_note string You must find the Lost Amulet of Zan.
```

You can build up a string over time with `appendline`:

```
varset journal string Day 1: Entered the forest.
varset journal appendline Day 2: Found a cave.
```

### Entity Reference Types

Entity-reference variables point to a live game entity. They are set by providing the entity's reference (a quick code like `$n`, a keyword, or a vnum).

| Type Keyword | What It Points To | Example |
|:-------------|:------------------|:--------|
| `room` | A room | `varset hometown room 3001` |
| `mobile` | A character (NPC) | `varset guard mobile $t` |
| `player` | A player character | `varset quest_giver player $n` |
| `object` | An object | `varset reward_item object $o` |
| `token` | A token | `varset buff_token token $t` |

#### Room

Stores a reference to a room by vnum.

```
varset spawn_room room 3001
mob transfer $n $<spawn_room>
```

#### Mobile / Player

Stores a reference to a character. Use `mobile` for NPCs and `player` for player characters.

```
varset my_target player $n
mob echoat $<my_target> The merchant waves at you.
```

{: .warning }
> Mobile and player references become invalid when the character logs out, dies (NPCs), or is otherwise removed from the game. Always check before using a stored reference.

#### Object

Stores a reference to an in-game object.

```
varset quest_item object $p
```

#### Token

Stores a reference to a token instance.

```
varset active_buff token $t
```

### Other `varset` Types

These types are less common but useful in advanced scripts.

| Type Keyword | Purpose |
|:-------------|:--------|
| `ed` | Extra description reference |
| `appendline` | Appends a line to an existing string variable |
| `rsg` | Binds a Random String Generator spec — generates a new string each time it is read |
| `generator` | Binds a generator |

#### RSG (Random String Generator)

RSG variables produce a different random string each time they are expanded. See [Advanced Scripting](advanced-scripting.md#random-string-generator-rsg-variables) for the full RSG specification syntax.

```
varset npc_name rsg yes sith_names:pattern:full_name
mob echo My name is $<npc_name>.
```

---

## Reading Variables

### In Text (Variable Expansion)

Use `$<varname>` anywhere in script text to insert a variable's value:

```
mob echo You have completed $<kill_count> of 10 kills.
mob say Welcome back, $<player_title>!
```

### In Conditions (IFChecks)

Use `$<varname>` on either side of a comparison:

```
if $<stage> == 3
  mob say You are on stage three.
endif

if $<kill_count> >= 10
  mob say Quest complete!
endif
```

For entity-type variables, you can use them wherever you would use a quick code:

```
varset my_target player $n
mob echoat $<my_target> You feel a chill.
```

### In Arithmetic

Variables can appear inside arithmetic expressions:

```
mob echo Total: $[$<base_price> * $<quantity>]
varset total integer $[$<base_price> * $<quantity>]
```

See [Quick Codes — Arithmetic](quick-codes.md) for the full expression syntax.

---

## Variable Commands

All variable commands work in every script type (mob, obj, room, token). The prefix (`mob`, `obj`, `room`, `tp`) changes depending on where the script lives, but the variable sub-command is the same.

{: .note }
> The examples below use `mob` as the prefix. Replace with `obj`, `room`, or `tp` as appropriate for your script type.

### varset — Set a Variable on Self

Sets a variable on the entity that owns the script.

**Syntax:**
```
mob varset <name> <type> <value>
```

**Examples:**
```
mob varset kill_count integer 0
mob varset greeting string Hello, adventurer!
mob varset is_hostile bool false
mob varset hometown room 3001
mob varset quest_target player $n
```

### varseton — Set a Variable on Another Entity

Sets a variable on a different entity (character, object, room, or token).

**Syntax:**
```
mob varseton <target> <name> <type> <value>
```

**Target** is a quick code (`$n`, `$t`, `$o`, etc.) or keyword identifying the entity.

**Examples:**
```
mob varseton $n quest_stage integer 1
mob varseton $n quest_name string The Lost Amulet
mob varseton $o is_enchanted bool true
```

### varclear — Remove a Variable from Self

Removes a variable entirely. The variable no longer exists after this command.

**Syntax:**
```
mob varclear <name>
```

**Example:**
```
mob varclear kill_count
```

### varclearon — Remove a Variable from Another Entity

Removes a variable from a target entity.

**Syntax:**
```
mob varclearon <target> <name>
```

**Example:**
```
mob varclearon $n quest_stage
```

### varcopy — Copy a Variable

Copies a variable's value to a new name on the same entity.

**Syntax:**
```
mob varcopy <oldname> <newname>
```

**Example:**
```
mob varcopy current_hp saved_hp
```

### varsave — Mark a Variable for Persistence

Controls whether a variable on the script's own entity survives server reboots. By default variables are **not** saved.

**Syntax:**
```
mob varsave <name> on|off
```

**Examples:**
```
mob varsave quest_stage on
mob varsave temp_counter off
```

### varsaveon — Mark a Variable on Another Entity for Persistence

Controls persistence for a variable on a target entity.

**Syntax:**
```
mob varsaveon <target> <name> on|off
```

**Example:**
```
mob varsaveon $n quest_stage on
```

---

## Tokens — The Data Model

A **token** is an invisible game object that can be attached to a character, an object, or a room. Tokens are not visible in inventory — they exist only for scripts to inspect and manipulate. Think of them as invisible tags or flags with storage.

### What Tokens Carry

Every token instance has:

| Field | Description |
|:------|:------------|
| **Vnum** | The template number (identifies what kind of token it is) |
| **Name** | A short name for identification |
| **Description** | A longer description (for builder reference) |
| **Type** | General, Quest, Affect, Skill, Spell, or Song |
| **Timer** | A tick countdown (or count-up). When it reaches zero, the token is removed and fires a `timer` trigger |
| **Values (0–7)** | Eight integer slots for storing data |
| **Flags** | Behaviour flags (removed on death, on quit, etc.) |
| **Variables** | A full variable list, just like any other entity |
| **Scripts** | Tokens can have their own script programs and triggers |
| **Extra Descriptions** | Additional text data |

### Token Types

| Type | Purpose |
|:-----|:--------|
| **General** | General-purpose — the most common type |
| **Quest** | Used for quest tracking |
| **Affect** | Creates character affects (stat modifiers, auras) |
| **Skill** | Defines a custom skill on the character |
| **Spell** | Defines a custom spell on the character |
| **Song** | Defines a custom song on the character |

### Token Flags

Flags control automatic removal behaviour.

| Flag | Effect |
|:-----|:-------|
| `purge_death` | Token is removed when the character dies |
| `purge_idle` | Token is removed on idle timeout |
| `purge_quit` | Token is removed when the character quits |
| `purge_reboot` | Token is removed on server reboot |
| `purge_rift` | Token is removed on rift (plane) travel |
| `reversetimer` | Timer counts **up** instead of down |
| `casting` | Token is protected during spell casting |
| `noskilltest` | Skips the skill test on action completion |
| `singular` | Only one instance of this token allowed on the entity |
| `see_all` | Token ignores sight/visibility rules |
| `spellbeats` | Token fires `spellbeat` triggers while casting |
| `permanent` | Token cannot be removed unless its owner is extracted |

### Where Tokens Attach

Tokens can live on three kinds of entity:

| Attached To | How It Works |
|:------------|:-------------|
| **Character** | The token is in the character's token list. Survives across rooms, saved with the character on quit. |
| **Object** | The token is in the object's token list. Saved with the object. |
| **Room** | The token is in the room's token list. Persists as long as the room exists. |

A token knows who owns it. In scripts on that token, `$(self.owner)` returns the character, `$(self.object)` returns the object, and `$(self.room)` returns the room.

### Token Values (val0–val7)

Every token has eight integer value slots, numbered 0 through 7. These are set in the token template (via the token editor) and can be read or modified at runtime.

```
if $(token.val0) > 5
  mob echo The buff is strong!
endif
```

Token values are commonly used for:
- Spell power / buff strength (val0)
- Duration or difficulty (val1)
- Target type (val2)
- Costs (val4)
- Custom per-token data

---

## Token Operations

### Giving a Token — `givetoken`

Attaches a token to a character by creating a new instance from the template vnum.

**Syntax:**
```
mob givetoken <target> <vnum>
```

**Example:**
```
mob givetoken $n 12345
mob say You have been marked by the curse!
```

After giving a token, scripts on that token begin responding to triggers.

### Removing a Token — `taketoken` / `junk`

Remove a token from a character. The exact command depends on the script type and context.

**`taketoken`** removes a specific token by vnum:
```
mob taketoken $n 12345
```

**`junk`** can be used in token programs (`tp`) to make a token remove itself:
```
tp junk self
```

### Checking for a Token — `hastoken`

The `hastoken` ifcheck tests whether an entity carries a token with a given vnum.

**Syntax:**
```
if hastoken <target> <vnum>
```

**Examples:**
```
if hastoken $n 12345
  mob say I see you bear the curse mark.
endif

if !hastoken $n 12345
  mob say You are free of the curse.
endif
```

### Reading Token Values in Scripts

When you are inside a token program (tp script), you can access the token's own values through `$(self.val0)` through `$(self.val7)` and its timer through `$(self.timer)`.

From **outside** the token, if you have stored a token reference in a variable:

```
mob varseton $n buff_tok token $t
if $(buff_tok.val0) >= 10
  mob echo The buff is at maximum power.
endif
```

### Token Timer

The timer field counts down by one each tick (roughly 4 seconds). When it reaches zero:

1. The token fires a `timer` trigger (if it has a script for one).
2. If the `reversetimer` flag is **not** set, the token is then removed.

Set the timer in the token editor or modify it at runtime via token scripts.

```
if $(self.timer) <= 1
  tp echoat $(self.owner) Your protection fades away...
endif
```

### Token Scripts

Tokens can have their own programs with their own triggers, just like mobs, objects, and rooms. See [Token Scripts](token-programs/) for the full trigger list and command reference.

Common token triggers:

| Trigger | Fires When |
|:--------|:-----------|
| `timer` | Token timer reaches zero |
| `attach` | Token is attached to an entity |
| `detach` | Token is about to be removed |
| `speech` | Someone speaks in the room |
| `act` | An act message matches a pattern |
| `random` | Periodic random chance |
| `fight` | Owner is in combat |
| `damage` | Owner takes damage |

---

## Variable Persistence

By default, variables do **not** survive a server reboot. You must explicitly mark them for saving.

### Making a Variable Persistent

```
mob varset quest_stage integer 1
mob varsave quest_stage on
```

Or on another entity:

```
mob varseton $n quest_stage integer 1
mob varsaveon $n quest_stage on
```

### What Survives What

| Event | Saved variables | Unsaved variables | Index variables |
|:------|:----------------|:------------------|:----------------|
| **Reboot** | ✅ Restored | ❌ Lost | ✅ Reloaded from area files |
| **Copyover** | ✅ In memory | ✅ In memory | ✅ In memory |
| **Area reset** | ✅ Kept | ✅ Kept | ✅ Re-copied to new instances |
| **NPC death** | ❌ Lost (NPC destroyed) | ❌ Lost | ✅ Restored on respawn |
| **Object destruction** | ❌ Lost | ❌ Lost | ✅ Restored on creation |
| **Player quit / login** | ✅ Saved and restored | ❌ Lost | N/A |

### Variable Scope

Variables are **entity-bound** — they live on the specific mob, object, room, or token they were set on. There are no global variables.

- A variable set on a player stays with that player.
- A variable set on an NPC stays with that specific NPC instance (and is lost when the NPC is removed).
- A variable set on a room stays with that room.
- A variable set on a token stays with that token (and is saved with whoever carries it, if the token survives).

### Index Variables

Index variables are set on the **template** (prototype) of a mob, object, room, or token via the OLC editors. When a new instance is created from that template, the index variables are automatically copied to it.

- Defined by builders in OLC editors, not by scripts at runtime.
- Saved in area files.
- Useful for giving every instance of a mob or object a default starting value.

---

## Token vs Variable: When to Use Which

Both tokens and variables store state, but they serve different purposes.

| Scenario | Use | Why |
|:---------|:----|:----|
| Track a quest stage (1, 2, 3…) | **Variable** on the player | Simple counter, one value, easy to read/write |
| Apply a temporary buff with a timer | **Token** on the player | Tokens have built-in timers that auto-remove; buff token can carry its own scripts for cleanup |
| Mark a mob as "alerted" | **Variable** on the mob | Simple boolean flag, no need for a full token |
| Give a player a custom skill | **Token** (Skill type) | Only tokens can define skills/spells/songs |
| Store a player's chosen faction | **Variable** on the player (with `varsave on`) | Simple string or number that needs to persist |
| Implement a cooldown | **Token** on the player | Use the timer field — when it expires, the cooldown is over and the token self-removes |
| Attach reactive behaviour to a player | **Token** with scripts | Tokens can respond to triggers independently |
| Remember which room a player entered from | **Variable** on the player | Room reference variable, no scripts needed |
| Multi-step quest with complex logic | **Token** with variables | Token scripts handle quest logic; token variables track sub-stages |

**Rule of thumb:**
- If you just need to store a value → **variable**.
- If you need a timer, automatic cleanup, or reactive scripts → **token**.
- If you need both → put **variables on a token**.

---

## Practical Patterns

### Quest Progress Tracking

Track a multi-stage quest with a variable on the player.

```
~ quest_giver_speech
if !hastoken $n 50001
  if $<quest_stage> == 0 or $<quest_stage> == null
    mob say Would you like to help me find my lost cat?
    mob varseton $n quest_stage integer 1
    mob varsaveon $n quest_stage on
  endif
endif

~ quest_giver_give (triggers when player gives the cat)
if $<quest_stage> == 1
  mob say You found her! Thank you!
  mob varseton $n quest_stage integer 2
  mob award $n qp 5
endif
```

### Buff with Timer

Give a temporary buff using a token. The token's timer handles automatic removal.

**On the NPC that casts the buff:**
```
if hastoken $n 60010
  mob say You are already protected.
else
  mob givetoken $n 60010
  mob say A golden shield surrounds you!
endif
```

**On the buff token (60010) — timer trigger:**
```
~ timer
tp echoat $(self.owner) The golden shield fades away.
```

Set the token template's timer to the desired duration (e.g. 30 ticks ≈ 2 minutes).

### Cooldown

Prevent an ability from being used too often.

```
if hastoken $n 60020
  mob say You must wait before using that again.
else
  mob givetoken $n 60020
  mob echo $N unleashes a powerful blast!
  mob damage $t 100
endif
```

Token 60020's template has a timer of 15 (≈ 1 minute cooldown). It self-removes when the timer expires, allowing the ability to be used again. No script is needed on the cooldown token — it simply exists as a flag.

### Kill Counter

Track kills toward a goal.

```
~ kill trigger on the quest NPC
mob varseton $n wolf_kills integer $[$<wolf_kills> + 1]
mob echoat $n {GWolf kill: $<wolf_kills> of 10.{x

if $<wolf_kills> >= 10
  mob echoat $n {YYou have slain enough wolves! Return to the hunter.{x
endif
```

### State Machine with Token + Variables

Use a token for reactive behaviour and variables on the token for state.

**Token 70001 — an escort quest tracker:**

```
~ attach trigger (when token is first given)
tp varset state integer 0
tp echoat $(self.owner) {CQuest started: Escort the merchant to safety.{x

~ random trigger
if $(self.val0) == 0
  ~ State 0: waiting for the player to reach checkpoint room
  if roomvnum $(self.owner) == 5010
    tp varset state integer 1
    tp echoat $(self.owner) {CCheckpoint reached! Now head to the city gate.{x
  endif
endif

if $(self.val0) == 1
  ~ State 1: waiting for the city gate
  if roomvnum $(self.owner) == 5050
    tp echoat $(self.owner) {GQuest complete! The merchant is safe.{x
    tp award $(self.owner) qp 10
    tp junk self
  endif
endif
```

### Persistent Player Preferences

Store a setting that survives reboots.

```
~ sayto trigger on a settings NPC
if $X == verbose
  mob varseton $n pref_verbose bool true
  mob varsaveon $n pref_verbose on
  mob say Verbose mode enabled.
elseif $X == quiet
  mob varseton $n pref_verbose bool false
  mob varsaveon $n pref_verbose on
  mob say Verbose mode disabled.
endif
```

---

## Quick Reference

### Variable Commands at a Glance

| Command | Target | Action |
|:--------|:-------|:-------|
| `varset <name> <type> <value>` | Self | Set a variable |
| `varseton <target> <name> <type> <value>` | Other entity | Set a variable on target |
| `varclear <name>` | Self | Remove a variable |
| `varclearon <target> <name>` | Other entity | Remove a variable from target |
| `varcopy <old> <new>` | Self | Copy a variable |
| `varsave <name> on\|off` | Self | Toggle persistence |
| `varsaveon <target> <name> on\|off` | Other entity | Toggle persistence on target |

### Token Commands at a Glance

| Command | Action |
|:--------|:-------|
| `givetoken <target> <vnum>` | Attach a token to a character |
| `taketoken <target> <vnum>` | Remove a token from a character |
| `junk self` | (Token script only) Token removes itself |

### Common IFChecks

| Check | Tests |
|:------|:------|
| `if hastoken $n <vnum>` | Does the target have the token? |
| `if $<varname> == <value>` | Does the variable equal the value? |
| `if $<varname> > <value>` | Numeric comparison |
| `if $<varname>` | Is the variable set and non-zero/non-empty? |

---

## See Also

- [Scripting Basics](scripting-basics.md) — control flow (`if`/`else`/`endif`), triggers, and first scripts
- [Quick Codes](quick-codes.md) — `$n`, `$<var>`, `$[math]`, entity expansion
- [Entity Reference](entity-reference.md) — entity targeting and field access
- [Advanced Scripting](advanced-scripting.md) — RSG variables, loops, sub-scripts
- [Token Scripts](token-programs/) — triggers and commands for token programs
- [Ifchecks Reference](ifchecks-reference.md) — all conditional checks including `hastoken`
