---
layout: default
title: Advanced Patterns
nav_order: 6
parent: Tutorials
grand_parent: Scripting
---

# Advanced Patterns
{: .no_toc }

By now you can write scripts that react to triggers, track state with variables, build quests, and script boss fights. This tutorial covers the patterns that tie complex systems together — asynchronous execution, sub-script architecture, cross-entity communication, dynamic text generation, remote commands, and performance considerations. These are the techniques that separate a collection of individual scripts from a cohesive, well-engineered system.

This tutorial assumes familiarity with [variables](working-with-variables.md), [combat scripting](combat-scripting.md), the [Shared Commands Reference](../shared-commands.md), and the [Advanced Scripting](../advanced-scripting.md) reference.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Delay and Async Execution

Scripts normally execute all their lines in a single instant — from the game's perspective, everything between the trigger firing and the script ending happens in one tick. Async tools let you spread actions over time, creating sequences that unfold across multiple ticks.

### delay: One-Shot Timer

The `delay` command sets a timer (in **pulses**, where 4 pulses ≈ 1 second). When the timer expires, the entity's **delay trigger** fires. Lines after the `delay` command in the current script **do not execute** — the script ends immediately at the `delay` line.

```
** A guard notices something suspicious — react after a pause
** Trigger: speech — Phrase: password

mob echo {D$I narrows $l eyes, studying you carefully.{x
mob delay 8

** ⚠️ Nothing below this line runs! Put follow-up in the delay trigger.
```

```
** The follow-up — what happens 2 seconds later
** Trigger: delay — Phrase: 100

if isnpc $n
  break
endif

say ...the password is accepted. You may pass.
mob echo {D$I steps aside, revealing a hidden passage.{x
```

**Key rules for delay:**

1. Only **one** delay can be pending per entity at a time — setting a new delay cancels the previous one.
2. The delay trigger receives the same `$n` (enactor) context that was active when `delay` was called.
3. Between the delay and the trigger, other scripts can fire normally — the entity is not frozen.

### cancel: Aborting a Pending Delay

If conditions change and you no longer want the delay to fire:

```
** Player flees — cancel whatever the boss was charging
** Trigger: exit — Phrase: 100

if isdelay $i
  mob cancel
  mob echo {D$I lowers $l arms. The gathering energy dissipates.{x
endif
```

`cancel` removes the pending delay silently. The delay trigger simply never fires.

### queue / dequeue: Multiple Timed Commands

While `delay` gives you a single timer, `queue` lets you stack multiple commands at different delay points. Each queued command runs independently at its scheduled time.

```
** A blacksmith forging sequence — triggered when player gives an item
** Trigger: give — Phrase: 100

** Prevent overlapping sequences
if hasqueue $i
  say Patience! I am still working on the last one.
  break
endif

mob echo {D$I examines the materials with a practiced eye.{x
mob queue 2 mob echo {Y$I places the metal on the anvil and raises $l hammer.{x
mob queue 3 mob echo {R*CLANG!* Sparks fly as $I strikes the hot metal!{x
mob queue 5 mob echo {R*CLANG!* *CLANG!* The shape begins to take form.{x
mob queue 7 mob echo {Y$I quenches the blade in a barrel of water. Steam hisses.{x
mob queue 8 say It is done. A fine blade, if I say so myself.
mob queue 8 mob oload 90100
mob queue 8 give blade $n
```

**queue vs delay — when to use which:**

| Feature | `delay` | `queue` |
|:--------|:--------|:--------|
| Number of timers | One at a time | Unlimited stacking |
| Follow-up logic | Separate delay trigger script | Inline commands |
| Complex branching on fire | Yes (full script in trigger) | No (just a command string) |
| Cancel mechanism | `cancel` | `dequeue` (cancels all) |
| Overlap prevention | `if isdelay $i` | `if hasqueue $i` |

{: .note }
> `queue` delays are in **seconds** (not pulses). `mob queue 2 say hello` fires 2 seconds later. This differs from `delay`, which uses pulses (4 pulses ≈ 1 second).

