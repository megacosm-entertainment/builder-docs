---
layout: default
title: Examples
nav_order: 3
parent: Object Programs
grand_parent: Scripting
---

# Object Program Examples
{: .no_toc }

Five complete, working examples that demonstrate common object program patterns. Each example includes the trigger configuration, the full script, and an explanation of how it works.

For trigger details see [Triggers](oprog-triggers.md). For command details see [Commands](oprog-commands.md) and the [Shared Commands Reference](../shared-commands.md).

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Example 1: Equipment with On-Wear/Remove Effects

**Scenario:** An Amulet of Frost Protection that grants cold resistance when worn and removes it when taken off. It also shows a visual effect to the room.

### Wear Program

| Setting | Value |
|:--------|:------|
| Trigger | `wear` |
| Phrase | `100` |

```
** Amulet of Frost Protection — wear trigger
** Grants cold resistance and shows visual feedback

obj echoat $n {CAs you clasp the amulet, a shimmering frost barrier forms around you.{x
obj echoaround $n {CA shimmering frost barrier forms around $N as $e clasps the amulet.{x

** Add the frost protection affect (named affect, 0 duration = until removed)
obj addaffectname $n frost_protection 0

** Change the object description to show it is active
obj stringobj $i short a {Cglowing{x amulet of frost protection
```

### Remove Program

| Setting | Value |
|:--------|:------|
| Trigger | `remove` |
| Phrase | `100` |

```
** Amulet of Frost Protection — remove trigger
** Strips the cold resistance and restores the object description

obj echoat $n {xThe frost barrier around you fades as you remove the amulet.{x
obj echoaround $n The frost barrier around $N fades.

** Remove the frost protection affect
obj stripaffectname $n frost_protection

** Restore original description
obj stringobj $i short an amulet of frost protection
```

### How It Works

1. When a player wears the amulet, the `wear` trigger fires with 100% chance.
2. The script sends personal feedback to the wearer (`echoat`) and a room message to everyone else (`echoaround`).
3. `addaffectname` applies a named affect called `frost_protection` with duration `0`, which means it lasts indefinitely until manually stripped.
4. `stringobj` updates the item's short description so it visually shows as "glowing" in inventory.
5. When the player removes the amulet, the `remove` trigger fires, strips the affect, and restores the original description.

**Key pattern:** Pair `wear` and `remove` triggers to create toggle effects. Always clean up in the `remove` trigger what you applied in `wear`.

---

## Example 2: Consumable Potion

**Scenario:** A Potion of Greater Healing that restores HP and mana when used, with a chance to grant a temporary regeneration buff. The potion destroys itself after use.

### Use Program

| Setting | Value |
|:--------|:------|
| Trigger | `use` |
| Phrase | `100` |

```
** Potion of Greater Healing — use trigger
** Restores health and mana, chance for regen buff, then self-destructs

** Visual effects
obj echoat $n {GYou drink the shimmering potion. Warmth floods through your body!{x
obj echoaround $n {G$N drinks a shimmering potion and looks revitalized.{x

** Restore the player
obj restore $n

** 30% chance for a bonus regeneration buff
if rand(100) <= 30
  obj echoat $n {YThe potion's magic lingers — you feel a gentle healing warmth.{x
  obj addaffectname $n potion_regen 60
endif

** Destroy the potion after use
obj selfdestruct
```

### How It Works

1. The `use` trigger fires when a player uses the potion (100% chance).
2. `restore` fully heals the player's HP, mana, and movement.
3. A random check (`rand(100) <= 30`) gives a 30% chance to also apply a regeneration buff lasting 60 ticks.
4. `selfdestruct` removes the potion from the game — this is the standard pattern for consumables.

**Key pattern:** For one-use items, always end with `obj selfdestruct`. Any commands after `selfdestruct` will not execute, so place it last.

---

## Example 3: Trapped Chest

**Scenario:** A trapped chest that explodes when someone tries to take an item from it, dealing damage. A rogue with the "detect traps" skill can disarm it. Once disarmed, the trap does not fire again.

### Get Program (Trap)

| Setting | Value |
|:--------|:------|
| Trigger | `get` |
| Phrase | `100` |

```
** Trapped Chest — get trigger
** Fires when someone takes an item from the chest

** Check if the trap has already been disarmed
if var($i, trap_disarmed) == 1
  ** Trap already disarmed, do nothing
else
  ** The trap fires!
  obj echo {R{CLICK!{x {rA hidden mechanism triggers!{x

  ** Deal damage to the thief
  obj echoat $n {RA blast of fire erupts from the chest, engulfing you!{x
  obj echoaround $n {RA blast of fire erupts from the chest, engulfing $N!{x
  obj damage $n 150

  ** 50% chance the trap resets, 50% it breaks
  if rand(100) <= 50
    obj echo {xThe mechanism clicks, resetting itself.{x
  else
    obj echo {xThe trap mechanism breaks apart with a snap.{x
    obj varset trap_disarmed 1
  endif
endif
```

