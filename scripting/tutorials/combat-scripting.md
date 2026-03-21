---
layout: default
title: Combat Scripting
nav_order: 5
parent: Tutorials
grand_parent: Scripting
---

# Combat Scripting
{: .no_toc }

Combat is where scripting brings mobs to life. A mob with no combat scripts is a punching bag — it sits there trading auto-attacks until it dies. Combat scripts turn that punching bag into a boss that breathes fire, heals itself when wounded, taunts the player at half health, and drops special loot when it finally falls. This tutorial shows you how to build that boss from scratch.

You should be comfortable with [variables](working-with-variables.md), [ifchecks](../ifchecks-reference.md), and [basic trigger attachment](../scripting-basics.md) before starting this tutorial.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Combat Trigger Overview

Combat triggers fire at different points during a fight. Understanding when each one fires is the key to writing effective combat scripts.

### Fight Triggers (During Combat)

| Trigger | When It Fires | Phrase Meaning |
|:--------|:--------------|:---------------|
| `startcombat` | Once, when combat begins | Percent chance to fire |
| `fight` | Every combat round while fighting | Percent chance per round |
| `preround` | Before each combat round (can prevent the round) | Percent chance |
| `hit` | When the mob lands a hit | Percent chance |

The `fight` trigger is your workhorse. It fires every round, and the phrase controls how often the script actually runs. A phrase of `25` means a 25% chance each round — on average, one special action every four rounds.

### HP Threshold Trigger

| Trigger | When It Fires | Phrase Meaning |
|:--------|:--------------|:---------------|
| `hpcnt` | When HP drops below a percentage | HP percentage threshold |

The `hpcnt` trigger fires when the mob's HP percentage falls below the phrase value. A phrase of `50` fires when the mob drops below 50% health. This is how you create phase transitions in boss fights.

{: .warning }
> The `hpcnt` trigger can fire **multiple times** — every combat round where the mob's HP is below the threshold. If you want a phase change to happen only once, you need a guard condition (explained below).

### Attack Skill Triggers

| Trigger | When It Fires | Phrase Meaning |
|:--------|:--------------|:---------------|
| `attack` | When the mob performs any attack skill | Percent chance |
| `attack_backstab` | When the mob backstabs | Percent chance |
| `attack_bash` | When the mob bashes | Percent chance |
| `attack_kick` | When the mob kicks | Percent chance |
| `attack_disarm` | When the mob disarms | Percent chance |

There are many more `attack_*` variants (`attack_bite`, `attack_circle`, `attack_counter`, `attack_cripple`, `attack_dirtkick`, `attack_rend`, `attack_slit`, `attack_smite`, `attack_tailkick`, `attack_trample`, `attack_turn`, etc.). Each fires when that specific combat skill is used.

### Defensive Triggers

| Trigger | When It Fires | Phrase Meaning |
|:--------|:--------------|:---------------|
| `defense` | When the mob uses a defensive combat style | Percent chance |
| `barrier` | When an energy barrier blocks an attack | Percent chance |
| `check_damage` | When damage is calculated against the mob | Percent chance |
| `damage` | When the mob takes damage | Percent chance |

### Death Triggers

| Trigger | When It Fires | Phrase Meaning |
|:--------|:--------------|:---------------|
| `death` | When the mob dies | Percent chance |
| `afterkill` | After the mob kills a target | Percent chance |
| `wimpy` | When the mob's wimpy threshold is reached | Percent chance |
| `prewimpy` | Before wimpy flee triggers (can prevent) | Percent chance |

---

## Your First Combat Script: A Taunting Fighter

Let's start simple. This mob taunts the player when combat begins and uses a special attack occasionally during the fight.

### The Startcombat Script

```
** Fires once when combat begins
** Trigger: startcombat — Phrase: 100

mob echo {Y$I draws $l blade and settles into a fighting stance.{x
say You will regret this, $N!
```

Attach it in the mob editor:

```
medit <mob_vnum>
addprog <script_vnum> startcombat 100
```

