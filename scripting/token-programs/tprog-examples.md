---
layout: default
title: Examples
nav_order: 3
parent: Token Programs
grand_parent: Scripting
---

# Token Program Examples
{: .no_toc }

These examples demonstrate common token patterns. Each is a complete, working script that you can adapt to your own tokens. They cover the most frequent use cases: timed buffs, quest tracking, cooldowns, combat modifiers, and persistent effects.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Example 1: Timed Buff with Expiry Message

A strength buff that lasts 60 ticks, displays a message when it expires, and cleans itself up. This is the most common token pattern — attach a token, apply an affect, let the timer handle removal.

### Token Setup

```
tpedit create 5001
name strength_surge power_buff
desc a surge of raw strength
type affect
flags purge_death purge_quit
timer 60
```

### Script: token_given (initialization)

Trigger: `token_given 100`

```
** Fires when this token is first attached to a character.
** Apply the strength buff affect and notify the player.
token addaffectname $n 'strength surge' 60 apply str 3 0
token echoat $n {YRaw strength surges through your muscles!{x
token echoaround $n {Y$N's muscles bulge with unnatural strength!{x
```

### Script: expire (cleanup)

Trigger: `expire 100`

```
** Fires when the 60-tick timer reaches zero.
** Remove the affect, display messages, and destroy the token.
token stripaffectname $n 'strength surge'
token echoat $n {xThe surge of strength fades from your body.{x
token echoaround $n {x$N's muscles return to normal.{x
token junk self
```

### How It Works

1. A script on another entity (e.g., a potion object or NPC) uses `token give $n 5001` to attach the buff.
2. The `token_given` trigger fires immediately, applying the affect and showing messages.
3. After 60 ticks, the `expire` trigger fires, strips the affect, shows the fade message, and junks the token.
4. If the player dies or quits before the timer expires, the `PURGE_DEATH` and `PURGE_QUIT` flags automatically remove the token.

---

## Example 2: Quest Progress Tracker

A token that tracks a multi-step quest using value slots and variables. The player must collect 5 wolf pelts and deliver them to the quest giver.

### Token Setup

```
tpedit create 5010
name wolf_hunt_quest quest_tracker
desc the wolf hunt quest tracker
type quest
flags purge_quit
timer 0
value 0 0
```

Value slot 0 tracks pelts collected (0–5).

### Script: token_given (quest start)

Trigger: `token_given 100`

```
** Initialize quest state
token adjust $i 0 = 0
token echoat $n {YQuest Started: {WThe Wolf Hunt{x
token echoat $n {xCollect 5 wolf pelts and return them to the huntsman.{x
```

### Script: get (pelt collection)

Trigger: `get wolf_pelt`

The phrase `wolf_pelt` matches when the player picks up an object with that keyword.

```
** Check if this is actually a wolf pelt (vnum 3050)
if objvnum($p) != 3050
  break
endif

** Increment pelt counter
token adjust $i 0 + 1

** Check progress
if tokenvalue($i, 0) >= 5
  token echoat $n {GYou have collected all 5 pelts! Return to the huntsman.{x
else
  token echoat $n {YWolf pelts: $0i(tokenvalue($i, 0))/5{x
endif
```

### Script: speech (quest turn-in)

Trigger: `speech pelts`

```
** Only respond if the speaker is the token owner and near the huntsman
if isnpc($n)
  break
endif

** Check if the quest giver (vnum 3001) is in the room
if !mobhere(3001)
  break
endif

** Check if player has all 5 pelts
if tokenvalue($i, 0) < 5
  token echoat $n {xThe huntsman says 'You still need $0i(5 - tokenvalue($i, 0)) more pelts.'{x
  break
endif

** Complete the quest
token echoat $n {GThe huntsman says 'Excellent work! Here is your reward.'{x
token award $n 500 xp
token award $n 100 gold
token echoat $n {YYou receive 500 experience and 100 gold!{x
token junk self
```

### How It Works

1. An NPC gives the quest token: `mob give $n 5010`.
2. The `token_given` trigger initializes the counter.
3. Each time the player picks up a wolf pelt, the `get` trigger increments value[0] and reports progress.
4. When the player says "pelts" near the quest giver with 5 pelts collected, the `speech` trigger completes the quest.

