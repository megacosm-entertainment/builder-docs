---
layout: default
title: Reputation Command Reference
nav_order: 2
parent: Reputation
---

# Reputation Command Reference

All `repedit` commands may be used in standalone form:

```
repedit <ref> <command> [arguments]
```

Or entered interactively by opening the editor with `repedit <ref>` and typing commands without the `repedit <ref>` prefix.

---

## Entering the Editor

| Command | Description |
|---------|-------------|
| `repedit list` | List all reputation factions in the current area |
| `repedit create <widevnum>` | Create a new reputation at the given widevnum |
| `repedit <ref>` | Open interactive editor for the specified faction |
| `show` | Display all current settings for the open faction |
| `commands` | List available commands |
| `done` | Save and exit the editor |

---

## Metadata

### name

```
name <text>
```

Sets the faction's display name shown to players.

**Example:** `name The Thornwood Wardens`

---

### description

```
description
```

Opens the string editor for a full prose description. Shown to players in the reputation panel. End input with `~` on its own line.

---

### comments

```
comments
```

Opens the string editor for internal builder notes. Not shown to players.

---

### flags

```
flags <flag>
```

Toggles a faction-level flag. See [Reputation Flags](repedit-flags-reference.html#reputation-flags) for valid values.

**Example:** `flags hidden`

---

## Initial Standing

### initial

```
initial <rank#> <points>
```

Sets where new players start in this reputation. `<rank#>` is the rank index (1 = first rank added). `<points>` is how many points into that rank they begin with.

**Example:** `initial 3 0`  
Places new players at rank 3 with zero points accumulated in that tier.

---

## Token

### token

```
token <widevnum|none>
```

Attaches a token definition to this reputation. Players who carry this token are considered "members" or faction-affiliated. Use `none` to remove.

**Example:** `token 5#600`

---

## Rank Management

### rank add

```
rank add <name>
```

Appends a new rank to the faction. Ranks are ordered by insertion. Each rank is assigned an index (1, 2, 3...).

**Example:** `rank add Neutral`

---

### rank clear

```
rank clear
```

Removes all ranks from the faction.

{: .warning}
This cannot be undone. All rank data is deleted.

---

### rank remove

```
rank <#> remove
```

Removes a specific rank by its index number.

---

### rank name

```
rank <#> name <text>
```

Sets the display name for rank `<#>`.

---

### rank capacity

```
rank <#> capacity <points>
```

The number of reputation points required to reach this rank from the rank below it. For the first rank, set to `0`.

---

### rank color

```
rank <#> color <color>
```

Terminal color displayed next to the rank name in listings:

| Color | Display |
|-------|---------|
| `blue` | Blue |
| `cyan` | Cyan |
| `dark` | Dark grey |
| `green` | Green |
| `magenta` | Magenta / purple |
| `red` | Red |
| `white` | White / bright |
| `yellow` | Yellow |

---

### rank flags

```
rank <#> flags <flag>
```

Toggles a rank-level flag. See [Rank Flags](repedit-flags-reference.html#rank-flags) for valid values.

**Example:** `rank 1 flags hostile`

---

### rank description

```
rank <#> description
```

Opens the string editor for a rank description — text shown to players when they view that rank's details.

---

### rank comments

```
rank <#> comments
```

Opens the string editor for internal notes on this rank.
