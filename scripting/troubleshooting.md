---
layout: default
title: Troubleshooting
nav_order: 12
parent: Scripting
---

# Scripting Troubleshooting
{: .no_toc }

Problem → solution reference for common scripting issues. Covers compilation
errors, runtime problems, debugging techniques, and performance pitfalls.

<details open markdown="block">
  <summary>Table of Contents</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## "My Script Isn't Firing" Checklist

Work through this list top to bottom. Most silent failures are one of these:

| # | Check | How to Verify |
|---|-------|---------------|
| 1 | **Is the script attached to the entity?** | `stat` the mob/obj/room and look for the program listing. |
| 2 | **Is it the correct trigger type?** | A `speech` trigger won't fire on `say hello` if the phrase is set to `greet`. Double-check trigger vs. phrase. |
| 3 | **Is the phrase correct?** | `speech hello` only matches if the player says something containing "hello". Partial matches work; exact-only does not. |
| 4 | **Is the percentage high enough?** | `greet 50` only fires 50 % of the time. Set to `100` while testing. |
| 5 | **Does the script compile?** | Open the script in the editor — if it has compile errors it won't run. Fix errors and re-compile. |
| 6 | **Is the script disabled?** | Check the script's flags for a `disabled` or `inactive` flag. |
| 7 | **Is the entity loaded in the world?** | An area that hasn't been reset won't have its mobs loaded. Force a reset with `reset <area>`. |
| 8 | **Is the security level sufficient?** | Scripts using restricted commands (e.g., `mload`, `transfer`, `damage`) need `security >= 0`. Check with `spedit` → `security`. |
| 9 | **Are you testing as a mortal?** | Many triggers ignore immortal characters by default. Use a mortal alt or `wizinvis off`. |
| 10 | **Is the enactor the right type?** | A `greet` trigger fires for characters entering the room. NPCs entering will also trigger it — make sure your `ispc $n` guard isn't filtering out the test case you expect. |

---

## Common Compilation Errors

### `Unknown command: <name>`

**Cause:** Typo in a command name, or using the wrong entity prefix.

```
** Wrong — "obj" prefix on a mob program
obj echo Hello!

** Right — use "mob" on mob programs
mob echo Hello!
```

**Fix:** Match the prefix to the entity type the script is attached to. See
[Command Availability Matrix](command-availability.md).

---

### `Exceeded nested if depth`

**Cause:** More than 24 levels of nested `if` blocks.

```
if ispc $n
  if level $n > 10
    if carries $n sword
      if fighting $n
        ** ... 20 more nesting levels ...
```

**Fix:** Flatten logic with early `end` exits:

```
if !ispc $n
  end
endif
if level $n <= 10
  end
endif
** Remaining logic at top level
```

---

### `Exceeded nested while depth`

**Cause:** More than 20 levels of nested `while` loops. This almost always
means unintended nesting.

**Fix:** Restructure to avoid deeply nested loops. Use `list`/`endlist` for
collection iteration instead.

---

### `Missing endif`

**Cause:** An `if` block was opened but never closed.

**Fix:** Count your `if`/`endif` pairs. Every `if` needs exactly one `endif`.
Indent consistently to make nesting visible:

```
if ispc $n
  if level $n > 5
    mob say You qualify.
  endif
endif
```

---

### `Missing endwhile` / `Missing endlist`

**Cause:** Same as above — an unclosed loop block.

**Fix:** Ensure every `while` has an `endwhile` and every `list` has an
`endlist <name>`.

---

### `Unknown ifcheck: <name>`

**Cause:** The condition name doesn't exist or is misspelled.

```
** Wrong
if isPlayer $n

** Right
if ispc $n
```

**Fix:** Check the [Ifchecks Reference](ifchecks-reference.md) for the correct
name. Ifcheck names are case-sensitive.

---

## Common Runtime Errors

### `Security violation`

**Meaning:** The script tried to run a command that requires a higher security
level than the script has.

| Command | Minimum Security |
|---------|:----------------:|
| Most restricted commands (`mload`, `transfer`, `damage`, etc.) | 0 |
| `xcall` | 5 |
| System commands | 10 |

