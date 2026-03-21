---
layout: default
title: Building a Quest
nav_order: 4
parent: Tutorials
grand_parent: Scripting
---

# Building a Quest
{: .no_toc }

Walk through creating a complete multi-stage quest with triggers, objectives, and rewards.
{: .fs-6 .fw-300 }

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## What You'll Build

In this tutorial you will create **The Herbalist's Request** — a three-stage quest
that sends a player out to gather rare herbs and return them for a reward.

| Stage | Goal | Trigger Flow |
|:------|:-----|:-------------|
| 1 — The Plea | Talk to the herbalist NPC | `quest_accepted` → `stage_commenced` |
| 2 — The Gathering | Collect 3 rare herbs from different locations | `objective_completed` (×3) → `stage_completed` |
| 3 — The Return | Bring the herbs back to the herbalist | `stage_commenced` → `objective_completed` → `quest_completed` |

By the end you will understand the full quest trigger lifecycle, how to track
progress with variables, and how to test every stage from the builder console.

---

## Prerequisites

Before starting this tutorial you should have:

- Completed the [Your First Script](your-first-script.md) and
  [Working with Variables](working-with-variables.md) tutorials
- Builder access with `qedit` and `qpedit` permissions
- A test area to work in — we will use area **10** with the widevnum prefix `10#`

{: .note }
> If you do not have `qedit` access, ask an administrator to grant the
> `olc_quest` security flag on your builder character.

---

## Step 1 — Design the Quest

Before touching any editor, sketch out the quest on paper (or in your head).
Here is our plan:

```
Quest: The Herbalist's Request (10#50)
Level range: 5–15

Stage 1 — The Plea
  Objective: Visit the herbalist (mob 10#200) and accept the quest

Stage 2 — The Gathering
  Objective A: Obtain Moonpetal Blossom (obj 10#401)
  Objective B: Obtain Thornroot Sprig   (obj 10#402)
  Objective C: Obtain Gloomcap Fungus   (obj 10#403)

Stage 3 — The Return
  Objective: Visit the herbalist again with all three herbs

Rewards: 500 gold, 10 quest points, Herbalist's Satchel (obj 10#410)
```

Good quest design follows a simple loop: **go somewhere → do something → come
back**. Each stage should feel like a self-contained goal.

**What we just did:** Planned the quest structure and identified the vnums we need.

---

## Step 2 — Create the Quest Definition

Open the quest editor and create the quest shell.

```
qedit 10#50 create
```

> **Game responds:**
> Quest 10#50 created.

Now configure the basic properties:

```
qedit 10#50
name The Herbalist's Request
minlevel 5
maxlevel 15
type standard
reward gold 500
reward qp 10
reward obj 10#410
done
```

{: .note }
> The `reward` lines tell the engine what to hand out automatically at
> `quest_completed`. You can add extra rewards in the script — we will
> do that in Step 8.

**What we just did:** Created quest `10#50` with a name, level range, and
reward table. No stages or scripts yet.

---

## Step 3 — Add Stages and Objectives

Still inside `qedit 10#50`, add the three stages and their objectives.

### Stage 1 — The Plea

```
qedit 10#50
addstage
stage 1 name The Plea
stage 1 addobjective
stage 1 obj 1 type visit
stage 1 obj 1 target 10#200
stage 1 obj 1 desc Speak to Elowen the Herbalist
done
```

### Stage 2 — The Gathering

```
qedit 10#50
addstage
stage 2 name The Gathering
stage 2 addobjective
stage 2 obj 1 type gather
stage 2 obj 1 target 10#401
stage 2 obj 1 desc Obtain a Moonpetal Blossom
stage 2 addobjective
stage 2 obj 2 type gather
stage 2 obj 2 target 10#402
stage 2 obj 2 desc Obtain a Thornroot Sprig
stage 2 addobjective
stage 2 obj 3 type gather
stage 2 obj 3 target 10#403
stage 2 obj 3 desc Obtain a Gloomcap Fungus
done
```

