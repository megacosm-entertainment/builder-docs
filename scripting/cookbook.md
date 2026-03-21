---
layout: default
title: Cookbook
nav_order: 11
parent: Scripting
---

# Scripting Cookbook
{: .no_toc }

Copy-paste recipe patterns for common scripting tasks. Each recipe includes the
goal, complete script, trigger type, and a brief explanation. Adapt vnums,
messages, and values to your area.

<details open markdown="block">
  <summary>Table of Contents</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## Beginner Recipes

### Greeting NPC

**Goal:** A mob welcomes every player who enters the room.

**Trigger:** `greet 100` (fires 100 % of the time when a character enters)

```
** Greeting NPC — greet trigger
if ispc $n
  mob say Welcome to the village, $N!
endif
```

**Attach:** `addprog <vnum> greet 100`

The `ispc` check prevents the mob from greeting other NPCs.

---

### Random Emote Mob

**Goal:** A mob performs a random idle emote every so often.

**Trigger:** `random 15` (15 % chance each tick)

```
** Random emote — random trigger
switch dice 1d4
case 1
  mob emote polishes a glass behind the bar.
case 2
  mob emote glances around the room suspiciously.
case 3
  mob emote whistles a quiet tune.
default
  mob emote stretches and yawns.
endswitch
```

**Attach:** `addprog <vnum> random 15`

`dice 1d4` rolls a four-sided die. Adjust the `random` percentage to control
frequency — lower values fire less often.

---

### Item With Wear Message

**Goal:** An object displays a message when equipped.

**Trigger:** `wear` (fires when worn)

```
** Wear message — object wear trigger
obj echoat $n The amulet hums softly as you clasp it around your neck.
obj echoaround $n $N puts on a strange amulet, and it begins to glow.
```

**Attach:** `addprog <vnum> wear 100` on the object.

Use `obj echoat` for the wearer and `obj echoaround` for everyone else.

---

### Room Description on Entry

**Goal:** The room displays flavour text when a player enters.

**Trigger:** `greet 100` (room program)

```
** Room atmosphere — room greet trigger
if ispc $n
  room echoat $n The air is thick with the scent of pine. Somewhere above,
  room echoat $n a hawk circles lazily in the pale sky.
endif
```

**Attach:** `addprog <vnum> greet 100` on the room.

Keep entry text short — it fires on every entry and should enhance, not
overwhelm.

---

## Intermediate Recipes

### Shopkeeper With Custom Dialogue

**Goal:** A shopkeeper responds to keywords and handles purchases with flavour.

**Triggers:** `greet 100`, `speech quest`, `buy`

```
** Shopkeeper — greet trigger
if ispc $n
  mob say Browse my wares, $N. Ask about a {Cquest{x if you're looking for work.
endif
```

```
** Shopkeeper — speech trigger (keyword "quest")
if ispc $n
  mob say The mines north of here are infested with spiders.
  mob say Bring me ten spider fangs and I'll make it worth your while.
  mob say I pay {Y200 gold{x per fang.
endif
```

```
** Shopkeeper — buy trigger
mob echoat $n The shopkeeper wraps your purchase in brown paper.
mob echoaround $n $N completes a purchase with the shopkeeper.
```

**Attach:**
- `addprog <greet_vnum> greet 100`
- `addprog <speech_vnum> speech quest`
- `addprog <buy_vnum> buy 100`

---

### Locked Door With Key Check

**Goal:** A mob guards a door and only opens it for players carrying the right
key item.

**Trigger:** `speech open`

```
** Door guard — speech trigger (keyword "open")
if !ispc $n
  end
endif

if carries $n 12050
  mob echo The guard inspects $N's key and nods.
  mob echoat $n The guard unlocks the heavy iron door.
  ** Unlock and open the north exit (direction 0)
  mob alterexit north locked false
  mob alterexit north closed false
  mob delay 20
else
  mob say You need the warden's key to pass.
endif
```

