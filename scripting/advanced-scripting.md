---
layout: default
title: Advanced Scripting Concepts
nav_order: 11
parent: Scripting
---

# Advanced Scripting Concepts
{: .no_toc }

This page covers advanced techniques: variables, arithmetic, entity expansion, loops, sub-scripts, and asynchronous flow control.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Variables

Scripts can store and retrieve named values on any entity using the variable commands. Variables persist for as long as the entity exists unless explicitly cleared or until a server reboot (unless saved with `varsave`).

### Setting Variables

```
varset <name> number <value>
varset <name> string <text>
varset <name> room <vnum>
varset <name> obj <vnum>
varset <name> mob <vnum>
varset <name> rsg yes <spec>
```

| Type | Description |
|------|-------------|
| `number` | Integer value |
| `string` | Plain text string |
| `room` | Widevnum reference to a room |
| `obj` | Widevnum reference to an object |
| `mob` | Widevnum reference to a mobile |
| `rsg` | Random String Generator — generates a new random string each time the variable is expanded |

Examples:
```
varset stage number 0
varset greeting string Hello there
varset hometown room 3001
varset mob_name rsg yes sith_names:pattern:full_name
varset mob_title rsg yes sith_names:class:title
```

### Random String Generator (RSG) Variables

`rsg` variables generate a random string from a generator spec each time the variable is expanded. The spec format is:

```
<rsg_name>:<pattern|class>:<entry_name>
```

| Part | Description |
|------|-------------|
| `rsg_name` | Name of the RSG definition (created with `rsgedit`) |
| `pattern` or `class` | Whether to use a pattern or a class from the RSG |
| `entry_name` | The specific pattern or class to draw from |

```
varset mob_name rsg yes sith_names:pattern:full_name
varset mob_title rsg yes sith_names:class:title
echo My name is $<mob_name> and my title is $<mob_title>.
```

Each time `$<mob_name>` is expanded, the RSG generates a new random string according to the pattern. The alias `generator` can be used in place of `rsg`:

```
varset mob_name generator yes sith_names:pattern:full_name
```

{: .info}
**Variable expansion in entity string fields:** Variables (including `rsg` type) can also be embedded in the name, short, long, and description fields of rooms, mobiles, objects, tokens, dungeons, blueprints, and quests using `$<var_name>` syntax. This allows dynamically generated text to appear in entity displays.

### Reading Variables with IFChecks

```
if varnumber $(enactor) stage == 2
  say You're on stage 2!
endif

if varstring $(enactor) greeting == hello
  say Same greeting!
endif

if vardefined $(enactor) my_flag
  say The flag exists.
endif
```

### Variable Expansion in Text

Use `$<varname>` to expand a variable stored on the **current entity** (the one the script runs on):

```
varset count number 0
echo The count is $<count>.
say Your stage is stage-$<stage>.
```

Variable names can be **composed dynamically** using nested `$<>` tags. The inner variable is expanded first to build the outer name:

```
// If 'ore_type' holds the string "iron", this expands the variable named "speed_iron"
echo My speed is $<speed_$<ore_type>>.
```

