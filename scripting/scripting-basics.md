---
layout: default
title: Scripting Basics
nav_order: 2
parent: Scripting
---

# Scripting Basics
{: .no_toc }

Sentience uses a custom scripting language for bringing the world to life. Scripts make NPCs talk, rooms react, objects do interesting things, and quests work. If you have never written code before, don't worry — this page starts from scratch and builds up one concept at a time.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## How Scripts Work

A script is a list of commands that the game runs from top to bottom. You attach a script to something in the world — a mobile (NPC), an object, a room, a token, an area, and so on — and then assign a **trigger** that tells the game *when* to run it. For example, "run this script whenever a player enters the room" or "run this script when someone says hello."

Each line in a script is one command. Here is a complete, working script:

```
mob echo {YThe innkeeper looks up and smiles.{x
mob say Welcome, $n! Looking for a room?
```

When this script runs, the NPC sends a yellow-colored emote to the room, then says a greeting that includes the triggering player's name.

---

## Entity Prefix

Every script command starts with a prefix that tells the game what type of entity owns the script. The prefix must match the entity the script is attached to.

| Prefix | Entity Type |
|:-------|:------------|
| `mob` | Mobiles (NPCs) |
| `obj` | Objects (items) |
| `room` | Rooms |
| `token` | Tokens |
| `area` | Areas |
| `instance` | Instances (dungeon runtime) |
| `dungeon` | Dungeon blueprints |
| `quest` | Quests |
| `event` | Events |

A script attached to a mobile uses `mob` commands. A script attached to an object uses `obj` commands. Using the wrong prefix will not compile.

```
** Mobile script — correct
mob echo The guard nods.
mob say Halt! Who goes there?

** Object script — correct
obj echoat $n You pick up the glowing orb.
obj echoaround $n $N picks up the glowing orb.
```

---

## Command Structure

Each line follows this pattern:

```
<prefix> <command> [arguments...]
```

- **One command per line.** You cannot put two commands on a single line.
- **Arguments are separated by spaces.** Quotes are not used to group arguments — most commands know how many arguments to expect, and everything after the last argument is treated as a text message.
- **Commands are case-insensitive.** `mob echo`, `MOB ECHO`, and `Mob Echo` all work.

### Common Commands at a Glance

| Command | What It Does |
|:--------|:-------------|
| `echo` | Send a message to everyone in the room |
| `echoat` | Send a message to one specific character |
| `echoaround` | Send a message to everyone *except* one character |
| `say` | Make the entity speak |
| `mload` | Load (create) a mobile by vnum |
| `oload` | Load (create) an object by vnum |
| `force` | Force a character to execute a command |
| `transfer` | Move a character to another room |
| `damage` | Inflict damage on a character |
| `purge` | Remove entities from the room |
| `varset` | Set a variable on the script owner |
| `varclear` | Remove a variable |
| `call` | Call another script as a subroutine |

There are over 150 available commands — see the command reference pages for each entity type for the full list.

---

## Comments

Lines that begin with `**` (after optional whitespace) are comments. The game ignores them completely. Use comments to explain what your script does and why.

```
** This script fires when a player enters the room.
** If they are evil-aligned, the paladin attacks.
if isevil $n
  mob say Evil shall not pass!
  mob kill $n
else
  mob say Welcome, traveler.
endif
```

Comments can also appear within color codes for builder-only notes that are visible in the script editor but stripped at runtime:

```
** {WThis is a builder note with color for readability.{X
** {WVariable 'forge' should be set by rprog 3850 before this runs.{X
```

---

## Quick Codes

Quick codes are `$` shortcuts that expand to the names of entities involved in the trigger. They are the simplest way to refer to characters and objects in your script output.

| Code | Who It Refers To | Example Output |
|:-----|:-----------------|:---------------|
| `$n` | The player/character who triggered the script | "Gandalf" |
| `$N` | Same, with capitalization forced | "Gandalf" |
| `$i` | The entity the script is attached to (self) | "the innkeeper" |
| `$I` | Same, capitalized | "The innkeeper" |
| `$t` | A secondary target (victim) | "the goblin" |
| `$p` | A second object involved | "a silver key" |
| `$q` | A target character (used in some token triggers) | "Frodo" |

