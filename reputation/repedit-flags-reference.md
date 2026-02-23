---
layout: default
title: Reputation Flags Reference
nav_order: 3
parent: Reputation
---

# Reputation Flags Reference

---

## Reputation Flags

Set with `flags <flag>` in the reputation editor. Flags are toggled (toggle on/off each time applied).

| Flag | Description |
|------|-------------|
| `hidden` | The faction is not visible in the player's reputation panel until they encounter it in-game. Use for secret factions or reputation systems revealed through gameplay. |
| `peaceful` | The faction does not trigger combat reactions. Even hostile-rank players will not be attacked by faction mobs based on reputation alone. |
| `on_update` | The faction performs a reputation-based tick update — allows scripts or system logic to react to reputation changes on regular intervals. |
| `on_encounter` | The faction evaluates reputation on encounter (proximity to mob or entry to area). Can trigger reactions such as mob greetings, attitude changes, or scripted responses based on standing. |

---

## Rank Flags

Set with `rank <#> flags <flag>` in the reputation editor. Applied per rank.

| Flag | Description |
|------|-------------|
| `norankup` | Players at this rank cannot increase their standing further. Used to cap a faction tier unless a special condition is met. |
| `paragon` | This rank (and above) is considered "paragon" standing — the highest tier of trust. Often required for exclusive rewards, content access, or special NPC interactions. |
| `reset_paragon` | Upon reaching this rank, any previous paragon status is reset. Used for faction reset mechanics or betrayal events. |
| `peaceful` | At this rank the character is treated as peaceful by faction mobs — they will not be attacked even if mob combat flags would normally trigger it. |
| `hostile` | At this rank the character is treated as hostile — faction mobs will attack on sight or encounter. Typically applied to the lowest (enemy) ranks. |