```
** Door guard — delay trigger (re-locks the door)
mob echo The guard locks the door again.
mob alterexit north closed true
mob alterexit north locked true
```

**Attach:**
- `addprog <speech_vnum> speech open`
- `addprog <delay_vnum> delay 100`

The door re-locks after 20 pulses (5 seconds). Adjust `mob delay` for longer
timers.

---

### Buff Token With Timer

**Goal:** A consumable item grants a temporary stat buff via a token that
expires after a set duration.

**Object trigger:** `use` (on the potion object)

```
** Buff potion — object use trigger
if hastoken $n 14000
  obj echoat $n You already have that effect active.
  end
endif

obj echoat $n You drink the potion. Warmth spreads through your limbs.
obj echoaround $n $N drinks a shimmering potion and glows briefly.
obj token give $n 14000
obj purge $i
```

**Token trigger:** `expire` (on token 14000)

```
** Buff token — expire trigger
token echoat $(self.owner) The warmth fades from your body.
token altermob $(self.owner) damroll - 5
```

**Token trigger:** `token_given` (on token 14000)

```
** Buff token — token_given trigger
token altermob $(self.owner) damroll + 5
token settimer $i 240
```

**Setup:**
1. Create token vnum 14000 — attach the `token_given` and `expire` programs.
2. Create the potion object — attach the `use` program.
3. `settimer` 240 = 60 seconds (240 pulses ÷ 4 pulses/sec).

See [Variables & Tokens](variables-and-tokens.md) for full token details.

---

### Mob That Remembers Players

**Goal:** A mob recognises returning players and reacts differently on
subsequent visits.

**Trigger:** `greet 100`

```
** Remembering NPC — greet trigger
if !ispc $n
  end
endif

if vardefined $(enactor) met_innkeeper
  mob say Welcome back, $N! The usual room?
else
  mob say A new face! I am Marta, keeper of this inn.
  mob say Speak to me anytime you need a room.
  mob varseton $(enactor) met_innkeeper bool true
  mob varsaveon $(enactor) met_innkeeper
endif
```

**Attach:** `addprog <vnum> greet 100`

`varsaveon` persists the variable across reboots. Without it, the mob forgets
on restart. See [Variables & Tokens](variables-and-tokens.md) for scoping
rules.

---

## Advanced Recipes

### Multi-Stage Quest Chain

**Goal:** A quest NPC guides the player through three stages: accept → kill
target → return for reward.

**Triggers:** `speech quest`, `greet 100`

```
** Quest giver — speech trigger (keyword "quest")
if !ispc $n
  end
endif

if questdone $n 50100
  mob say You've already completed this quest, $N.
  end
endif

if hasquest $n 50100
  mob say You're already on the quest. Bring me proof of the beast's defeat.
  end
endif

mob say A dire wolf terrorises the northern farms.
mob say Slay it and bring me its pelt as proof.
mob questaccept $n 50100
mob echoat $n {G[Quest Accepted: The Dire Wolf]{x
```

```
** Quest giver — greet trigger
if !ispc $n
  end
endif

if !hasquest $n 50100
  end
endif

if questdone $n 50100
  end
endif

** Stage check: player has the pelt?
if carries $n 50110
  mob echo $N presents the bloodied pelt to the hunter.
  mob remove $n 50110 1
  mob questcomplete $n 50100
  mob award $n experience 5000
  mob award $n gold 200
  mob echoat $n {G[Quest Complete: The Dire Wolf]{x
  mob echoat $n {YYou receive 5000 experience and 200 gold.{x
  mob say Well done, $N! The farms are safe again.
else
  mob say Still hunting, $N? The dire wolf roams the forest to the north.
endif
```

**Setup:**
1. Create quest vnum 50100 with a single stage (objective: kill dire wolf mob).
2. The dire wolf mob's `death` trigger should `oload 50110` (the pelt) into the
   room or onto the killer.
3. Attach both programs to the quest giver mob.

See [Quest Programs](quest-programs/) for quest system details.