Quick codes also have pronoun variants (`$e` he/she/it, `$m` him/her/it, `$s` his/her/its). See the [Quick Codes Reference](quick-codes.md) for the complete list.

### Example

```
mob echoat $n $I hands you a steaming cup of tea.
mob echoaround $n $I hands $n a steaming cup of tea.
```

A player named Gandalf would see: "The innkeeper hands you a steaming cup of tea."
Everyone else would see: "The innkeeper hands Gandalf a steaming cup of tea."

---

## Entity References

Entity references use the `$(entity)` syntax to refer to the entities involved in a trigger. They are more readable than quick codes and support field access.

| Expression | Who It Refers To |
|:-----------|:----------------|
| `$(self)` | The entity the script is attached to |
| `$(enactor)` | The character who triggered the script |
| `$(victim)` | A secondary target character |
| `$(victim2)` | A third character (rare) |
| `$(here)` | The room the script runs in |
| `$(obj1)` | The first object involved |
| `$(obj2)` | A second object involved |
| `$(token)` | The token involved (if applicable) |
| `$(random)` | A random character in the room |

### Field Access

Entity references can chain a dot-separated field to pull specific information:

```
** Get the enactor's name, level, current room name
mob echoat $n Your name is $(enactor.name).
mob echoat $n You are level $(enactor.level).
mob echoat $n You are in $(here.name).

** Get inventory contents, area rooms
list items thing $(enactor.inv)
list rooms thisroom $(here.area.rooms)
```

See the [Entity Reference](entity-reference.md) for all available fields.

### Quick Code Equivalents

| Quick Code | Entity Reference |
|:-----------|:-----------------|
| `$n` | `$(enactor)` |
| `$i` | `$(self)` |
| `$t` | `$(victim)` |
| `$o` | `$(obj1)` |
| `$q` | `$(token)` or target character |
| `$r` | `$(random)` |

Both styles work everywhere. Quick codes are shorter; entity references are more readable and support field access.

---

## Variables

Variables let scripts store and retrieve data. You can set variables on the script's own entity or on other entities. Variables persist as long as the entity exists (and optionally across reboots if marked persistent).

### Setting Variables

```
mob varset <name> <type> <value>
```

| Type | What It Stores | Example |
|:-----|:---------------|:--------|
| `number` | An integer | `mob varset count number 0` |
| `string` | Text | `mob varset greeting string Hello there` |
| `room` | A room reference | `mob varset home room 3001` |
| `mobile` | A mobile reference | `mob varset target mobile $(enactor)` |
| `object` | An object reference | `mob varset sword object $(obj1)` |

### Incrementing Numbers

You can increment a numeric variable without knowing its current value:

```
mob varset count inc 1
```

### Reading Variables

Use `$<varname>` to expand a variable's value into a command:

```
mob varset greeting string Welcome to the inn!
mob echoat $n $<greeting>
```

### Variables on Other Entities

Use `varseton` to set a variable on a different entity, and reference it with `$(entity)` plus the variable name:

```
mob varseton $n quest_step number 1
mob varseton $(here) visited number 1
```

Read a variable from another entity using the `varnumber` or `varstring` ifchecks:

```
if varnumber $n quest_step == 1
  mob say I see you've started the quest.
endif
```

### Clearing Variables

```
mob varclear <name>
mob varclearon $n <name>
```

### Persistent Variables

By default, variables disappear when the entity is removed from the game. To make a variable survive reboots:

```
mob persist <varname>
```

See [Variables and Tokens](variables-and-tokens.md) for the full variable system reference.

---

## Variable Expansion

Use `$<varname>` anywhere in a command to insert a variable's value:

```
mob varset dest room 3001
mob transfer $n $<dest>
```

For variables stored on other entities, use the entity reference with a field:

```
** Read a string variable from the token object
mob echoat $n $(token.formatted_list:str)
```