---

## Example 3: Cooldown Ability

A token that grants a custom combat ability ("Power Strike") with a 10-tick cooldown. The player activates it with a custom verb, and the token enforces the cooldown timer.

### Token Setup

```
tpedit create 5020
name power_strike cooldown_ability
desc a devastating power strike technique
type skill
flags purge_death
timer 0
value 0 0
```

Value[0]: 0 = ready, 1 = on cooldown.

### Script: verb (activation)

Trigger: `verb powerstrike`

```
** Check if on cooldown
if tokenvalue($i, 0) == 1
  token echoat $n {RPower Strike is still on cooldown!{x
  break
endif

** Check if in combat
if !fighting($n)
  token echoat $n {xYou must be in combat to use Power Strike.{x
  break
endif

** Execute the ability
token echoat $n {WYou channel your strength into a devastating blow!{x
token echoaround $n {W$N channels raw power into a devastating strike!{x
token echobattlespam $n {R*** POWER STRIKE ***{x
token damage $t 150 bash

** Set cooldown
token adjust $i 0 = 1
token settimer $i 10
```

### Script: expire (cooldown reset)

Trigger: `expire 100`

```
** Reset cooldown flag
token adjust $i 0 = 0
token settimer $i 0
token echoat $n {GPower Strike is ready again.{x
```

### Script: combatstyle (combat integration)

Trigger: `combatstyle 100`

```
** During combat style selection, announce availability
if tokenvalue($i, 0) == 0
  token echoat $n {x[{GPower Strike: READY{x]{x
endif
```

### How It Works

1. The player types `powerstrike` during combat (matched by the `verb` trigger).
2. The script checks cooldown state, verifies the player is in combat, then deals 150 bash damage.
3. Value[0] is set to 1 (cooldown) and the timer is set to 10 ticks.
4. When the timer expires, the `expire` trigger resets value[0] to 0 and notifies the player.
5. The `combatstyle` trigger provides passive feedback showing when the ability is ready.

---

## Example 4: Combat Modifier / Damage Boost

A token that boosts fire damage by 25% during combat. It reacts to damage events and modifies the output. Also applies a damage boost when the owner's HP drops below 25%.

### Token Setup

```
tpedit create 5030
name fire_mastery flame_boost
desc mastery over fire magic
type affect
flags purge_death
timer 0
value 0 25
```

Value[0] stores the damage boost percentage (25%).

### Script: fight (combat round flavor)

Trigger: `fight 30`

```
** 30% chance each round to show flavor text
token echoat $n {rFlames dance along your weapons...{x
token echobattlespam $n {R$N's weapons flicker with hungry flames.{x
```

### Script: hit (damage modification)

Trigger: `hit 100`

```
** Check if the damage type is fire
** $register1 contains the damage amount
** $register2 contains the damage type

** Only boost fire-type damage
if $register2 != fire
  break
endif

** Calculate bonus damage (25% of original)
token varset bonus integer $0i($register1 * tokenvalue($i, 0) / 100)

** Apply the extra damage
token damage $t $bonus fire
token echoat $n {R(+$bonus fire damage from Fire Mastery){x
```

### Script: hpcnt (low HP bonus)

Trigger: `hpcnt 25`

```
** When HP drops below 25%, activate a stronger fire aura
if tokenvalue($i, 0) >= 50
  ** Already boosted, don't stack
  break
endif

token adjust $i 0 = 50
token echoat $n {RDesparate flames erupt around you — fire damage doubled!{x
token echoaround $n {R$N is engulfed in desperate flames!{x
```

### How It Works

1. The token passively rides on the character, providing a 25% fire damage boost.
2. The `fight` trigger adds occasional combat flavor text (30% chance per round).
3. The `hit` trigger fires on every hit, checks if the damage is fire-type, and adds bonus damage.
4. When HP drops below 25%, the `hpcnt` trigger kicks in and doubles the fire bonus to 50%.

---

## Example 5: Persistent Curse That Survives Logout

