---
layout: default
title: Quest Command Reference
nav_order: 2
parent: Quests
---

# Quest Command Reference

All `qedit` commands use this format:

```
qedit <ref> <command> [arguments]
```

Where `<ref>` is the quest's widevnum: `<auid>#<vnum>`, `#<vnum>` (current area), or bare `<vnum>` (in-area shorthand).

---

## Entering the Editor

| Syntax | Description |
|--------|-------------|
| `qedit <ref> create` | Create a new quest at the given widevnum |
| `qedit <ref> show` | Show all quest settings (short view) |
| `qedit <ref> show all` | Show all tabs including stages and objectives |
| `qedit <ref> tab <name\|number>` | Switch active view tab |

---

## Quest Metadata

### name

```
qedit <ref> name <text>
```

Sets the display title of the quest shown in the player journal and quest logs.

**Example:** `qedit 5#1000 name Kill the Rats`

---

### summary

```
qedit <ref> summary <text>
```

A one-line teaser shown in the quest journal entry list and NPC dialogue.

**Example:** `qedit 5#1000 summary Rid the cellar of rats.`

---

### description

```
qedit <ref> description [none]
```

Opens the string editor for full quest narrative text. Type your description and end with `~` on a line by itself. Pass `none` to clear.

---

### class

```
qedit <ref> class <narrative|mission>
```

The broad quest category:

- `narrative` — story content, part of a chain
- `mission` — job or task, typically standalone

{: .note}
`mode` is accepted as an alias for `class`.

---

### type

```
qedit <ref> type <main|side|unlock|class|event|other>
```