### Stage 3 — The Return

```
qedit 10#50
addstage
stage 3 name The Return
stage 3 addobjective
stage 3 obj 1 type visit
stage 3 obj 1 target 10#200
stage 3 obj 1 desc Return to Elowen the Herbalist
done
```

{: .warning }
> Objective numbering restarts at 1 inside each stage. Do not try to number
> objectives globally — `stage 2 obj 1` is the first objective of stage 2,
> not a continuation of stage 1.

**What we just did:** Gave the quest three stages with objectives. The engine
knows *what* the player must do — quest programs define *how* the game reacts.

---

## Step 4 — Write the Acceptance Script

When a player accepts the quest, the `quest_accepted` trigger fires. We use it
to welcome the player, set up tracking variables, and point them toward stage 1.

Create the quest program and write the script:

```
qpedit 10#300 create
```

```
qpedit 10#300
code
quest echoat $n {Y---------------------------------------------------{x
quest echoat $n {YThe Herbalist's Request{x
quest echoat $n {Y---------------------------------------------------{x
quest echoat $n
quest echoat $n {cElowen clasps your hands gratefully.{x
quest echoat $n
quest echoat $n '{GThank you, $n! The forest has grown dangerous and{x
quest echoat $n {GI can no longer gather the herbs I need. Please bring{x
quest echoat $n {Gme a Moonpetal Blossom, a Thornroot Sprig, and a{x
quest echoat $n {GGloomcap Fungus. You will be well rewarded.{x'
quest echoat $n
quest varset herbs_collected 0
quest varset quest_start_time $T
quest wiznet $n accepted The Herbalist's Request
~
done
```

Now attach the program to the quest:

```
qedit 10#50
addprog 10#300 quest_accepted 100
done
```

Let's break down the key parts:

| Line | Purpose |
|:-----|:--------|
| `quest echoat $n` | Sends a message only to the player (`$n`) |
| `quest varset herbs_collected 0` | Creates a quest variable to track progress |
| `quest varset quest_start_time $T` | Records the accept timestamp for a speed bonus later |
| `quest wiznet` | Sends a message to the builder channel so you can monitor during testing |

{: .note }
> Quest variables created with `quest varset` live on the quest instance
> itself. They are automatically cleaned up when the quest ends. Use
> `quest varseton $n` instead if you need a variable that persists on the
> *player* after the quest is over.

**What we just did:** Created a quest program that fires when the player
accepts the quest — flavour text, tracking variables, and wiznet logging.

---

## Step 5 — Write Stage Scripts

The `stage_commenced` trigger fires each time a new stage begins. We will write
one program that handles all three stages by checking the current stage number.

```
qpedit 10#301 create
```

```
qpedit 10#301
code
if questvar $(self) current_stage == 1
  quest echoat $n {cYou should find Elowen in her hut at the edge of the village.{x
  quest echoat $n {cLook for the path leading south from the market square.{x
endif
if questvar $(self) current_stage == 2
  quest echoat $n {Y--- Stage 2: The Gathering ---{x
  quest echoat $n {cElowen needs three rare herbs:{x
  quest echoat $n {G  * Moonpetal Blossom{x  — {cfound near the moonlit pond{x
  quest echoat $n {G  * Thornroot Sprig{x    — {cgrows in the thorny thicket{x
  quest echoat $n {G  * Gloomcap Fungus{x    — {chidden in the deep caves{x
  quest echoat $n {cGather all three and return to Elowen.{x
  quest varset herbs_collected 0
  quest wiznet $n began The Gathering (stage 2)
endif
if questvar $(self) current_stage == 3
  quest echoat $n {Y--- Stage 3: The Return ---{x
  quest echoat $n {cYou have all three herbs! Hurry back to Elowen{x
  quest echoat $n {cbefore they lose their potency.{x
  quest wiznet $n began The Return (stage 3)
endif
~
done
```

