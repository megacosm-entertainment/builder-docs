---
layout: default
title: Examples
nav_order: 3
parent: Mobile Programs
grand_parent: Scripting
---

# Mobile Program Examples
{: .no_toc }

Five practical examples of mobile programs, from simple to complex. Each example includes the complete script, trigger attachment, and a line-by-line explanation.

For the full trigger and command references, see [Triggers](mprog-triggers.md) and [Commands](mprog-commands.md).

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Example 1: Greeting NPC

A friendly NPC who welcomes players when they enter the room.

**Trigger:** `grall` — Fires every time a player character enters the room.

**Attachment:**

```
medit <mob vnum>
addprog <script vnum> grall 100
```

The phrase `100` means 100% chance — the script fires every time.

**Script:**

```
** Greeting script for a friendly shopkeeper
** Fires on grall (every player entering the room)
if sex $n == 1
  say Welcome to my shop, sir! Browse to your heart's content.
elseif sex $n == 2
  say Welcome to my shop, ma'am! Browse to your heart's content.
else
  say Welcome to my shop! Browse to your heart's content.
endif
mob echoaround $n The shopkeeper smiles warmly at $n.
```

**How it works:**

- `if sex $n == 1` — Checks if the entering player (`$n`) is male. `sex` returns 1 for male, 2 for female.
- `say` — The mob speaks. No prefix needed for regular commands like `say`, `smile`, `wave`.
- `mob echoaround $n` — Sends a message to everyone in the room *except* the triggering player.
- `$n` in text is replaced with the player's name.

{: .note }
> `grall` fires for every player who enters. If you only want the mob to react once per visit, use `greet` instead — it fires only if the mob is not already fighting and has not recently greeted someone.

---

## Example 2: Quest-Giving NPC

An NPC who checks whether the player has completed a task (tracked by carrying a specific item) and rewards them accordingly.

**Triggers:** Two scripts — one for `grall` (greeting/checking), one for `speech` (responding to keywords).

**Attachment:**

```
medit <mob vnum>
addprog <greet script vnum> grall 100
addprog <speech script vnum> speech help
```

**Greeting script (grall trigger, phrase `100`):**

```
** Quest NPC: check if player has the quest item
if carries $(enactor) 265515
  say I see you have found the claw I was looking for!
  mob remove $(enactor) 265515 1
  mob junk claw
  mob oload 265867
  give obelisk $(enactor)
  say Many thanks, $n!
else
  say Greetings, $n! Are you here to buy a pet or to {Rhelp{C me?{x
endif
```

**Speech script (speech trigger, phrase `help`):**

```
** Responds when a player says "help"
say Well, I would be delighted if you could bring me an owl's claw!
say I'll even reward you handsomely.
```

**How it works:**

