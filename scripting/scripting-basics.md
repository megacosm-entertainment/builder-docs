---
layout: default
title: Scripting Basics
nav_order: 1
parent: Scripting
---

# Basics of Scripting
{: .no_toc }

Sentience uses a custom scripting language called **MobProgs** (expanded over the years to cover rooms, objects, tokens, areas, and more). Scripts let you create talking NPCs, triggered events, custom quests, dynamic combat, and virtually any game logic you can imagine.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Script Editors

Every entity type has its own script editor, but they all share identical commands. The editors are:

| Editor | Command | Attaches To |
|:-------|:--------|:------------|
| MPEdit | `mpedit` | Mobiles |
| OPEdit | `opedit` | Objects |
| RPEdit | `rpedit` | Rooms |
| TPEdit | `tpedit` | Tokens |
| APEdit | `apedit` | Areas |
| IPEdit | `ipedit` | Instances (blueprints) |
| DPEdit | `dpedit` | Dungeons |
| QPEdit | `qpedit` | Quests |

### Common Script Editor Commands

```
commands       list           create         code           show           
comments       compile        name           flags          depth          
security       ?              
```

| Command | Syntax | Description |
|:--------|:-------|:------------|
| `create` | `mpedit create <vnum>` | Creates a new script at the given widevnum. |
| `name` | `name <text>` | A descriptive internal name for the script. |
| `code` | `code` | Opens the string editor. Type your script, end with `@` on a blank line. |
| `compile` | `compile` | Compiles the script and reports syntax errors. |
| `flags` | `flags <flag>` | Toggles script-level flags (see below). |
| `security` | `security <1-9>` | Minimum security to view/edit this script. |
| `depth` | `depth <number>` | Max recursion depth for this script. |
| `show` | `show` | Displays the script's current state and source code. |
| `comments` | `comments` | Builder-only notes about the script. |
| `list` | `list` | Lists all scripts of this type in the current area. |

### Script Flags

| Flag | Effect |
|:-----|:-------|
| `secured` | Prevents builders with lower security from viewing or editing. |
| `system` | Marks the script as a core system component — do not modify lightly. |
| `disabled` | Prevents the script from running without removing it from its entity. |

---

## The Scripting Workflow

1. **Create the script** in the appropriate editor:
   ```
   mpedit create 500
   name my_greeting_script
   ```

2. **Write the code:**
   ```
   code
   say Welcome to the city, $(enactor)!
   smile $(enactor)
   @
   ```

3. **Compile** to check for errors:
   ```
   compile
   ```

4. **Attach the script** to its entity using `addmprog` / `addoprog` / etc.:
   ```
   done
   medit 1200
   addmprog 500 greet 100
   done
   ```

5. **Test** in-game.

---

## Script Structure

A script is a sequence of commands executed top-to-bottom. Execution can branch using `if`/`else`/`endif` blocks and loop using `while`/`endwhile`. Comments start with `//`.

```
// This is a comment
if ispc $(enactor)
  say Hello, brave adventurer!
  if level $(enactor) > 20
    say You must be quite experienced.
  else
    say Come back when you're stronger.
  endif
else
  say Away with you, creature!
endif
```

### Basic Syntax Rules

- One command per line.
- Arguments are separated by spaces. Quotes are **not** used to group arguments — most commands take a fixed number of words.
- Entity references use `$(entity)` expansion (e.g. `$(enactor)` = the triggering actor). Short codes like `$n` are equivalent and still work, but the `$(...)` style is preferred.
- Numeric comparisons use `>`, `<`, `==`, `!=`, `>=`, `<=`.
- String comparisons use `==` and `!=`.

---

## Entity References

Scripts use `$(entity)` expressions to refer to the entities involved in a trigger. The available named primaries are:

| Expression | Who It Refers To |
|:-----------|:----------------|
| `$(self)` | The entity the script is attached to |
| `$(enactor)` | The entity that triggered the script (e.g. the player who walked in) |
| `$(target)` | A secondary target (e.g. the recipient of a `give` command) |
| `$(here)` | The room the script executes in |
| `$(obj1)` | An object involved in the trigger |
| `$(token)` | The token/quest involved (if applicable) |
| `$(random)` | A random character in the room |