### A Simple Fight Script

```
** 20% chance each combat round to use a special attack
** Trigger: fight — Phrase: 20

mob echoat $n {R$I lunges at you with a vicious overhead strike!{x
mob echoaround $n {R$I lunges at $N with a vicious overhead strike!{x
mob damage $n 75 150
```

{: .note }
> The `damage` command deals random damage between the low and high values. Without the `kill` or `lethal` flag, damage stops at 1 HP — it cannot deliver the killing blow. Add `lethal` if the special attack should be able to finish the player off: `mob damage $n 75 150 lethal`.

---

## Building a Boss with HP Phases

Boss encounters feel dynamic when the mob changes behavior as it takes damage. The `hpcnt` trigger makes this possible — but you need a technique to ensure each phase only triggers once.

### The One-Shot Phase Pattern

The `hpcnt` trigger fires every round the mob's HP is below the threshold. To make a phase change happen exactly once, load a marker object onto the mob the first time the script runs, then check for that object at the top:

```
** Phase 2 transition — fires when HP drops below 75%
** Trigger: hpcnt — Phrase: 75

** Guard: only fire once. "oo" prefix makes the object invisible/internal.
if carries $i oo90001oo
  break
endif

** Load the marker so this script never fires again
mob oload 90001

mob echo {M$I snarls and $l eyes begin to glow with an eerie light!{x
mob echo {MA wave of dark energy ripples outward from $I!{x
mob damage all 50 100
```

The `oo<vnum>oo` syntax searches by vnum specifically — it will not accidentally match another object with the same keyword. This is the standard pattern for one-shot triggers.

{: .note }
> You need to create the marker object in the area editor (`oedit create`). It can be a simple object with no properties — its only purpose is to exist in the mob's inventory as a flag.

### A Three-Phase Boss

Here is a boss with three distinct phases at 75%, 50%, and 25% HP. Each phase introduces a new ability.

**Phase 2 — 75% HP: Area damage burst**

```
** Trigger: hpcnt — Phrase: 75

if carries $i oo90001oo
  break
endif
mob oload 90001

mob echo {M$I throws $l head back and howls!{x
mob echo {RDark energy erupts from $I, searing everyone nearby!{x
mob damage all 50 100
say You think you can defeat me? I am just getting started!
```

**Phase 3 — 50% HP: Self-heal and summon**

```
** Trigger: hpcnt — Phrase: 50

if carries $i oo90002oo
  break
endif
mob oload 90002

mob echo {Y$I slams $l fist into the ground!{x
mob echo {YThe earth cracks open and a shadowy minion claws its way out!{x
mob mload 90010
mob echo {C$I channels dark power and $l wounds begin to close!{x
mob at 1 mob cast 'cure critical'
mob at 1 mob cast 'cure critical'
```

{: .note }
> `mob at 1 mob cast 'cure critical'` temporarily moves the mob to room 1 (a void room) to cast the heal, then returns it. This prevents the player from seeing "The boss casts a spell" messages — they only see your custom echo text. This is a common pattern for disguising spellcasting.

**Phase 4 — 25% HP: Enrage**

```
** Trigger: hpcnt — Phrase: 25

if carries $i oo90003oo
  break
endif
mob oload 90003

mob echo {R$I roars in fury! $L eyes turn blood-red!{x
mob echo {R****** The ground shakes violently! ******{x
mob echo {D$I is consumed by rage — $l attacks become frenzied!{x
say I WILL NOT FALL!
```

### Enrage: Making the Boss Hit Harder

The enrage phase above is dramatic, but it does not actually make the boss hit harder. You can use a variable to track the phase and modify the fight script accordingly:

```
** Main combat script — varies by phase
** Trigger: fight — Phrase: 30

if isdelay $i
  break
endif

** Check if enraged (phase marker object loaded at 25%)
if carries $i oo90003oo
  ** Enraged: double damage, more dramatic
  mob echoat $n {R$I savagely rips at you with frenzied claws!{x
  mob echoaround $n {R$I savagely rips at $N with frenzied claws!{x
  mob damage $n 200 400 lethal
  mob delay 1
else
  ** Normal: standard special attack
  if rand 50
    mob echoat $n {Y$I slashes at you with razor-sharp claws!{x
    mob echoaround $n {Y$I slashes at $N with razor-sharp claws!{x
    mob damage $n 100 200
  else
    mob echoat $n {C$I breathes a gout of frost at you!{x
    mob echoaround $n {C$I breathes a gout of frost at $N!{x
    mob damage $n 75 175
  endif
  mob delay 1
endif
```

{: .warning }
> Always check `if isdelay $i` at the top of fight scripts. Without it, the mob can trigger multiple special attacks in the same round — one from each fight script that fires. The delay acts as a cooldown.

---

## Death Scripts: Conditional Loot and Last Words

The `death` trigger fires when the mob dies. Use it for special loot drops, dramatic death sequences, or cleanup.

### Basic Death Loot

```
** Drop a guaranteed reward on death
** Trigger: death — Phrase: 100

mob oload 90050
drop treasure
```

The `oload` creates the object in the mob's inventory (or on the ground if inventory is full), and `drop` puts it on the ground for players to loot.

### Conditional Loot Based on a Quest

```
** Drop quest item only if the killer is on the right quest stage
** Trigger: death — Phrase: 100

if queststate $n 90001 == active
  mob oload 90060
  drop ancient_key
  mob echoat $n {YA glowing key tumbles from the wreckage!{x
  mob echoaround $n {YA glowing key tumbles from the wreckage!{x
endif
```

### Rare Loot with Random Chance

```
** Rare drop: 10% chance for an epic item, guaranteed common drop
** Trigger: death — Phrase: 100

mob oload 90051
drop trophy

if rand 10
  mob oload 90099
  drop blade
  mob echo {WA brilliant light erupts as an extraordinary weapon materializes!{x
endif
```

### Boss Death with Cleanup

When a phased boss dies, you should clean up the marker objects and any summoned minions:

```
** Boss death — clean up and reward
** Trigger: death — Phrase: 100

** Remove phase marker objects
mob junk oo90001oo
mob junk oo90002oo
mob junk oo90003oo

** Dramatic death sequence
mob echo {R$I staggers, clutching $l chest!{x
mob echo {DA pillar of dark energy erupts from $I and dissipates into nothing!{x
mob echo {WThe ground stops shaking. Silence falls over the chamber.{x

** Load the loot
mob oload 90070
drop eye
mob oload 90071
drop grimoire

** Reward message
mob echoat $n {YVictory! The dark lord has been vanquished!{x
mob echoaround $n {YVictory! $N has vanquished the dark lord!{x
```

{: .note }
> `mob junk` destroys an object in the mob's inventory. Since the mob is dying, this cleanup is mostly for tidiness — but it prevents the marker objects from appearing in the corpse loot.

---

## Defensive Triggers: Damage Mitigation

Defensive triggers let a mob react when it takes damage or blocks attacks, making fights feel more interactive.

### The damage Trigger

The `damage` trigger fires when the mob takes damage. Use it to react to being hit:

```
** React to taking damage
** Trigger: damage — Phrase: 30

if hpcnt $i < 50
  mob echo {D$I winces from the blow, dark ichor dripping from the wound.{x
else
  mob echo {Y$I barely flinches, sneering at your efforts.{x
endif
```

### The check_damage Trigger

The `check_damage` trigger fires when damage is being *calculated* against the mob — before it is applied. This is useful for narrating how the mob handles incoming attacks:

```
** Narrate damage resistance
** Trigger: check_damage — Phrase: 20

if rand 50
  mob echoat $n {C$I's armor absorbs part of the blow!{x
else
  mob echoat $n {C$I twists aside, partially deflecting your strike!{x
endif
```

### The barrier Trigger

The `barrier` trigger fires when an energy barrier blocks an attack. Use it to add flavor to magical defenses:

```
** Barrier blocks an attack
** Trigger: barrier — Phrase: 100

mob echo {B$I's barrier shimmers and crackles as it absorbs the impact!{x
mob echoat $n {BYour attack strikes a wall of energy and dissipates harmlessly!{x
```

### Combining Defense with Counter-Attacks

A mob that fights back when hit makes for a memorable encounter:

```
** Counter-attack when taking damage
** Trigger: damage — Phrase: 15

if isdelay $i
  break
endif

if fighting $i
  mob echoat $n {R$I catches your arm and retaliates with a savage headbutt!{x
  mob echoaround $n {R$I catches $N's arm and retaliates with a savage headbutt!{x
  mob damage $n 50 100
  mob delay 1
endif
```

---

## Making Combat Dynamic

Raw damage numbers make a fight functional. Echoes, delays, and pacing make it *feel* like a real encounter.

### Color Codes for Impact

Use color codes to differentiate attack types and severity:

| Code | Color | Typical Use |
|:-----|:------|:------------|
| `{R` | Red | Heavy damage, fire, blood |
| `{Y` | Yellow | Warning, lightning, wind-up |
| `{C` | Cyan | Cold, water, magic |
| `{M` | Magenta | Shadow, dark magic, phase changes |
| `{W` | White | Holy, explosions, dramatic moments |
| `{D` | Dark grey | Narrative, atmosphere |
| `{G` | Green | Poison, acid, nature |
| `{B` | Blue | Barriers, shields, protective magic |
| `{x` | Reset | Always close your color tags |

### Wind-Up Attacks with Delay

A two-part attack with a wind-up creates tension. The mob telegraphs the attack one round, then delivers it on the next:

```
** Phase 1: Wind-up — fight trigger on the mob
** Trigger: fight — Phrase: 15

if isdelay $i
  break
endif

mob echo {Y$I raises $l arms, gathering crackling energy between $l palms!{x
mob echoat $n {YYou sense something terrible is about to happen!{x
mob delay 4
```

```
** Phase 2: Delivery — delay trigger on the mob
** Trigger: delay — Phrase: 100

mob echo {R$I unleashes a devastating blast of energy!{x
mob echo {R****** BOOOOM!! ******{x
mob damage all 150 300 lethal
```

{: .warning }
> `mob delay` sets a timer in **pulses** (4 pulses ≈ 1 second). The delay trigger fires when the timer expires. Only **one** delay can be pending at a time — setting a new delay cancels the previous one.

### Using Queue for Multi-Step Sequences

For longer combat sequences with multiple timed messages, `queue` is more flexible than `delay` because you can stack multiple commands:

```
** A dramatic multi-step attack — fight trigger
** Trigger: fight — Phrase: 10

if isdelay $i
  break
endif
if hasqueue $i
  break
endif

mob echo {D$I begins chanting in a forgotten tongue...{x
mob queue 2 mob echo {MThe air crackles with arcane energy!{x
mob queue 4 mob echo {RShadowy tendrils erupt from the ground!{x
mob queue 4 mob damage all 100 200 lethal
mob queue 5 mob echo {DThe dark energy fades, leaving scorch marks on the stone.{x
```

{: .note }
> Check `hasqueue $i` at the top to prevent overlapping sequences. Without it, the mob could start a new chanting sequence while the previous one is still playing out.

### Echoat vs Echoaround vs Echo

Use the right output command for immersion:

- **`mob echo`** — Everyone in the room sees this (including combatants and bystanders)
- **`mob echoat $n`** — Only the target sees this (use for "you" perspective messages)
- **`mob echoaround $n`** — Everyone *except* the target sees this (third-person perspective)

A well-scripted attack uses all three:

```
mob echoat $n {R$I's blade slices across your chest, drawing blood!{x
mob echoaround $n {R$I's blade slices across $N's chest, drawing blood!{x
mob damage $n 100 200
```

The target sees "your chest" while everyone else sees "$N's chest" — just like the game's built-in combat messages.

---

## Complete Boss Example: The Shadow Warden

