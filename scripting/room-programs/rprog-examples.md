---
layout: default
title: Examples
nav_order: 3
parent: Room Programs
grand_parent: Scripting
---

# Room Program Examples
{: .no_toc }

Practical examples of room programs covering common use cases. Each example includes the complete script code, the trigger attachment, and an explanation of how it works.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

{: .important }
> All room program commands use the `room` prefix. Comments use `**` on their own line.

---

## Example 1: Trap Room — Fire Damage on Entry

A room that damages players when they enter, simulating a fire trap. The trap has a chance to fire and warns the player with atmospheric text before dealing damage.

### Script Code

```
** Fire trap — damages anyone who enters
** Trigger: greet 100
room echo {RFlames erupt from vents in the floor!{x
room echoat $n {RYou are engulfed in searing flames!{x
room echoaround $n {r$N is engulfed in searing flames!{x
room damage $n 75 fire
```

### Attachment

```
redit <room vnum>
addprog <script vnum> greet 100
```

- **Trigger:** `greet` — fires when a visible character enters
- **Phrase:** `100` — 100% chance (always fires)

### How It Works

1. A player walks into the room.
2. The `greet` trigger fires with `$n` set to the entering player.
3. `room echo` sends the flames message to everyone in the room.
4. `room echoat` sends a personal message to the victim.
5. `room echoaround` sends a third-person message to everyone else.
6. `room damage` deals 75 fire damage to the entering player.

### Variations

**50% chance trap** — Change the trigger phrase to `50`:
```
addprog <script vnum> greet 50
```

**Disarmable trap** — Check a variable before firing:
```
** Check if trap has been disarmed
if varexists $self trap_disarmed
  ** Trap was disarmed, do nothing
else
  room echo {RFlames erupt from vents in the floor!{x
  room damage $n 75 fire
endif
```

**One-shot trap** — Disable after first triggering:
```
room echo {RFlames erupt from vents in the floor!{x
room damage $n 75 fire
room varset trap_disarmed number 1
room echo {DThe flames sputter and die. The fuel is spent.{x
```

---

## Example 2: Puzzle Room — Speech-Activated Mechanism

A room with a locked door that opens when a player says the correct passphrase. Speaking the wrong phrase triggers a different response.

### Script Code — Correct Passphrase

```
** Puzzle: say the passphrase to open the north door
** Trigger: speech open sesame
room echo {YThe ancient runes along the north wall blaze with golden light!{x
room echoat $n {WYou feel the magic respond to your words.{x
room alterexit north door open
room echo {GThe stone door grinds open, revealing a passage to the north!{x
** Set a variable so we know the door is open
room varset door_opened number 1
** Close it again after 30 seconds (120 pulses)
room delay 120
```

### Script Code — Delay Handler (Door Closes)

```
** Auto-close the door after the delay expires
** Trigger: delay 100
if varexists $self door_opened
  room alterexit north door closed
  room echo {DThe stone door slides shut with a heavy thud.{x
  room varclear door_opened
endif
```

### Script Code — Wrong Passphrase (Optional)

```
** React to incorrect attempts at magic words
** Trigger: speech open
room echo {dThe runes flicker briefly, then go dark.{x
room echoat $n {DNothing happens. That wasn't quite right.{x
```

### Attachment

```
redit <room vnum>
addprog <passphrase script> speech open sesame
addprog <delay script> delay 100
addprog <wrong phrase script> speech open
```

### How It Works

1. A player says "open sesame" in the room.
2. The `speech` trigger matches the keyword substring "open sesame".
3. The script opens the north exit with `room alterexit` and sets a variable.
4. `room delay 120` schedules the delay trigger to fire in ~30 seconds.
5. When the delay fires, the door closes again.
6. If someone says just "open" without "sesame", the wrong-phrase script fires instead.

{: .note }
> Speech triggers match substrings. The phrase `open sesame` will fire when a player says "open sesame" or "I say open sesame please". The more specific phrase is checked first.

---

## Example 3: Environmental Narration — Random Atmospheric Descriptions

A room that periodically displays atmospheric descriptions to create ambience. Uses the `random` trigger to fire at variable intervals.

### Script Code

```
** Atmospheric narration for the Haunted Library
** Trigger: random 15
** Choose a random message each time
if rand 25
  room echo {DA book falls from a shelf with a dusty thud.{x
elseif rand 33
  room echo {DThe candles flicker as an unseen draft passes through.{x
elseif rand 50
  room echo {DYou hear the faint sound of pages turning somewhere nearby.{x
else
  room echo {DThe shadows in the corners seem to shift and writhe.{x
endif
```

### Attachment

```
redit <room vnum>
addprog <script vnum> random 15
```

- **Trigger:** `random` — fires on the room's random pulse
- **Phrase:** `15` — 15% chance each pulse

### How It Works

1. Each random pulse, there is a 15% chance the script fires.
2. Inside the script, `if rand` checks create weighted random selection among four messages.
3. The first message has a 25% chance, the second ~25% (33% of the remaining 75%), the third ~25% (50% of the remaining 50%), and the fourth gets everything else.
4. The result is varied atmospheric text that doesn't repeat too predictably.

### Variations

**Time-sensitive narration** — Different messages based on game time:
```
if hour $null > 18
  room echo {DThe library grows dark as night falls outside the windows.{x
elseif hour $null > 6
  room echo {wSunlight streams through the dusty windows.{x
else
  room echo {DThe library is eerily silent in the pre-dawn hours.{x
endif
```