Attach it to the quest:

```
qedit 10#50
addprog 10#301 stage_commenced 100
done
```

{: .note }
> The `$(self)` quick code refers to the quest itself. When you write
> `questvar $(self) current_stage`, you are reading the built-in variable
> that the engine sets automatically each time a stage starts. You do not
> need to set it yourself.

**What we just did:** Created a single program that fires at the start of every
stage, branching on `current_stage` to give each stage its own setup logic.

---

## Step 6 — Track Objective Progress

The `objective_completed` trigger fires each time the player finishes one
objective. We use it to give feedback and update our `herbs_collected` counter.

```
qpedit 10#302 create
```

```
qpedit 10#302
code
if questvar $(self) current_stage == 2
  quest varset herbs_collected $[$herbs_collected + 1]
  if questvar $(self) herbs_collected == 1
    quest echoat $n {GYou found the first herb! Two more to go.{x
  endif
  if questvar $(self) herbs_collected == 2
    quest echoat $n {GTwo herbs collected — only one remains!{x
  endif
  if questvar $(self) herbs_collected == 3
    quest echoat $n {GYou have all three herbs! Return to Elowen quickly.{x
  endif
  quest wiznet $n collected herb ($herbs_collected/3)
else
  quest wiznet $n completed objective (stage $current_stage)
endif
~
done
```

Attach it:

```
qedit 10#50
addprog 10#302 objective_completed 100
done
```

Let's look at the arithmetic expression:

```
quest varset herbs_collected $[$herbs_collected + 1]
```

The `$[... ]` syntax performs inline math — `$herbs_collected` reads the current
value, `+ 1` increments it, and the result is stored back.

{: .warning }
> Make sure you initialize counter variables (we did this in Step 5 with
> `quest varset herbs_collected 0`). Reading an uninitialized variable in
> arithmetic will produce unpredictable results.

**What we just did:** Added progress feedback after each herb. The
`herbs_collected` variable tracks cumulative progress.

---

## Step 7 — Handle Stage Transitions

The `stage_completed` trigger fires when all objectives in a stage are done.
Use it for intermediate rewards, narrative, and cleanup.

```
qpedit 10#303 create
```

```
qpedit 10#303
code
if questvar $(self) current_stage == 1
  quest echoat $n {cElowen smiles warmly at you.{x
  quest echoat $n '{GYou came! I knew someone brave would answer my plea.{x
  quest echoat $n {GLet me tell you what I need...{x'
  quest wiznet $n completed stage 1 — advancing to The Gathering
endif
if questvar $(self) current_stage == 2
  quest echoat $n {Y*** Stage Complete: The Gathering ***{x
  quest echoat $n {cYou carefully bundle the three herbs together.{x
  quest echoat $n {cThe mingled scent is almost overwhelming.{x
  quest echoat $n {GYou receive 50 gold for your efforts so far.{x
  quest oload 10#420
  quest echoat $n {cA shimmering waypoint appears, guiding you back to Elowen.{x
  quest wiznet $n completed stage 2 — advancing to The Return
endif
if questvar $(self) current_stage == 3
  quest echoat $n {cElowen takes the herbs reverently and begins to work.{x
  quest wiznet $n completed stage 3 — quest finishing
endif
~
done
```

Attach it:

```
qedit 10#50
addprog 10#303 stage_completed 100
done
```

{: .note }
> After a `stage_completed` trigger finishes executing, the engine
> automatically advances the player to the next stage and fires
> `stage_commenced`. You do not need to advance stages manually.

**What we just did:** Added narrative transitions between stages with wiznet
logging for debugging.

---

## Step 8 — Write the Completion Script

The `quest_completed` trigger fires after the engine has already given out the
standard rewards from Step 2. Use it for bonus rewards, cleanup, and a
satisfying ending.

