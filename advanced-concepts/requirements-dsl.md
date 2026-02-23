---
layout: default
title: Requirements DSL
nav_order: 5
parent: Advanced Concepts
---

# Requirements DSL

The **requirements system** provides a reusable, builder-readable way to define conditions that a character must meet before they can perform an action. It is evaluated in several places:

| System | Field | Checked when |
|--------|-------|--------------|
| Objects (`oedit`) | `prerequisites` | A player tries to **wear** the item |
| Quests (`qedit`) | `prerequisites` | A player tries to **accept** the quest |

By default, prerequisites are **invisible to players** — the block is silent and no indication appears in listings or inspect views. You can opt in to player-visible output by appending a message with the comma-suffix syntax described in the [Player-visible message](#player-visible-message) section below. Immortals bypass the check.

---

## Syntax overview

Requirements are written in a plain-text DSL that is compiled to JSON when saved. You never have to write raw JSON.

```
tot_level 50
race elf AND tot_level 20
(race elf OR race human) AND class_current warrior
quest_completed 5#1000 AND reputation 5#10 rank >= 3
```

### Atoms

Each atom is a single condition:

```
<check_name> <arguments>
```

### Logical combinators

| Keyword | Meaning |
|---------|---------|
| `AND` | Both sides must be true. Binds more tightly than `OR`. |
| `OR` | Either side must be true. |
| `(…)` | Explicit grouping when the default precedence is wrong. |

**Precedence example:**

```
race elf OR race gnome AND class_current mage
```

This is parsed as `race elf OR (race gnome AND class_current mage)` — more precisely: `AND` binds tighter. Use parentheses to force a different grouping:

```
(race elf OR race gnome) AND class_current mage
```

### Comparison operators

Where an atom accepts an optional operator `[op]`, these are valid:

| Operator | Meaning |
|----------|---------|
| `>=` | Greater than or equal (default when omitted) |
| `<=` | Less than or equal |
| `>` | Strictly greater than |
| `<` | Strictly less than |
| `==` | Exactly equal |
| `!=` | Not equal |

---

## Atom reference

### tot_level

```
tot_level [op] <number>
```

Total character level. Default operator is `>=`.

```
tot_level 50                 # tot_level >= 50
tot_level >= 30
tot_level < 100
```

---

### quest_points

```
quest_points [op] <number>
```

The character's accumulated quest points.

```
quest_points 200
quest_points >= 500
```

---

### staff_rank

```
staff_rank [op] <rank_name_or_number>
```

Character's staff / immortal rank. Rarely used on player-facing content.

```
staff_rank builder
staff_rank >= 2
```

---

### race

```
race <race_id>
```

Character's current race matches the given race ID (e.g. `elf`, `human`, `dwarf`).

```
race elf
race human
```

---

### class_current

```
class_current <class_name>
```

The named class is the character's **active** class (the one providing its current level benefits).

```
class_current warrior
class_current mage
```

---

### class_available

```
class_available <class_name>
```

The character has **any levels** in the named class (it has been unlocked), regardless of which class is currently active.

```
class_available shadow_dancer
```

---

### class_level

```
class_level <class_name> [op] <number>
```

The character's level in a specific class. Default operator is `>=`.

```
class_level warrior 20
class_level shadow_dancer >= 10
class_level mage == 50
```

---

### token

```
token <widevnum> [count <number>]
```

The character is carrying a token of the given widevnum. Optionally require at least `count` tokens.

```
token 5#50
token 5#50 count 3
```

---

### quest_completed

```
quest_completed <widevnum>
```

The character has previously **completed** the named quest (quest v2 widevnum).

```
quest_completed 5#1000
quest_completed 3#200
```

---

### quest_active

```
quest_active <widevnum>
```

The character currently **has the quest active** (accepted but not yet completed).

```
quest_active 5#1000
```

---

### reputation

```
reputation <widevnum> [rank [op] <number>]
```

The character has a reputation entry for the given faction. Optionally also require their rank to satisfy a comparison. Default rank operator is `>=`.

```
reputation 5#10                    # any rank at all
reputation 5#10 rank 3             # rank >= 3
reputation 5#10 rank >= 4
reputation 5#10 rank == 2
```

---

### plr_flag

```
plr_flag <flag_name> [true|false]
```

The character has (or does not have) a specific player flag set. Omitting `true|false` defaults to requiring the flag to be present (`true`).

```
plr_flag pkill
plr_flag pkill true
plr_flag pkill false          # the flag must NOT be set
```

---

### script

```
script [<phrase>]
```

Fires a script trigger on the owning entity (the object being worn or the mob/quest being started) using the actor as the test character. Returns true if the trigger fires successfully (returns a non-zero value). The trigger used depends on the owning entity type:

- Object → `TRIG_PREWEAR`
- Quest / mob / room / token → `TRIG_PREQUEST`

```
script check_eligibility
script
```

---

## Player-visible message

By default, a failing prerequisite is entirely silent — no output appears in the quest list, object inspect view, or any other listing. To surface the requirement to players, append a comma followed by a display message:

```
<expression>, <player-visible message>
```

### Variants

| Suffix | Player sees (when unmet) |
|--------|-------------------------|
| `<expr>` | Nothing (silent) |
| `<expr>, <message>` | Your custom message |
| `<expr>, hidden` | A generic "additional requirements" hint |
| `<expr>, <message>, hidden` | Your custom message and also marked hidden |

```
tot_level 50, You must be at least level 50.
quest_completed 5#900 AND reputation 5#10 rank 2, Complete the introductory questline first.
class_current warrior, Warriors only.
race elf, hidden
```

### How the message appears by context

**Objects — identify / lore / inspect:**

| Condition | Output |
|-----------|--------|
| `player_string` set, requirement met | `Requires: ✓ <message>` |
| `player_string` set, unmet | `Requires: ✗ <message>` |
| `hidden` flag, unmet, no message | `* Additional requirements not met.` (grey) |
| Neither | *(nothing shown)* |

Note: the wear-attempt itself always shows a generic "You don't meet the requirements to use this item." regardless of the message suffix.

**Quests — quest list and inspect:**

| Condition | Output |
|-----------|--------|
| `player_string` set, unmet | `(locked: <message>)` in red next to quest name |
| `hidden` flag, unmet, no message | `(locked: additional requirements)` in grey |
| Neither, unmet | `(locked)` in grey |
| Met | *(nothing shown)* |

And in the quest inspect view, the `Requires:` line shows a green tick or red cross alongside your message.

---

## Examples

**Minimum level with completed quest:**

```
tot_level 50 AND quest_completed 5#1000
```

**With a player-visible message:**

```
tot_level 50, Level 50 required.
quest_completed 5#900, Complete the introductory questline first.
```

**Hidden gate (shows a generic hint but not the reason):**

```
tot_level 50, hidden
```

**Class-restricted item (warrior or paladin):**

```
class_current warrior OR class_current paladin
```

**Race and class gate with minimum level:**

```
(race elf OR race half_elf) AND class_available ranger AND tot_level 20
```

**Reputation-gated quest:**

```
reputation 5#10 rank >= 3 AND quest_completed 5#500
```

**Token-gated (requires an access pass):**

```
token 5#999 AND tot_level 25
```

---

## Setting requirements in the editors

### oedit (objects)

```
oedit <ref> prerequisites <dsl>
oedit <ref> prerequisites none
```

The check fires when a player tries to wear the object. See [oedit command reference](../objects/oedit-command-reference.html#prerequisites).

### qedit (quests)

```
qedit <ref> prerequisites <dsl>
qedit <ref> prerequisites none
```

The check fires when a player tries to accept the quest. See [qedit command reference](../quests/qedit-command-reference.html#prerequisites).