**Fix:** Open the script in `spedit` and set `security <level>` to the
required minimum. Ask an implementor if you don't have permission to change
security levels.

---

### `Call depth exceeded`

**Meaning:** A script called another script (`call` or `xcall`) more than 15
levels deep. This is usually infinite recursion.

**Example of accidental recursion:**

```
** Script A calls Script B
mob call 50001

** Script B calls Script A (infinite loop!)
mob call 50000
```

**Fix:** Trace the `call`/`xcall` chain and break the cycle. Add a depth guard
variable if mutual calls are intentional:

```
if varnumber $i call_guard > 0
  end
endif
mob varset call_guard integer 1
mob call 50001
mob varclear call_guard
```

---

### `Entity reference is null`

**Meaning:** A quick code (`$n`, `$t`, `$o`, etc.) refers to an entity that
doesn't exist in the current context.

**Common cause:** Using `$t` (victim) in a trigger that doesn't provide a
victim — e.g., `greet` triggers have `$n` (enactor) but not `$t`.

**Fix:** Check which entities your trigger provides. See
[Entity Reference](entity-reference.md) for trigger → entity mappings.

---

### `Variable not found: $<varname>`

**Meaning:** `$<varname>` was used but the variable was never set or has been
cleared.

**Fix:** Always check `vardefined` before using a variable that might not
exist:

```
if vardefined $i quest_stage
  if varnumber quest_stage == 3
    mob say Stage 3 in progress.
  endif
else
  mob say You haven't started the quest.
endif
```

---

### Economy command returns 0 (failure)

**Meaning:** `chargebank`, `chargemoney`, or `wiretransfer` couldn't complete
the transaction — usually because the target lacks sufficient funds.

**Fix:** Always check funds before charging, and check `lastreturn` after:

```
if bank $n >= 500
  mob chargebank $n 500
  if lastreturn == 1
    ** Success
  else
    ** Unexpected failure
  endif
else
  mob say You need at least 500 gold in the bank.
endif
```

---

## Debugging Techniques

### Echo Breadcrumbs

The simplest debugging tool: add `echoat` lines that print to **you only** so
you can trace execution flow.

```
mob echoat $n {D[DEBUG] Entering greet script{x
if ispc $n
  mob echoat $n {D[DEBUG] ispc passed{x
  if level $n > 10
    mob echoat $n {D[DEBUG] level check passed: $(enactor.level){x
  else
    mob echoat $n {D[DEBUG] level check FAILED: $(enactor.level){x
  endif
endif
```

Use `{D...{x` (dark grey) to visually distinguish debug output. Remove
breadcrumbs before going live.

### Print Variable Values

Dump variable state to confirm values:

```
mob echoat $n {D[DEBUG] stage=$<stage> kills=$<kills> target=$<target>{x
mob echoat $n {D[DEBUG] enactor.level=$(enactor.level) enactor.gold=$(enactor.gold){x
```

### Wiznet Script Tracing

Immortals can monitor script activity in real time:

```
wiznet script         ** Toggle script debugging channel
```