```
qpedit 10#304 create
```

```
qpedit 10#304
code
quest echoat $n
quest echoat $n {Y===================================================={x
quest echoat $n {Y  Quest Complete: The Herbalist's Request{x
quest echoat $n {Y===================================================={x
quest echoat $n
quest echoat $n {cElowen beams with delight as she examines the herbs.{x
quest echoat $n
quest echoat $n '{GThese are perfect, $n! With these I can brew{x
quest echoat $n {Genough medicine to last the village through winter.{x
quest echoat $n {GPlease, take this satchel — and my deepest thanks.{x'
quest echoat $n
quest echoat $n {GYou receive:{x
quest echoat $n {G  * 500 gold{x
quest echoat $n {G  * 10 quest points{x
quest echoat $n {G  * Herbalist's Satchel{x
if level $n >= 10
  quest echoat $n {YBonus! Your experience earned you an extra 100 gold.{x
  quest wiznet $n received level-scaled bonus (level $j)
endif
quest echoat $n {cElowen waves farewell as you leave.{x
quest echoat $n
quest varclear herbs_collected
quest varclear quest_start_time
quest wiznet $n completed The Herbalist's Request
~
done
```

Attach it:

```
qedit 10#50
addprog 10#304 quest_completed 100
done
```

Here is what the new pieces do:
| Line | Purpose |
|:-----|:--------|
| `if level $n >= 10` | Checks the player's level for a scaled bonus |
| `$j` | Quick code that expands to the player's level |
| `quest varclear herbs_collected` | Removes the variable — keeps the quest instance clean |
| `quest varclear quest_start_time` | Cleans up the timestamp we set in Step 4 |

{: .warning }
> Always clean up quest variables in `quest_completed` (or `quest_failed`).
> Leftover variables do not cause errors, but they waste memory and can
> confuse future debugging.

**What we just did:** Added a finale script that displays the reward summary,
gives a level-scaled bonus, and cleans up all quest variables.

---

## Step 9 — Test Your Quest

Testing a quest requires walking through every stage as a player would.

### Start the Quest

```
quest start 10#50
```

> **Game responds:**
> You have accepted The Herbalist's Request.

You should see the acceptance message from Step 4 and the stage 1 hints from
Step 5.

### Check Quest Status

```
quest info
```

> **Game responds:**
> The Herbalist's Request (Stage 1 — The Plea)
>   [ ] Speak to Elowen the Herbalist

### Walk Through Stage 1

Visit the room containing mob `10#200`. The visit objective completes
automatically and you will see the stage 1 completion message from Step 7.

### Walk Through Stage 2

The engine advances to stage 2. Check progress any time:

```
quest info
```

> **Game responds:**
> The Herbalist's Request (Stage 2 — The Gathering)
>   [ ] Obtain a Moonpetal Blossom
>   [ ] Obtain a Thornroot Sprig
>   [ ] Obtain a Gloomcap Fungus

Gather each herb. After each one you should see the progress message from
Step 6. Once all three are collected, stage 2 completes.

### Walk Through Stage 3

Return to mob `10#200`. The visit triggers stage 3 completion, then the
full quest completion message from Step 8.

### Debugging Tips

If something does not work as expected:

```
quest debug 10#50
```

This toggles verbose output showing every trigger fire, variable change, and
ifcheck evaluation. You can also inspect quest variables mid-quest:

```
quest vars
```

> **Game responds:**
> Quest variables for The Herbalist's Request:
>   herbs_collected = 2
>   quest_start_time = 1720000000

{: .note }
> Use `quest reset 10#50` to abandon and restart the quest without
> logging out. This is invaluable during testing.

**What we just did:** Walked through the full testing workflow — start, inspect,
play through each stage, and debug.

---

## Step 10 — Try It Yourself

You now have a working quest. Here are three challenges to extend it.

### Challenge 1 — Add a Failure Handler