### dequeue: Clearing Queued Commands

`dequeue` removes **all** pending queued commands. Use it for cleanup — when a mob dies, is purged, or circumstances change:

```
** If combat starts while the blacksmith is forging, cancel the sequence
** Trigger: startcombat — Phrase: 100

if hasqueue $i
  mob dequeue
  mob echo {D$I drops the half-finished blade and draws $l sword!{x
endif
say How dare you disturb my forge!
```

### scriptwait: In-Line Pause

`scriptwait` pauses script execution without yielding — the script literally stops for a duration, then continues from the next line. Unlike `delay`, subsequent lines **do** execute.

```
** Dramatic reveal — speech trigger
mob echo {DThe ancient door begins to creak open...{x
mob scriptwait 2
mob echo {YLight floods in from the chamber beyond!{x
mob scriptwait 1
mob echo {WYou see a vast treasure hoard stretching into the distance!{x
```

{: .warning }
> `scriptwait` blocks the entity's script processing for the duration. During a scriptwait, no other scripts can fire on that entity. Use it sparingly — for short dramatic pauses only. For anything longer than a few seconds, use `delay` or `queue` instead. Available on mob, obj, and token scripts only.

---

## Sub-Script Calls: call and xcall

As your scripts grow more complex, you will find yourself repeating the same logic in multiple triggers. Sub-scripts let you extract shared logic into reusable scripts that can be called from anywhere.

### call: Same-Entity Sub-Script

`call` executes another script in the context of the **same entity**. The called script runs immediately, and when it returns, the calling script continues from the next line.

```
<prefix> call <script_vnum> [enactor] [victim] [obj1] [obj2]
```

**Example: A merchant with complex greeting logic used by multiple triggers**

The greeting script (vnum 90200) — called by other scripts:

```
** Shared greeting logic — called as a sub-script
** Decides how to greet based on player reputation

if varnumber $n reputation > 100
  say Ah, my most valued customer! What can I do for you today?
  bow $n
elseif varnumber $n reputation > 50
  say Welcome back, $n. Good to see you again.
elseif varnumber $n reputation < -50
  say You again? Make it quick.
  mob echoat $n {D$I eyes you with barely concealed contempt.{x
else
  say Welcome, traveller. Browse my wares.
endif
```

Called from a greet trigger:

```
** Trigger: greet — Phrase: 100
if isnpc $n
  break
endif
mob call 90200 $n
```

Called from a speech trigger:

```
** Trigger: speech — Phrase: hello
mob call 90200 $n
say So, what are you looking for?
```

The greeting logic exists in one place. Change it once, and both triggers use the updated version.

### Passing Context to Sub-Scripts

The optional parameters after the vnum set the context variables in the called script:

| Parameter | Sets | In Called Script |
|:----------|:-----|:-----------------|
| enactor | `$n` | The character who triggered the script |
| victim | `$t` | A secondary target |
| obj1 | `$o` | First object parameter |
| obj2 | `$p` | Second object parameter |

Use `NULL` for parameters you do not need:

```
** Pass the enactor but no victim or objects
mob call 90200 $n NULL NULL
```

### Return Values with lastreturn

A called script can return a value using `end <number>`. The caller reads it with the `lastreturn` ifcheck:

```
** Called script: Check if the player qualifies (vnum 90300)
if level $n < 20
  end 0
endif
if !hastoken $n 90001
  end 0
endif
end 1
```

```
** Calling script: Use the result
mob call 90300 $n

if lastreturn == 1
  say You are worthy. Enter the inner sanctum.
  mob alterexit north flags remove locked
else
  say You are not yet ready for what lies beyond.
endif
```

Return values are integers. By convention, `0` means failure/false and non-zero means success/true — but you can use any integer value.

### xcall: Cross-Entity Sub-Script

`xcall` executes a script in the context of a **different entity** — another mob, an object, a room, or a token. This is powerful but requires script security level 5 or higher.

```
<prefix> xcall <target_entity> <script_vnum> [enactor] [victim] [obj1] [obj2]
```