---

## Arithmetic Expressions

Wrap math in `$[...]` to evaluate arithmetic inline. The result is always a number.

### Operators

| Operator | Meaning | Example |
|:---------|:--------|:--------|
| `+` | Add | `$[5 + 3]` → 8 |
| `-` | Subtract | `$[10 - 4]` → 6 |
| `*` | Multiply | `$[3 * 4]` → 12 |
| `/` | Divide | `$[12 / 3]` → 4 |
| `%` | Remainder | `$[10 % 3]` → 1 |
| `:` | Random range | `$[1:10]` → random number 1–10 |
| `()` | Grouping | `$[(2 + 3) * 4]` → 20 |

### Using Variables in Arithmetic

You can reference variables inside arithmetic expressions with `$<varname>`:

```
mob varset count number 5
mob echoat $n The count is $[$<count> + 1].
```

### Function Calls in Arithmetic

Inside `$[...]`, you can call ifcheck-style functions using square brackets `[function args]`. This is how you pull numeric values from entities:

```
** Get an entity's level
mob echoat $n Your level is $[[level $n]].

** Get a token value
mob echoat $n Token value 0 is $[[tokenvalue $n 100 0]].

** Get a vnum
mob echoat $n This room is vnum $[[vnum $(here)]].

** Combine with math
mob varset reward number $[[level $n] * 100]
mob echoat $n You earn $<reward> gold.
```

### Real Example: Counting Items

This script from the travel system builds a destination list and counts entries:

```
mob varset cur_dest number 1
list mydests thisdest $(self.dests:str)
  mob varset max_dests number $[[varnumber max_dests] + 1]
endlist mydests
```

---

## Color Codes

Color codes use the `{X` format where `X` is a letter. Place them in any echo, say, or text output.

| Code | Color | Code | Color |
|:-----|:------|:-----|:------|
| `{R` | Bright red | `{r` | Dark red |
| `{G` | Bright green | `{g` | Dark green |
| `{B` | Bright blue | `{b` | Dark blue |
| `{Y` | Bright yellow | `{y` | Dark yellow |
| `{M` | Bright magenta | `{m` | Dark magenta |
| `{C` | Bright cyan | `{c` | Dark cyan |
| `{W` | Bright white | `{D` | Dark grey |
| `{x` | Reset to default | `{X` | Reset (alternate) |

### Example

```
mob echo {YThe forge glows with intense heat.{x
mob echoat $n {RWarning:{x You are about to enter a dangerous area.
```

Always end colored text with `{x` to reset the color, or the color will bleed into subsequent lines.

---

## Control Flow: if / else / endif

The `if` statement lets your script make decisions. If the condition is true, the commands inside run. If false, they are skipped.

```
if <ifcheck> [arguments...]
  ** commands to run if true
endif
```

### Adding an else Branch

```
if ispc $n
  mob say Hello, adventurer!
else
  mob say Away with you, creature!
endif
```

### Multiple Conditions with elseif

Use `elseif` to test additional conditions without nesting:

```
if level $n > 50
  mob say You are truly powerful.
elseif level $n > 20
  mob say You have some experience.
else
  mob say You look like a beginner.
endif
```

### Combining Conditions: and / or

You can chain conditions on separate lines using `and` and `or`. Each goes on its own line immediately after the `if` or another `and`/`or`:

```
if ispc $n
and level $n > 10
and !isimmort $n
  mob say You qualify for this quest!
endif
```

```
if race $n lich
or race $n vampire
or race $n wraith
  mob say I shall avenge my family!
  mob kill $n
endif
```

### Negation

Prefix an ifcheck with `!` to negate it (check for the opposite):

```
if !hastoken $n 3700
  token give $n 3700
endif
```

### Comparison Operators

For ifchecks that return numeric values, use comparison operators:

| Operator | Meaning |
|:---------|:--------|
| `==` | Equal to |
| `!=` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |

```
if level $n >= 10
  mob say You're experienced enough.
endif

if varnumber $n quest_step == 3
  mob say You're on the final step!
endif
```

