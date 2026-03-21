---
layout: default
title: Triggers
nav_order: 1
parent: Mobile Programs
grand_parent: Scripting
---

# Mobile Program Triggers
{: .no_toc }

This page documents all 153 triggers available for mobile programs (mob progs). Triggers define *when* a script fires — the event that causes your code to run.

For general trigger concepts and phrase matching, see [Scripting Basics](../scripting-basics.md). For the commands you can use inside a triggered script, see [Commands](mprog-commands.md).

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## How Triggers Work

When you attach a script to a mobile, you specify:

1. **Trigger type** — The event name (e.g., `greet`, `speech`, `fight`)
2. **Phrase** — Controls *when* within that event the script fires

The phrase meaning varies by trigger type:

| Phrase Type | Meaning | Example |
|------------|---------|---------|
| Percent chance | Fires randomly with that % probability | `50` = 50% chance |
| Keyword match | Fires when text contains the keyword | `hello` matches "hello world" |
| `*` (wildcard) | Always fires | `*` |
| Number threshold | Fires when value meets condition | `25` for HP < 25% |
| Direction number | Matches exit direction | `0` = north |
| Vnum match | Matches object/mob by vnum | `3001` |

## Entity Bindings

When a trigger fires, your script can reference the entities involved:

| Variable | Type | Description |
|----------|------|-------------|
| `$i` / `$(self)` | Mobile | The scripted NPC (self) |
| `$n` / `$(enactor)` | Mobile | The character who triggered the event |
| `$t` / `$(victim)` | Mobile | The victim/target of the action |
| `$r` / `$(random)` | Mobile | A random character in the room |
| `$p` / `$(obj1)` | Object | Primary object involved |
| `$o` / `$(obj2)` | Object | Secondary object involved |
| `$q` / `$(token)` | Token | Token parameter |
| `$(here)` | Room | Current room of the NPC |
| `$(phrase)` | String | The matched phrase/argument |

See [Variables and Tokens](../variables-and-tokens.md) for the complete reference.

---

## Trigger Quick Reference

All 153 mob triggers at a glance:

| Trigger | When It Fires | Phrase |
|---------|--------------|-------|
| `act` | Another character performs a visible action | keyword substring — Phrase is matched as a substring against... |
| `afterkill` | Called after someome kills a target.  TODO: Damage will beco... | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `animate` | When the mob is animated/summoned | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `assist` | When the mob assists another in combat | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack` | When the mob performs any attack | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_backstab` | When the mob performs a backstab | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_bash` | When the mob performs a bash | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_behead` | When the mob performs a behead | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_bite` | When the mob performs a bite | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_blackjack` | When the mob performs a blackjack | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_circle` | When the mob performs a circle | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_counter` | When the mob performs a counter-attack | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_cripple` | When the mob performs a cripple | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_dirtkick` | When the mob performs a dirt kick | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_disarm` | When the mob performs a disarm | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_intimidate` | When the mob performs intimidate | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_kick` | When the mob performs a kick | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_rend` | When the mob performs a rend | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_slit` | When the mob performs a slit | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_smite` | When the mob performs a smite | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_tailkick` | When the mob performs a tail kick | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_trample` | When the mob performs a trample | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `attack_turn` | When the mob performs a turn undead | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `barrier` | When an energy barrier blocks an attack | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `board` | When a character boards a vehicle/mount | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `bribe` | When a player gives gold to the mob | gold amount ≥ threshold — Phrase is a gold amount. Trigger f... |
| `buy` | When a player buys from the mob's shop | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `check_buyer` | Called when a stock item is not an object, used to check whe... | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `check_damage` | When damage is calculated against the mob | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `contract_complete` | When a contract is completed | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `custom_price` | Called when a stock item has custom pricing | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `damage` | When the mob takes damage | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `death` | When the mob dies | percent chance — Phrase is percent chance. Fires when entity... |
| `death_protection` | When death protection activates | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `death_timer` | When a death timer expires | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `defense` | Only on tokens | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `delay` | When a previously set delay timer expires | percent chance — Phrase is percent chance. Fires after a scr... |
| `drink` | When a character drinks in the room | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `drop` | When a character drops an object in the room | vnum/name match (room prog) or percent — On room prog: match... |
| `eat` | When a character eats in the room | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `emote` | NIB : 20140508 : untargeted emote | exact name match — Phrase must exactly match the emote name ... |
| `emoteat` | NIB : 20140508 : targeted emote | exact name match — Phrase must exactly match the targeted em... |
| `entry` | When the mob enters a new room | percent chance — Phrase is percent chance. Fires when the sc... |
| `exall` | When a character exits the room (any direction) | direction number (exact) — Phrase is a direction number. Lik... |
| `examine` | When a character examines something | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `exit` | When a character leaves via a specific direction | direction number (exact) — Phrase is a direction number (0=n... |
| `extract` | When the mob is extracted/removed from the world | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `fight` | Each combat round while the mob is fighting | percent chance — Phrase is percent chance. Fires each combat... |
| `flee` | When a character flees from the room | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `forcedismount` | When a character is forcibly dismounted | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `get` | When a character picks up an object | vnum/name match (obj prog) or percent — On obj prog: matches... |
| `give` | When a character gives an object to the mob | vnum/name match — Phrase matches the vnum or keyword of the ... |
| `grall` | When a character enters (fires for all players) | percent chance — Phrase is a percent chance (1-100). Like gr... |
| `greet` | When a character enters (fires once per character) | percent chance — Phrase is a percent chance (1-100). Fires w... |
| `grouped` | When a character joins a group | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `hidden` | After the mob has hidden (mob and token only) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `hide` | Act of hiding (mob, object and token) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `hit` | When the mob lands a hit on a target | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `hitgain` | On each HP regeneration tick | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `hpcnt` | When mob's HP falls below a threshold | HP% < threshold — Phrase is an HP percentage threshold. Trig... |
| `inspect` | When a character inspects the mob | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `inspect_custom` | Called when asking a shopkeeper to inspect a custom stock it... | exact keyword match — Phrase must exactly match the shop sto... |
| `kill` | When a character initiates combat with the mob | percent chance — Phrase is percent chance. Fires when entity... |
| `land` | When a flying character lands | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `level` | When a character levels up in the room | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `login` | When a player logs in near the mob | direct execution — No phrase matching. Fires when a player l... |
| `lore` | When a character uses lore on an item | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `lorex` | Extended lore check | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `managain` | On each mana regeneration tick | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `mount` | When a character mounts the mob | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `move_char` | When the mob is moved by an external force | direction name string — Phrase is the direction name (e.g., ... |
| `movegain` | On each movement regeneration tick | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `multiclass` | Called when a player multiclasses | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `postquest` | Called after all quest rewards and messages are given | percent chance — Called after all quest rewards and messages... |
| `practice` | When a character practices at the mob | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `preanimate` | Before the mob is animated (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `preassist` | Before the mob assists (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prebite` | Before a bite attack (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prebuy` | Before a shop purchase (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prebuy_obj` | Before buying a specific object (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `predeath` | Before the mob dies (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `predismount` | Before dismounting (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `predrink` | Before drinking (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `preeat` | Before eating (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `preflee` | Before fleeing (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prehide` | NIB 20140522 - trigger for testing if you can hide yourself ... | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prehidein` | NIB 20140522 - trigger for testing if the container or mob y... | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prekill` | Before initiating combat (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `premount` | Before mounting (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prepractice` | Before practicing (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prepracticeother` | Before practicing on another (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prepracticethat` | Before practicing a specific skill (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prequest` | Allows custom checking for questing, also allows setting the... | percent chance — Allows custom checking for questing and set... |
| `prereckoning` | Before a reckoning event (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prerehearse` | Before rehearsing a song (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prerenew` | Used by "RENEW" to get QP cost and whether the item can be r... | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prerest` | Before resting (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `preresurrect` | Before resurrection (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `preround` | Before each combat round (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `presell` | Before selling to a shop (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `presit` | Before sitting (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `presleep` | Before sleeping (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prestand` | Before standing (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `pretrain` | Before training (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prewake` | Before waking (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `prewimpy` | Before wimpy flee triggers (can prevent) | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `pull` | When a character pulls an object | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `pullon` | NIB : 20070121 | keyword substring — Phrase is matched as a substring against... |
| `pulse` | Every server pulse (frequent timer) | percent chance — Phrase is percent chance. Fires every game ... |
| `quest_cancel` | When a quest is cancelled | percent chance — Fires when a quest is cancelled. |
| `quest_complete` | Prior to awards being given, called when the quest turned in... | percent chance — Prior to awards, when quest is turned in co... |
| `quest_incomplete` | Prior to awards being given, called when the quest turned in... | percent chance — Prior to awards, when quest is turned in in... |
| `quest_part` | Used to generate a custom quest part when selected. | percent chance — Used to generate a custom quest part when s... |
| `random` | DEPRECATE | percent chance — Phrase is a percent chance (1-100). Fires o... |
| `reckoning` | When a reckoning event fires | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `regen` | Custom regenerations | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `regen_hp` | Modification on ALL hp regens | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `regen_mana` | Modification on ALL mana regens | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `regen_move` | Modification on ALL move regens | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `remort` | Called when a player remorts | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `renew` | Used by "RENEW" | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `renew_list` | Used by "RENEW LIST" | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `repop` | When the mob repops/spawns into the world | percent chance — Phrase is percent chance. Fires when entity... |
| `rest` | When a character rests in the room | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `restocked` | When the mob's shop restocks | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `restore` | When the mob is restored to full stats | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `resurrect` | When the mob is resurrected | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `save` | When a nearby player saves | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `sayto` | NIB : 20070121 | keyword substring — Phrase is matched as a substring against... |
| `showcommands` | When listing available commands | direct execution — No phrase matching. Fires to display avai... |
| `sit` | When a character sits in the room | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `skill_berserk` | When the mob uses berserk | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `skill_sneak` | When the mob uses sneak | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `skill_warcry` | When the mob uses warcry | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `sleep` | When a character sleeps in the room | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `speech` | When a character says something in the room | keyword substring — Phrase is matched as a substring against... |
| `spell_cure` | When a cure spell is cast on/near the mob | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `spell_dispel` | When a dispel spell is cast on/near the mob | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `spellcast` | When a spell is cast in the room | exact name match — Phrase is matched exactly against the spe... |
| `spellreflect` | When a spell is reflected by the mob | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `stand` | When a character stands in the room | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `startcombat` | When combat begins for the mob | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `takeoff` | When a character removes/takes off equipment | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `throw` | When a character throws an object | vnum/name match — Phrase matches the vnum or keyword of the ... |
| `tick` | Every game tick | percent chance — Phrase is percent chance. Fires every game ... |
| `toxingain` | On toxin/poison tick | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `ungrouped` | When a character leaves a group | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `verb` | When a custom verb is used on the mob | keyword match — Phrase matches the custom verb keyword. Scri... |
| `wake` | When a character wakes in the room | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `wear` | When a character puts on equipment | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `whisper` | When a character whispers to the mob | keyword substring — Phrase is matched as a substring against... |
| `wimpy` | When the mob's wimpy threshold is reached | percent chance — Numeric 1–100 for percent chance; or keywor... |
| `xpgain` | When a character gains experience near the mob | percent chance — Numeric 1–100 for percent chance; or keywor... |

---

## General Triggers

### `board`

| Property | Value |
|----------|-------|
| **When it fires** | When a character boards a vehicle/mount |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `bribe`

| Property | Value |
|----------|-------|
| **When it fires** | When a player gives gold to the mob |
| **Phrase** | gold amount ≥ threshold — Phrase is a gold amount. Trigger fires if bribe amount ≥ the phrase number. |
| **Also available on** | Mob, Tok |

**Example:** Accept gold payment

Trigger: `bribe` — Phrase: `5000`

```
say Thank you for the generous donation, $n.
mob remember $n
```

---

### `buy`

| Property | Value |
|----------|-------|
| **When it fires** | When a player buys from the mob's shop |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

**Example:** After a shop purchase

Trigger: `buy` — Phrase: `100`

```
say Pleasure doing business, $n!
```

---

### `check_buyer`

| Property | Value |
|----------|-------|
| **When it fires** | When checking if buyer can receive a shop item |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |
| **Notes** | Called when a stock item is not an object, used to check whether the buyer can GET the item |

---

### `contract_complete`

| Property | Value |
|----------|-------|
| **When it fires** | When a contract is completed |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `custom_price`

| Property | Value |
|----------|-------|
| **When it fires** | When a shop item has custom pricing |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |
| **Notes** | Called when a stock item has custom pricing |

---

### `delay`

| Property | Value |
|----------|-------|
| **When it fires** | When a previously set delay timer expires |
| **Phrase** | percent chance — Phrase is percent chance. Fires after a script-set delay completes. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** Delayed action

Trigger: `delay` — Phrase: `100`

```
say ...as I was saying, the treasure is hidden in the cave.
```

---

### `drink`

| Property | Value |
|----------|-------|
| **When it fires** | When a character drinks in the room |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** When someone drinks

Trigger: `drink` — Phrase: `100`

```
say That looks refreshing!
```

---

### `drop`

| Property | Value |
|----------|-------|
| **When it fires** | When a character drops an object in the room |
| **Phrase** | vnum/name match (room prog) or percent — On room prog: matches vnum/keyword of dropped object. On obj prog: uses percent chance. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** When an item is dropped

Trigger: `drop` — Phrase: `100`

```
get all
```

---

### `eat`

| Property | Value |
|----------|-------|
| **When it fires** | When a character eats in the room |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** When someone eats

Trigger: `eat` — Phrase: `100`

```
say Enjoying the food, $n?
```

---

### `examine`

| Property | Value |
|----------|-------|
| **When it fires** | When a character examines something |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Tok |

---

### `flee`

| Property | Value |
|----------|-------|
| **When it fires** | When a character flees from the room |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `forcedismount`

| Property | Value |
|----------|-------|
| **When it fires** | When a character is forcibly dismounted |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `get`

| Property | Value |
|----------|-------|
| **When it fires** | When a character picks up an object |
| **Phrase** | vnum/name match (obj prog) or percent — On obj prog: matches vnum/keyword of object. On room prog: uses percent chance. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `give`

| Property | Value |
|----------|-------|
| **When it fires** | When a character gives an object to the mob |
| **Phrase** | vnum/name match — Phrase matches the vnum or keyword of the given object. |
| **Also available on** | Mob, Tok |

**Example:** Receive an item

Trigger: `give` — Phrase: `50100`

```
say Thank you for the artifact, $n!
mob junk artifact
mob oload 50300
give reward $n
```

---

### `grouped`

| Property | Value |
|----------|-------|
| **When it fires** | When a character joins a group |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `hidden`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob detects a hidden character |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |
| **Notes** | After the mob has hidden (mob and token only) |

---

### `hide`

| Property | Value |
|----------|-------|
| **When it fires** | When a character hides in the room |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Tok |
| **Notes** | Act of hiding (mob, object and token) |

---

### `hpcnt`

| Property | Value |
|----------|-------|
| **When it fires** | When mob's HP falls below a threshold |
| **Phrase** | HP% < threshold — Phrase is an HP percentage threshold. Trigger fires when mob HP% drops below this value. |
| **Also available on** | Mob, Tok |

**Example:** HP threshold reaction

Trigger: `hpcnt` — Phrase: `25`

```
mob echo {R$I roars in fury, summoning dark energies!{x
mob cast heal
```

---

### `inspect`

| Property | Value |
|----------|-------|
| **When it fires** | When a character inspects the mob |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Tok |

---

### `inspect_custom`

| Property | Value |
|----------|-------|
| **When it fires** | Custom inspection handler for the mob |
| **Phrase** | exact keyword match — Phrase must exactly match the shop stock keyword. |
| **Also available on** | Mob, Tok |
| **Notes** | Called when asking a shopkeeper to inspect a custom stock item |

---

### `kill`

| Property | Value |
|----------|-------|
| **When it fires** | When a character initiates combat with the mob |
| **Phrase** | percent chance — Phrase is percent chance. Fires when entity kills another. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** When attacked

Trigger: `kill` — Phrase: `100`

```
say You dare attack me, $n?!
mob echo $I draws $l weapon!
```

---

### `level`

| Property | Value |
|----------|-------|
| **When it fires** | When a character levels up in the room |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `login`

| Property | Value |
|----------|-------|
| **When it fires** | When a player logs in near the mob |
| **Phrase** | direct execution — No phrase matching. Fires when a player logs in. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `lore`

| Property | Value |
|----------|-------|
| **When it fires** | When a character uses lore on an item |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Tok |

---

### `lorex`

| Property | Value |
|----------|-------|
| **When it fires** | Extended lore check |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Tok |

---

### `mount`

| Property | Value |
|----------|-------|
| **When it fires** | When a character mounts the mob |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `multiclass`

| Property | Value |
|----------|-------|
| **When it fires** | When a character multiclasses |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |
| **Notes** | Called when a player multiclasses |

---

### `postquest`

| Property | Value |
|----------|-------|
| **When it fires** | After a quest stage completes |
| **Phrase** | percent chance — Called after all quest rewards and messages are given. |
| **Also available on** | Mob, Tok |
| **Notes** | Called after all quest rewards and messages are given |

---

### `practice`

| Property | Value |
|----------|-------|
| **When it fires** | When a character practices at the mob |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `prebite`

| Property | Value |
|----------|-------|
| **When it fires** | Before a bite attack (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `prebuy`

| Property | Value |
|----------|-------|
| **When it fires** | Before a shop purchase (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `prebuy_obj`

| Property | Value |
|----------|-------|
| **When it fires** | Before buying a specific object (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `predeath`

| Property | Value |
|----------|-------|
| **When it fires** | Before the mob dies (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `predismount`

| Property | Value |
|----------|-------|
| **When it fires** | Before dismounting (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `predrink`

| Property | Value |
|----------|-------|
| **When it fires** | Before drinking (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `preeat`

| Property | Value |
|----------|-------|
| **When it fires** | Before eating (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `preflee`

| Property | Value |
|----------|-------|
| **When it fires** | Before fleeing (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `prehide`

| Property | Value |
|----------|-------|
| **When it fires** | Before hiding (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Tok |
| **Notes** | NIB 20140522 - trigger for testing if you can hide yourself or the object in question |

---

### `prehidein`

| Property | Value |
|----------|-------|
| **When it fires** | Before hiding in a container (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Tok |
| **Notes** | NIB 20140522 - trigger for testing if the container or mob you are trying to hide an object in will allow it |

---

### `prekill`

| Property | Value |
|----------|-------|
| **When it fires** | Before initiating combat (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `premount`

| Property | Value |
|----------|-------|
| **When it fires** | Before mounting (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `prepractice`

| Property | Value |
|----------|-------|
| **When it fires** | Before practicing (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `prepracticeother`

| Property | Value |
|----------|-------|
| **When it fires** | Before practicing on another (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `prepracticethat`

| Property | Value |
|----------|-------|
| **When it fires** | Before practicing a specific skill (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `prequest`

| Property | Value |
|----------|-------|
| **When it fires** | Before a quest begins (can prevent) |
| **Phrase** | percent chance — Allows custom checking for questing and setting quest part count. |
| **Also available on** | Mob, Tok |
| **Notes** | Allows custom checking for questing, also allows setting the number of quest parts. |

---

### `prerehearse`

| Property | Value |
|----------|-------|
| **When it fires** | Before rehearsing a song (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `prerenew`

| Property | Value |
|----------|-------|
| **When it fires** | Before a quest renews (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |
| **Notes** | Used by "RENEW" to get QP cost and whether the item can be renewed |

---

### `prerest`

| Property | Value |
|----------|-------|
| **When it fires** | Before resting (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `presell`

| Property | Value |
|----------|-------|
| **When it fires** | Before selling to a shop (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `presit`

| Property | Value |
|----------|-------|
| **When it fires** | Before sitting (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `presleep`

| Property | Value |
|----------|-------|
| **When it fires** | Before sleeping (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `prestand`

| Property | Value |
|----------|-------|
| **When it fires** | Before standing (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `pretrain`

| Property | Value |
|----------|-------|
| **When it fires** | Before training (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `prewake`

| Property | Value |
|----------|-------|
| **When it fires** | Before waking (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `pull`

| Property | Value |
|----------|-------|
| **When it fires** | When a character pulls an object |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Tok |

---

### `pullon`

| Property | Value |
|----------|-------|
| **When it fires** | When a character pulls on the mob |
| **Phrase** | keyword substring — Phrase is matched as a substring against the target argument. |
| **Also available on** | Mob, Obj, Tok |
| **Notes** | NIB : 20070121 |

---

### `quest_cancel`

| Property | Value |
|----------|-------|
| **When it fires** | When a quest is cancelled |
| **Phrase** | percent chance — Fires when a quest is cancelled. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `quest_complete`

| Property | Value |
|----------|-------|
| **When it fires** | When a quest is completed |
| **Phrase** | percent chance — Prior to awards, when quest is turned in complete. Allows editing rewards. |
| **Also available on** | Mob, Obj, Room, Tok |
| **Notes** | Prior to awards being given, called when the quest turned in complete, allowing editing of the awards |

---

### `quest_incomplete`

| Property | Value |
|----------|-------|
| **When it fires** | When a quest fails/expires incomplete |
| **Phrase** | percent chance — Prior to awards, when quest is turned in incomplete. Allows editing rewards. |
| **Also available on** | Mob, Obj, Room, Tok |
| **Notes** | Prior to awards being given, called when the quest turned in incomplete , allowing editing of the awards |

---

### `quest_part`

| Property | Value |
|----------|-------|
| **When it fires** | When a quest part/objective completes |
| **Phrase** | percent chance — Used to generate a custom quest part when selected. |
| **Also available on** | Mob, Tok |
| **Notes** | Used to generate a custom quest part when selected. |

---

### `remort`

| Property | Value |
|----------|-------|
| **When it fires** | When a character remorts near the mob |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |
| **Notes** | Called when a player remorts |

---

### `renew`

| Property | Value |
|----------|-------|
| **When it fires** | When a quest renews/resets |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |
| **Notes** | Used by "RENEW" |

---

### `renew_list`

| Property | Value |
|----------|-------|
| **When it fires** | When a quest list is renewed |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |
| **Notes** | Used by "RENEW LIST" |

---

### `rest`

| Property | Value |
|----------|-------|
| **When it fires** | When a character rests in the room |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `restocked`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob's shop restocks |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `sit`

| Property | Value |
|----------|-------|
| **When it fires** | When a character sits in the room |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `skill_berserk`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob uses berserk |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `skill_sneak`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob uses sneak |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `skill_warcry`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob uses warcry |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `sleep`

| Property | Value |
|----------|-------|
| **When it fires** | When a character sleeps in the room |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `stand`

| Property | Value |
|----------|-------|
| **When it fires** | When a character stands in the room |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `throw`

| Property | Value |
|----------|-------|
| **When it fires** | When a character throws an object |
| **Phrase** | vnum/name match — Phrase matches the vnum or keyword of the thrown object. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `ungrouped`

| Property | Value |
|----------|-------|
| **When it fires** | When a character leaves a group |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `wake`

| Property | Value |
|----------|-------|
| **When it fires** | When a character wakes in the room |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `wear`

| Property | Value |
|----------|-------|
| **When it fires** | When a character puts on equipment |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** When equipment is worn

Trigger: `wear` — Phrase: `100`

```
mob echo $I adjusts $l armor.
```

---

### `xpgain`

| Property | Value |
|----------|-------|
| **When it fires** | When a character gains experience near the mob |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

## Speech Triggers

### `sayto`

| Property | Value |
|----------|-------|
| **When it fires** | When a character says something to the mob directly |
| **Phrase** | keyword substring — Phrase is matched as a substring against the sayto message. |
| **Also available on** | Mob, Obj, Tok |
| **Notes** | NIB : 20070121 |

**Example:** Direct speech to the mob

Trigger: `sayto` — Phrase: `yes`

```
say Excellent! Then we have a deal.
```

---

### `speech`

| Property | Value |
|----------|-------|
| **When it fires** | When a character says something in the room |
| **Phrase** | keyword substring — Phrase is matched as a substring against the spoken message. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** Respond to a keyword

Trigger: `speech` — Phrase: `help`

```
say I can help you, $n! Just tell me what you need.
```

---

### `whisper`

| Property | Value |
|----------|-------|
| **When it fires** | When a character whispers to the mob |
| **Phrase** | keyword substring — Phrase is matched as a substring against the whispered message. |
| **Also available on** | Mob, Tok |

**Example:** When whispered to

Trigger: `whisper` — Phrase: `password`

```
mob echoat $n $I nods slowly and opens a hidden door.
```

---

## Random/Timed Triggers

### `hitgain`

| Property | Value |
|----------|-------|
| **When it fires** | On each HP regeneration tick |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `managain`

| Property | Value |
|----------|-------|
| **When it fires** | On each mana regeneration tick |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `movegain`

| Property | Value |
|----------|-------|
| **When it fires** | On each movement regeneration tick |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `prereckoning`

| Property | Value |
|----------|-------|
| **When it fires** | Before a reckoning event (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok, Area, Event |

---

### `pulse`

| Property | Value |
|----------|-------|
| **When it fires** | Every server pulse (frequent timer) |
| **Phrase** | percent chance — Phrase is percent chance. Fires every game pulse. |
| **Also available on** | Mob, Tok |

---

### `random`

| Property | Value |
|----------|-------|
| **When it fires** | Randomly each tick (percent chance) |
| **Phrase** | percent chance — Phrase is a percent chance (1-100). Fires on each random tick. |
| **Also available on** | Mob, Obj, Room, Tok, Area, Inst, Dung, Event |
| **Notes** | DEPRECATE |

**Example:** Random idle behavior

Trigger: `random` — Phrase: `5`

```
if rand 50
  mob echo $I whistles a tune.
else
  mob echo $I leans against the wall.
endif
```

---

### `reckoning`

| Property | Value |
|----------|-------|
| **When it fires** | When a reckoning event fires |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok, Area, Event |

---

### `regen`

| Property | Value |
|----------|-------|
| **When it fires** | On each regeneration tick |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |
| **Notes** | Custom regenerations |

---

### `regen_hp`

| Property | Value |
|----------|-------|
| **When it fires** | On HP regeneration specifically |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |
| **Notes** | Modification on ALL hp regens |

---

### `regen_mana`

| Property | Value |
|----------|-------|
| **When it fires** | On mana regeneration specifically |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |
| **Notes** | Modification on ALL mana regens |

---

### `regen_move`

| Property | Value |
|----------|-------|
| **When it fires** | On movement regeneration specifically |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |
| **Notes** | Modification on ALL move regens |

---

### `tick`

| Property | Value |
|----------|-------|
| **When it fires** | Every game tick |
| **Phrase** | percent chance — Phrase is percent chance. Fires every game tick. |
| **Also available on** | Mob, Obj, Room, Tok, Area, Inst, Dung, Event |

---

### `toxingain`

| Property | Value |
|----------|-------|
| **When it fires** | On toxin/poison tick |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

## Movement Triggers

### `entry`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob enters a new room |
| **Phrase** | percent chance — Phrase is percent chance. Fires when the scripted entity enters a room. |
| **Also available on** | Mob, Obj, Room, Tok, Inst, Dung |

**Example:** Act when entering a room

Trigger: `entry` — Phrase: `100`

```
say I have arrived!
```

---

### `exall`

| Property | Value |
|----------|-------|
| **When it fires** | When a character exits the room (any direction) |
| **Phrase** | direction number (exact) — Phrase is a direction number. Like exit but fires regardless of mob position/sight. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** React to someone leaving

Trigger: `exall` — Phrase: `100`

```
wave $n
```

---

### `exit`

| Property | Value |
|----------|-------|
| **When it fires** | When a character leaves via a specific direction |
| **Phrase** | direction number (exact) — Phrase is a direction number (0=north, 1=east, etc.). Fires when character tries to exit in that direction. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** React to a specific exit

Trigger: `exit` — Phrase: `0`

```
say Going north? Be careful out there.
```

---

### `grall`

| Property | Value |
|----------|-------|
| **When it fires** | When a character enters (fires for all players) |
| **Phrase** | percent chance — Phrase is a percent chance (1-100). Like greet but fires regardless of mob position/sight. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** Greet every player who enters

Trigger: `grall` — Phrase: `100`

```
if isimmort $n
  bow $n
endif
```

---

### `greet`

| Property | Value |
|----------|-------|
| **When it fires** | When a character enters (fires once per character) |
| **Phrase** | percent chance — Phrase is a percent chance (1-100). Fires when character enters room (sight-based). |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** Greet arriving players

Trigger: `greet` — Phrase: `100`

```
say Welcome to my shop, $n!
```

---

### `land`

| Property | Value |
|----------|-------|
| **When it fires** | When a flying character lands |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `move_char`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob is moved by an external force |
| **Phrase** | direction name string — Phrase is the direction name (e.g., "north"). Fires when character is moved programmatically. |
| **Also available on** | Mob, Room, Tok |

---

### `takeoff`

| Property | Value |
|----------|-------|
| **When it fires** | When a character removes/takes off equipment |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

## Action Triggers

### `act`

| Property | Value |
|----------|-------|
| **When it fires** | Another character performs a visible action |
| **Phrase** | keyword substring — Phrase is matched as a substring against the action message. Use `*` to match all. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** React to visible actions

Trigger: `act` — Phrase: `smiles`

```
smile $n
say How kind of you!
```

---

### `emote`

| Property | Value |
|----------|-------|
| **When it fires** | When a character emotes in the room |
| **Phrase** | exact name match — Phrase must exactly match the emote name (e.g., "smile", "nod"). |
| **Also available on** | Mob, Tok |
| **Notes** | NIB : 20140508 : untargeted emote |

---

### `emoteat`

| Property | Value |
|----------|-------|
| **When it fires** | When a character emotes at the mob |
| **Phrase** | exact name match — Phrase must exactly match the targeted emote name. |
| **Also available on** | Mob, Tok |
| **Notes** | NIB : 20140508 : targeted emote |

---

## Fight Triggers

### `afterkill`

| Property | Value |
|----------|-------|
| **When it fires** | After the mob kills a target |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |
| **Notes** | Called after someome kills a target.  TODO: Damage will become forbidden in this trigger. |

---

### `assist`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob assists another in combat |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs any attack |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `fight`

| Property | Value |
|----------|-------|
| **When it fires** | Each combat round while the mob is fighting |
| **Phrase** | percent chance — Phrase is percent chance. Fires each combat round while fighting. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** Combat round action

Trigger: `fight` — Phrase: `20`

```
mob echoat $n $I slashes at you with razor claws!
mob damage $n 100 200 lethal
```

---

### `preassist`

| Property | Value |
|----------|-------|
| **When it fires** | Before the mob assists (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `preround`

| Property | Value |
|----------|-------|
| **When it fires** | Before each combat round (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `prewimpy`

| Property | Value |
|----------|-------|
| **When it fires** | Before wimpy flee triggers (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `startcombat`

| Property | Value |
|----------|-------|
| **When it fires** | When combat begins for the mob |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** When combat begins

Trigger: `startcombat` — Phrase: `100`

```
mob echo $I enters a fighting stance!
```

---

### `wimpy`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob's wimpy threshold is reached |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

## Repop/Death/Lifecycle Triggers

### `death`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob dies |
| **Phrase** | percent chance — Phrase is percent chance. Fires when entity dies. |
| **Also available on** | Mob, Room, Tok |

**Example:** On death behavior

Trigger: `death` — Phrase: `100`

```
mob oload 50100
drop treasure
```

---

### `death_protection`

| Property | Value |
|----------|-------|
| **When it fires** | When death protection activates |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Room, Tok |

---

### `death_timer`

| Property | Value |
|----------|-------|
| **When it fires** | When a death timer expires |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Room, Tok |

---

### `extract`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob is extracted/removed from the world |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room |

---

### `preanimate`

| Property | Value |
|----------|-------|
| **When it fires** | Before the mob is animated (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `preresurrect`

| Property | Value |
|----------|-------|
| **When it fires** | Before resurrection (can prevent) |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `repop`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob repops/spawns into the world |
| **Phrase** | percent chance — Phrase is percent chance. Fires when entity repops/resets. |
| **Also available on** | Mob, Obj, Tok, Inst, Dung |

**Example:** Spawn equipment

Trigger: `repop` — Phrase: `100`

```
mob oload 50200
wear all
```

---

### `restore`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob is restored to full stats |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `resurrect`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob is resurrected |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `save`

| Property | Value |
|----------|-------|
| **When it fires** | When a nearby player saves |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Tok |

---

## Verb Triggers

### `showcommands`

| Property | Value |
|----------|-------|
| **When it fires** | When listing available commands |
| **Phrase** | direct execution — No phrase matching. Fires to display available custom commands. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `verb`

| Property | Value |
|----------|-------|
| **When it fires** | When a custom verb is used on the mob |
| **Phrase** | keyword match — Phrase matches the custom verb keyword. Scripts accessed via `verb <keyword>`. |
| **Also available on** | Mob, Obj, Room, Tok |

---

## Attacks Triggers

### `attack_backstab`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a backstab |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_bash`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a bash |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_behead`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a behead |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_bite`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a bite |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_blackjack`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a blackjack |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_circle`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a circle |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_counter`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a counter-attack |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_cripple`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a cripple |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_dirtkick`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a dirt kick |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_disarm`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a disarm |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_intimidate`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs intimidate |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_kick`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a kick |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_rend`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a rend |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_slit`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a slit |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_smite`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a smite |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_tailkick`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a tail kick |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_trample`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a trample |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `attack_turn`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob performs a turn undead |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `barrier`

| Property | Value |
|----------|-------|
| **When it fires** | When an energy barrier blocks an attack |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

## Hits Triggers

### `hit`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob lands a hit on a target |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

## Damage Triggers

### `check_damage`

| Property | Value |
|----------|-------|
| **When it fires** | When damage is calculated against the mob |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `damage`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob takes damage |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Room, Tok |

**Example:** When taking damage

Trigger: `damage` — Phrase: `100`

```
if hpcnt $i 50
  mob echo $I staggers from the blow!
endif
```

---

## Spell Triggers

### `spell_cure`

| Property | Value |
|----------|-------|
| **When it fires** | When a cure spell is cast on/near the mob |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

### `spell_dispel`

| Property | Value |
|----------|-------|
| **When it fires** | When a dispel spell is cast on/near the mob |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Obj, Tok |

---

### `spellcast`

| Property | Value |
|----------|-------|
| **When it fires** | When a spell is cast in the room |
| **Phrase** | exact name match — Phrase is matched exactly against the spell name being cast. |
| **Also available on** | Mob, Obj, Room, Tok |

---

### `spellreflect`

| Property | Value |
|----------|-------|
| **When it fires** | When a spell is reflected by the mob |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---

## Combat Style Triggers

### `defense`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob uses a defensive combat style |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |
| **Notes** | Only on tokens |

---

## Animate Triggers

### `animate`

| Property | Value |
|----------|-------|
| **When it fires** | When the mob is animated/summoned |
| **Phrase** | percent chance — Numeric 1–100 for percent chance; or keyword string for exact match; `*` matches always. |
| **Also available on** | Mob, Tok |

---