To read a variable on **another entity**, use [entity cast syntax](#entity-cast-syntax) via `$()`:

```
// Numeric variable 'kills' on the enactor
say You have killed $(enactor.kills:num) monsters.

// String variable 'stage' on self
say My stage is $(self.stage:str).
```

### Saving Variables Permanently

By default variables are lost on reboot. Use `varsave` to persist them in the entity's data file:

```
varset score number 100
varsave $(enactor) score
```

### Variables on Rooms / Objects

Use `varseton` / `varclearon` to set variables on entities that are not the target:

```
// Set variable on the enactor's current room
varseton $(here) my_var number 1

// Clear it later
varclearon $(here) my_var

// Copy from one entity to another
varcopy $(enactor) my_score $(enactor) total_score
```

---

## Arithmetic Expansion

Arithmetic expressions use the `$[...]` syntax. Inside the brackets, variable names (bare alpha words) refer to variables on the current entity, and `$()` entity chains can be used for values from other entities:

```
varset x number 5
varset y number 3
echo Sum: $[x + y]
echo Diff: $[x - y]
echo Product: $[x * y]
echo Division: $[x / y]
echo Modulo: $[x % y]
```

Arithmetic can also be used inline wherever a number is expected (including ifcheck arguments):

```
if varnumber $(enactor) kills + 5 >= 20
  say You're almost there!
endif
```

IFChecks can be embedded in `$[...]` expressions for numeric outcomes:

```
echo Your effective level is $[$(enactor.level) + 5].
```

Supported operators: `+` `-` `*` `/` `%` (modulo), `(` `)` for grouping, `!` for logical negation.

---

## Entity Cast Syntax

Variables that hold **entity references** (mob, room, object, token, etc.) can be cast to their type to access that entity's fields, using the syntax:

```
$(varname:type)
$(varname:type.field)
$(varname:type.field.field)
```

The `:type` suffix tells the engine how to interpret the stored reference. Type names match those in `varset` (e.g. `mob`, `room`, `obj`, `token`, `quest`, `inst`, `dung`, `ship`, `area`).

**Examples:**

```
// Variable 'home_room' holds a room reference — read its sector
echo Your home is in sector $(home_room:room.sector).

// Variable 'target_mob' holds a mob vnum — read its short description
echo Target: $(target_mob:mob.short).

// Chain deeper: variable 'start_room' → room → area → area name
echo You start in $(start_room:room.area.name).
```

When accessing a variable on a **different entity** first, chain through it:

```
// Variable 'home_room' on the enactor, cast to room, get name
echo $(enactor.home_room:room.name).

// Variable 'stage' on the enactor, read as number
say Your quest stage is $(enactor.stage:num).
```

{: .info}
All valid `entity_types` from `script_const.c` work as cast targets: `num`, `str`, `mob`, `obj`, `room`, `exit`, `token`, `area`, `skill`, `spell`, `inst`, `dung`, `ship`, `quest`, `sect`, `church`, `aff`, `bool`, and more. The resulting entity can then be chained with any field that type supports.

---

## Entity Quick Code Expansion

Within any text argument to a script command, `$` codes expand to entity properties. These are shorthand for the equivalent `$(...)` entity chain:

| Code | Expands To | `$(...)` Equivalent |
|:-----|:-----------|:--------------------|
| `$i` | Self name (keyword) | `$(self.name)` |
| `$I` | Self short description | `$(self.short)` |
| `$n` | Actor name (keyword) | `$(enactor.name)` |
| `$N` | Actor short description | `$(enactor.short)` |
| `$t` | Target name (keyword) | `$(target.name)` |
| `$T` | Target short description | `$(target.short)` |
| `$e` | Actor subjective pronoun (he/she/it) | `$(enactor.subjective)` |
| `$E` | Target subjective pronoun | `$(target.subjective)` |
| `$m` | Actor objective pronoun (him/her/it) | `$(enactor.objective)` |
| `$M` | Target objective pronoun | `$(target.objective)` |
| `$s` | Actor possessive pronoun (his/her/its) | `$(enactor.possessive)` |
| `$S` | Target possessive pronoun | `$(target.possessive)` |
| `$p` | Object name (keyword) | `$(obj1.name)` |
| `$P` | Object short description | `$(obj1.short)` |
| `$r` | Room vnum | `$(here.vnum)` |

See [Entity Reference](entity-reference) for the complete list.

---

## Loops with `while` / `endwhile`

```
while <condition>
  // body
endwhile
```

{: .warning }
Always ensure your loop has a reachable exit condition. Infinite loops will crash the script.

### Counting loop example

```
varset i number 1
while varnumber $(self) i <= 5
  say Iteration $<i>.
  varset i number $[i + 1]
endwhile
```

### Iterating over zone mobiles

Some advanced loops use `mobs`, `people`, and similar ifchecks to process groups. Consult the [IFChecks Reference](ifchecks-reference) for iteration helpers.

---

## Break and Continue

Use `break` to exit a `while` loop early:

```
varset i number 0
  while varnumber $(self) i < 10
  if varnumber $(self) i == 5
    break
  endif
  varset i number $[i + 1]
endwhile
```

---

## Sub-scripts with `call` and `xcall`

Break complex logic into reusable sub-scripts.

### `call`

Calls another script (on the current entity) and **continues execution after it returns**:

```
call 501
```

### `xcall`

Calls a script on a **different entity type** (e.g. an mprog calling an oprog, or a room prog calling a mob prog):

```
// mprog calling another mprog on vnum 200
xcall mob 200 501

// mprog calling an oprog on object vnum 800
xcall obj 800 601
```

---

## Asynchronous Flow with `delay`

The `delay` command suspends the current script and schedules resumption after N pulses (1 pulse ≈ 0.25 seconds):

```
say I'll be back in 5 seconds.
delay 20
say Back again!
emote stretches.
```

{: .notice }
`delay` releases the mob between ticks. Other progs can fire in the meantime. Use `cancel` to abort a pending delay.

---

## `scriptwait`

`scriptwait <pulses>` pauses the script for N pulses but does **not** yield in the same way as `delay`. Use it for fine-grained timing within a single script run:

```
say Round one!
scriptwait 4
say Round two!
```

---

## The `at` Command

Execute a command from a different room context without physically moving:

```
at <vnum> <command>

// example: load a mob in another room
at 3100 mload 500
at 3200 echo Something stirs in the darkness.
```

---

## Interrupt and Queue

`interrupt` cancels a pending delayed-script chain. `queue` schedules a command for later without blocking:

```
// schedule for next pulse
queue 1 say I'll say this next pulse

// cancel any delayed script on self
interrupt
```

---

## Registers and Temp Storage

For ephemeral scratch values within a single script execution, use the temp registers:

```
if tempstore1 $(enactor) > 0
  say You have a temp value stored.
endif
```

These are available via ifchecks `tempstore1` through `tempstore4` and are automatically cleared at the end of each script execution.

---

## The `condition` Command

Evaluates an expression and branches with no explicit `if`/`endif` block — mostly used internally. Prefer explicit `if`/`endif` for readability.

---

## Practical Example: Multi-Stage Quest Script

This example shows a complete multi-stage quest using variables, delay, and sub-progs:

```
// Attached to an NPC with trigger: sayto "quest"
// Script vnum: 700

if isnpc $(self)
  if ispc $(enactor)
    if !vardefined $(enactor) thornwood_stage
      varset $(enactor) thornwood_stage number 0
    endif
    if varnumber $(enactor) thornwood_stage == 0
      say Well met, $(enactor)! I have a task for you.
      say Slay the corrupted wolf in the southern woods.
      varseton $(enactor) thornwood_stage number 1
      varsave $(enactor) thornwood_stage
    else
      if varnumber $(enactor) thornwood_stage == 1
        say You haven't finished yet. Find the corrupted wolf!
      else
        if varnumber $(enactor) thornwood_stage == 2
          say You've done it! Here is your reward.
          oload 800
          give sword $(enactor)
          award $(enactor) 500 xp
          varseton $(enactor) thornwood_stage number 3
          varsave $(enactor) thornwood_stage
        else
          say You've already completed this quest.
        endif
      endif
    endif
  endif
endif
```