### Real Example: Clay Golem Assembly

This script checks whether all required parts are present, then assembles a golem:

```
if objhere 31
and objhere 1400
and objhere 7418
and objhere 151510
and objhere 264108
  mob echo {YSuddenly, the mud rushes forth to claim the golem body parts....{x
  mob purge 'clay golem body part left leg'
  mob purge 'clay golem body part left arm'
  mob purge 'clay golem body part right arm'
  mob purge 'clay golem body part right leg'
  mob purge 'clay golem body part torso'
  mob mload 265605
  mob echo {YThe parts have merged into a golem!{x
endif
```

See the [IFChecks Reference](ifchecks-reference.md) for all available checks.

---

## Control Flow: switch / case / endswitch

A `switch` statement selects one branch from several possible values. It is cleaner than a long chain of `elseif` when you are comparing one value against many options.

```
switch <expression>
case <value>
  ** commands for this value
case <value>
  ** commands for this value
default
  ** commands if no case matched
endswitch
```

### Real Example: Portal Destinations

This script from the astral plane routes a portal to different destinations based on a destination code:

```
switch $[dest]
case 275
  end 11079
case 432590
  end 1611
case 145458
  end 4214
case 13105
  end 265859
default
  end 0
endswitch
```

Comments can appear between cases to annotate them:

```
switch $[dest]
** Plith
case 275
  end 11079
** Dungeon Mystica
case 432590
  end 1611
default
  end 0
endswitch
```

---

## Loops: list / endlist

The `list` loop is the most common loop in Sentience scripting. It iterates over a collection of entities — every item in a container, every mobile in a room, every room in an area, and so on.

```
list <listname> <varname> <collection>
  ** commands — use $<varname> to refer to the current item
endlist <listname>
```

- **listname** — a name you give this loop (used to exit it early).
- **varname** — the variable that holds the current item each time through.
- **collection** — an entity field that produces a list, such as `$(here.mobiles)`, `$(enactor.inv)`, `$(self.contents)`, `$(here.area.rooms)`, or `$(game.persist.objects)`.

### Example: Find an Item in a Container

```
list contents content $(self.contents)
  if !objextra $<content> burnproof
    obj varset burnthis object $<content>
    exitlist contents
  endif
endlist contents
```

### Example: Inventory Display

```
token echoat $(enactor) {W===== Tattoos ====={x
list mytattoos thistattoo $(enactor.inv)
  if objtype $<thistattoo> tattoo
    token echoat $(enactor) {B* {W$(thistattoo:obj.short){x
  endif
endlist mytattoos
```

### Exiting a List Early

Use `exitlist <listname>` to stop iterating and continue with the commands after `endlist`:

```
list mobiles mob $(here.mobiles)
  if vnum $<mob> == 3886
    room varset guardian mobile $<mob>
    exitlist mobiles
  endif
endlist mobiles
```

### Accessing Fields of List Items

When a list variable holds an entity, access its fields with the `:type.field` syntax:

```
list items thing $(enactor.inv)
  mob echoat $n {W$(thing:obj.short){x - vnum $[[vnum $(thing:obj)]]{x
endlist items
```

---

## Loops: for / endfor

The `for` loop repeats a block a fixed number of times, counting from a start value to an end value.

```
for <loopname> <varname> <start> <end> <step>
  ** commands — use $<varname> for the current counter
endfor <loopname>
```

- **loopname** — a name you give this loop (used to exit it early).
- **varname** — the variable that holds the current counter value.
- **start** — the starting number.
- **end** — the ending number (inclusive).
- **step** — how much to add each time (usually `1`).

### Example: Spawn Multiple Mobs

```
mob varset me mobile $(self)
for rats rat 1 8 1
  mob mload 4109 thisrat
  mob force $<thisrat> follow $<me>
endfor rats
mob varclear me
```

### Example: Reset Variables in a Range

```
for variables var 1 5 1
  token varset loaded_774$<var> number 0
endfor variables
```

