---
layout: default
title: Quick Codes
nav_order: 6
parent: Scripting
---

# Quick Codes
{: .no_toc }

Quick codes are `$`-prefixed shorthand tokens that expand at runtime into entity names, pronouns, descriptions, variable values, and computed results. They are the primary way scripts produce dynamic output.

When a script says `mob echo $n has arrived!`, the engine replaces `$n` with the actor's keyword name before displaying the text. Quick codes work anywhere script text is expanded: `echo`, `say`, `emote`, `mob echo`, `mob echoat`, and most other output commands.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## How Quick Codes Work

Every trigger sets up a **context** — a set of entity references that quick codes can refer to. The most common entities are:

| Entity | What It Is | Set By |
|:-------|:-----------|:-------|
| **Self** | The mob, object, room, or token running the script | Always available |
| **Actor** | The character who triggered the script (usually a player) | Most triggers |
| **Victim** | A secondary character target | Combat, social, and targeted triggers |
| **Object 1** | The primary object involved | Object-related triggers (get, drop, give, wear, etc.) |
| **Object 2** | A secondary object | Two-object triggers (put, give) |
| **Tracked Target** | A persistent target pointer (follows across waits) | Target-tracking triggers |
| **Random** | A random character in the room (lazy-loaded on first use) | Always available |

Quick codes are **case-sensitive**. Lowercase codes produce keyword names; uppercase codes produce short descriptions (the "pretty" name players see). For example:

- `$n` → `aragorn` (keyword)
- `$N` → `Aragorn the Ranger` (short description)

If a referenced entity is not available (e.g., no victim in a greet trigger), the code expands to `someone`, `something`, or `someone's` as appropriate.

---

## Self Codes

These refer to the entity the script is attached to — the NPC running a mob prog, the object running an object prog, the room running a room prog, or the token running a token prog.

### Name and Description

| Code | Expands To |
|:-----|:-----------|
| `$i` | **Keyword** of self. For mobs/objects: the first keyword. For rooms/tokens: the widevnum string. |
| `$I` | **Short description** of self. For mobs: short description. For objects: short description. For rooms: room name. For tokens: token name. |

### Self Pronouns