---

### Boss Fight With Phases

**Goal:** A boss mob changes behaviour at health thresholds — summons adds at
75 %, enrages at 25 %.

**Triggers:** `hpcnt 75`, `hpcnt 25`, `death`

```
** Boss — hpcnt trigger (phrase 75 = fires when HP drops below 75%)
if vardefined $i phase1_done
  end
endif

mob varset phase1_done bool true
mob echo {R$I roars and slams the ground!{x
mob echo Cracks split the floor as minions crawl forth!
mob mload 50205
mob mload 50205
mob echoat $n {Y[Phase 2: Minions have appeared!]{x

** Make the adds aggressive
mob at here mob kill $r
```

```
** Boss — hpcnt trigger (phrase 25 = fires when HP drops below 25%)
if vardefined $i phase2_done
  end
endif

mob varset phase2_done bool true
mob echo {R$I lets out a blood-curdling shriek!{x
mob echo {RThe beast's eyes burn with fury — it has enraged!{x
mob altermob $i damroll + 15
mob altermob $i hitroll + 10
mob echoat $n {Y[Phase 3: The boss has enraged!]{x
```

```
** Boss — death trigger
mob echo {WThe beast collapses with a final, shuddering breath.{x
mob echo {WA gleaming chest materialises where it fell.{x
mob oload 50220
mob gecho {YThe fearsome beast in the depths has been slain!{x
```

**Attach:**
- `addprog <phase1_vnum> hpcnt 75`
- `addprog <phase2_vnum> hpcnt 25`
- `addprog <death_vnum> death 100`

The `vardefined` guards prevent phases from re-triggering. Variables reset when
the mob repops.

---

### Instanced Content

**Goal:** Spawn a personal dungeon instance for a player and teleport them in.

**Trigger:** `speech enter` (on a portal NPC or object)

```
** Instance launcher — speech trigger (keyword "enter")
if !ispc $n
  end
endif

** Check cooldown token
if hastoken $n 60010
  mob say The portal has not yet recharged for you.
  end
endif

mob echo The portal flares to life!
mob echoat $n You step into the swirling light...
mob echoaround $n $N vanishes into the portal.

** Spawn dungeon and transfer player
mob spawndungeon 60000
mob transfer $n $(dungeon.entrance)

** Set a cooldown token (expires in 10 minutes = 2400 pulses)
mob token give $n 60010
```

**Cooldown token (60010) — token_given trigger:**

```
** Cooldown token — token_given trigger
token settimer $i 2400
```

**Cooldown token (60010) — expire trigger:**

```
** Cooldown token — expire trigger
token echoat $(self.owner) The dungeon portal is ready for you again.
```

**Setup:**
1. Design the dungeon blueprint (vnum 60000) with rooms, mobs, and a boss.
2. Create cooldown token 60010 with the timer programs above.
3. Attach the speech program to the portal NPC.

See [Dungeon Programs](dungeon-programs/) and
[Instance Programs](instance-programs/) for full details.

---

### Area-Wide Event System

**Goal:** Trigger an area-wide event at a specific game hour — a nightly
undead invasion.

**Trigger:** `random 100` on an area program (fires every tick; we check the
hour ourselves)

```
** Area event controller — random trigger
** Only fire at hour 22 (night begins)
if hour != 22
  end
endif

** Only once per night
if vardefined $i invasion_active
  end
endif

area varset invasion_active bool true
area zecho {DThe ground trembles. The dead are rising!{x

** Spawn undead in key rooms
area at 70001 area mload 70050
area at 70001 area mload 70050
area at 70005 area mload 70051
area at 70010 area mload 70052

** Schedule cleanup at dawn
area delay 96
```

```
** Area event controller — delay trigger (dawn cleanup)
area zecho {YThe first rays of dawn scatter the remaining undead.{x

** Purge any surviving event mobs
area at 70001 area purge 70050
area at 70005 area purge 70051
area at 70010 area purge 70052

area varclear invasion_active
```