Categorisation for filtering and conditional logic. See [Flags & Types](qedit-flags-reference.html#quest-types) for descriptions.

---

### category

```
qedit <ref> category <none|regional|class|story|church|dungeon|crafting|event|other>
```

Journal log grouping category. See [Flags & Types](qedit-flags-reference.html#log-categories) for descriptions.

---

### scope

```
qedit <ref> scope <character|group|church>
```

Who the quest tracks progress for:

- `character` — each player individually
- `group` — the whole party
- `church` — the whole guild or church

---

### flags

```
qedit <ref> flags <flag>
```

Toggles a quest flag. Currently available: `group_snapshot`

See [Quest Flags](qedit-flags-reference.html#quest-flags) for descriptions.

---

## Quest Settings

### repeat

```
qedit <ref> repeat <once|repeatable>
```

Whether a player can do this quest multiple times.

---

### allowance

```
qedit <ref> allowance <cost>
```

Point cost to start the quest (deducted on accept).

---

### cost

```
qedit <ref> cost <amount>
```

Currency cost to accept the quest.

---

### entry

```
qedit <ref> entry <stage_id>
```

Sets which stage the quest begins at. Typically stage `1`.

**Example:** `qedit 5#1000 entry 1`

---

### enabled

```
qedit <ref> enabled <on|off>
```

Quests are disabled by default. Set `on` when ready for players.

---

### prerequisites

```
qedit <ref> prerequisites <dsl>
qedit <ref> prerequisites none
```

Sets conditions a player must meet before they can accept this quest. Written in the [Requirements DSL](../advanced-concepts/requirements-dsl.html). Use `none` to clear.

**Examples:**
```
qedit 5#1000 prerequisites tot_level 30
qedit 5#1000 prerequisites quest_completed 5#900 AND reputation 5#10 rank >= 2
qedit 5#1000 prerequisites none
```

When the check fails, the accept is blocked. Immortals are exempt.

By default, unmet prerequisites appear as a grey `(locked)` tag next to the quest name in listings. Append `, <message>` to display a specific reason in red:

```
qedit 5#1000 prerequisites tot_level 30, Level 30 required.
qedit 5#1000 prerequisites quest_completed 5#900, Complete the introductory questline first.
qedit 5#1000 prerequisites class_current warrior, hidden
```

See [Requirements DSL — Player-visible message](../advanced-concepts/requirements-dsl.html#player-visible-message) for full details.

---

### seedpolicy

```
qedit <ref> seedpolicy <auto|fixed>
```

Controls how the random seed used in generated stages is determined:

- `auto` — a seed is assigned automatically on quest accept
- `fixed` — use the seed value set via the `seed` command

---

### seed

```
qedit <ref> seed <number|none>
```

Sets the fixed seed for `seedpolicy fixed`. Use `none` to clear.

---

## Variables

### varset

```
qedit <ref> varset <name> <number|string|room|rsg> <yes|no> <value>
```

Defines a quest-level variable. Types: `number`, `string`, `room`, `rsg`. The `yes/no` flag sets whether the variable is saved persistently. Used by scripts and conditions.

Use `rsg` (or `generator`) for random-string-generator variables — spec: `<rsg_name>:<pattern|class>:<entry_name>`.

**Examples:**
```
qedit 5#1000 varset rat_count number no 0
qedit 5#1000 varset quest_name rsg yes region_names:class:title
```

---

### varclear

```
qedit <ref> varclear <name>
```

Removes a defined variable.

---

## Quest Programs

### addqprog

```
qedit <ref> addqprog <widevnum> <trigger> <phrase>
```

Attaches a quest script program. `<widevnum>` refers to the script, `<trigger>` is the event type (e.g. `accept_prog`, `complete_prog`), and `<phrase>` is the keyword trigger.

**Example:** `qedit 5#1000 addqprog 5#900 accept_prog ~`

---

### delqprog

```
qedit <ref> delqprog <group#> [trigger#]
```

Removes a quest program. Use `show` to see the program group numbers.

---

## Stages

Stages are the sequential steps of a quest. Each stage has objectives and links to a next stage.

### stage list

```
qedit <ref> stage list
```

Lists all stages defined on the quest.

---

### stage add

```
qedit <ref> stage add <stage_id>
```

Creates a new stage with the given ID number. IDs should be positive integers. Use `1`, `2`, `3` etc. in sequence.

---

### stage del

```
qedit <ref> stage del <stage_id>
```

Removes a stage and all its objectives.

---

### stage show

```
qedit <ref> stage show <stage_id>
```

Shows settings for a specific stage.

---

### stage summary / description

```
qedit <ref> stage <stage_id> summary <text>
qedit <ref> stage <stage_id> description [none]
```

Short label and long narrative for the stage.

---

### stage source

```
qedit <ref> stage <stage_id> source <static|generated>
```

- `static` — objectives are fixed (standard)
- `generated` — objectives are generated procedurally using the seed

---

### stage complete

```
qedit <ref> stage <stage_id> complete <all|any|custom>
```

How objectives must be resolved to complete the stage:

- `all` — every objective must be done
- `any` — just one objective completes the stage
- `custom` — script-controlled completion

---

### stage next

```
qedit <ref> stage <stage_id> next <stage_id|none>
```

Which stage follows this one on completion. Use `none` for the final stage.

---

### stage autocommence

```
qedit <ref> stage <stage_id> autocommence <on|off>
```

If `on`, the stage begins automatically when the previous stage completes, without waiting for a player trigger.

---

### stage enter / exit

```
qedit <ref> stage <stage_id> enter <text|none>
qedit <ref> stage <stage_id> exit <text|none>
```

Text sent to the player when they enter or exit this stage.

---

### stage profile / salt

```
qedit <ref> stage <stage_id> profile <text|none>
qedit <ref> stage <stage_id> salt <number|none>
```

Advanced: used with generated stages to control seeding behaviour.

---

## Objectives

Objectives define what a player must do within a stage to progress.

### objective list

```
qedit <ref> objective list <stage_id>
```

Lists all objectives for a stage.

---

### objective add / del / show

```
qedit <ref> objective add <stage_id> <objective_id>
qedit <ref> objective del <stage_id> <objective_id>
qedit <ref> objective show <stage_id> <objective_id>
```

---

### objective type

```
qedit <ref> objective <stage_id> <objective_id> type <kill|collect|talk|travel|locate|rescue|escort|custom>
```

Sets what the objective requires. See [Objective Types](qedit-flags-reference.html#objective-types) for full descriptions.

---

### objective required

```
qedit <ref> objective <stage_id> <objective_id> required <count>
```

How many times this objective must be completed. For `kill`, this is kill count. For `collect`, it is item quantity.

---

### objective quantity

```
qedit <ref> objective <stage_id> <objective_id> quantity <count>
```

For `collect` objectives: the quantity of the item delivered per hand-in.

---

### objective optional

```
qedit <ref> objective <stage_id> <objective_id> optional <on|off>
```

If `on`, this objective does not need to be completed for the stage to progress (when using `any` or `custom` complete mode).

---

### objective strict

```
qedit <ref> objective <stage_id> <objective_id> strict <on|off>
```

If `on`, only the exact target (not random mobs of the same vnum) satisfies this objective.

---

### objective summary

```
qedit <ref> objective <stage_id> <objective_id> summary <text|none>
```

A label shown in the player's quest journal for this objective. Often the NPC or item name.

---

### objective description

```
qedit <ref> objective <stage_id> <objective_id> description [none]
```

Long description shown when the player inspects the objective.

---

## Objective Target

The **target** is the entity the player must interact with (mob to kill, NPC to speak to, item to collect, etc.).

### target wnum

```
qedit <ref> objective <stage_id> <objective_id> target wnum <auid>#<vnum|none>
```

Bind to a specific entity by widevnum. Use `none` to clear.

**Example:** `qedit 5#1000 objective 1 1 target wnum 5#201`

---

### target mode

```
qedit <ref> objective <stage_id> <objective_id> target mode <exact|pool>
```

- `exact` — a single fixed entity
- `pool` — pick one from a weighted pool (see below)

---

### target pool

```
qedit <ref> objective <stage_id> <objective_id> target pool list
qedit <ref> objective <stage_id> <objective_id> target pool add <auid>#<vnum> <weight>
qedit <ref> objective <stage_id> <objective_id> target pool del <entry_id>
```

When target mode is `pool`, manage pool entries here. Each entry is a widevnum with a weight. The engine picks one random entry when the quest is accepted.

---

### target ref

```
qedit <ref> objective <stage_id> <objective_id> target ref <stage_id>:<objective_id|$refname|none>
```

Cross-reference another objective's resolved target (e.g. carry the escort NPC that was resolved in an earlier objective).

---

### target name

```
qedit <ref> objective <stage_id> <objective_id> target name <refname|none>
```

Assign a named reference to this objective's resolved target for use in later stage cross-refs.

---

## Objective Destination

For `travel`, `escort`, and `locate` objectives: the **destination** is the place the player or entity must reach.

```
qedit <ref> objective <stage_id> <objective_id> destination wnum <auid>#<vnum|$refname|none>
qedit <ref> objective <stage_id> <objective_id> destination ref <$refname|none>
qedit <ref> objective <stage_id> <objective_id> destination name <refname|none>
```

---

## Objective Token

Some objectives attach a token to the player or target when completed.

```
qedit <ref> objective <stage_id> <objective_id> token wnum <auid>#<vnum|$refname|none>
qedit <ref> objective <stage_id> <objective_id> token ref <$refname|none>
qedit <ref> objective <stage_id> <objective_id> token name <refname|none>

qedit <ref> objective <stage_id> <objective_id> destination token wnum <auid>#<vnum|$refname|none>
qedit <ref> objective <stage_id> <objective_id> destination token ref <$refname|none>
qedit <ref> objective <stage_id> <objective_id> destination token name <refname|none>
```

---

## Rewards

Rewards are given to the player on quest completion.

### reward list / add / del / show

```
qedit <ref> reward list
qedit <ref> reward add [type]
qedit <ref> reward del <index>
qedit <ref> reward show <index>
```

---

### reward type

```
qedit <ref> reward <index> type <points|currency|reputation|token|item|script>
```

See [Reward Types](qedit-flags-reference.html#reward-types) for full descriptions.

---

### reward amount

```
qedit <ref> reward <index> amount <number>
```

The quantity of the reward (points, gold coins, token count, etc.).

---

### reward target

```
qedit <ref> reward <index> target <auid>#<vnum|none>
```

For `token`, `item`, and `reputation` rewards: the widevnum of the item, token, or reputation faction.

---

### reward currency

```
qedit <ref> reward <index> currency <gold|silver|none>
```

For `currency` rewards: the currency type. Use `none` to clear.

---

### reward script

```
qedit <ref> reward <index> script <text|none>
```

For `script` rewards: the script action to execute when the reward is delivered. Use `none` to clear.


---

### reward display

```
qedit <ref> reward <index> display <text|none>
```

Overrides the default reward delivery message shown to the player when this reward is given. If not set, the system generates a default message based on the reward type and amount. Use `none` to clear an override.

**Example:** `qedit 5#1000 reward 1 display You feel a warm sense of satisfaction for your work.`