**Only when players are present** — The random trigger fires even in empty rooms. Check for occupants:
```
if people $self > 0
  room echo {DA cold draft whispers through the corridor.{x
endif
```

---

## Example 4: Teleporter Room — Entry Trigger with Transfer

A magical teleportation circle that activates when a player enters, displaying dramatic effects before transporting them to a distant location.

### Script Code

```
** Teleportation circle — transports player on entry
** Trigger: greet 100
room echoat $n {CYou step onto the glowing circle of runes.{x
room echoaround $n {C$N steps onto the glowing circle of runes.{x
room echo {WThe runes blaze with brilliant white light!{x
room echoat $n {WThe world dissolves around you in a flash of light...{x
room echoaround $n {W$N vanishes in a flash of brilliant light!{x
room transfer $n 5200
room at 5200 room echo {W$N materializes in a flash of light!{x
room at 5200 room echoat $n {WYou materialize in an unfamiliar place.{x
```

### Attachment

```
redit <room vnum>
addprog <script vnum> greet 100
```

### How It Works

1. A player enters the teleporter room, triggering `greet`.
2. Atmospheric messages display to the player and room.
3. `room transfer $n 5200` moves the player to room 5200.
4. `room at 5200` executes commands in the destination room — welcoming the player with arrival messages.

### Variations

**Conditional teleporter** — Only teleport players with a specific token:
```
if tokencount $n 6000 > 0
  room echoat $n {CThe portal recognizes your attunement crystal.{x
  room transfer $n 5200
else
  room echoat $n {DThe portal flickers but nothing happens. You lack the attunement.{x
endif
```

**Two-way portal** — Pair scripts on both rooms to create a bidirectional teleporter:
```
** In room A: transfer to room B
room transfer $n <room_B_vnum>

** In room B: transfer to room A (separate script)
room transfer $n <room_A_vnum>
```

**Group teleporter** — Transfer the entire group together:
```
room echo {WThe portal expands, drawing in $N and their companions!{x
room gtransfer $n 5200
room at 5200 room echo {WA group of adventurers materializes from the portal!{x
```

---

## Example 5: Ambush Room — Greet Trigger with Mob Loading and Combat

A room that spawns hostile mobs when a player enters and forces them into combat. The ambush only triggers once, then disarms itself until the area resets.

### Script Code — Ambush

```
** Ambush: spawn bandits when a player enters
** Trigger: greet 100
** Only fire if the ambush hasn't been triggered yet
if varexists $self ambush_sprung
else
  ** Mark the ambush as triggered
  room varset ambush_sprung number 1

  ** Atmospheric buildup
  room echoat $n {rYou hear a rustle in the shadows...{x
  room echo {DA sharp whistle cuts through the air!{x

  ** Spawn the bandits
  room mload 4500
  room mload 4500
  room mload 4501

  ** Narrate the ambush
  room echo {RTwo bandits leap from the shadows! A bandit leader steps forward!{x
  room echoat $n {R'Your gold or your life!' snarls the bandit leader.{x

  ** Force the bandits to attack
  room vforce 4501 kill $n
  room vforce 4500 kill $n
endif
```

### Script Code — Reset Handler

```
** Reset the ambush on area repop
** Trigger: reset 100
room varclear ambush_sprung
```

### Attachment

```
redit <room vnum>
addprog <ambush script> greet 100
addprog <reset script> reset 100
```

### How It Works

1. A player enters the room, triggering `greet`.
2. The script checks if `ambush_sprung` variable exists — if so, the ambush was already triggered and nothing happens.
3. First time through: sets `ambush_sprung`, displays atmospheric text.
4. `room mload` spawns two bandit mobs (vnum 4500) and one bandit leader (vnum 4501).
5. `room vforce` forces all mobs of each vnum to attack the entering player.
6. On area reset, the `reset` trigger clears the variable, re-arming the ambush.

### Variations

**Level-scaled ambush** — Spawn different mobs based on player level:
```
if level $n > 30
  room mload 4510
  room mload 4510
  room echo {RElite assassins drop from the ceiling!{x
elseif level $n > 15
  room mload 4505
  room mload 4505
  room echo {RBandits leap from behind the rocks!{x
else
  room mload 4500
  room echo {RA lone highwayman steps from the shadows.{x
endif
```

**Delayed ambush** — Give the player a moment before the attack:
```
** On greet: set up the ambush
room echoat $n {rYou feel uneasy. Something is watching you.{x
room remember $n
room delay 12

** On delay: spring the ambush
room mload 4500
room echo {RBandits attack!{x
room vforce 4500 kill $n
```

---

## Tips for Room Programs

- **Test with `rpstat`** — Use `rpstat` in a room to see all attached programs and their triggers.
- **Use `stat room`** — Verify the room's properties (flags, exits, sector) to confirm your scripts are working.
- **Variables persist on rooms** — Room variables survive reboots if saved with `room varsave`. Use them for puzzle state, trap status, and progression tracking.
- **`$self` is the room** — In room progs, `$self` and `$here` are the same entity. Use `$self` when operating on the room itself.
- **Combine triggers** — A single room can have many programs. Use `greet` for entry effects, `speech` for puzzles, `random` for atmosphere, and `reset` for cleanup.
- **Mind the `break`** — Use `break` in pre-triggers (`prerecall`, `exit`, `preenter`) to prevent the action from completing.
- **Direction numbers** — For `exit`, `exall`, `knock`, `knocking`, `open`, and `close` triggers: 0=north, 1=east, 2=south, 3=west, 4=up, 5=down.