Entity expressions can chain field access: `$(enactor.name)`, `$(here.sector)`, `$(obj1.vnum)`. See the [Entity Reference](entity-reference) for all available fields.

### `$` Quick Codes

The older short codes are still valid and are common in legacy scripts:

| Code | Equivalent | Who It Refers To |
|:-----|:-----------|:----------------|
| `$i` | `$(self)` | The entity the script is attached to |
| `$n` | `$(enactor)` | The actor who triggered the script |
| `$t` | `$(target)` | A secondary target |
| `$r` | `$(here)` | The current room |
| `$o` | `$(obj1)` | An object involved in the trigger |
| `$q` | `$(token)` | The token/quest involved |

For text display, the quick codes also have capitalized variants and pronoun forms (`$N`, `$e`, `$s`, etc.) — see the [Quick Codes Reference](quick-codes) for the full list.

---

## Attaching Scripts (Triggers)

After creating a script, you attach it to an entity with an `add*prog` command from within that entity's editor:

```
addmprog <script_vnum> <trigger> <phrase|percentage>
addoprog <script_vnum> <trigger> <phrase|percentage>
addrprog <script_vnum> <trigger> <phrase|percentage>
addtprog <script_vnum> <trigger> <phrase|percentage>
addaprog <script_vnum> <trigger> <phrase|percentage>
```

- **`<trigger>`** — the event name that fires the script (e.g. `greet`, `speech`, `random`, `death`).
- **`<phrase>`** — for speech/sayto triggers, the text to match. For most triggers this is the **percentage** chance (0–100) to run.

### Example

```
// In mpedit:
mpedit create 100
name innkeeper_greet
code
mpecho {YThe innkeeper looks up and smiles.{x
say Welcome, $(enactor)! Looking for a room?
@
compile
done

// In medit:
medit 200
addmprog 100 greet 100
done
```

---

## Widevnums in Scripts

Sentience uses **widevnums** instead of plain vnums in many contexts. A widevnum references an entity in another area using the format:

```
area_uid#vnum
```

For example, `2#500` means vnum 500 in area UID 2. When referencing entities in the *same* area, you can use the plain vnum. In scripts, the `mload`, `oload`, `teleport`, and similar commands accept widevnums.

---

## `if` / `else` / `endif`

```
if <check> <entity> [args...] [operator value]
  // runs if true
else
  // runs if false (optional)
endif
```

Examples:
```
if ispc $(enactor)
  say You're a player!
endif

if level $(enactor) >= 10
  say You're experienced enough to enter.
else
  say Come back when you're level 10.
endif

if hastoken $(enactor) 100
  say You already have the quest token.
endif
```

See the [IFChecks Reference](ifchecks-reference) for all available checks.

---

## `while` / `endwhile`

Loops while a condition is true. Use with caution — always include an exit condition.

```
varset count number 0
while varget(count) < 3
  say Still going...
  varset count number $[count + 1]
endwhile
```

---

## Variables

Scripts can read and write variables on entities with `varset`, `varclear`, and `varseton`/`varclearon`.

```
varset <name> number 0     // set numeric variable to 0
varset <name> string hello // set string variable
varset <name> room 3001    // set room reference variable
varclear <name>            // clear a variable
```

Variables on an entity are accessible in scripts using `$<varname>` (expansion) or tested with ifchecks like `varnumber $(enactor) my_var`.

---

## Compiling Your Script

Always compile after writing or editing code:

```
compile
```

The compiler reports:
- **Line number** of the error
- **Error message** describing the problem

Common errors:
- Unknown command name → typo in a script command
- Unmatched `if`/`endif` → missing `endif`
- Unknown ifcheck → typo in an `if` condition
- Wrong argument count → check the command reference

{: .notice }
Compilation errors prevent the script from running. Always fix errors before testing.