- `if carries $(enactor) 265515` — Checks if the player is carrying object vnum 265515 (the owl's claw). `$(enactor)` is the long form of `$n`.
- `mob remove $(enactor) 265515 1` — Removes one instance of object 265515 from the player.
- `mob junk claw` — Destroys the claw from the mob's possession.
- `mob oload 265867` — Creates the reward object (vnum 265867) in the mob's inventory.
- `give obelisk $(enactor)` — Gives the reward to the player.
- `{R...{x` — Color codes: `{R` starts red text, `{C` starts cyan, `{x` resets to default.
- The `speech` trigger with phrase `help` fires whenever a player says anything containing the word "help".

{: .tip }
> For more complex quest tracking, use token values instead of carrying items. See [Variables and Tokens](../variables-and-tokens.md) for `tokenvalue` checks.

---

## Example 3: Shopkeeper with Custom Dialogue

An innkeeper who greets visitors, handles room rental through speech and bribe triggers, and takes back keys when guests leave.

**Triggers:** `grall`, `speech` (multiple phrases), `bribe`, `repop`

**Attachment:**

```
medit <mob vnum>
addprog <greet script vnum> grall 100
addprog <speech script vnum> speech yes
addprog <bribe script vnum> bribe 3000
addprog <repop script vnum> repop 100
```

**Greet script (grall trigger, phrase `100`):**

```
** Check if the arriving guest has a room key
if carries $n 265638
  or carries $n 265637
  say Leaving so soon? Hope you enjoyed your stay!
  wave
  mob remove $n 265638
  mob remove $n 265637
  mob echoat $n $I takes your key from you.
  mob junk all
  mob oload 265638
  mob oload 265637
else
  mob echo {x$I smiles happily.
  say Will you be purchasing a room today, $n?
endif
```

**Speech script (speech trigger, phrase `yes`):**

```
** Player said "yes" — quote the price
say Very well then, the fee is 3000 silver for the night.
say If you'll be so kind as to give me the appropriate amount
say of currency I'll gladly bestow upon you a key to a room.
```

**Bribe script (bribe trigger, phrase `3000`):**

```
** Player gave at least 3000 silver coins
if carries $n 265638
  or carries $n 265637
  say Sorry, $n, I already gave you a key.
  give 3000 silver $n
else
  give key $n
  say Go east, south, up, and then north.
  say See which door the key fits and that's your room for the night.
  say Be sure to return the key when you leave.
  smile
endif
```

**Repop script (repop trigger, phrase `100`):**

```
** Load fresh keys when the innkeeper spawns
mob oload 265637
mob oload 265638
```

**How it works:**

- **`bribe` trigger** fires when a player gives gold to the mob. Phrase `3000` means the trigger only fires if the bribe is 3000 or more.
- **`carries $n 265638`** checks if the player already has a key (prevents double-issuing).
- **`mob remove`** and **`mob junk`** handle taking back and destroying the key.
- **`mob oload`** in the repop script ensures the innkeeper always has keys after respawning.
- Multiple `speech` triggers can be attached to the same mob with different phrases — e.g., `yes`, `ok`, `sure` can all point to the same script.

---

## Example 4: Combat Boss with HP Phases

A dragon boss that uses special attacks during combat and triggers a dramatic phase change when its HP drops below 20%.

**Triggers:** `grall`, `fight` (multiple), `hpcnt`, `death`

**Attachment:**

```
medit <mob vnum>
addprog <greet script vnum> grall 100
addprog <tail script vnum> fight 25
addprog <breath script vnum> fight 30
addprog <heal script vnum> fight 30
addprog <phase script vnum> hpcnt 20
addprog <death script vnum> death 100
```

**Greet script (grall trigger, phrase `100`):**

```
** Boss greets players who enter and initiates combat
mob echoat $n {D$I grins widely with $l toothy maw at your arrival.{x
say So $N, you have come to die, have you?
say I shall appease your curiosity!
scream
mob kill $n
```

**Tail swipe script (fight trigger, phrase `25`):**

```
** 25% chance each round — tail swipe attack
if isdelay $i
  break
endif
if rand 75
  mob echo {Y$I rears $l tail readying to strike{x
  mob echoat $n {W$I swings $l tail at you with extraordinary strength!{x
  mob echoaround $n {W$I swings $l tail at $N with extraordinary strength!{x
  if rand 50
    mob echoat $n {R$I's tail hits you with a crushing force!{x
    mob echoaround $n {R$I's tail hits $N with a crushing force!{x
    mob damage $n 200 500 lethal
    mob delay 1
  else
    mob echoat $n You deftly maneuver out of the way!
    mob echoaround $n $N deftly maneuvers out of the way!
    mob delay 1
  endif
endif
```

**Breath weapon script (fight trigger, phrase `30`):**

```
** 30% chance each round — choose a special attack
if isdelay $i
  break
endif
if rand 60
  cast 'fire breath'
  mob delay 1
  break
endif
if rand 40
  mob call 263555 $n
  mob delay 1
  break
endif
```

**Self-heal script (fight trigger, phrase `30`):**

```
** Self-preservation: cure debuffs and heal
if isdelay $i
  break
endif
if affected $i 'blind'
  cast 'cure blind'
  mob delay 1
  break
endif
if affected $i 'poison'
  cast 'cure poison'
  mob delay 1
  break
endif
if hpcnt $i 20
  cast 'cure critical'
  mob delay 1
  break
endif
```

**HP Phase script (hpcnt trigger, phrase `20`):**

```
** Fires when HP drops below 20% — dramatic phase transition
if carries $i oo263562oo
  break
endif
mob oload 263562
mob echoat $n {W$I roars loudly, deafening you momentarily!{x
mob echoaround $n {W$I roars loudly, $N covers $m ears in pain!{x
mob echo The ground trembles as $I spreads $l wings!
mob echo {W****** BOOOOOM!! ******{x
mob echo {DAn explosion of shadowy tendrils envelope $I, restoring $l anew!{x
mob at 1 mob cast heal
mob at 1 mob cast heal
mob at 1 mob cast heal
mob at 1 mob cast heal
```

**Death script (death trigger, phrase `100`):**

```
** Drop special loot on death
mob junk oo263562oo
mob oload 263561
drop eye
```

**How it works:**

- **Multiple `fight` triggers** run independently each combat round. The phrase is the percent chance per round.
- **`if isdelay $i` / `break`** prevents overlapping actions — if the mob already has a pending delay, skip this round's action.
- **`mob delay 1`** sets a 1-pulse cooldown so the mob does not spam actions.
- **`hpcnt` trigger** fires when the mob's HP percentage falls below the phrase value (20%). The `if carries $i oo263562oo` check ensures the phase only triggers once by loading a marker object.
- **`mob at 1 mob cast heal`** — `mob at 1` temporarily moves to room 1 (a void room) to cast heal privately, avoiding player-visible spam.
- **`mob call 263555 $n`** calls another script by vnum as a subroutine, passing `$n` as a parameter.
- **`mob damage $n 200 500 lethal`** deals between 200-500 damage to the target. The `lethal` flag means it can deal the finishing blow.

{: .warning }
> Always use `mob delay` between combat actions to prevent the mob from performing multiple special attacks in the same pulse. Without delays, scripts can stack and fire all at once.

---

## Example 5: Patrol Guard with Speech Responses

A town guard who patrols randomly, responds to speech, and enforces access control based on citizenship.

**Triggers:** `random`, `speech`, `grall`, `repop`

**Attachment:**

```
medit <mob vnum>
addprog <idle script vnum> random 2
addprog <speech script vnum> speech help
addprog <grall script vnum> grall 100
addprog <repop script vnum> repop 100
```

**Random idle script (random trigger, phrase `2`):**

```
** 2% chance per tick — random idle actions
if rand 80
  whistle
elseif rand 60
  say People should really clean up after themselves.
  say There's so much work to do!
elseif rand 40
  mob echo $I wipes the sweat off of his brow.
elseif rand 20
  mob echo $I mops up a mess on the cobblestones.
endif
```

**Speech script (speech trigger, phrase `help`):**

```
** Respond when a player asks for help
say I'm just a humble guard, $n.
say If you need directions, try asking at the town hall.
say And stay out of trouble!
```

**Grall script (grall trigger, phrase `100`):**

```
** Check arriving players for citizenship
if room $(here) >= 265200
  and room $(here) <= 266199
  if carries $(enactor) 265800
    or isimmort $(enactor)
  elseif tokenvalue $(enactor) 265817 0 >= 1000
    if rand 10
      mob echoat $(enactor) $(self.short.capital) salutes you.
      mob echoaround $(enactor) $(self.short.capital) salutes $(enactor.short).
      say Hail Champion!
    endif
  else
    say You are no Achaean citizen, leave the castle immediately!
    mob transfer $n 265351
  endif
endif
```

**Repop script (repop trigger, phrase `100`):**

```
** Equip sword when spawning
mob oload 265500
hold sword
```

**How it works:**

- **`random` with phrase `2`** means a 2% chance per tick. Low values prevent spam — the guard occasionally does something rather than constantly acting.
- **Nested `if rand`** inside a random trigger creates weighted outcomes. The first check (`rand 80`) succeeds 80% of the time; if it fails, the next is checked, and so on.
- **`room $(here) >= 265200`** checks if the guard is within a specific vnum range (the castle area).
- **`carries $(enactor) 265800`** checks for a citizenship token item.
- **`tokenvalue $(enactor) 265817 0 >= 1000`** checks if the player has earned enough reputation points on token vnum 265817.
- **`$(self.short.capital)`** resolves to the mob's short description with the first letter capitalized.
- **`mob transfer $n 265351`** teleports non-citizens to the castle entrance.

{: .tip }
> Use low `random` percentages (1-5%) for ambient behavior. Higher values make NPCs act too frequently and feel unnatural.

---

## Tips for Writing Mob Programs

1. **Start simple.** Get a basic script working before adding complexity. Test after every change.
2. **Use comments.** Mark sections with `**` comments so you (and others) can understand the script later.
3. **Guard against repeat firing.** Use `carries $i` checks with marker objects or `isdelay $i` checks to prevent triggers from stacking.
4. **Use `mob delay`** between combat actions to prevent action spam.
5. **Test with `mpstat`.** Use `mpstat <mob name>` to see all attached programs and their trigger types.
6. **Keep output clear.** Use `mob echoat` for private messages and `mob echoaround` for public ones — avoid flooding the room with redundant text.
7. **Use color sparingly.** Color codes like `{R`, `{Y`, `{x` add emphasis but overuse makes text hard to read.

## See Also

- [Triggers](mprog-triggers.md) — All 153 mob triggers
- [Commands](mprog-commands.md) — All 167 mob commands
- [Shared Commands](../shared-commands.md) — Detailed command documentation
- [Variables and Tokens](../variables-and-tokens.md) — Quick codes, entity references, token system
- [If-Checks Reference](../ifchecks-reference.md) — Conditional checks available in scripts
- [Scripting Basics](../scripting-basics.md) — Fundamentals of the scripting system
