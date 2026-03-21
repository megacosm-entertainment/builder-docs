---
layout: default
title: Scripting
nav_order: 9
has_children: true
---

# Scripting System
{: .no_toc }

<details open markdown="block">
  <summary>On this page</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

Sentience's scripting system lets builders bring the world to life — from a shopkeeper who remembers your name to a dungeon that reshapes itself around the players inside it. Scripts respond to over 500 triggers across 9 entity types, with 181+ commands and 482 ifchecks at your disposal.

What makes the system powerful is its breadth and persistence. Scripts can communicate across entities, react to nearly any player action, and maintain state that survives reboots and server restarts. Whether you need a simple greeting or a fully dynamic zone that reshapes around its players, the same unified scripting language covers it all.

> **Brand new to scripting?** Start with the [Getting Started](tutorials/getting-started.md) tutorial — it walks you through your very first script in about five minutes. Then follow the rest of the [tutorial series](tutorials/getting-started.md) to build up from greet triggers to full quest systems.
{: .highlight }

---

## How Scripts Work

Every script follows the same pattern: a **trigger** fires when something happens in the game, and the script's **commands** execute in response. Triggers can be anything — a player entering a room, speaking a keyword, killing a mob, or even a timed tick. Commands then act on the world: sending messages, moving entities, checking conditions, setting variables, or calling other scripts.

Here's the basic flow in three lines:

```
trigger GREET_PROG 100
say Welcome back, $n! I've been expecting you.
~
```

A trigger type (`GREET_PROG`), a percentage chance (`100`), one or more commands, and a terminator (`~`). Everything in the scripting system builds on this foundation.

Within a script you can branch with `if`/`else`/`endif`, loop with counters, set and read variables, call sub-scripts, and communicate with other entities. The [Scripting Basics](scripting-basics.md) reference covers the full syntax, and the [Getting Started](tutorials/getting-started.md) tutorial walks you through it hands-on.

---

## What's in This Section

This section is organized in layers — tutorials first for learning, reference pages for daily use, per-entity guides for deep dives, and practical guides for solving real problems fast.

### Tutorials

Step-by-step guides that walk you through scripting from scratch. Start here if you're new — each tutorial builds on the last, taking you from zero to quest-building.

- [Getting Started](tutorials/getting-started.md) — What scripts are and how they work
- [Your First Script](tutorials/your-first-script.md) — Write greet and speech triggers
- [Working with Variables](tutorials/working-with-variables.md) — Variables, tokens, and persistence
- [Combat Scripting](tutorials/combat-scripting.md) — Combat triggers and boss fights
- [Advanced Patterns](tutorials/advanced-patterns.md) — Delays, sub-scripts, and cross-entity communication
- [Building a Quest](tutorials/building-a-quest.md) — Complete quest walkthrough from start to finish

### Core Reference

The fundamentals of the scripting language itself. Use these pages when you need to look up syntax rules, variable behavior, or how a specific language feature works.

- [Scripting Basics](scripting-basics.md) — Syntax, control flow, and comments
- [Advanced Scripting](advanced-scripting.md) — RSG, entity cast, sub-scripts, and async flow
- [Variables & Tokens](variables-and-tokens.md) — Variable types, the token system, and persistence
- [Quick Codes](quick-codes.md) — All 39 quick codes (`$n`, `$i`, `$t`, and more)
- [Entity Reference](entity-reference.md) — All 9 entity types and their ~260 fields

### Command & Ifcheck Reference

Comprehensive lookup tables for every command and condition check. Bookmark these — you'll use them constantly.

- [Shared Commands](shared-commands.md) — 154 commands available across entity types
- [Command Availability](command-availability.md) — Matrix of 181 commands by entity type
- [Ifchecks Reference](ifchecks-reference.md) — 482 ifchecks organized into 22 categories

### Entity-Type Guides

Each guide covers triggers, commands, and examples specific to that entity type. Once you know the basics, these are your primary reference for building scripts on a particular entity.

- [Mobile Programs](mobile-programs/) — Mob scripts (153 triggers, 167 commands)
- [Object Programs](object-programs/) — Object scripts (100 triggers, 165 commands)
- [Room Programs](room-programs/) — Room scripts (71 triggers, 157 commands)
- [Token Programs](token-programs/) — Token scripts (206 triggers, 165 commands)
- [Area Programs](area-programs/) — Area-wide scripts (6 triggers, 44 commands)
- [Instance Programs](instance-programs/) — Instance scripts (7 triggers, 48 commands)
- [Dungeon Programs](dungeon-programs/) — Dungeon scripts (9 triggers, 48 commands)
- [Quest Programs](quest-programs/) — Quest scripts (13 triggers, 44 commands)
- [Event Programs](event-programs/) — Event scripts (6 triggers, 44 commands)

### Practical Guides

Grab-and-go solutions for real building tasks, plus help when things go wrong.

- [Cookbook](cookbook.md) — 14 ready-to-use recipes from beginner to advanced
- [Troubleshooting](troubleshooting.md) — 32 common issues and how to debug them

---

## What Scripts Can Do

The scripting system covers virtually every aspect of gameplay. Here are some of the things builders create with scripts every day:

- **Make NPCs react to players** — greet them by name, respond to speech, accept items, or fight back with custom combat logic
- **Create interactive objects** — weapons that trigger effects on hit, containers with combination locks, equipment that grants buffs when worn
- **Build multi-stage quests** — track objectives, gate progress behind conditions, and deliver rewards dynamically
- **Script boss fights with phase transitions** — change behavior at health thresholds, summon adds, swap combat styles
- **Create procedural dungeons** — generate randomized layouts, scale difficulty, and populate rooms on the fly
- **Track persistent state across sessions** — variables and tokens survive reboots, so NPCs remember players and quests stay complete
- **Automate area-wide events** — weather changes, invasion spawns, timed resets, and zone-wide announcements
- **Cross-entity communication** — scripts on mobs, objects, rooms, and tokens can trigger each other and share data
- **Run scheduled events** — event programs fire on timers for recurring world events, seasons, or automated maintenance
- **Build multi-room puzzles** — room scripts that coordinate across an area to create levers, pressure plates, and sequence puzzles

For specific recipes and patterns, see the [Cookbook](cookbook.md). For complete per-entity coverage, dive into the [Entity-Type Guides](#entity-type-guides) below.

---

## The 9 Entity Types

Scripts can be attached to any of the nine entity types below. Each type has its own set of triggers (events it responds to) and commands (actions it can take). Most builders work primarily with mobile, object, and room programs, but tokens and the higher-level types unlock powerful patterns for quests, dungeons, and events.

| Type | Use Case | Triggers | Commands |
|:-----|:---------|:--------:|:--------:|
| [Mobile](mobile-programs/) | NPCs, shopkeepers, bosses | 153 | 167 |
| [Object](object-programs/) | Weapons, containers, interactive items | 100 | 165 |
| [Room](room-programs/) | Environments, traps, puzzles | 71 | 157 |
| [Token](token-programs/) | Persistent state, buffs, quest flags | 206 | 165 |
| [Area](area-programs/) | Zone-wide events, resets, weather | 6 | 44 |
| [Instance](instance-programs/) | Instanced dungeon logic | 7 | 48 |
| [Dungeon](dungeon-programs/) | Procedural dungeon generation | 9 | 48 |
| [Quest](quest-programs/) | Quest progression and rewards | 13 | 44 |
| [Event](event-programs/) | Scheduled and global events | 6 | 44 |

> **Tokens are the most versatile type.** With 206 triggers and full command access, token programs can attach to any entity and are the go-to choice for persistent state, buffs, debuffs, and custom mechanics that don't fit neatly into another type.
{: .note }

**Not sure which entity type to use?** Here are a few rules of thumb:

- If the behavior belongs to an NPC → **Mobile Programs**
- If it's tied to an item the player carries or uses → **Object Programs**
- If it's about the environment or location itself → **Room Programs**
- If you need state that persists on a player or entity → **Token Programs**
- If the logic spans an entire zone → **Area Programs**
- If you're building a generated dungeon → **Dungeon Programs** + **Instance Programs**
- If it's a quest with stages and rewards → **Quest Programs**
- If it runs on a schedule independent of player action → **Event Programs**

When in doubt, start with the entity the player interacts with most directly. You can always layer in tokens and cross-entity calls later.

---

## Common Tasks

Not sure where to start? Find your goal below and jump straight to the most relevant page.

| I want to... | Start here |
|:-------------|:-----------|
| Learn scripting from scratch | [Getting Started](tutorials/getting-started.md) |
| Make a mob react to players | [Mobile Programs](mobile-programs/) |
| Create an interactive object | [Object Programs](object-programs/) |
| Make an object with special effects | [Object Programs](object-programs/) &middot; [Variables & Tokens](variables-and-tokens.md) |
| Script a boss fight | [Combat Scripting](tutorials/combat-scripting.md) |
| Build a quest | [Quest Programs](quest-programs/) &middot; [Building a Quest](tutorials/building-a-quest.md) |
| Create a procedural dungeon | [Dungeon Programs](dungeon-programs/) &middot; [Instance Programs](instance-programs/) |
| Run an area-wide event | [Event Programs](event-programs/) &middot; [Area Programs](area-programs/) |
| Script a multi-room puzzle | [Room Programs](room-programs/) &middot; [Advanced Scripting](advanced-scripting.md) |
| Track state across sessions | [Variables & Tokens](variables-and-tokens.md) |
| Look up a command | [Shared Commands](shared-commands.md) &middot; [Command Availability](command-availability.md) |
| Check a condition | [Ifchecks Reference](ifchecks-reference.md) |
| Find a ready-made example | [Cookbook](cookbook.md) |
| Debug a broken script | [Troubleshooting](troubleshooting.md) |

---

## Next Steps

If you're just getting started, the fastest path is:

1. **Read** [Getting Started](tutorials/getting-started.md) to understand what scripts are
2. **Build** your first greet trigger in [Your First Script](tutorials/your-first-script.md)
3. **Learn** variables and persistence in [Working with Variables](tutorials/working-with-variables.md)
4. **Browse** the [Cookbook](cookbook.md) for ready-made patterns you can adapt

Once you're comfortable with the basics, use the [Entity-Type Guides](#entity-type-guides) as your daily reference and the [Ifchecks Reference](ifchecks-reference.md) and [Command Availability](command-availability.md) pages as your lookup tables.

> **Tip:** Keep the [Quick Codes](quick-codes.md) page handy — you'll use `$n`, `$i`, and `$t` in almost every script you write.
{: .note }