**When to use xcall over call:**

- **call**: The logic belongs to *your* entity and uses your entity's variables, position, and context
- **xcall**: The logic belongs to the *other* entity — you want the script to run as if the target entity's own trigger fired

**Example: A ritual altar that activates a token on the player**

The altar (an object) calls a script on the player's token:

```
** Object script on the altar — activated by a "use" command
** Trigger: speech — Phrase: activate

** Find the player's ritual token
if !hastoken $n 90500
  obj echoat $n {DNothing happens. You lack the ritual focus.{x
  break
endif

** Pass data to the token, then call its activation script
obj varset tok token $n 90500
obj varseton $<tok> altar object $(self)
obj xcall $<tok> 90501 $n

** Check what the token's script returned
if lastreturn > 0
  obj echo {WThe altar pulses with energy as the ritual succeeds!{x
  obj queue $[[lastreturn]] obj call 90502
else
  obj echo {DThe altar's glow fades. The ritual failed.{x
endif
```

The token's activation script (vnum 90501) runs in the token's context and can access the altar reference through the variable that was set on it:

```
** Token script — runs in the token's context
** $<altar> was set by the calling object script via varseton

if tokenvalue $(self) 0 < 3
  ** Not enough charges
  end 0
endif

token echoat $(self.owner) {MYour ritual focus glows as power flows through it!{x
token adjust $(self) val0 -1
end 4
```

{: .note }
> `xcall` requires script security ≥ 5. Set this in the script editor: `spedit <vnum>` then `security 5`. Without sufficient security, the xcall silently fails.

### Call Depth Limits

To prevent infinite recursion, the engine enforces hard limits:

| Limit | Value |
|:------|:------|
| Maximum nested `call` / `xcall` depth | 15 |
| Maximum nested `if` depth | 24 |
| Maximum nested `while` loops | 20 |

If you hit the call depth limit, further calls silently fail. Design your sub-script architecture to stay well within these limits — a call depth of 3–4 is typical for well-structured scripts.

---

## Cross-Entity Communication

Scripts on different entities often need to coordinate. There are four patterns for this, each with different trade-offs.

### Pattern 1: Variable Sharing with varseton

The simplest form of communication — one entity sets a variable on another:

```
** Room script sets a flag that mob scripts can read
room varseton guard alarm_active bool true
```

```
** Guard's random trigger checks the flag
** Trigger: random — Phrase: 10

if varbool alarm_active
  mob echo {R$I shouts: Intruder alert! All guards to your posts!{x
  mob varclear alarm_active
endif
```

**varseton works on any entity you can reference:**

```
** Set a variable on a player
mob varseton $n quest_progress integer 3

** Set a variable on an object in the room
mob varseton sword enchant_level integer 5

** Set a variable on a specific mob by variable reference
mob varseton $<guard_ref> alert_target mobile $n
```

### Pattern 2: Token Passing

Tokens are invisible objects that carry variables, timers, and scripts. Give a token to a player, object, or room to bundle data and behavior together:

```
** Give the player a "burning" debuff token
mob givetoken $n 90600

** Set severity on the token
mob varseton $n.token.90600 severity integer 3
```

The token's own timer script handles the ongoing effect:

```
** Token timer script — fires when the token's timer counts down to zero
** Trigger: timer — Phrase: 100

token echoat $(self.owner) {RThe flames searing your body intensify!{x
token damage $(self.owner) $[$<severity> * 10] $[$<severity> * 20]

** Reduce severity over time
token varset severity integer $[$<severity> - 1]
if $<severity> <= 0
  token echoat $(self.owner) {YThe flames finally die out.{x
  token junk self
else
  ** Reset timer for the next tick (token timers are in game ticks)
  token settimer $(self) 2
endif
```

**When tokens beat variables:**

- When the data needs its own behavior (timer scripts, triggers)
- When you need automatic cleanup (tokens can be purged on death, quit, or reboot via flags)
- When multiple instances of the same effect should stack or coexist

### Pattern 3: Cross-Entity Calls with Data