Here is a complete boss mob with all the techniques from this tutorial assembled together. The Shadow Warden has three HP phases, multiple attack patterns, a summoned minion, and conditional loot.

### Trigger Attachment

```
medit <mob_vnum>
addprog <vnum_1> startcombat 100
addprog <vnum_2> fight 25
addprog <vnum_3> fight 30
addprog <vnum_4> hpcnt 75
addprog <vnum_5> hpcnt 50
addprog <vnum_6> hpcnt 25
addprog <vnum_7> death 100
```

### Script 1: Combat Opens (startcombat, phrase 100)

```
** The Shadow Warden greets challengers
mob echo {D$I's eyes flare to life, twin points of violet fire in the darkness.{x
say So... another fool seeks to claim my sanctum.
say Very well. I shall grant you the death you crave.
mob echo {M$I draws a blade of condensed shadow from thin air!{x
```

### Script 2: Shadow Slash (fight, phrase 25)

```
** Primary special attack — shadow-infused melee strike
if isdelay $i
  break
endif

if rand 60
  ** Shadow slash
  mob echoat $n {M$I strikes at you with $l shadow blade!{x
  mob echoaround $n {M$I strikes at $N with $l shadow blade!{x
  mob damage $n 100 200
  mob delay 1
else
  ** Draining touch — heals the boss slightly
  mob echoat $n {D$I reaches out and touches your chest — you feel your life drain away!{x
  mob echoaround $n {D$I reaches out and touches $N's chest — a dark glow passes between them!{x
  mob damage $n 75 125
  mob at 1 mob cast 'cure light'
  mob delay 1
endif
```

### Script 3: Shadow Bolt (fight, phrase 30)

```
** Ranged attack — uses delay to check the mob isn't busy
if isdelay $i
  break
endif

mob echo {Y$I raises a hand, gathering a sphere of roiling darkness!{x
mob delay 4

** The actual damage happens in the delay trigger (Script 3b)
```

**Script 3b: Shadow Bolt Delivery (delay, phrase 100)**

```
** Deliver the shadow bolt
if !fighting $i
  ** Combat ended while charging — cancel the attack
  mob echo {DThe sphere of darkness fizzles and dissipates.{x
  break
endif

mob echo {R$I hurls the shadow bolt!{x
mob echoat $n {RThe bolt of darkness slams into you!{x
mob echoaround $n {RThe bolt of darkness slams into $N!{x
mob damage $n 150 250 lethal
```

### Script 4: Phase 2 — Shadow Nova (hpcnt, phrase 75)

```
** One-shot phase transition at 75% HP
if carries $i oo90001oo
  break
endif
mob oload 90001

mob echo {M$I staggers, then laughs — a cold, hollow sound.{x
say You actually drew blood. Impressive.
say Let me show you what shadow truly means.
mob echo {R$I slams $l blade into the ground!{x
mob echo {R****** A nova of shadow energy erupts outward! ******{x
mob damage all 75 150
```

### Script 5: Phase 3 — Summon Shade (hpcnt, phrase 50)

```
** One-shot phase transition at 50% HP — summon a minion
if carries $i oo90002oo
  break
endif
mob oload 90002

mob echo {D$I kneels and presses $l palm to the floor.{x
mob echo {MInky darkness pools beneath $I's hand, and a shape rises from it!{x
mob mload 90010
mob echo {DA shade materializes — a twisted mirror of $I's form!{x
say My shadow fights alongside me now. Can you handle us both?
```

### Script 6: Phase 4 — Enrage (hpcnt, phrase 25)

```
** One-shot phase transition at 25% HP — enrage
if carries $i oo90003oo
  break
endif
mob oload 90003

mob echo {R$I howls in rage!{x
mob echo {R$L form distorts as shadow energy surges uncontrollably!{x
mob echo {R****** The chamber grows dark as the Warden loses control! ******{x
say ENOUGH! I WILL CONSUME YOU!
mob damage all 100 200 lethal
** Self-heal from the surge
mob at 1 mob cast 'cure critical'
mob at 1 mob cast 'cure critical'
```