**Attach:**
- `addprog <random_vnum> random 100`
- `addprog <delay_vnum> delay 100`

Both programs go on the **area** itself. Adjust `hour` and room vnums for your
zone. See [Area Programs](area-programs/) for area-level scripting.

---

### Economy Interactions — Charging and Banking

**Goal:** An NPC service that charges gold — a healer who takes payment from
the player's bank account or carried gold.

**Trigger:** `speech heal`

```
** Healer NPC — speech trigger (keyword "heal")
if !ispc $n
  end
endif

** Already at full health?
if hp $n >= maxhp $n
  mob say You look healthy to me, $N.
  end
endif

** Cost scales with level
mob varset cost integer $[$(enactor.level) * 10]

** Try bank first, then carried gold
if bank $n >= $<cost>
  mob chargebank $n $<cost>
  if lastreturn == 1
    mob say Payment of {Y$<cost> gold{x taken from your bank, $N.
    mob restore $n
    mob echoat $n A warm glow envelops you. Your wounds close.
    mob echoaround $n The healer lays hands on $N, who glows briefly.
  else
    mob say Something went wrong with the transaction.
  endif
elseif gold $n >= $<cost>
  mob chargemoney $n $<cost> gold
  if lastreturn == 1
    mob say Payment of {Y$<cost> gold{x received, $N.
    mob restore $n
    mob echoat $n A warm glow envelops you. Your wounds close.
    mob echoaround $n The healer lays hands on $N, who glows briefly.
  else
    mob say I couldn't collect the payment.
  endif
else
  mob say Healing costs {Y$<cost> gold{x. You don't have enough.
  mob say Try the bank — I accept transfers too.
endif
```

**Attach:** `addprog <vnum> speech heal`

`lastreturn` checks whether the charge command succeeded. Always verify before
delivering the service. See [Shared Commands Reference](shared-commands.md) for
all economy commands.

---

### Bank Transfer Between Players

**Goal:** An NPC banker that lets one player wire gold to another.

**Trigger:** `speech wire`

```
** Banker — speech trigger (keyword "wire")
** Expected syntax: "wire <amount> <player>"
if !ispc $n
  end
endif

mob say Wire transfers cost a 5%% handling fee.
mob say To proceed, give me the gold you wish to send.
mob varset wire_target string $t
mob varseton $(enactor) pending_wire bool true
```

```
** Banker — bribe trigger (player gives gold)
if !ispc $n
  end
endif

if !vardefined $(enactor) pending_wire
  mob say I didn't ask for gold. Say {Cwire{x to start a transfer.
  end
endif

** $phrase contains the gold amount given
mob varset amount integer $phrase
mob varset fee integer $[$<amount> / 20]
mob varset total integer $[$<amount> + $<fee>]

mob echoat $n Transfer of {Y$<amount> gold{x with {Y$<fee> gold{x fee processed.
mob wiretransfer $n $<wire_target> $<amount>
mob varclearon $(enactor) pending_wire
```

**Attach:**
- `addprog <speech_vnum> speech wire`
- `addprog <bribe_vnum> bribe 1`

---

### Reputation-Gated Vendor

**Goal:** A vendor who adjusts prices and dialogue based on the player's faction standing, and awards reputation on purchase.

**Trigger:** `greet 100` and `buy 100`

```
** Reputation vendor — greet trigger
** Checks the player's standing with the Artisan Guild (vnum 1#50)
if !ispc $n
  end
endif

if hasreputation $n 1#50
  if varnumber $n reputation > 200
    mob say {YWelcome back, Guildmaster $N!{x I have some special items for you.
    bow $n
  elseif varnumber $n reputation > 50
    mob say Good to see you, $N. Let me show you the full catalog.
  else
    mob say Ah, a new member. Welcome to the Artisan Guild shop.
  endif
else
  mob say Hmm. You are not a member of the Artisan Guild.
  mob say {cSpeak to Guildmaster Thorn in Achaeus to join.{x
endif
```