Combine `varseton` with `xcall` for a request-response pattern:

```
** Controller mob sends a command to a worker mob
mob varseton $<worker_ref> task string patrol_north
mob varseton $<worker_ref> task_source mobile $i
mob xcall $<worker_ref> 90700 $n
if lastreturn == 1
  say The guard has been dispatched.
else
  say The guard is unavailable.
endif
```

The worker receives the data through the variables set on it:

```
** Worker mob's task handler (vnum 90700)
if varstr task == patrol_north
  mob echo $I salutes and heads north.
  north
  mob varclear task
  mob varclear task_source
  end 1
endif
end 0
```

### Pattern 4: Room Variables as Shared State

Rooms are always present and accessible to all entities in them. This makes room variables natural shared state:

```
** Lever object sets a room variable when pulled
** Trigger: speech — Phrase: pull lever

obj varseton $(here) lever_pulled bool true
obj echo {YThe lever clicks into place. You hear grinding stone.{x
```

```
** Door in the room checks the variable
** Trigger: random — Phrase: 5

if varbool $(here.lever_pulled)
  room echo The stone door slowly slides open!
  room alterexit north flags remove closed
  room alterexit north flags remove locked
  room varclear lever_pulled
endif
```

{: .note }
> `$(here)` refers to the room the script's entity is currently in. This is a convenient way for objects, mobs, and tokens to reference their environment without knowing the room vnum.

---

## Random String Generators (RSG)

RSG variables generate random text from predefined generators. Each time you read the variable, it produces a **new** random result. This is perfect for NPCs that feel alive — different greetings, varied combat taunts, randomized names for spawned mobs.

### Setting an RSG Variable

```
mob varset <name> rsg yes <generator>:<pattern|class>:<entry>
```

- **generator**: The RSG data file name (defined by area builders or system)
- **pattern** or **class**: Whether to use a pattern template or a word class
- **entry**: The specific pattern or class name within the generator

### Example: Varied NPC Dialogue

```
** Set up a random insult generator on the mob
mob varset taunt rsg yes combat_taunts:pattern:melee_taunt

** Each time we read $<taunt>, we get a different result
mob say $<taunt>
```

If the `combat_taunts` generator has patterns like "You fight like a {animal}!" and "Is that the best you can do, {insult}?", each expansion of `$<taunt>` produces a fresh random combination.

### Example: Random Names for Spawned Mobs

```
** Spawn a minion with a random name
mob mload 90010
mob varset minion_name rsg yes sith_names:pattern:full_name

** Set the spawned mob's short description to the random name
mob stringmob 'a shadowy minion' short $<minion_name>
mob stringmob 'a shadowy minion' name $<minion_name> minion shadow
```

### Embedding RSG in Entity Fields

RSG variables can be embedded in entity string fields set through `stringmob`, `stringobj`, extra descriptions, and more. Each time the field is displayed, the RSG generates a new value — making the entity's description subtly different each time someone looks at it.

{: .note }
> RSG generators are data files that must be created separately. The variable system only references generators that already exist. Ask your lead builder or check existing areas for available generator names.

---

## The at Command: Remote Actions

The `at` command temporarily shifts an entity's context to a different room, executes a command, and then shifts back. This is useful for two purposes: performing actions in remote rooms, and hiding spellcasting from players.

### Syntax

```
<prefix> at <room_vnum> <command>
```

### Hiding Spellcasting

When a mob casts a spell, the game shows "X utters the words..." to the room. If you want a boss to heal itself without that message, cast from a void room:

```
** Heal the boss without showing cast messages
mob at 1 mob cast 'cure critical'
```

Room 1 is conventionally a void/limbo room with no players. The mob teleports there, casts, and returns — players only see whatever custom echo you provide.

### Remote Room Manipulation

```
** A wizard mob seals the exits in a distant room
mob at 5020 mob alterexit north flags add closed
mob at 5020 mob alterexit north flags add locked
mob echo {M$I waves $l hand and you hear a distant rumble.{x
say The northern passage is now sealed. None shall escape.
```