### Script 7: Death (death, phrase 100)

```
** Boss death — loot, cleanup, and drama
** Clean up phase markers
mob junk oo90001oo
mob junk oo90002oo
mob junk oo90003oo

** Dramatic death
mob echo {D$I drops to $l knees, the shadow blade dissolving into mist.{x
mob echo {M...no... the darkness... was supposed to be... eternal...{x
mob echo {WA blinding flash of light fills the chamber as the shadows are purged!{x
mob echo {YWhen your vision clears, all that remains is a pile of dark ash and glinting metal.{x

** Guaranteed loot
mob oload 90050
drop pendant

** Quest item for active questers
if queststate $n 90001 == active
  mob oload 90060
  drop essence
  mob echoat $n {YA swirling dark essence rises from the ashes — your quest objective!{x
endif

** Rare drop: 15% chance
if rand 15
  mob oload 90099
  drop blade
  mob echo {WA magnificent blade materializes from the dissipating shadows!{x
endif
```

---

## Tips and Common Pitfalls

### Always Guard Against Overlapping Actions

Every `fight` trigger script should start with:

```
if isdelay $i
  break
endif
```

Without this, a mob with three fight scripts could fire all three in the same round, dumping a wall of text and damage on the player.

### Guard One-Shot Triggers

Every `hpcnt` script that should only fire once needs the marker object pattern:

```
if carries $i oo<vnum>oo
  break
endif
mob oload <vnum>
```

Use a different marker vnum for each phase.

### damage vs damage lethal

By default, `mob damage` will not kill the target — it stops at 1 HP. Use `lethal` or `kill` when the attack should be able to finish someone off. Reserve `lethal` for boss specials, not routine fight scripts, to avoid frustrating instant deaths.

### Multiple Fight Scripts

You can attach multiple `fight` trigger scripts to the same mob. Each one is evaluated independently each round. This is how the Shadow Warden has both a melee attack (25% per round) and a ranged attack (30% per round). The `isdelay` guard ensures only one fires per round.

### Fight Trigger Phrase is a Percentage

The phrase on a `fight` trigger is the percent chance it fires each combat round. A phrase of `100` fires every round — which is almost never what you want for special attacks. Good ranges:

| Mob Role | Suggested Phrase | Fires Roughly |
|:---------|:-----------------|:--------------|
| Trash mob | 10–15 | Rarely, keeps things moving |
| Mini-boss | 20–30 | Once every few rounds |
| Boss | 25–35 per script | Frequent but not overwhelming |

---

## What You Learned

In this tutorial you covered:

- **Combat triggers**: `startcombat`, `fight`, `hpcnt`, `death`, and the `attack_*` / `defense` / `barrier` / `damage` variants
- **Fight scripts**: Using percent-chance triggers with `isdelay` guards to pace special attacks
- **HP phases**: The `hpcnt` trigger with marker-object guards for one-shot phase transitions
- **Death scripts**: Conditional loot with `oload`, quest checks, rare drops with `rand`, and cleanup with `junk`
- **Defensive triggers**: Reacting to incoming damage with `damage`, `check_damage`, and `barrier` triggers
- **Dynamic combat**: Color codes, `echoat`/`echoaround`/`echo` for perspective, wind-up attacks with `delay`, and multi-step sequences with `queue`
- **The complete boss pattern**: Multiple scripts working together — combat opening, phased abilities, varied attacks, and a cinematic death

---

## Next Steps

- Read [Advanced Patterns](advanced-patterns.md) to learn about sub-script calls, cross-entity communication, and async execution patterns
- See the [Shared Commands Reference](../shared-commands.md) for all available combat commands (`damage`, `entercombat`, `stopcombat`, `peace`, and more)
- Review the [Mobile Programs Triggers](../mobile-programs/mprog-triggers.md) reference for the complete list of available triggers
- Explore the [Mobile Programs Examples](../mobile-programs/mprog-examples.md) for more real-world combat scripts from live areas