A curse token that persists across logout, reboot, and death. It weakens the player over time, gets stronger each login, and can only be removed by visiting a specific NPC.

### Token Setup

```
tpedit create 5040
name shadow_curse dark_affliction
desc a lingering shadow curse
type affect
flags permanent
timer 0
value 0 1
value 1 0
```

- Value[0]: Curse intensity (starts at 1, increases each login).
- Value[1]: Total logins while cursed (tracking).
- Flags: `PERMANENT` means it cannot be casually removed.

### Script: token_given (initial curse)

Trigger: `token_given 100`

```
** Apply initial curse effect
token addaffectname $n 'shadow curse' -1 apply str -1 0
token addaffectname $n 'shadow curse' -1 apply dex -1 0
token echoat $n {DA dark shadow wraps around your soul...{x
token echoaround $n {D$N shudders as a dark shadow envelops them.{x
token varsave $n shadow_curse_active on
token varseton $n shadow_curse_active bool true
token varsaveon $n shadow_curse_active on
```

### Script: login (curse intensifies)

Trigger: `login 100`

```
** Each login, the curse gets slightly worse
token adjust $i 0 + 1
token adjust $i 1 + 1

** Reapply the curse affects at the new intensity
token stripaffectname $n 'shadow curse'
token varset intensity integer $0i(tokenvalue($i, 0))
token addaffectname $n 'shadow curse' -1 apply str $0i(0 - $intensity) 0
token addaffectname $n 'shadow curse' -1 apply dex $0i(0 - $intensity) 0

if tokenvalue($i, 0) >= 5
  token echoat $n {DThe shadow curse has grown powerful. You feel terribly weakened.{x
  token addaffectname $n 'shadow curse' -1 apply con -2 0
else
  token echoat $n {DThe shadow curse pulses within you... it feels stronger.{x
endif
```

### Script: quit (save state)

Trigger: `quit 100`

```
** Ensure the curse data is saved
token saveplayer $n
```

### Script: speech (removal check)

Trigger: `speech cure`

```
** Check if the healer NPC (vnum 4001) is present
if !mobhere(4001)
  break
endif

** The healer can remove the curse, but it costs gold based on intensity
token varset cost integer $0i(tokenvalue($i, 0) * 200)

if gold($n) < $cost
  token echoat $n {xThe healer says 'I can remove this curse, but I need $cost gold.'{x
  break
endif

** Remove the curse
token deduct $n $cost gold
token stripaffectname $n 'shadow curse'
token echoat $n {WThe healer chants over you, and the shadow dissolves!{x
token echoaround $n {WThe healer lifts the dark curse from $N!{x
token varclearon $n shadow_curse_active
token junk self
```

### How It Works

1. A mob or trap applies the curse: `mob give $n 5040`.
2. The `token_given` trigger applies initial stat penalties and saves state.
3. The `PERMANENT` flag prevents casual removal — players can't just strip the affect.
4. Each time the player logs in, the `login` trigger strengthens the curse (higher stat penalties).
5. The `quit` trigger ensures the player data is saved with the curse intact.
6. To remove the curse, the player must find the healer NPC and say "cure" — the cost scales with curse intensity.
7. Because the token has no `PURGE_DEATH`, `PURGE_QUIT`, or `PURGE_REBOOT` flags and is `PERMANENT`, it truly persists through everything.

---

## Tips for Token Scripts

- **Always `junk self` last.** Once a token junks itself, script execution stops immediately.
- **Use `token_given` for initialization.** It fires right when the token is attached — perfect for applying initial effects.
- **Use `expire` for cleanup.** Pair it with `settimer` or the token's built-in timer for timed effects.
- **Use value slots for simple counters.** Faster and simpler than variables for integer data.
- **Use variables for complex data.** Strings, entity references, and persistent data need `varset`/`varsave`.
- **Set appropriate purge flags.** Decide upfront whether the token should survive death, quit, reboot, or rift travel.
- **Use `SINGULAR` to prevent stacking.** If a buff should only have one instance, set the `SINGULAR` flag.
- **Test with `tokinfo`.** The in-game `tokinfo` command shows all tokens on a character, their values, timers, and variables.