This shows which scripts fire, on which entities, and any error messages. Use
it to confirm your trigger is (or isn't) firing.

### Check Compilation Inline

Open the script in `spedit <vnum>` and look for error messages at the top. The
editor reports compile errors immediately. If the script won't compile, it
cannot run.

### Isolate With a Test Mob

1. Create a throwaway mob: `medit create`
2. Attach only the script you're debugging
3. Test in a private room
4. Delete when done

This eliminates interference from other scripts on the same entity.

---

## Performance Guidance

### Avoid Expensive Operations in Random Triggers

`random` triggers fire every tick for every entity that has one. Heavy
operations here multiply quickly.

**Bad:**
```
** random 50 — fires often, iterates all mobs every time
list mobs mob $(here.area.rooms)
  ** expensive iteration over every room in the area
endlist mobs
```

**Better:** Use a lower random chance and limit the work:

```
** random 5 — fires rarely
if rand 20
  mob emote does something interesting.
endif
```

### Loop Limits

Always ensure `while` loops will terminate:

```
** DANGEROUS — if target is never found, loops forever
mob varset count integer 0
while !mobhere 5000
  mob varset count integer $[$<count> + 1]
  ** No exit condition!
endwhile
```

**Fix:** Add a safety counter:

```
mob varset count integer 0
while !mobhere 5000
  mob varset count integer $[$<count> + 1]
  if varnumber count > 20
    ** Safety exit
    end
  endif
endwhile
```

### Use `list`/`endlist` Over Manual Loops

`list` handles iteration limits internally and is optimised for entity
collections. Prefer it over manual `while` counting:

```
** Preferred
list items item $(here.objects)
  if objtype $<item> sword
    mob echo Found sword.
    exitlist items
  endif
endlist items
```

### Minimise `gecho` and `zecho`

These broadcast to every player in the game (or zone). Use sparingly — once
per event, not per tick.

### Avoid `scriptwait` in Frequent Triggers

`scriptwait` blocks the entity's script engine for the specified duration. On a
`random` or `fight` trigger, this stalls processing.

**Use `delay` or `queue` instead** — they're non-blocking:

```
** Non-blocking alternative
mob echo Phase 1!
mob queue 8 mob echo Phase 2!
mob queue 16 mob echo Phase 3!
```

---

## Script Depth and Call Limits

| Limit | Maximum | What Happens |
|-------|:-------:|-------------|
| `call` / `xcall` depth | **15** | Further calls silently fail |
| Nested `if` depth | **24** | Compile error |
| Nested `while` loops | **20** | Compile error |

### Tips

- Keep `call` chains shallow. If you need more than 3–4 levels, reconsider
  your design.
- Flatten `if` nesting with early `end` returns (see compilation errors above).
- `xcall` counts toward the same depth limit as `call`.

---

## Security Levels Explained

Scripts have a security level that controls which commands they can use.

| Level | Who Can Set It | What It Unlocks |
|-------|---------------|-----------------|
| **-2** (default) | — | Unrestricted commands only (echo, say, emote) |
| **0** | Builders | Most commands: `mload`, `oload`, `transfer`, `damage`, `purge`, etc. |
| **1–4** | Senior builders | Same as 0 with higher trust |
| **5** | Implementors | `xcall` (cross-entity script calling) |
| **10** | System only | Internal system scripts — not for builders |

**To set security:** `spedit <vnum>` → `security <level>`

If your script uses any entity-modifying command and security is still at -2,
those commands will silently fail. **Always set security to at least 0 for
functional scripts.**

---

## Variable Scope Gotchas

### Local vs. Entity vs. Persistent

| Scope | Set With | Lifetime | Survives Reboot? |
|-------|----------|----------|:----------------:|
| Entity (self) | `varset` | Until entity is removed | No |
| Entity (other) | `varseton` | Until entity is removed | No |
| Persistent (self) | `varset` + `varsave` | Permanent | Yes |
| Persistent (other) | `varseton` + `varsaveon` | Permanent | Yes |

### Common Mistakes

**Forgetting `varsave`:**

```
** Variable is lost on reboot!
mob varset quest_stage integer 1

** Persistent — survives reboots
mob varset quest_stage integer 1
mob varsave quest_stage
```

**Setting on self when you meant the player:**

```
** Wrong — sets on the mob (resets when mob repops)
mob varset quest_done bool true

** Right — sets on the player
mob varseton $(enactor) quest_done bool true
mob varsaveon $(enactor) quest_done
```

**Reading a variable before it's set:**

```
** Crashes or returns empty if "kills" doesn't exist yet
if varnumber kills > 10

** Safe version
if vardefined $i kills
  if varnumber kills > 10
    ** ...
  endif
endif
```

**Variable name collisions:** If two scripts on the same mob both use a
variable called `stage`, they'll overwrite each other. Use descriptive,
prefixed names:

```
** Bad — generic name
mob varset stage integer 1

** Good — unique to this quest
mob varset wolf_quest_stage integer 1
```

See [Variables & Tokens](variables-and-tokens.md) for the full scoping guide.

---

## Common Mistakes

### Forgetting `endif`

**Symptom:** Compile error or script doesn't behave as expected.

```
** Wrong
if ispc $n
  mob say Hello!
** Missing endif!
```

**Fix:** Every `if` needs an `endif`. Indent your code to make nesting obvious.

---

### Wrong Entity Prefix

**Symptom:** `Unknown command` compile error.

```
** Wrong — "obj" commands in a mob program
obj echo Hello!

** Right
mob echo Hello!
```

The prefix **must** match the entity type the script is attached to:
- Mob script → `mob`
- Object script → `obj`
- Room script → `room`
- Token script → `token`
- Area script → `area`

---

### Case Sensitivity in Ifchecks

Ifcheck names are case-sensitive. Command names are not.

```
** Wrong — capital P
if isPC $n

** Right
if ispc $n
```

---

### Using `$t` When No Victim Exists

Not all triggers provide a victim entity. Using `$t` in a `greet` trigger
(which only provides `$n`) produces a null reference.

**Check the trigger's entity bindings** in
[Entity Reference](entity-reference.md) before using `$t`, `$o`, or `$p`.

---

### Infinite Loops

```
** This loops forever because count is never updated
while varnumber count < 5
  mob echo Loop!
endwhile
```

**Fix:** Always update the loop variable:

```
mob varset count integer 0
while varnumber count < 5
  mob echo Loop $<count>!
  mob varset count integer $[$<count> + 1]
endwhile
```

---

### Forgetting `ispc` Guards

Without an `ispc` check, scripts fire for NPCs too — including mobs wandering
into the room, summoned creatures, or script-loaded mobs. This can cause
chain-reaction triggers.

```
** Always guard player-specific logic
if !ispc $n
  end
endif
```

---

### Dollar Sign in Output

A literal `$` in echo/say text is interpreted as a quick code. To print a
literal dollar sign, use `$$`:

```
** Wrong — tries to expand "$5"
mob say The fee is $5.

** Right
mob say The fee is $$5.
```

---

### `end` vs. `endif`

`end` terminates the **entire script**. `endif` closes an **if block**. Don't
confuse them:

```
** This stops the WHOLE script — nothing after runs
if !ispc $n
  end
endif
mob say This line runs for players.

** vs. closing just the if block
if ispc $n
  mob say Hello player!
endif
mob say This line runs for EVERYONE.
```

---

## Quick Reference: Error → Fix

| Error / Symptom | Likely Cause | Fix |
|-----------------|-------------|-----|
| Script never fires | Not attached, wrong trigger, or disabled | See [checklist above](#my-script-isnt-firing-checklist) |
| `Unknown command` | Wrong entity prefix or typo | Match prefix to entity type |
| `Unknown ifcheck` | Misspelled or wrong case | Check [Ifchecks Reference](ifchecks-reference.md) |
| `Missing endif` | Unclosed `if` block | Add matching `endif` |
| `Exceeded nested if depth` | Too many nested ifs | Flatten with early `end` |
| `Security violation` | Security level too low | `spedit` → `security 0` (or higher) |
| `Call depth exceeded` | Infinite `call`/`xcall` recursion | Break the cycle, add guards |
| `Entity reference is null` | Using `$t`/`$o` in wrong trigger | Check trigger entity bindings |
| `Variable not found` | Variable not set or cleared | Check `vardefined` first |
| Commands silently fail | Security at default (-2) | Set `security 0` |
| Mob greets other mobs | Missing `ispc $n` guard | Add `ispc` check |
| `$$` appears in output | Wanted literal `$` | Already correct — use `$$` |
| Charge command returns 0 | Insufficient funds | Check balance before charging |

---

## See Also

- [Scripting Basics](scripting-basics.md) — Language fundamentals
- [Command Availability Matrix](command-availability.md) — Which commands work where
- [Entity Reference](entity-reference.md) — Trigger → entity mappings
- [Ifchecks Reference](ifchecks-reference.md) — All conditional checks
- [Variables & Tokens](variables-and-tokens.md) — Scoping and persistence
- [Advanced Scripting](advanced-scripting.md) — Deep dives
- [Cookbook](cookbook.md) — Copy-paste recipe patterns
