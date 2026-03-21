---
layout: default
title: Advanced Scripting
nav_order: 3
parent: Scripting
---

# Advanced Scripting
{: .no_toc }

This page covers features that go beyond basic control flow — variable deep-dives, sub-scripts, asynchronous execution, remote commands, and script-to-script communication. Read [Scripting Basics](scripting-basics.md) first.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Variable System Deep-Dive

Scripts store named values on entities with `varset`. Variables live as long as the entity exists — they are lost on reboot unless explicitly saved. For the complete variable and token reference, see [Variables and Tokens](variables-and-tokens.md).

### Variable Types at a Glance

| Type Keyword | Stores | Example |
|:-------------|:-------|:--------|
| `number` | Integer | `mob varset stage number 0` |
| `string` | Text | `mob varset greeting string Hello there` |
| `bool` | Boolean | `mob varset active bool true` |
| `room` | Room reference | `mob varset hometown room 3001` |
| `mobile` | Mobile reference | `mob varset guard mobile 5000` |
| `object` | Object reference | `mob varset reward object 800` |
| `token` | Token reference | `mob varset tok token $(enactor) 102899` |
| `rsg` | Random String Generator | `mob varset mob_name rsg yes names:pattern:full` |

Many more entity types exist (`area`, `quest`, `skill`, `spell`, `dungeon`, `instance`, `ship`, etc.) — see [Variables and Tokens](variables-and-tokens.md) for the full list.

### Scope and Lifetime

| Scope | Set With | Lifetime | Survives Reboot? |
|:------|:---------|:---------|:-----------------|
| Self | `varset` | Until entity is purged or cleared | No (unless `varsave`) |
| Other entity | `varseton` | Until that entity is purged or cleared | No (unless `varsaveon`) |
| Persistent | `varsave` / `varsaveon` | Written to entity's save file | Yes |

```
** Set a variable on this entity
mob varset quest_stage number 1

** Set a variable on the enactor (a different entity)
mob varseton $(enactor) quest_stage number 1

** Mark it for persistent save
mob varsaveon $(enactor) quest_stage
```

### Reading Variables

**On self** — use `$<varname>`:

```
mob varset count number 3
mob echo The count is $<count>.
```

**On another entity** — use `$(entity.varname:type)`:

```
mob say You have killed $(enactor.kills:num) monsters.
mob say Your stage is $(enactor.quest_stage:str).
```

### Variable Commands Summary

| Command | Purpose |
|:--------|:--------|
| `varset` | Set a variable on self |
| `varseton` | Set a variable on another entity |
| `varclear` | Remove a variable from self |
| `varclearon` | Remove a variable from another entity |
| `varcopy` | Copy a variable to a new name on self |
| `varsave` | Mark a variable for persistent save on self |
| `varsaveon` | Mark a variable for persistent save on another entity |
| `persist` | Mark a variable for persistent storage |

---

## Random String Generators (RSG)

RSG variables produce a **new random string each time they are expanded**. They draw from generators created with the `rsgedit` OLC editor.

### Syntax

```
mob varset <name> rsg yes <generator>:<pattern|class>:<entry>
```

The alias `generator` works identically:

```
mob varset <name> generator yes <generator>:<pattern|class>:<entry>
```

| Part | Description |
|:-----|:------------|
| `generator` | Name of the RSG definition (created with `rsgedit`) |
| `pattern` or `class` | Whether to use a named pattern or a class from the RSG |
| `entry` | The specific pattern or class entry to draw from |

### Example

```
** Assign random names from the "sith_names" RSG
mob varset mob_name rsg yes sith_names:pattern:full_name
mob varset mob_title rsg yes sith_names:class:title
mob echo My name is $<mob_name> and my title is $<mob_title>.
```

Each expansion of `$<mob_name>` generates a fresh random result. This is useful for giving unique names to spawned mobs, generating randomized dialogue, or creating procedural content.

{: .info }
RSG variables can also be embedded in entity string fields (name, short, long, description) using `$<var_name>` syntax, letting mob and object descriptions change dynamically.

---

## Entity Cast Syntax

When a variable holds an **entity reference** (mob, room, object, token, etc.), you can cast it to access that entity's fields:

```
$(varname:type)
$(varname:type.field)
$(varname:type.field.field)
```

The `:type` suffix tells the engine how to interpret the stored value.

### Basic Examples

```
** Variable 'home_room' holds a room reference — read its sector
mob echo Your home sector is $(home_room:room.sector).

** Variable 'target_mob' holds a mob reference — read its short desc
mob echo Target: $(target_mob:mob.short).

** Chain deeper: variable → room → area → area name
mob echo You start in $(start_room:room.area.name).
```

### Cross-Entity Access

When the variable is on a **different entity**, chain through that entity first:

