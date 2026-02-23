---
layout: default
title: Quick Codes Reference
nav_order: 4
parent: Scripting
---

# Quick Codes Reference
{: .no_toc }

Quick codes are `$` shorthand tokens that expand to entity names, pronouns, and descriptions in script output (echo, say, emote, etc.).

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Standard Quick Codes

These codes are available in most script contexts. Exactly which entities are bound depends on the trigger type — see the relevant trigger reference for details.

| Code | Entity | Expands To |
|:-----|:-------|:-----------|
| `$i` | Self | Keyword name of the entity the script is attached to |
| `$I` | Self | Short description of the entity the script is attached to |
| `$n` | Actor | Keyword name of the entity that triggered the script |
| `$N` | Actor | Short description of the triggering entity |
| `$t` | Target | Keyword name of the secondary target |
| `$T` | Target | Short description of the secondary target |
| `$r` | Room | Vnum of the room where the actor is |
| `$p` | Object | Keyword name of the triggering object |
| `$P` | Object | Short description of the triggering object |
| `$q` | Token | Keyword name of a token involved in the trigger |
| `$Q` | Token | Short description of the token involved |
| `$e` | Actor | Subjective pronoun: **he** / **she** / **it** |
| `$E` | Target | Subjective pronoun: **he** / **she** / **it** |
| `$m` | Actor | Objective pronoun: **him** / **her** / **it** |
| `$M` | Target | Objective pronoun: **him** / **her** / **it** |
| `$s` | Actor | Possessive pronoun: **his** / **her** / **its** |
| `$S` | Target | Possessive pronoun: **his** / **her** / **its** |

---

## Variable Expansion

Named variables stored on the current entity are expanded with `$<varname>`:

```
echo Your kill count is $<killcount>.
```

To read a variable from another entity, use entity cast syntax via `$()`:

```
echo $(enactor.name:str) has $(enactor.kills:num) kills.
say My stage is $(self.stage:num).
```

---

## Usage Examples

### Say with pronouns
```
say Well met, $n! I hear $e is a capable fighter.
```
Output: `The innkeeper says 'Well met, Aragorn! I hear he is a capable fighter.'`

### Echo to a room
```
echo $N looks confused as $e searches for the door.
```
Output: `Aragorn the Ranger looks confused as he searches for the door.`

### Emote with target
```
emote bows respectfully to $T.
```
Output: `Elaine the innkeeper bows respectfully to Brandis the Sellsword.`

### Referring to an object
```
echo $n picks up $p and examines it curiously.
```
Output: `Aragorn picks up rusted key and examines it curiously.`

---

## Notes on `$i` vs `$n`

- `$i` is **the entity the script is attached to** (the NPC, object, room, or token running the script).
- `$n` is **the entity that caused the trigger** (usually a player, sometimes another NPC).

In room progs, `$i` refers to the room itself.
In object progs, `$i` refers to the object.

{: .notice }
Quick codes are case-sensitive. `$N` is the long/short description; `$n` is the keyword name.