### Exiting a For Loop Early

Use `exitfor <loopname>`:

```
for tvals tval 0 7 1
  if tokenvalue $(self) 6884 $<tval> == 0
    mob varset needparts number 1
    exitfor tvals
  endif
endfor tvals
```

### Arithmetic in For Bounds

The start and end values can use `$[...]` arithmetic:

```
for guards guard 1 $[1:5] 1
  token mload 11009 thisguard
endfor guards
```

---

## Loops: while / endwhile

The `while` loop repeats as long as a condition remains true. **Use with caution** — always make sure the condition will eventually become false, or you will create an infinite loop.

```
while <ifcheck> [arguments...]
  ** commands
endwhile
```

### Example

```
mob varset count number 0
while varnumber count < 3
  mob echo Counting: $<count>
  mob varset count number $[$<count> + 1]
endwhile
mob varclear count
```

---

## Stopping Script Execution: end

The `end` command immediately stops the current script. You can optionally pass a return value for scripts called with `call`:

```
** Stop with no return value
end

** Stop and return a value (used with the 'call' command)
end 1
end 0
```

### Common Pattern: Early Exit on Invalid State

```
if !ispc $n
  end
endif
** Rest of the script only runs for players
mob say Hello, $n!
```

```
if objval1 $(self) & $[[flagcontainer 'closed']]
  obj echoat $(enactor) You might want to open $I first.
  end 1
endif
```

---

## Calling Other Scripts

The `call` command runs another script as a subroutine. The called script can use `end <value>` to return a value, which you can test with `lastreturn`:

```
mob call 27989 $(enactor)
if lastreturn != 0
  mob say The check passed!
endif
```

This is useful for sharing common logic between multiple scripts without duplicating code.

---

## Script Editors

Every entity type has its own script editor, but they all share identical commands:

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

### Editor Commands

| Command | Syntax | Description |
|:--------|:-------|:------------|
| `create` | `mpedit create <vnum>` | Create a new script at the given vnum |
| `name` | `name <text>` | Set a descriptive name for the script |
| `code` | `code` | Open the string editor — type your script, end with `@` on a blank line |
| `compile` | `compile` | Compile the script and check for errors |
| `flags` | `flags <flag>` | Toggle a script flag |
| `security` | `security <0-9>` | Set the minimum security level to view/edit |
| `depth` | `depth <number>` | Set maximum recursion depth |
| `show` | `show` | Display the script's current state and code |
| `comments` | `comments` | Open builder-only notes (not part of the script code) |
| `list` | `list` | List all scripts of this type in the current area |

---

## Script Flags

Script flags are toggled with the `flags` command inside a script editor.

| Flag | Effect |
|:-----|:-------|
| `secured` | Prevents builders whose security level is lower than the script's security from viewing or editing the script. The script resets runtime security to its own level when it runs. |
| `system` | Marks the script as a core system component. System scripts can only run when security is at system level. Do not modify system scripts unless you are certain of what you are doing. |
| `disabled` | Prevents the script from executing without removing it from its entity. Useful for temporarily turning off a script while debugging. |

---

## Security Levels

Every script has a **security level** from 0 to 9. Security controls two things:

1. **Who can edit the script.** A builder can only view or modify scripts at or below their own security level.
2. **What the script can do.** Some commands are **restricted** — they require a minimum security level to execute. This prevents low-security scripts from doing dangerous things like transferring players, dealing damage, or loading items.

Set security in the editor:

```
security 5
```

A security of 0 means anyone can edit it. A security of 9 means only the highest-level builders can touch it. When a `secured` script runs, it resets the runtime security to its own level, preventing called scripts from exceeding it.

---

## The Scripting Workflow

Putting it all together, here is the typical workflow for creating a script:

**1. Create the script** in the appropriate editor:

```
mpedit create 500
name guard_greet
```

**2. Write the code:**

```
code
** Greet players who enter the room
if ispc $n
  mob say Halt! State your business in the city.
  if isevil $n
    mob say I have my eye on you...
  endif
endif
@
```