```
** 'home_room' variable on the enactor, cast to room, get name
mob echo $(enactor.home_room:room.name).

** 'quest_stage' on the enactor, read as number
mob say Your quest stage is $(enactor.quest_stage:num).
```

### Real Example: Mining System (from crafting area)

This script reads a variable holding an object reference, casts it, and accesses fields on the referenced object:

```
** 'deposit' is a variable holding an object reference on the token
token echoat $(self.owner) You finish mining $(self.deposit:obj.short).
token varseton $<deposit> damage number $[[varnumber $<deposit> damage]-[dice $(self.owner.eq_wield1)]]
```

Here `$(self.deposit:obj.short)` means: "take the variable `deposit` on `self`, treat it as an object, and get its short description."

### Available Cast Types

Common cast types: `num`, `str`, `bool`, `mob`, `obj`, `room`, `exit`, `token`, `area`, `skill`, `spell`, `quest`, `inst`, `dung`, `ship`, `sect`, `church`, `aff`. The resulting entity can be chained with any field that type supports.

---

## Nested Variable Expansion

Variable names can be **built dynamically** by nesting `$<>` tags. The inner variable is resolved first to construct the outer name:

```
** If 'ore_type' holds "iron", this expands the variable named "speed_iron"
mob echo Speed is $<speed_$<ore_type>>.
```

### Real Example: Tracking Last Random Choice (from Achaeus)

This mob remembers its last random line to avoid repeats:

```
mob varset chance number $[1:9]
if varnumber chance == $<lastchance>
  end 1
endif

if varnumber chance == 9
  say Citizens! Pah! They're all a bunch of fat, lazy, bums!
elseif varnumber chance == 5
  say Can ya spare a silver piece?
  mob varset wants_silver number 5
elseif varnumber chance == 1
  fart
endif

** Remember what we said so we don't repeat it
mob varset lastchance number $<chance>
mob varclear chance
```

The `$<lastchance>` expansion reads the stored value from the previous trigger, and `$[1:9]` generates a random number in that range.

---

## Sub-Scripts: `call` and `xcall`

Break complex logic into reusable pieces by calling other scripts.

### `call` — Same-Entity Sub-Script

`call` executes a script on the **same entity** and returns control when it finishes. The called script inherits the current enactor, victim, and objects.

```
mob call <vnum> [enactor] [victim] [obj1] [obj2]
```

Use `NULL` for arguments you want to leave empty:

```
mob call 265981 $n NULL NULL
```

**Real example** — a king delegates conversation logic to a separate script (from Achaeus):

```
if carries $(enactor) 265800
or tokenvalue $(enactor) 265817 0 >= 1000
or isimmort $(enactor)
  if rand 80
    say $(enactor.short.capital)! Come! Have a seat!
  endif
  mob call 5002000 $(enactor)
else
  say Get out of my sight, peasant!
  mob transfer $(enactor) 265351
endif
```

### `xcall` — Cross-Entity Sub-Script

`xcall` calls a script on a **different entity** — a mob calling a token script, an object calling a room script, etc. Requires script security level 5+.

```
mob xcall <entity> <vnum> [enactor] [victim] [obj1] [obj2]
```

Where `<entity>` resolves to the target entity (mob, object, token, room, etc.).

**Real example** — an altar object calls a script on a player's summoning token (from Astral):

```
** Get the player's summoning token
obj varset tok token $(enactor) $[[tokenvalue $(enactor) 102899 0]]

** Set data on the token before calling
token adjust $<tok> 3 = $<element>
obj varseton $<tok> altar object $(self)

** Call the token's script
obj xcall $<tok> $[[tokenvalue $<tok> 2]]

** Check the return value
if lastreturn > 0
  obj queue $[[lastreturn]] obj call 102896
  obj alterobj $i extra | $[[flagextra glow]]
else
  obj echo The glow around $I fades immediately.
endif
```

### Return Values with `lastreturn`

When a called script finishes, it can set a return value using `end <value>`. The calling script reads it with the `lastreturn` ifcheck:

```
** In the called script (sub-script)
if varnumber stage > 3
  end 1
endif
end 0

** In the calling script
mob call 500
if lastreturn == 1
  mob echo Sub-script reported success.
endif
```

`lastreturn` is also set by `xcall`.

### Call Depth Limits

Scripts enforce a maximum call depth of **15 nested calls** (`MAX_CALL_LEVEL`). If a script chain exceeds this depth, further `call`/`xcall` commands silently fail. This prevents infinite recursion from crashing the server.

Additionally, `if`/`elseif`/`endif` blocks nest to a maximum depth of **24** (`MAX_NESTED_LEVEL`), and `while` loops nest to **20** (`MAX_NESTED_LOOPS`).

---

## Asynchronous Flow

### `delay` — Delayed Script Resumption