Create a `quest_failed` program that fires when the player abandons the quest.
Display a disappointed message from Elowen and clean up variables.

```
qpedit 10#305 create
```

```
qpedit 10#305
code
quest echoat $n
quest echoat $n {cElowen sighs sadly.{x
quest echoat $n
quest echoat $n '{GI understand, $n. Perhaps another time...{x'
quest echoat $n
quest varclear herbs_collected
quest varclear quest_start_time
quest wiznet $n abandoned The Herbalist's Request
~
done
```

```
qedit 10#50
addprog 10#305 quest_failed 100
done
```

### Challenge 2 — Add a Speed Bonus

Remember the `quest_start_time` variable from Step 4? Use it in the completion
script to reward fast players. Add this block to program `10#304` before the
`varclear` lines:

```
if questvar $(self) quest_elapsed <= 600
  quest echoat $n
  quest echoat $n {YSpeed Bonus! You finished in under 10 minutes.{x
  quest echoat $n {GYou receive an extra 5 quest points!{x
  quest echoat $n
  quest wiznet $n earned speed bonus (elapsed: $quest_elapsed)
endif
```

### Challenge 3 — Add Dynamic Targets

Instead of hard-coding herb locations, use `stage_target_resolved` to pick a
random spawn point each time the quest is accepted:

```
qpedit 10#306 create
```

```
qpedit 10#306
code
quest echoat $n {cThe herbs could be in different places this time...{x
quest wiznet $n resolving dynamic targets for stage 2
~
done
```

```
qedit 10#50
addprog 10#306 stage_target_resolved 100
done
```

See the [Quest Programs](../quest-programs/) reference for full details on
dynamic target and destination resolution.

**What we just did:** Extended the quest with a failure handler, a time-based
bonus, and dynamic target resolution.

---

## Key Takeaways

### The Trigger Lifecycle

Every quest follows the same trigger flow:

```
quest_accepted
  └─► stage_commenced (stage 1)
        └─► objective_completed (×N)
              └─► stage_completed (stage 1)
                    └─► stage_commenced (stage 2)
                          └─► objective_completed (×N)
                                └─► stage_completed (stage 2)
                                      └─► ... (repeat for each stage)
                                            └─► quest_completed
```

If the player abandons or times out, `quest_failed` fires instead.

### Variable Management Pattern

1. **Initialize** variables in `quest_accepted`
2. **Update** variables in `objective_completed` and `stage_commenced`
3. **Clean up** variables in `quest_completed` and `quest_failed`

### The `quest` Command Prefix

All quest program commands start with `quest`:

| Command | Purpose |
|:--------|:--------|
| `quest echoat $n <msg>` | Send message to the player |
| `quest varset <name> <value>` | Set a quest variable |
| `quest varclear <name>` | Remove a quest variable |
| `quest mload <vnum>` | Load a mobile |
| `quest oload <vnum>` | Load an object |
| `quest wiznet <msg>` | Send debug message to builder channel |

### Quick Codes Reference

| Code | Expands To |
|:-----|:-----------|
| `$n` | Player name |
| `$i` / `$(self)` | The quest itself |
| `$T` | Current timestamp |
| `$j` | Player level |

### Testing Workflow

1. `quest start <vnum>` — begin the quest
2. `quest info` — check current stage and objectives
3. `quest vars` — inspect quest variables
4. `quest debug <vnum>` — toggle verbose trigger logging
5. `quest reset <vnum>` — abandon and restart cleanly

---

## What's Next

- [Advanced Patterns](advanced-patterns.md) — Learn about delays, sub-scripts,
  and cross-entity communication to make your quests more dynamic
- [Quest Programs](../quest-programs/) — Full reference for all quest triggers,
  commands, and ifchecks
- [Scripting Basics](../scripting-basics.md) — Review the fundamentals if any
  concepts in this tutorial were unclear