### Remote Echo Messages

```
** Announce something to a room the mob is not in
mob at 5020 mob echo {RThe ground shakes as a tremor ripples through the cavern!{x
```

### Practical Example: A King Banishing an Intruder

```
** The king banishes someone to the dungeon from anywhere
mob transfer $n 3050
mob at 3050 mob echo {D$N appears in a flash of light, thrown here by royal decree!{x
mob at 3050 mob echoat $n {DYou find yourself in a cold, dark cell.{x
say Guards! See that the prisoner does not escape.
```

{: .warning }
> `at` changes the entity's room context for one command only. If you need multiple commands in a remote room, use `at` for each one. Do not assume the entity stays in the remote room.

---

## Performance Tips

Scripts run within the game's main loop. Poorly written scripts can cause lag for all players. These guidelines help you write scripts that behave well under load.

### Avoid Heavy Work in random Triggers

The `random` trigger fires every tick for every mob that has it, regardless of whether anything interesting is happening. Keep random-triggered scripts lightweight:

```
** ✅ Good: Quick check, early exit
** Trigger: random — Phrase: 3

if !inroom $i == 5000
  break
endif
say I grow weary of guarding this post.
```

```
** ❌ Bad: Heavy operations every tick
** Trigger: random — Phrase: 50

mob varset target mobile 0
** ... complex multi-step search logic ...
** ... multiple varseton calls ...
** ... xcall chains ...
```

Low phrase percentages (1–5%) reduce how often the script runs. Combine a low phrase with an early `break` for conditions that rarely apply.

### Guard Against Redundant Execution

Use state variables or marker objects to prevent scripts from doing work they have already done:

```
** ✅ Only do the expensive setup once
if varbool setup_done
  break
endif
mob varset setup_done bool true

** ... one-time initialization ...
```

### Avoid Deep Recursion

The engine limits call depth to 15, but hitting that limit means something is wrong with your design. Keep call chains shallow:

```
** ❌ Bad: Script A calls Script B, which calls Script A
** This risks hitting the recursion limit and makes debugging painful.

** ✅ Good: Flat structure — one coordinator, multiple workers
** Coordinator calls Worker 1, Worker 2, Worker 3 independently.
** Each worker returns a value. Coordinator decides what to do next.
```

### Minimize Cross-Entity Variable Traffic

`varseton` on other entities is not free — it involves looking up the target entity and modifying its data. In tight loops, prefer local variables and set external data once at the end:

```
** ✅ Calculate locally, set on target once
mob varset total integer $[$<base> + $<bonus> + $<modifier>]
mob varseton $n final_score integer $<total>

** ❌ Setting on the target multiple times
mob varseton $n score integer $<base>
mob varseton $n score integer $[$(enactor.score:num) + $<bonus>]
mob varseton $n score integer $[$(enactor.score:num) + $<modifier>]
```

### Queue Cleanup

If a mob dies or is purged while it has queued commands, those commands are lost. But if a mob *should* stop its queued sequence due to a script event (like combat starting), explicitly dequeue:

```
** Cleanup on startcombat
if hasqueue $i
  mob dequeue
endif
```

### Script Security Levels

Scripts with higher security levels can do more (like `xcall`), but they also bypass more safety checks. Only elevate security when the script genuinely needs it:

| Security Level | Capabilities |
|:---------------|:-------------|
| 0–4 (default) | Standard script commands |
| 5+ | `xcall` (cross-entity calls) |
| 7+ | Certain restricted entity modifications |
| 9–10 | Full access (admin-level scripts only) |

Set the minimum security level your script needs — no more.

---

## Reputation and Faction Integration

The reputation system lets you track a player's standing with factions — guilds, cities, enemy forces, or any group. Scripts can check reputation, award or deduct points, and gate content by faction rank. This section covers the scripting patterns; factions themselves are created with `repedit`.

### Checking Reputation

Use `hasreputation` to test whether a player has *any* standing with a faction, and `varnumber` to check their exact point total:

```
** Guard at the guild hall entrance
if !hasreputation $n 10#50
  mob say You are not a member of the Silvershield Order.
  mob say Speak to Commander Valen in the barracks to enlist.
  end
endif

if varnumber $n reputation > 500
  mob say Hail, Champion! The council awaits you inside.
elseif varnumber $n reputation > 100
  mob say Welcome, knight. The training grounds are to your left.
else
  mob say Recruit, report to the quartermaster for your assignment.
endif
```

**Trigger:** `greet 100`

The `hasreputation $n <widevnum>` ifcheck takes a faction widevnum (e.g., `10#50`). It returns true if the player has any standing at all — even negative. Use point thresholds to distinguish between ranks.

{: .note }
> Faction ranks have `capacity` values set in `repedit`. A rank with capacity 100 means the player advances to the next rank after earning 100 points in the current one. The point thresholds you check in scripts should align with your rank definitions.

### Awarding Reputation

Use `award` to grant reputation points and `deduct` (or negative `award`) to reduce them:

```
** Quest completion reward — grants reputation
mob award $n reputation 'Silvershield Order' 100
mob echoat $n {GYou gain {Y100{G reputation with the {WSilvershield Order{G.{x
```

```
** Crime punishment — loses reputation
mob award $n reputation 'Silvershield Order' -50
mob echoat $n {rYou lose {Y50{r reputation with the {WSilvershield Order{r.{x
```

The faction can be referenced by name (string match) or widevnum:

```
** Both of these are equivalent:
mob award $n reputation 'Silvershield Order' 100
mob award $n reputation 10#50 100
```

The system handles rank-ups and rank-downs automatically. When a player crosses a rank boundary, they see a message and any rank flags (like `peaceful` or `hostile`) take effect immediately.

### Opposing Factions Pattern

A common design has two factions where gaining reputation with one costs reputation with the other:

```
** Orc scout death — awards Guardsmen, penalizes Orc Horde
if !ispc $n
  end
endif

mob award $n reputation 'City Guardsmen' 25
mob award $n reputation 'Orc Horde' -15
mob echoat $n {x
mob echoat $n {GYou gain {Y25{G reputation with the {WCity Guardsmen{G.{x
mob echoat $n {rYou lose {Y15{r reputation with the {WOrc Horde{r.{x
```

**Trigger:** `death 100`

{: .warning }
> For simple kill-based reputation grants, prefer using the mob's `mob_reputations` field in `medit` instead of scripts. It is more efficient and requires no scripting. Use scripts only when you need conditional logic (level checks, time-of-day, quest state, etc.).

### Reputation-Gated Content

Combine reputation checks with shop gating, quest prerequisites, or area access:

```
** Gatekeeper to a faction-only area
if !hasreputation $n 10#50
  mob say Only members of the Silvershield Order may pass.
  end
endif

if varnumber $n reputation < 200
  mob say Your rank is insufficient. Prove yourself first.
  end
endif

mob say Welcome, $N. You may enter.
mob transfer $n 10#500
```

**Trigger:** `speech enter`

For shop gating without scripts, set `reputation` and `min_reputation_rank` directly on the shop or stock item in `shedit`. The game enforces the requirement automatically.

### Quick Reference

| Task | Command / Ifcheck |
|:-----|:------------------|
| Check if player has faction standing | `hasreputation $n <widevnum>` |
| Check faction association (NPC) | `hasfaction $n <value>` |
| Check point total | `varnumber $n reputation > <threshold>` |
| Grant reputation | `mob award $n reputation '<name>' <amount>` |
| Remove reputation | `mob award $n reputation '<name>' -<amount>` |
| Grant paragon level | `mob award $n paragon '<name>' 1` |
| Gate by rank (shop, no script) | Set in `shedit` → `reputation` + `min_reputation_rank` |

---

## Putting It All Together: Elemental Ritual System

Here is a practical example that combines most of the patterns from this tutorial — an altar object that performs a ritual using delays, cross-entity calls, variable sharing, and return values.