`delay` stops the current script and schedules the **next script** on the entity to fire after N pulses. One pulse ≈ 0.25 seconds (4 pulses = 1 second).

```
mob delay <pulses>
```

The entity's delay trigger fires when the timer expires. Commands after `delay` in the current script **do not execute** — put follow-up logic in the delay trigger script.

```
** Script on trigger: speech "trade"
mob echoat $n $I takes the items and begins to chant!
mob delay 4

** Script on trigger: delay
mob echo $I completes the ritual!
mob oload 265919
give sword $n
```

**Real example** — a wizard performing a trade (from Achaeus):

```
if carries $n 265865
and carries $n 1355
  mob remove $n 265865
  mob remove $n 1355
  mob echoat $n {Y$I takes the shrunken head and quarterstaff and begins to chant!{x
  mob delay 1
else
  say I see you have a shrunken head there. Possibly for trade?
endif
```

{: .warning }
`delay` releases the entity between ticks. Other scripts can fire in the meantime. Only **one** delay can be pending per entity at a time.

### `cancel` — Cancel a Pending Delay

Abort a previously set delay timer on the current entity:

```
mob cancel
```

This is useful when an event (like the mob dying or the player leaving) should abort a timed sequence.

### `scriptwait` — In-Line Pause

`scriptwait` pauses script execution for N pulses **without** yielding to other triggers. The script resumes from the next line when the wait ends.

```
mob scriptwait <pulses>
```

Available on: mob, obj, token.

```
mob say Round one!
mob scriptwait 4
mob say Round two!
mob scriptwait 4
mob say Round three!
```

Use `scriptwait` for short dramatic pauses. For longer waits or multi-script sequences, use `delay`.

### `queue` / `dequeue` — Event Queue

`queue` schedules a command to execute after a number of pulses, **without** stopping the current script. Multiple commands can be queued at different delays.

```
mob queue <pulses> <command>
```

`dequeue` removes **all** queued events from the entity.

**Real example** — a blacksmith crafting sequence (from Aethil):

```
if !hasqueue $(self)
  if carries $(enactor) 1917
    say Hmm, what a treat! A fang from the rare Pyrohydra!
    mob remove $(enactor) 1917
    mob queue 1 mob echoat $(enactor) You hand the fang to Morrollin.
    mob queue 1 mob echoaround $(enactor) $(enactor.short) hands the fang to Morrollin.
    mob queue 2 mob echo $I lays the fang on his forge and works it with a hammer.
    mob queue 3 mob oload 3407
    mob queue 4 give blade $(enactor)
  else
    say What I could do with one of its fangs..
    mob queue 1 ponder
    mob queue 2 say The Pyrohydra could be found in the Dungeon Mystica.
  endif
else
  mob echoat $(enactor) $(self.short) is busy right now, try again later.
endif
```

Key points:
- `hasqueue` ifcheck tests whether the entity has pending queued events (prevents overlapping sequences)
- Multiple `queue` commands with the same pulse count execute in order
- `dequeue` is useful for cleanup when an entity should stop all pending actions

### `interrupt`

`interrupt` cancels a target character's current action (such as casting a spell):

```
mob interrupt $(enactor)
```

---

## Registers and Temporary Storage

### tempstore (Token Temporary Storage)

Tokens have four integer scratch registers — `tempstore1` through `tempstore4` — for ephemeral data within a single script execution chain. They are accessed via ifchecks:

```
if tempstore1 $(enactor) > 0
  token echo Temp value is set.
endif
```

These are cleared between unrelated script invocations and are useful for passing small amounts of data between scripts on the same token without creating full variables.

### Token Values

Every token has **8 integer value slots** (value 0 through value 7), accessed with `tokenvalue`:

```
if tokenvalue $(enactor) 24001 0 >= 2
  mob echo Quest stage is advanced.
endif

** Adjust a token's value
token adjust $(enactor) 24001 0 + 1
```

Token values are defined in the token template and persist with the token.

---

## The `at` Command

Execute a command **from a different room's context** without physically moving the entity:

```
mob at <vnum> <command>
```

Available on: mob, obj, room.

```
** Load a mob in room 3100 without going there
mob at 3100 mob mload 500

** Send a message to a different room
mob at 3200 mob echo Something stirs in the darkness.

** Echo around a transferred player in their new location
mob at 265351 mob echoaround $(enactor) A knight drags $(enactor.short) outside.
```

**Real example** — a king banishing an intruder (from Achaeus):

```
say Guards! Take this miscreant from my throne room!
mob transfer $(enactor) 265351
mob at 265351 mob echoaround $(enactor) An Achaean Knight drags $(enactor.short) outside and drops $(enactor.him) to the ground.
```

---

## The `condition` Command

`condition` sets or modifies the condition/durability value of an object or NPC stat:

```
mob condition <target> <value>
```

For most builder work, explicit `if`/`endif` blocks are clearer. `condition` is primarily used for adjusting object condition values (wear and tear) or NPC stat modifications from within scripts.

---

## Script-to-Script Communication

Scripts on different entities often need to coordinate. Here are the common patterns.

### Token Passing

Give a token to an entity and read its values from another script:

```
** Script A: Give the player a tracking token
token give $(enactor) 102100
token adjust $(enactor) 102100 0 = 0
token adjust $(enactor) 102100 1 = 0
token adjust $(enactor) 102100 2 = 102101

** Script B (on a different entity): Read the player's token
if tokenvalue $(enactor) 102100 0 >= 5
  mob say You have completed enough tasks.
endif
```

### Variable Sharing with `varseton`

Set variables on a shared entity (like a room or the enactor) that other scripts can read:

```
** Script on object A: Mark the room
obj varseton $(here) ritual_active number 1

** Script on object B: Check the room variable
if vardefined $(here) ritual_active
  obj echo The ritual energies are active in this room.
endif
```

### Cross-Entity Calls with Data

Combine `varseton` + `xcall` to pass complex data to another entity's script:

```
** Set data on the target token, then call its script
obj varseton $<tok> altar object $(self)
obj xcall $<tok> $[[tokenvalue $<tok> 2]]

** The called script can read $(self.altar:obj.short) to see the altar
```

### Return Values as Communication

Use `end <value>` in a called script and `lastreturn` in the caller:

```
** Called script returns how long to keep the altar glowing
if varnumber charge > 10
  end 20
else
  end 0
endif

** Calling script reads the return
obj xcall $<tok> 102900
if lastreturn > 0
  obj queue $[[lastreturn]] obj call 102896
endif
```

---

## Practical Example: Elemental Altar System

This real example from the Astral area shows many advanced features working together — variables, entity casting, `xcall`, `lastreturn`, `queue`, and cross-script communication:

```
** Invoked when a player uses an elemental altar during a summoning.
** Trigger: use

** Check if they are currently summoning
if tokenvalue $(enactor) 102899 0 > 0
  ** Check if this altar has been attuned to an element
  if varnumber element > 0
    ** Check if this altar is already in use
    if hasqueue $i
      obj echoat $(enactor) $I is currently being harnessed. Try a different altar.
    else
      obj echoat $(enactor) {$<glow>You invoke $I, causing it to glow.{x
      obj echoaround $(enactor) {$<glow>$N invokes $I, causing it to glow.{x

      ** Get the player's summoning token
      obj varset tok token $(enactor) $[[tokenvalue $(enactor) 102899 0]]

      ** Pass our element to the summoning token
      token adjust $<tok> 3 = $<element>

      ** Tell the token which altar is being used
      obj varseton $<tok> altar object $(self)

      ** Call the summoning script on the token
      obj xcall $<tok> $[[tokenvalue $<tok> 2]]

      ** If the called script wants the altar to stay glowing
      if lastreturn > 0
        obj queue $[[lastreturn]] obj call 102896
        obj alterobj $i extra | $[[flagextra glow]]
      else
        obj echo The glow around $I fades immediately.
      endif
    endif
  else
    obj echoat $(enactor) That altar has no elemental essence within it.
  endif
else
  obj echoat $(enactor) You don't appear to be summoning anything.
endif
```

This script demonstrates:
- **Variable reads**: `$<element>`, `$<glow>` from the altar's own variables
- **Entity casting**: `token $(enactor) $[[tokenvalue ...]]` to get a token reference
- **Cross-entity variable setting**: `varseton $<tok> altar object $(self)`
- **`xcall`**: calling a script on the player's token from the altar's object script
- **`lastreturn`**: reading the called script's return value to decide next steps
- **`queue`**: scheduling a delayed follow-up without blocking
- **`hasqueue`**: preventing overlapping invocations

---

## Quick Reference: Script Limits

| Limit | Value | What Happens |
|:------|:------|:-------------|
| Max call depth | 15 | Further `call`/`xcall` silently fail |
| Max nested `if` depth | 24 | Compile error on the script |
| Max nested `while` loops | 20 | Compile error on the script |
| `xcall` minimum security | 5 | `xcall` requires script security ≥ 5 |

---

## See Also

- [Scripting Basics](scripting-basics.md) — control flow, `if`/`elseif`/`endif`, `while`, basic commands
- [Variables and Tokens](variables-and-tokens.md) — complete variable type reference, token system
- [IFChecks Reference](ifchecks-reference.md) — all conditional checks including `lastreturn`, `hasqueue`, `vardefined`
- [Command Availability](command-availability.md) — which commands are available in each script type
- [Entity Reference](entity-reference.md) — all `$()` entity fields and quick codes