**3. Compile** to check for errors:

```
compile
```

**4. Attach the script** to its entity using the `addmprog` command from the entity editor:

```
done
medit 1200
addmprog 500 greet 100
done
```

The `100` means a 100% chance to fire. For a `speech` trigger, you would put the matching phrase instead.

**5. Test** in-game by walking into the room or performing the triggering action.

---

## Widevnums in Scripts

Sentience uses **widevnums** to reference entities across different areas. The format is:

```
area_uid#vnum
```

For example, `2#500` means vnum 500 in area UID 2. When referencing entities in the **same** area, you can use the plain vnum. Commands like `mload`, `oload`, `transfer`, and `call` all accept widevnums.

```
** Load a mob from another area
mob mload 2#500

** Load a mob from the same area (plain vnum)
mob mload 500
```

---

## Attaching Scripts (Triggers)

After creating a script, you attach it to an entity with an `add*prog` command from within that entity's OLC editor:

```
addmprog <script_vnum> <trigger> <phrase|percentage>
addoprog <script_vnum> <trigger> <phrase|percentage>
addrprog <script_vnum> <trigger> <phrase|percentage>
addtprog <script_vnum> <trigger> <phrase|percentage>
addaprog <script_vnum> <trigger> <phrase|percentage>
```

- **script_vnum** — the vnum of the script you created.
- **trigger** — the event that fires the script (e.g. `greet`, `speech`, `random`, `death`, `give`, `entry`).
- **phrase|percentage** — for `speech` triggers, the word or phrase to match. For most other triggers, a percentage chance (0–100) to fire.

### Example

```
medit 200
addmprog 500 greet 100
addmprog 501 speech hello
done
```

---

## Compilation and Error Handling

Always compile after writing or editing code. Type `compile` in the script editor.

The compiler checks for:

- **Unknown commands** — typos in command names
- **Unmatched blocks** — an `if` without `endif`, a `list` without `endlist`, etc.
- **Unknown ifchecks** — typos in `if` conditions
- **Wrong argument counts** — missing required arguments

The compiler reports errors with a line number and a description:

```
Line 5: Unknown ifcheck 'lvel' — did you mean 'level'?
Line 12: Unmatched 'if' — missing 'endif'
```

{: .warning }
A script with compilation errors **will not run**. Always fix all errors before testing in-game.

---

## Quick Reference

| Concept | Syntax | More Info |
|:--------|:-------|:----------|
| Comment | `** this is a comment` | — |
| Entity prefix | `mob`, `obj`, `room`, `token`, `area`, `instance`, `dungeon`, `quest`, `event` | — |
| Quick codes | `$n`, `$i`, `$t`, `$p`, `$q` | [Quick Codes](quick-codes.md) |
| Entity references | `$(enactor)`, `$(self)`, `$(here)` | [Entity Reference](entity-reference.md) |
| Field access | `$(enactor.name)`, `$(here.area.rooms)` | [Entity Reference](entity-reference.md) |
| Variable set | `mob varset name number 0` | [Variables](variables-and-tokens.md) |
| Variable read | `$<varname>` | [Variables](variables-and-tokens.md) |
| Arithmetic | `$[2 + 3]`, `$[1:10]` | — |
| Function in arithmetic | `$[[level $n]]`, `$[[vnum $(here)]]` | — |
| If/else | `if` ... `else` ... `endif` | [IFChecks Reference](ifchecks-reference.md) |
| Elseif | `if` ... `elseif` ... `endif` | [IFChecks Reference](ifchecks-reference.md) |
| And/or | `if X` / `and Y` / `or Z` | — |
| List loop | `list name var collection` ... `endlist name` | — |
| For loop | `for name var start end step` ... `endfor name` | — |
| While loop | `while check args` ... `endwhile` | — |
| Switch | `switch expr` / `case val` / `default` / `endswitch` | — |
| Early exit | `exitlist name`, `exitfor name`, `end`, `end <value>` | — |
| Color | `{R` red, `{Y` yellow, `{x` reset | — |