**The setup:** An altar object in a room. A player with a ritual token can say "begin ritual" to start a multi-step sequence. The altar communicates with the player's token to validate, consume charges, and produce an effect.

### Altar Object Script (speech trigger, phrase "begin ritual")

```
** Only respond to players, not NPCs
if isnpc $n
  break
endif

** Prevent overlapping rituals
if hasqueue $(self)
  obj echoat $n {DThe altar is already active. Wait for the ritual to complete.{x
  break
endif

** Check for the ritual token
if !hastoken $n 95000
  obj echoat $n {DYou place your hands on the altar but feel no connection.{x
  obj echoat $n {DPerhaps you need a ritual focus.{x
  break
endif

** Store references for the queued follow-ups
obj varset tok token $n 95000
obj varset ritualist mobile $n

** Pass data to the token and call its validation script
obj varseton $<tok> altar object $(self)
obj xcall $<tok> 95001 $n

if lastreturn == 0
  obj echoat $n {DThe altar flickers and goes dark. Your focus lacks sufficient power.{x
  break
endif

** Ritual begins — multi-step sequence
obj echo {MThe altar begins to hum with power!{x
obj queue 2 obj echo {MRunes along the altar's surface begin to glow.{x
obj queue 4 obj echo {YThe glow intensifies — energy spirals upward!{x
obj queue 6 obj echo {WA column of light erupts from the altar!{x
obj queue 6 obj call 95002
```

### Token Validation Script (vnum 95001)

```
** Called via xcall — runs in the token's context
** Check if the token has enough charges (val0)

if tokenvalue $(self) 0 < 1
  end 0
endif

** Consume a charge
token adjust $(self) val0 -1
token echoat $(self.owner) {CYour ritual focus pulses as it expends a charge.{x
end 1
```

### Altar Completion Script (vnum 95002, called by queue)

```
** Final step — reward the ritualist
obj echo {WThe light fades, leaving behind a shimmering object on the altar.{x
obj oload 95050
obj echoat $<ritualist> {YYou feel a surge of power as the ritual completes!{x
obj award $<ritualist> xp 500

** Clean up
obj varclear tok
obj varclear ritualist
```

This system demonstrates:

- **`hasqueue`** preventing overlapping invocations
- **`varseton`** passing a reference from the altar to the token
- **`xcall`** calling the token's validation script from the altar's context
- **`lastreturn`** reading the validation result to decide whether to proceed
- **`queue`** staging a multi-step sequence with timed messages
- **`call`** from a queued command to run the completion logic
- **Variable references** (`$<tok>`, `$<ritualist>`) persisting across the queued timeline

---

## What You Learned

In this tutorial you covered:

- **`delay`** for single-shot timers with follow-up logic in a delay trigger, and **`cancel`** to abort them
- **`queue` / `dequeue`** for stacking multiple timed commands, with `hasqueue` to prevent overlap
- **`scriptwait`** for inline pauses (use sparingly)
- **`call`** for same-entity sub-scripts — extract shared logic, pass context, read return values with `lastreturn`
- **`xcall`** for cross-entity sub-scripts — run logic in another entity's context (requires security ≥ 5)
- **Cross-entity communication** via `varseton`, token passing, `xcall` with data, and room variables as shared state
- **RSG variables** for dynamic random text generation
- **The `at` command** for remote actions and hiding spellcasting
- **Performance tips**: lightweight random triggers, recursion limits, minimizing cross-entity traffic, cleanup patterns

---

## Next Steps

- Consult the [Advanced Scripting](../advanced-scripting.md) reference for the full details on entity casting, nested variable expansion, and script limits
- See the [Variables and Tokens](../variables-and-tokens.md) reference for the complete variable type list, token flags, and persistence rules
- Review the [Shared Commands Reference](../shared-commands.md) for the full syntax of every command mentioned in this tutorial
- Explore the [IFChecks Reference](../ifchecks-reference.md) for all conditional checks available in scripts
- Study real examples in the [Mobile Programs Examples](../mobile-programs/mprog-examples.md) to see these patterns in live game areas