### Verb Program (Disarm)

| Setting | Value |
|:--------|:------|
| Trigger | `verb` |
| Phrase | `disarm` |

```
** Trapped Chest — verb trigger for 'disarm'
** Allows rogues to safely disarm the trap

if var($i, trap_disarmed) == 1
  obj echoat $n The trap has already been disarmed.
else
  ** Check if the character is a rogue
  if class($n) == thief
    obj echoat $n {YYou carefully examine the mechanism and find the trip wire.{x
    obj echoaround $n $N carefully examines the chest, probing its mechanism.

    ** Skill check — higher level = better chance
    if rand(100) <= level($n)
      obj echoat $n {GWith a deft twist, you disarm the trap!{x
      obj echoaround $n {G$N disarms a hidden trap on the chest!{x
      obj varset trap_disarmed 1
    else
      obj echoat $n {RYou fumble the mechanism — the trap fires!{x
      obj damage $n 100
    endif
  else
    obj echoat $n {xYou don't know enough about traps to disarm this.{x
  endif
endif
```

### Showcommands Program

| Setting | Value |
|:--------|:------|
| Trigger | `showcommands` |
| Phrase | `100` |

```
** Show the 'disarm' verb as available on this object
obj showcommand $n disarm
```

### How It Works

1. The `get` trigger fires when someone takes an item from the chest.
2. The script checks a variable (`trap_disarmed`) to see if the trap has been disabled.
3. If armed, it deals 150 damage and has a 50/50 chance of resetting or breaking.
4. The `verb` trigger on "disarm" provides an alternate path — rogues can attempt to disarm the trap using a level-based skill check.
5. The `showcommands` trigger lets the game display "disarm" as an available action when players examine the chest.
6. The `trap_disarmed` variable persists across trigger firings, so once disarmed (by skill or by breakage), the trap stays inactive.

**Key pattern:** Use `varset`/`var()` to track object state. Combine `get` or `open` triggers with damage for traps. Provide alternate paths with `verb` triggers.

---

## Example 4: Key That Unlocks on Speech

**Scenario:** A crystal key that only activates when someone speaks the correct passphrase near it. Once activated, it unlocks a specific door and then shatters.

### Speech Program

| Setting | Value |
|:--------|:------|
| Trigger | `speech` |
| Phrase | `starlight guide me` |

```
** Crystal Key — speech trigger
** Activates when someone says the passphrase

** Check that the speaker is carrying the key
if carries($n, $i) == 0
  ** They don't have the key, ignore
else
  ** The key responds to the passphrase
  obj echoat $n {CThe crystal key in your pack suddenly flares with brilliant light!{x
  obj echoaround $n {CSomething in $N's pack flares with brilliant light!{x

  ** Brief delay for dramatic effect
  obj queue 2 obj echo {WThe crystal key rises into the air, spinning slowly.{x
  obj queue 4 obj echo {WA beam of light shoots from the key toward the northern door!{x

  ** Unlock and open the north exit after the dramatic pause
  obj queue 6 obj alterexit north flags remove locked
  obj queue 6 obj alterexit north flags remove closed
  obj queue 6 obj echo {YThe northern door swings open with a soft chime.{x

  ** Key shatters after use
  obj queue 8 obj echo {xThe crystal key shatters into a thousand glittering fragments.{x
  obj queue 8 obj selfdestruct
endif
```

### How It Works

1. The `speech` trigger fires when anyone in the room says something containing "starlight guide me" (keyword substring match).
2. The script verifies the speaker is carrying the key with `carries()`.
3. `queue` schedules commands at future times (measured in pulses) to create a dramatic sequence — the key glows, rises, shoots light, and then the door unlocks.
4. `alterexit` modifies the north exit to remove both the `locked` and `closed` flags.
5. After the sequence, `selfdestruct` destroys the key.

**Key pattern:** Use `speech` triggers with keyword phrases for password/passphrase puzzles. Use `queue` to create timed sequences of events. Always check `carries()` or similar to ensure the speaker has the relevant item.

---

## Example 5: Growing/Evolving Weapon

**Scenario:** A Soulblade that grows more powerful with each kill. It tracks kills using a variable, and at certain thresholds it evolves — changing its name, description, and combat abilities.

### Fight Program (Combat Effect)

| Setting | Value |
|:--------|:------|
| Trigger | `fight` |
| Phrase | `15` |

```
** Soulblade — fight trigger (15% per round)
** Shows combat flavor based on evolution tier

if var($i, tier) >= 3
  obj echobattlespam {RThe Soulblade howls with bloodlust, cleaving through defenses!{x
  obj damage $(victim) 30
elseif var($i, tier) >= 2
  obj echobattlespam {MThe Soulblade pulses with dark energy!{x
  obj damage $(victim) 15
elseif var($i, tier) >= 1
  obj echobattlespam {mThe Soulblade whispers as it strikes.{x
  obj damage $(victim) 5
else
  obj echobattlespam {xThe dormant blade stirs slightly.{x
endif
```

