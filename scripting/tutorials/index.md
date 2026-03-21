---
layout: default
title: Tutorials
nav_order: 1
parent: Scripting
has_children: true
---

# Tutorials
{: .no_toc }

Step-by-step guides that teach you to build scripts in Sentience, from your first line of code to advanced patterns.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Who Are These Tutorials For?

These tutorials are for **builders who have never written a script before**. No programming experience is required. Each tutorial builds on the previous one, introducing new concepts gradually with hands-on examples you can follow in-game.

If you already have scripting experience and want a reference instead, see [Scripting Basics](../scripting-basics.md) or the entity-specific reference pages like [Mobile Programs](../mobile-programs/).

---

## Recommended Reading Order

Work through the tutorials in order. Each one assumes you have completed the ones before it.

| # | Tutorial | What You'll Learn |
|---|----------|-------------------|
| 1 | [Getting Started](getting-started.md) | What scripts are, the 9 entity types, how to open an editor, create a script, write code, and compile it |
| 2 | [Your First Script](your-first-script.md) | Greet and speech triggers, quick codes, ifchecks, and attaching scripts to mobs |
| 3 | [Working with Variables](working-with-variables.md) | Variables, tokens, persistence, and the token system for tracking state |
| 4 | [Building a Quest](building-a-quest.md) | Multi-stage quests with objectives, quest triggers, rewards, and testing |
| 5 | [Combat Scripting](combat-scripting.md) | Combat triggers, HP thresholds, phase transitions, and boss fight patterns |
| 6 | [Advanced Patterns](advanced-patterns.md) | Delays, sub-scripts, cross-entity communication, and complex systems |

---

## Before You Begin

Before starting these tutorials, you should be comfortable with:

- **Basic building** — Creating areas, rooms, mobiles, and objects with the OLC editors (`aedit`, `redit`, `medit`, `oedit`)
- **Widevnums** — The `area_uid#vnum` format used to reference game entities (e.g., `5#200`)
- **Navigation** — Moving around the game world and using `goto`

If you have not built an area before, work through the [Builder Tutorial](../../tutorial/) first — particularly Parts 1 through 4.

---

## How Tutorials Are Structured

Each tutorial follows the same pattern:

1. **Concept introduction** — A brief explanation of *what* you are learning and *why* it matters
2. **Step-by-step walkthrough** — Numbered instructions showing exactly what to type and what the game responds
3. **Try it yourself** — A practice exercise to reinforce what you learned
4. **Key takeaways** — A summary of the important points

Code blocks show what you type at the game prompt:

```
mpedit create
```

When the game responds, the response is shown separately and labeled:

> **Game responds:**
> ```
> MobProgram Code Created.
> ```

---

## Quick Links

| Resource | Description |
|----------|-------------|
| [Scripting Basics](../scripting-basics.md) | Language reference — commands, variables, control flow |
| [Quick Codes](../quick-codes.md) | Short codes like `$n`, `$i`, `$t` that reference entities |
| [Variables & Tokens](../variables-and-tokens.md) | Storing and retrieving data across script runs |
| [Shared Commands](../shared-commands.md) | Commands available to all entity types |
| [Mobile Programs](../mobile-programs/) | Mob-specific triggers, commands, and examples |
| [Object Programs](../object-programs/) | Object-specific triggers, commands, and examples |
| [Room Programs](../room-programs/) | Room-specific triggers, commands, and examples |
