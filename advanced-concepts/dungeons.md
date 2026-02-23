---
layout: default
title: Dungeons
nav_order: 3
parent: Advanced Concepts
---

# Dungeons (dngedit)

A **dungeon** is the top-level definition for instanced content. It references a blueprint and configures the player-facing rules: group requirements, death behavior, floor structure, timers, and more. When a player enters a dungeon portal, the server uses the dungeon definition to spawn a blueprint instance.

{: .warning}
**Instancing is an advanced feature.** Create rooms in `redit`, sections in `bsedit`, blueprints in `bpedit`, then configure the dungeon here.

---

## Relationship Summary

```
Dungeon (dngedit)
└── Floor(s)
    └── Blueprint(s) (bpedit)
        └── Section(s) (bsedit)
            └── Room(s) (redit)
```

A dungeon has one or more **floors**. Each floor references a blueprint. Multi-floor dungeons can present different blueprints as players progress.

---

## Entering the Editor

```
dngedit create <widevnum>
dngedit <widevnum>
```

**Example:**
```
dngedit create 5#900
dngedit 5#900
```

---

## Commands

### name

```
name <text>
```

Sets the dungeon's display name.

---

### description

```
description
```

Opens the string editor for full narrative description. End with `~`.

---

### comments

```
comments
```

Internal builder notes.

---

### flags

```
flags <flag>
```

Toggle dungeon flags:

| Flag | Description |
|------|-------------|
| `failure_on_empty` | The dungeon fails (marks as failed) if all players leave before it completes. |
| `failure_on_wipe` | The dungeon fails if the entire party is wiped (all players die or are released). |
| `group_commence` | Requires a full group to be present before the dungeon officially commences. |
| `idle_on_complete` | The instance remains accessible after completion rather than immediately destroying. |
| `locked` | The dungeon entrance is locked; players cannot enter once it has commenced. |
| `no_idle` | Disables the idle timeout — the instance never times out while players are present. |
| `no_save` | Instance state is not saved across restarts. |
| `shared` | All players in the dungeon share instance state (default for group content). |
| `solo_instance` | Each player gets their own private instance regardless of group. |

---

### areawho

```
areawho <title>
```

Sets the area title shown in the who list for players inside this dungeon.

---

### repop

Sets the repop timer (in seconds) for instances of this dungeon.

---

### idletimeout

```
idletimeout <seconds>
```

How long (in seconds) an instance remains active with no players inside before being destroyed. Set to `0` to disable (unless `no_idle` flag is set).

---

### mingroup / maxgroup / maxplayers

```
mingroup <number>
maxgroup <number>
maxplayers <number>
```

| Command | Description |
|---------|-------------|
| `mingroup` | Minimum number of players required to start the dungeon |
| `maxgroup` | Maximum group size allowed |
| `maxplayers` | Absolute maximum number of players across all groups |

---

### deathrelease

```
deathrelease <normal|isolated|to_floor|to_checkpoint|failure>
```

Controls what happens when a player dies inside the dungeon:

| Value | Description |
|-------|-------------|
| `normal` | Standard death behavior (corpse drops, player sent to recall) |
| `isolated` | Player is sent to an isolated room inside the instance |
| `to_floor` | Player is sent to the start of the current floor |
| `to_checkpoint` | Player is sent to the last checkpoint room they reached |
| `failure` | Player death counts as a dungeon failure |

---

### entry / exit

```
entry <widevnum>
exit <widevnum>
```

Sets the physical room in the world where players enter and exit from. These are the portal rooms outside of the instance.

---

### zoneout / portalout / mountout

```
zoneout <text>
portalout <text>
mountout <text>
```

Controls zone-transition messages and destination behavior:
- `zoneout` — the zone players are sent to when they leave the instance via a regular exit
- `portalout` — the portal destination text/widevnum
- `mountout` — text or command run for mounts when players exit

---

### floors

Floors are the sequential levels of the dungeon. Each floor references a blueprint.

```
floors add <blueprint_widevnum>
floors list
```

Adds a blueprint as a new floor of the dungeon.

---

### levels

The `levels` command controls the procedurally determined sequence of floors (for dungeons with random floor ordering).

```
levels list
levels add static <floor#>
levels add weighted <floor#> <weight>
levels add grouped
levels move <#> up|down|top|bottom|first|last
levels weight <#> add <weight> <floor#>
levels weight <#> set <#> <weight> <floor#>
levels weight <#> remove <#>
levels group <#> add static <floor#>
levels group <#> add weighted <weight> <floor#>
levels group <#> move <#> up|down|top|bottom|first|last
levels group <#> weight <#> add <weight> <floor#>
levels group <#> weight <#> set <#> <weight> <floor#>
```

The levels system determines the order floors are presented:
- `static` — always use this floor number
- `weighted` — randomly selected by weight from a pool
- `grouped` — more complex grouping of floor choices

---

### channel

```
channel add <channel_id>
channel list
channel del <#>
```

Associates communication channels with this dungeon.

---

### special

```
special list
special add <section_name> <room_widevnum> <name>
special # remove
special # name <text>
special # room <widevnum>
```

Defines named special locations inside the dungeon instances, referenced by scripts.

---

### adddprog / deldprog

```
adddprog <widevnum> <trigger> <phrase>
deldprog <group#>
```

Attaches/removes dungeon programs (scripts fired on dungeon-level events like commencement, completion, failure, floor change).

---

### varset / varclear

```
varset <name> <number|string|room|rsg> <yes|no> <value>
varclear <name>
```

Defines or removes dungeon-level variables used by scripts.

---

## Example

```
dngedit create 5#900
dngedit 5#900

name The Thornwood Cellar
description
A damp, rat-infested cellar beneath the Thornwood Inn.
Rumors speak of worse things lurking in the lower chambers.
~

flags failure_on_wipe locked

areawho Thornwood Cellar
idletimeout 300
mingroup 1
maxgroup 5
maxplayers 5
deathrelease to_floor

entry 5#10
exit 5#10

floors add 5#800

show
done
```