| Code | Type | Expands To |
|:-----|:-----|:-----------|
| `$j` | Subjective | **he** / **she** / **it** (based on self's sex) |
| `$k` | Objective | **him** / **her** / **it** |
| `$l` | Possessive | **his** / **her** / **its** |
| `$v` | Absolute possessive | **his** / **hers** / **its** |
| `$F` | Reflexive | **himself** / **herself** / **itself** |

{: .note }
Self pronouns use the mob's sex directly without a visibility check. They are only meaningful on mob progs (where self is a character). On object/room/token progs, they default to neutral (`it`, `its`).

---

## Actor Codes

The **actor** (also called the **enactor**) is the character who triggered the script — usually a player, sometimes another NPC.

### Name and Description

| Code | Expands To |
|:-----|:-----------|
| `$n` | **Keyword** of the actor (first keyword from their name). Shows `someone` if not visible. |
| `$N` | **Short description** of the actor. For NPCs or morphed characters: short description. For players: capitalized name. Shows `someone` if not visible. |

### Actor Pronouns

| Code | Type | Expands To |
|:-----|:-----|:-----------|
| `$e` | Subjective | **he** / **she** / **it** |
| `$f` | Reflexive | **himself** / **herself** / **itself** |
| `$m` | Objective | **him** / **her** / **it** |
| `$s` | Possessive | **his** / **her** / **its** |
| `$u` | Absolute possessive | **his** / **hers** / **its** |

All actor codes respect visibility — if the script's entity cannot see the actor, the code shows `someone` or `someone's`.

---

## Victim Codes

The **victim** is a secondary character target. It is set by combat triggers, social triggers, targeted actions, and similar triggers that involve two characters.

### Name and Description

| Code | Expands To |
|:-----|:-----------|
| `$t` | **Keyword** of the victim. Shows `someone` if not visible. |
| `$T` | **Short description** of the victim. For NPCs/morphed: short description. For players: capitalized name. Shows `someone` if not visible. |

### Victim Pronouns

| Code | Type | Expands To |
|:-----|:-----|:-----------|
| `$E` | Subjective | **he** / **she** / **it** |
| `$x` | Reflexive | **himself** / **herself** / **itself** |
| `$M` | Objective | **him** / **her** / **it** |
| `$S` | Possessive | **his** / **her** / **its** |
| `$U` | Absolute possessive | **his** / **hers** / **its** |

---

## Object Codes

Two objects can be referenced: **Object 1** (the primary object) and **Object 2** (the secondary object). Which trigger sets which depends on context — see the trigger reference pages for details.

| Code | Entity | Expands To |
|:-----|:-------|:-----------|
| `$o` | Object 1 | **Keyword** of the primary object. Shows `something` if not visible. |
| `$O` | Object 1 | **Short description** of the primary object. Shows `something` if not visible. |
| `$p` | Object 2 | **Keyword** of the secondary object. Shows `something` if not visible. |
| `$P` | Object 2 | **Short description** of the secondary object. Shows `something` if not visible. |

{: .tip }
In many common triggers (get, drop, give, wear), **Object 1** (`$o`/`$O`) is the object being acted upon. **Object 2** (`$p`/`$P`) is used in two-object scenarios like `put sword chest` where `$o` = sword and `$p` = chest.

---

## Tracked Target Codes

The **tracked target** is a persistent target pointer that survives across `wait` commands. It is used by triggers that set up long-running target tracking.

### Name and Description

| Code | Expands To |
|:-----|:-----------|
| `$q` | **Keyword** of the tracked target. Shows `someone` if not visible. |
| `$Q` | **Short description** of the tracked target. For NPCs/morphed: short description. For players: capitalized name. Shows `someone` if not visible. |

### Tracked Target Pronouns

| Code | Type | Expands To |
|:-----|:-----|:-----------|
| `$X` | Subjective | **he** / **she** / **it** |
| `$Y` | Objective | **him** / **her** / **it** |
| `$Z` | Possessive | **his** / **her** / **its** |
| `$w` | Absolute possessive | **his** / **hers** / **its** |
| `$z` | Reflexive | **himself** / **herself** / **itself** |

---

## Random Character Codes

The **random character** is a randomly selected character from the room. It is lazy-loaded — the first time any `$r`/`$R`/`$J`/`$K`/`$L`/`$V`/`$y` code is expanded, a random character is picked and reused for the rest of that expansion.

### Name and Description

| Code | Expands To |
|:-----|:-----------|
| `$r` | **Keyword** of a random character in the room. Shows `someone` if not visible. |
| `$R` | **Short description** of the random character. For NPCs/morphed: short description. For players: capitalized name. Shows `someone` if not visible. |

### Random Character Pronouns

| Code | Type | Expands To |
|:-----|:-----|:-----------|
| `$J` | Subjective | **he** / **she** / **it** |
| `$K` | Objective | **him** / **her** / **it** |
| `$L` | Possessive | **his** / **her** / **its** |
| `$V` | Absolute possessive | **his** / **hers** / **its** |
| `$y` | Reflexive | **himself** / **herself** / **itself** |

---

## Complete Quick Code Reference

All single-letter quick codes at a glance:

| Code | Entity | Expands To |
|:-----|:-------|:-----------|
| `$e` | Actor | he/she/it (subjective) |
| `$f` | Actor | himself/herself/itself (reflexive) |
| `$i` | Self | keyword name |
| `$j` | Self | he/she/it (subjective) |
| `$k` | Self | him/her/it (objective) |
| `$l` | Self | his/her/its (possessive) |
| `$m` | Actor | him/her/it (objective) |
| `$n` | Actor | keyword name |
| `$o` | Object 1 | keyword name |
| `$p` | Object 2 | keyword name |
| `$q` | Tracked Target | keyword name |
| `$r` | Random | keyword name |
| `$s` | Actor | his/her/its (possessive) |
| `$t` | Victim | keyword name |
| `$u` | Actor | his/hers/its (absolute possessive) |
| `$v` | Self | his/hers/its (absolute possessive) |
| `$w` | Tracked Target | his/hers/its (absolute possessive) |
| `$x` | Victim | himself/herself/itself (reflexive) |
| `$y` | Random | himself/herself/itself (reflexive) |
| `$z` | Tracked Target | himself/herself/itself (reflexive) |
| | | |
| `$E` | Victim | he/she/it (subjective) |
| `$F` | Self | himself/herself/itself (reflexive) |
| `$I` | Self | short description |
| `$J` | Random | he/she/it (subjective) |
| `$K` | Random | him/her/it (objective) |
| `$L` | Random | his/her/its (possessive) |
| `$M` | Victim | him/her/it (objective) |
| `$N` | Actor | short description / capitalized name |
| `$O` | Object 1 | short description |
| `$P` | Object 2 | short description |
| `$Q` | Tracked Target | short description / capitalized name |
| `$R` | Random | short description / capitalized name |
| `$S` | Victim | his/her/its (possessive) |
| `$T` | Victim | short description / capitalized name |
| `$U` | Victim | his/hers/its (absolute possessive) |
| `$V` | Random | his/hers/its (absolute possessive) |
| `$X` | Tracked Target | he/she/it (subjective) |
| `$Y` | Tracked Target | him/her/it (objective) |
| `$Z` | Tracked Target | his/her/its (possessive) |

{: .note }
Codes not listed here (e.g., `$a`, `$b`, `$c`, `$d`, `$g`, `$h`, `$A`, `$B`, `$C`, `$D`, `$G`, `$H`, `$W`) are **unused** and will produce a placeholder error token if used. Use `$$` to output a literal dollar sign.

---

## Variable Expansion: `$<varname>`

To display a [variable's](variables-and-tokens.html) value inline, wrap the variable name in angle brackets:

```
echo Your kill count is $<killcount>.
echo The door is $<doorstate>.
```

The engine looks up the named variable on the script's entity and inserts its value as text. The output depends on the variable's type:

| Variable Type | Output |
|:--------------|:-------|
| Integer | The number (e.g., `42`) |
| Boolean | `true` or `false` |
| String | The string value |
| Mobile | First keyword of the mobile |
| Object | First keyword of the object |
| Room | Widevnum string |
| Token | Widevnum string |

If the variable does not exist, `(null-var)` is shown.

---

## Entity Field Access: `$(entity.field)`

The entity access syntax lets you read fields from any entity in the trigger context. See [Entity Reference](entity-reference.html) for the complete list of fields.

### Basic Syntax

```
$(entity.field)
```

**Available entities:**

| Entity Name | What It References |
|:------------|:-------------------|
| `self` or `this` | The entity running the script |
| `enactor` | The actor who triggered the script |
| `victim` | The victim/target character |
| `victim2` | A second victim (rare) |
| `target` | The tracked target |
| `random` | A random character in the room |
| `obj1` | Object 1 |
| `obj2` | Object 2 |
| `here` | The room where the script is running |
| `token` | The token associated with the trigger |
| `phrase` | The trigger phrase (as a string) |
| `trigger` | The trigger text (as a string) |
| `prior` | The calling script's context (for nested scripts) |
| `game` | Global game state |
| `event` | The event context |

**Examples:**

```
echo $(enactor.name) is level $(enactor.level).
echo This room is called $(here.name).
echo The token is $(token.name).
say I am $(self.shortdescr).
```

### Variable Access With Type Cast: `$(entity.varname:type)`

To read a variable stored on another entity, use the type-cast suffix to specify what type the variable holds:

```
$(entity.varname:type)
```

**Common type suffixes:**

| Suffix | Variable Type |
|:-------|:-------------|
| `:num` | Integer |
| `:str` | String |
| `:mob` | Mobile reference |
| `:obj` | Object reference |
| `:room` | Room reference |
| `:token` | Token reference |
| `:area` | Area reference |

**Examples:**

```
echo $(enactor.quest_stage:num) out of 5 stages complete.
echo The guard's name is $(self.guard_name:str).
if $(enactor.flagged:num) == 1
  say You've already been here, $n.
endif
```

### Chained Field Access

You can chain fields to navigate from one entity to another:

```
echo $(enactor.room.name)
echo $(self.leader.name)
echo $(obj1.carriedby.name)
```

---

## Arithmetic Expressions: `$[expression]`

The `$[...]` syntax evaluates a mathematical expression and inserts the result as a number.

**Supported operators:**

| Operator | Meaning |
|:---------|:--------|
| `+` | Addition |
| `-` | Subtraction (or negation) |
| `*` | Multiplication |
| `/` | Integer division |
| `%` | Modulo (remainder) |
| `:` | Random number (e.g., `1:10` = random 1–10) |
| `!` | Logical NOT |
| `(` `)` | Grouping |

**Examples:**

```
echo You need $[$[10] - $(enactor.quest_kills:num)] more kills.
echo Rolling a d20: $[1:20]
echo Double damage: $[$(self.damage:num) * 2]
```

Variables and entity fields can be used inside expressions. The result is always an integer.

---

## Practical Examples

### Greeting a player

```
say Welcome to my shop, $N! What can I get for $m today?
```

**Output:** `The shopkeeper says 'Welcome to my shop, Aragorn the Ranger! What can I get for him today?'`

### Self-referencing with pronouns

```
emote flexes $l muscles and grins at $N.
```

**Output:** `The barbarian flexes his muscles and grins at Elindra the Sorceress.`

### Echo with actor and victim

```
mob echoaround $n $N lunges at $T with $s sword!
```

**Output:** `Aragorn the Ranger lunges at the goblin chief with his sword!`

### Using both object codes

```
echo $N puts $O into $P carefully.
```

**Output:** `Elindra puts a glowing gem into a leather pouch carefully.`

### Variable in output

```
mob varset self quest_step 3
echo You are on step $<quest_step> of the quest.
```

**Output:** `You are on step 3 of the quest.`

### Entity field access in speech

```
say I see you are level $(enactor.level), $n. $(enactor.class.name) is a fine calling.
```

**Output:** `The sage says 'I see you are level 42, Aragorn. warrior is a fine calling.'`

### Arithmetic in output

```
echo You have $[$(enactor.gold) / 100] gold crowns and $[$(enactor.gold) % 100] silver pieces.
```

### Random character interaction

```
mob echoaround $r $I glances at $R and mutters something.
```

**Output:** `The old hermit glances at Brandis the Sellsword and mutters something.`

### Tracked target across a wait

```
mob target $n
wait 10
say Ah, $Q! You waited for me. How kind of $Y.
```

**Output (after 10 seconds):** `The merchant says 'Ah, Aragorn the Ranger! You waited for me. How kind of him.'`

---

## Understanding `$i` vs `$n`

This distinction trips up new builders frequently:

- **`$i`** is **self** — the entity the script is attached to (the NPC running the mob prog, the object running the object prog, the room, or the token).
- **`$n`** is **the actor** — the character who caused the trigger to fire (usually a player, sometimes another NPC).

| Script Type | `$i` refers to | `$I` shows |
|:------------|:---------------|:-----------|
| Mob prog | The NPC | NPC's short description |
| Object prog | The object | Object's short description |
| Room prog | The room | Room's widevnum |
| Token prog | The token | Token's widevnum |

```
~ A mob prog:
say My name is $I and you are $N.
```

**Output:** `The innkeeper says 'My name is the innkeeper and you are Aragorn the Ranger.'`

---

## Pronoun Types Explained

Quick codes support five types of pronouns, each used in different grammatical positions:

| Type | Male | Female | Neutral | Example Usage |
|:-----|:-----|:-------|:--------|:-------------|
| **Subjective** | he | she | it | `$e smiles` → "he smiles" |
| **Objective** | him | her | it | `looks at $m` → "looks at him" |
| **Possessive** | his | her | its | `$s sword` → "his sword" |
| **Absolute possessive** | his | hers | its | `it is $u` → "it is his" |
| **Reflexive** | himself | herself | itself | `hurts $f` → "hurts himself" |

---

## Tips

- **Use `$N`/`$T` for display.** Uppercase codes give the "pretty" name players expect to see. Use lowercase `$n`/`$t` when you need keyword matching or targeting.
- **Use `$$` for a literal `$`.** If you need an actual dollar sign in output, double it.
- **Check your trigger's entity bindings.** Not all triggers set all entities. A greet trigger sets the actor but has no victim. Using `$T` in a greet prog will show `someone`. Check the specific [trigger reference pages](index.html) for what each trigger provides.
- **Visibility matters.** If the script's entity (self) cannot see the actor or victim, quick codes show `someone`/`something` instead of the real name. Self pronoun codes (`$j`, `$k`, `$l`, `$v`, `$F`) bypass visibility since they reference the entity's own sex.
- **Variable details.** For more about storing and managing variables, see [Variables and Tokens](variables-and-tokens.html). For the complete list of entity fields accessible via `$(entity.field)`, see [Entity Reference](entity-reference.html).
