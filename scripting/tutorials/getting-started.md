---
layout: default
title: Getting Started
nav_order: 1
parent: Tutorials
grand_parent: Scripting
---

# Getting Started with Scripts
{: .no_toc }

Your first script — from opening an editor to seeing it run in-game.

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## What Are Scripts?

Scripts are small programs that make the game world come alive. They give NPCs the ability to talk, react, fight intelligently, and interact with players. They make objects do things when picked up or worn. They make rooms respond when players enter or leave.

Without scripts, a mob is just a stationary target. With scripts, that mob becomes a quest giver who remembers your name, a shopkeeper who refuses to serve thieves, or a boss who changes tactics at half health.

**Why builders use scripts:**

- Make NPCs greet players, hold conversations, and offer quests
- Create objects that trigger effects when used, worn, or dropped
- Build rooms that react to player actions — traps, puzzles, environmental effects
- Drive quest progression — tracking objectives, granting rewards, updating journals
- Add combat AI — phase transitions, special attacks, fleeing behavior

---

## The 9 Entity Types

Scripts can be attached to nine different entity types. Each type uses a command prefix that tells the engine *who* is performing the action.

| Entity | Prefix | Editor | What It Scripts |
|--------|--------|--------|----------------|
| **Mobile** | `mob` | `mpedit` | NPC behavior — dialogue, combat AI, quest logic |
| **Object** | `obj` | `opedit` | Item effects — use, wear, get, drop, timer actions |
| **Room** | `room` | `rpedit` | Environment — entry/exit effects, traps, puzzles |
| **Token** | `token` | `tpedit` | Attached markers — buffs, debuffs, quest flags on entities |
| **Area** | `area` | `apedit` | Zone-wide — repopulation, area-level random events |
| **Instance** | `instance` | `ipedit` | Instanced zones — dungeon events, phased content |
| **Dungeon** | `dungeon` | `dpedit` | Procedural dungeons — floor generation, boss spawns |
| **Quest** | `quest` | `qpedit` | Quest systems — stage transitions, objective tracking |
| **Event** | `event` | `epedit` | Global events — world events, scheduled activities |

The prefix matters. When a mob script says `mob echo Hello!`, the mob performs the echo. When a room script says `room echo The ground trembles.`, the room performs it. Every command in a script must start with the correct prefix for its entity type.

{: .note }
> **Mob scripts** are by far the most common. If you are just starting out, focus on mob scripts first. The concepts transfer directly to all other types.

---

## The Script Lifecycle

Every script goes through the same five steps from creation to execution:

```
Create  →  Write  →  Compile  →  Attach Trigger  →  Test
```

Here is what each step means:

### 1. Create

Open a script editor and create a new, empty script. The game assigns it a vnum (virtual number) that uniquely identifies it.

### 2. Write

Enter the code editor and type your script — the commands, conditions, and logic that define what happens when the script runs.

### 3. Compile

The game checks your script for errors (unknown commands, mismatched `if`/`endif` blocks, bad syntax). If it compiles successfully, the script is ready to use.

### 4. Attach Trigger

Link the compiled script to an entity (a mob, object, room, etc.) and specify *when* it should fire. The "when" is called a **trigger** — for example, `greet_prog` fires when a player enters the room, `speech_prog` fires when a player says a keyword.

### 5. Test

Interact with the entity in-game to verify the script fires correctly and does what you expect.

---

## Accessing the Script Editors

Each entity type has its own editor command. To open an editor, type the command followed by a widevnum:

```
mpedit <widevnum>
```

To create a brand-new script, add `create`:

```
mpedit create
```

Here are all nine editors:

| Command | Creates/Edits |
|---------|---------------|
| `mpedit` | Mobile (mob) programs |
| `opedit` | Object programs |
| `rpedit` | Room programs |
| `tpedit` | Token programs |
| `apedit` | Area programs |
| `ipedit` | Instance programs |
| `dpedit` | Dungeon programs |
| `qpedit` | Quest programs |
| `epedit` | Event programs |

{: .important }
> All nine editors work identically. Once you learn one, you know them all. The only difference is the command prefix used inside the script (`mob`, `obj`, `room`, etc.).

---

## Basic Editor Commands

Once inside any script editor, you have access to these commands:

| Command | What It Does |
|---------|-------------|
| `show` | Display the current script — its name, flags, code, and status |
| `name <text>` | Set a descriptive name for the script |
| `code` | Open the code editor to write or edit the script body |
| `compile` | Check the script for errors and compile it |
| `comments` | Add builder notes (not executed — just for documentation) |
| `flags` | Toggle script flags (e.g., `DISABLED`, `WIZNET`) |
| `security` | Set the security level (0–9, restricted to implementors) |
| `depth` | Set the maximum recursion depth for `call` chains |
| `list` | Show all scripts of this type in the current area |
| `create` | Create a new script |
| `commands` | Show the list of available editor commands |
| `?` | Same as `commands` — show help |

Type `done` to save and exit the editor.

---

## Walkthrough: Your First Script

Let's create a mob script that makes an NPC greet players when they enter the room. We will go through every step, showing exactly what to type.

{: .note }
> This walkthrough assumes you have an existing area with at least one mob. If you do not, work through the [Builder Tutorial](../../tutorial/) first to create one.

### Step 1 — Open the Editor and Create a Script

Type the following to create a new mob script:

```
mpedit create
```

> **Game responds:**
> ```
> MobProgram Code Created.
> ```

The game creates an empty script and opens the editor. Note the vnum it assigns — you will need it later. For this walkthrough, we will assume the vnum is `100` in your area.

### Step 2 — Name Your Script

Give the script a clear, descriptive name. You will thank yourself later when you have dozens of scripts to manage:

```
name Friendly NPC greet script
```

> **Game responds:**
> ```
> Name set.
> ```

### Step 3 — Write the Code

Enter the code editor:

```
code
```

This opens a line-based text editor. Type your script, then type a single period (`.`) on its own line to finish:

```
** This script makes the NPC greet players who enter the room
if ispc $(enactor)
  mob echo {YThe innkeeper looks up and smiles warmly.{x
  say Welcome to my inn, $n! You look like you could use a rest.
endif
.
```

Let's break down what each line does:

| Line | Meaning |
|------|---------|
| `** This script makes...` | A comment. Lines starting with `**` are ignored when the script runs. Use them to explain your intent. |
| `if ispc $(enactor)` | Check if the entity that triggered the script is a player character (not another NPC). `$(enactor)` refers to whoever caused the trigger to fire. |
| `mob echo {Y...{x` | Display colored text to everyone in the room. `{Y` starts yellow text, `{x` resets to default. The `mob` prefix is required because this is a mob script. |
| `say Welcome...` | The NPC speaks aloud. `$n` is a quick code that inserts the triggering player's name. |
| `endif` | Closes the `if` block. Every `if` must have a matching `endif`. |

{: .warning }
> Don't forget the `.` on its own line to exit the code editor. If you forget, you will still be in the editor and any commands you type will be added to the script as code.

### Step 4 — Compile the Script

Check your script for errors:

```
compile
```

If everything is correct:

> **Game responds:**
> ```
> Script compiled successfully.
> ```

If there are errors, the game tells you what went wrong and which line the problem is on. Fix the code with the `code` command and compile again.

### Step 5 — Review Your Work

Check what the script looks like:

```
show
```

This displays the script name, vnum, flags, status, and the full code. Verify everything looks right.

### Step 6 — Save and Exit the Script Editor

```
done
```

### Step 7 — Attach the Script to a Mob

Now you need to tell the game *which* mob should run this script and *when*. Open the mob in its editor:

```
medit <mob widevnum>
```

Replace `<mob widevnum>` with your mob's actual widevnum (e.g., `5#200`).

Attach the script with a greet trigger:

```
addmprog <script widevnum> greet_prog ~
```

Replace `<script widevnum>` with the vnum of the script you just created. The `~` as the phrase means the trigger fires for every player who enters — no keyword filtering.

> **Game responds:**
> ```
> Mprog Added.
> ```

Save the mob:

```
done
```

### Step 8 — Test It

Go to the room where the mob is located:

```
goto <room widevnum>
```

If the mob is already in the room, leave and come back to trigger the greet:

```
north
south
```

You should see:

> ```
> The innkeeper looks up and smiles warmly.
> The innkeeper says 'Welcome to my inn, Yourname! You look like you could use a rest.'
> ```

Congratulations — your first script is working!

---

## Understanding What Just Happened

Let's trace the full sequence of what the game did:

1. **You walked into the room** — this generated a "greet" event on every mob in the room
2. **The mob had a `greet_prog` trigger attached** — so the engine started running script `100`
3. **`if ispc $(enactor)`** — the engine checked if you (the enactor) are a player character. You are, so it entered the `if` block.
4. **`mob echo ...`** — the engine displayed the echo text to everyone in the room
5. **`say ...`** — the engine made the mob speak, substituting `$n` with your character name
6. **`endif`** — the engine reached the end of the `if` block and the script finished

This is the fundamental pattern of all scripting in Sentience: **an event happens → a trigger fires → the script runs → commands execute**.

---

## Try It Yourself

Now that you have the basics, try these exercises:

### Exercise 1: Add a Second Message

Edit your greet script to also have the mob perform an emote:

```
mpedit <script widevnum>
code
```

Add a line after the `say` command:

```
** This script makes the NPC greet players who enter the room
if ispc $(enactor)
  mob echo {YThe innkeeper looks up and smiles warmly.{x
  say Welcome to my inn, $n! You look like you could use a rest.
  mob echoat $(enactor) The innkeeper gestures toward an empty chair by the fire.
endif
.
```

The `mob echoat` command sends a message only to the specified player, not the whole room. Compile and test it.

### Exercise 2: Create a Speech Response

Create a second script that responds when a player says "hello":

1. `mpedit create` — create a new script
2. Write code that responds to being spoken to:
   ```
   if ispc $(enactor)
     say Hello there, $n! What can I do for you today?
   endif
   .
   ```
3. `compile` — check for errors
4. Attach it to your mob with a speech trigger: `addmprog <vnum> speech_prog hello`
5. Test it by saying "hello" in the mob's room

---

## Key Takeaways

- **Scripts** are blocks of code that run when specific events happen in the game
- **Nine entity types** can have scripts: mob, obj, room, token, area, instance, dungeon, quest, event
- **Every command** in a script starts with the entity prefix (`mob`, `obj`, `room`, etc.)
- **Comments** start with `**` and are ignored when the script runs
- The **lifecycle** is: create → write → compile → attach trigger → test
- **All nine editors** (`mpedit`, `opedit`, `rpedit`, etc.) work identically
- `$(enactor)` refers to whoever triggered the script; `$n` is the enactor's name
- Always **compile** before testing — the compiler catches most errors for you

---

## What's Next?

Now that you can create and attach a basic script, the next steps are:

- Read [Scripting Basics](../scripting-basics.md) for a complete reference on the scripting language — variables, conditions, loops, and more
- Browse the [Mobile Programs](../mobile-programs/) section for the full list of mob triggers and commands
- Look at the [Builder Tutorial Part 5](../../tutorial/tutorial-part5-scripts.md) for a practical example of scripting a quest

---

*This is Tutorial 1 of the Sentience Scripting Tutorials series.*
