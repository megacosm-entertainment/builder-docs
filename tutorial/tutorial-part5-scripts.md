---
layout: default
title: "Part 5: Mobile Scripts"
nav_order: 5
parent: Tutorial
---

# Part 5: Mobile Scripts

In this part we write scripts that make the quest-giver NPC offer the quest on greeting and accept completion on talk. We also add a death script for Blagtooth.

---

## Script Plan

| Script | Widevnum | Purpose |
|--------|----------|---------|
| Aldric Greet Script | 5#400 | Offer quest when player enters room |
| Aldric Complete Script | 5#401 | Complete quest when talked to |
| Blagtooth Death Script | 5#402 | Drop pouch on death |

---

## Step 1 — Create Aldric's Greet Script

This script fires when a player enters the room. If they have no active quest from Aldric, he offers it. If they're on stage 3 (talk objective), he thanks them.

Open the script editor for a mobile script:

```
mpedit 5#400 create
mpedit 5#400
```

Set trigger type:

```
trigger greet_prog
phrase ~
```

Write the script body:

```
if ispc $(enactor)
  if !questactive $(enactor) 5#500
    if !questdone $(enactor) 5#500
      say Ah, a capable-looking sort! Could you spare a moment?
      emote looks at you with worry etched into his face.
      say The cellar beneath this inn has been overrun with rats. Foul creatures,
      say and there's something worse down there -- a great brute they call
      say Blagtooth. I'll pay well if you can clear them out.
      questaccept $(enactor) 5#500
    endif
  else
    say Good to see you again. Any progress with those rats?
  endif
endif
```

Save the script:

```
done
```

---

## Step 2 — Attach the Greet Script to Aldric

```
medit 5#200
addmprog 5#400 greet_prog ~
done
```

{: .notice}
**Note:** The `~` phrase means the script fires for every player that enters, regardless of what they say.

---

## Step 3 — Create Aldric's Quest Complete Script

This script fires when a player speaks to Aldric, and completes the quest if the player is on stage 3.

```
mpedit 5#401 create
mpedit 5#401

trigger speech_prog
phrase rats done done
```

Write the script:

```
if ispc $(enactor)
  if questactive $(enactor) 5#500
    if queststage $(enactor) 5#500 == 3
      say Remarkable! You've actually done it. I'm in your debt.
      emote reaches beneath the bar and produces a fine-looking blade.
      say Take this -- it belonged to a warden I once knew. You've earned it.
      questcomplete $(enactor) 5#500
    endif
  endif
endif
```

Save:

```
done
```

Attach to Aldric:

```
medit 5#200
addmprog 5#401 speech_prog done
done
```

{: .info}
The `questcomplete` command triggers the reward distribution and marks the quest finished. The journal updates automatically.

---

## Step 4 — Create Blagtooth's Death Script

When Blagtooth dies, he drops the leather pouch.

```
mpedit 5#402 create
mpedit 5#402

trigger death_prog
phrase ~
```

Write the script:

```
oload 5#302 room
emote lets out a final shriek as it collapses, a small pouch tumbling from beneath its bulk.
```

Save:

```
done
```

Attach to Blagtooth:

```
medit 5#203
addmprog 5#402 death_prog ~
done
```

---

## Step 5 — Test the Scripts

1. Go to the inn: `goto 5#111`
2. Aldric should greet you and offer the quest
3. Check your quest journal: `quest`
4. Go kill 10 rats (`goto 5#112`, fight rats)
5. Collect 3 rat tails
6. Kill Blagtooth in 5#115 — pouch should drop
7. Return to inn — Aldric should complete the quest

{: .warning}
If scripts don't fire, check for syntax errors by looking at the error log: `wiznet`

---

## Script Quick Reference

Key commands used in these scripts:

| Command | Description |
|---------|-------------|
| `say <text>` | Mob speaks |
| `emote <text>` | Mob action (uses `$(self)` for self) |
| `questaccept $(enactor) <widevnum>` | Offers and assigns quest to player |
| `questcomplete $(enactor) <widevnum>` | Completes quest and distributes rewards |
| `oload <widevnum> room` | Loads an object into the current room |
| `ispc $(enactor)` | Check if triggering entity is a player |
| `questactive $(enactor) <widevnum>` | Check if player has quest active |
| `questdone $(enactor) <widevnum>` | Check if player has completed quest |
| `queststage $(enactor) <widevnum>` | Returns current stage number |

See the full [Script Commands Reference](../scripting/mobile-programs/mprog-script-commands.html) for more commands.

---

**Next:** [Part 6: The Reputation Faction](tutorial-part6-reputation.html)