```
** Reputation vendor — buy trigger
** Awards a small amount of reputation for each purchase
if !ispc $n
  end
endif

if hasreputation $n 1#50
  mob award $n reputation 'Artisan Guild' 5
  mob echoat $n {DYour standing with the {WArtisan Guild{D improves slightly.{x
endif
```

**Attach:**
- `addprog <greet_vnum> greet 100`
- `addprog <buy_vnum> buy 100`

**Key points:**
- `hasreputation $n 1#50` checks if the player has *any* standing with faction widevnum `1#50`
- `varnumber $n reputation` reads the player's current reputation points (requires the reputation variable on the player)
- `mob award $n reputation 'Artisan Guild' 5` grants 5 reputation points; the system handles rank-ups automatically
- Combine with shop `min_reputation_rank` in `shedit` to gate individual items by rank

---

### Kill-Based Reputation With Opposing Factions

**Goal:** Killing mobs grants reputation with one faction and loses reputation with another.

**Trigger:** `death 100` (on the mob)

```
** Orc Raider — death trigger
** Grants Guardsmen rep, reduces Orc Horde rep
if !ispc $n
  end
endif

mob award $n reputation 'City Guardsmen' 25
mob echoat $n {GYou gain {Y25{G reputation with the {WCity Guardsmen{G.{x

mob award $n reputation 'Orc Horde' -15
mob echoat $n {rYou lose {Y15{r reputation with the {WOrc Horde{r.{x
```

**Attach:** `addprog <vnum> death 100`

**Key points:**
- Negative values in `award` reduce reputation — the system handles rank-downs
- Place this on every mob in the faction for consistent gains
- Use `mob_reputations` on the mob prototype via `medit` for automated kill-based grants instead of scripts (no script needed)

---

## Quick Reference

| Recipe | Difficulty | Trigger(s) | Key Commands |
|--------|-----------|------------|--------------|
| Greeting NPC | Beginner | `greet` | `say` |
| Random Emote | Beginner | `random` | `emote`, `switch` |
| Wear Message | Beginner | `wear` | `echoat`, `echoaround` |
| Room Entry Text | Beginner | `greet` (room) | `echoat` |
| Custom Shopkeeper | Intermediate | `greet`, `speech`, `buy` | `say`, `echo` |
| Locked Door | Intermediate | `speech`, `delay` | `alterexit`, `carries` |
| Buff Token | Intermediate | `use`, `token_given`, `expire` | `token give`, `settimer`, `altermob` |
| Remembering Mob | Intermediate | `greet` | `varseton`, `varsaveon`, `vardefined` |
| Quest Chain | Advanced | `speech`, `greet` | `questaccept`, `questcomplete`, `award` |
| Boss Phases | Advanced | `hpcnt`, `death` | `mload`, `altermob`, `vardefined` |
| Instanced Content | Advanced | `speech` | `spawndungeon`, `transfer`, `token give` |
| Area Event | Advanced | `random`, `delay` (area) | `zecho`, `mload`, `at` |
| Economy Healer | Advanced | `speech` | `chargebank`, `chargemoney`, `lastreturn` |
| Bank Transfer | Advanced | `speech`, `bribe` | `wiretransfer` |
| Reputation Vendor | Advanced | `greet`, `buy` | `hasreputation`, `award`, `varnumber` |
| Kill Reputation | Advanced | `death` | `award` (positive + negative) |

---

## See Also

- [Scripting Basics](scripting-basics.md) — Language fundamentals
- [Quick Codes](quick-codes.md) — `$n`, `$i`, `$t` and other expansions
- [Variables & Tokens](variables-and-tokens.md) — Variable scoping and token system
- [Shared Commands Reference](shared-commands.md) — Full command list
- [Ifchecks Reference](ifchecks-reference.md) — All conditional checks
- [Troubleshooting](troubleshooting.md) — Debugging and common mistakes