### Afterkill Program (Kill Tracking and Evolution)

| Setting | Value |
|:--------|:------|
| Trigger | `afterkill` |
| Phrase | `100` |

```
** Soulblade — afterkill trigger
** Tracks kills and evolves the weapon at milestones

** Increment the kill counter
if var($i, kills) == ""
  obj varset kills 1
else
  obj varset kills $math(var($i, kills) + 1)
endif

** Save the variable so it survives reboots
obj persist kills

** Check evolution thresholds
if var($i, kills) == 10
  ** Tier 1: Awakened
  obj varset tier 1
  obj persist tier

  obj echoat $n {M{The blade trembles and a faint glow appears along its edge!{x
  obj echoaround $n $N's blade begins to glow with a faint, eerie light.
  obj stringobj $i short a {mfaintly glowing{x Soulblade
  obj stringobj $i name soulblade awakened blade glowing

elseif var($i, kills) == 50
  ** Tier 2: Hungering
  obj varset tier 2
  obj persist tier

  obj echoat $n {R{The Soulblade shudders violently — dark energy crackles along its length!{x
  obj echoaround $n $N's blade erupts with dark, crackling energy!
  obj gecho {RA dark pulse of energy ripples across the land...{x
  obj stringobj $i short a {Rdark, crackling{x Soulblade
  obj stringobj $i name soulblade hungering dark crackling blade

elseif var($i, kills) == 100
  ** Tier 3: Devouring
  obj varset tier 3
  obj persist tier

  obj echoat $n {R{The Soulblade screams — its hunger is insatiable!{x
  obj echoaround $n $N's weapon transforms, radiating waves of dark power!
  obj gecho {DA wave of dread washes over the world as something terrible awakens...{x
  obj stringobj $i short the {DDevouring{x Soulblade
  obj stringobj $i name soulblade devouring terrible dark blade
endif

** Regular kill message
obj echoat $n {xThe blade drinks deep. ({W$var($i, kills){x souls claimed){x
```

### Wear Program (Status Display)

| Setting | Value |
|:--------|:------|
| Trigger | `wear` |
| Phrase | `100` |

```
** Soulblade — wear trigger
** Display current status when equipped

if var($i, tier) >= 3
  obj echoat $n {DThe Devouring Soulblade sings with terrible joy as you draw it.{x
  obj echoat $n {xSouls claimed: {W$var($i, kills){x
elseif var($i, tier) >= 2
  obj echoat $n {RDark energy crackles as you grip the Hungering Soulblade.{x
  obj echoat $n {xSouls claimed: {W$var($i, kills){x
elseif var($i, tier) >= 1
  obj echoat $n {mThe Awakened Soulblade hums softly as you draw it.{x
  obj echoat $n {xSouls claimed: {W$var($i, kills){x
else
  obj echoat $n {xThe dormant blade feels cold and heavy in your hand.{x
  obj echoat $n {xThis weapon hungers for souls. Slay enemies to awaken it.{x
endif
```

### How It Works

1. The `fight` trigger fires 15% of combat rounds. Based on the weapon's tier variable, it deals bonus damage ranging from 0 (dormant) to 30 (tier 3).
2. The `afterkill` trigger fires every time the wielder kills something. It increments a `kills` counter and checks against evolution thresholds (10, 50, 100 kills).
3. At each threshold, the weapon evolves: its tier variable increases, its name and description change via `stringobj`, and dramatic messages are sent.
4. `persist` ensures the `kills` and `tier` variables survive server reboots.
5. The `wear` trigger displays the weapon's current status whenever the player equips it.

**Key pattern:** Use variables with `varset` and `persist` to track long-term state. Use `afterkill` for kill-counting mechanics. Use `stringobj` to dynamically change item appearance. Use `echobattlespam` for combat effects so players can filter them if desired.

---

## Common Patterns Summary

| Pattern | Triggers | Key Commands |
|:--------|:---------|:-------------|
| Toggle effect on equip | `wear` + `remove` | `addaffectname`, `stripaffectname` |
| One-use consumable | `use`, `eat`, or `drink` | `restore`, `selfdestruct` |
| Trap mechanic | `get`, `open`, or `preget` | `damage`, `varset` |
| Password puzzle | `speech` or `sayto` | `alterexit`, `queue`, `carries()` |
| Evolving item | `afterkill` + `fight` | `varset`, `persist`, `stringobj` |
| Custom verb | `verb` + `showcommands` | `showcommand`, class/level checks |
| Timed sequence | any trigger | `queue`, `delay` |
| Prevent action | any `pre*` trigger | `break` to cancel the action |
