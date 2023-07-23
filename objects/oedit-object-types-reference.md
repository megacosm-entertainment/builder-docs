---
layout: default
title: OEdit Object Types Reference
nav_order: 4
parent: Object Editor
---

## Light
Object is a light. Recommend `take light` as wear flags.

Values:
- v2 `<number>` -- Sets the duration of the light, in ticks.

## Scroll
Object is a scroll. Scrolls are used with the `recite` command, and will cast the spell(s) set on them via `addspell`. Recommend `take` wear flags

## Wand
Object is a wand. May be used to `zap <target>` if held. Recommend `take hold` as wear flags.<br/>
Wands will use the spells added by `addspell` command.

Values:
- v0 `<number>` -- Level. This is deprecated in favour of using the spell level from `addspell`
- v1 `<number>` -- Maximum Charges
- v2 `<number>` -- Remaining Charges

## Staff
Staves are essentially the same as wands. The only difference is the command used to activate them. For staves, this is `brandish`. Otherwise, the same settings and recommendations apply.

Values:
- v0 `<number>` -- Level - Deprecated
- v1 `<number>` -- Maximum Charges
- v2 `<number>` -- Remaining Charges

## Weapon
An extremely common item type. Recommend `take wield` wear flags. Condition will gradually degrade with use.

Values:
- v0 `<weapon class>` - Sets the [class](oedit-flags-reference#weapon-classes) of weapon, determines skill used.
- v1 `<number>` - Number of damage dice
- v2 `<number>` - Type of damage dice (sides)
- v3 `<name>` - Sets the [damage type](oedit-flags-reference#) and associated attack string.
- v4 `<flags>` - Sets [special weapon flags](oedit-flags-reference#special-weapon-flags).

## Treasure
Often used for reward items, but there is no special handling for these.

## Armour
Used for worn gear that should apply AC to the wearer. `take <location>` wear flags recommended. If you set level after setting the type, this is given a set of defaults.

Armour values are calculated based on level and v4 value. Exotic is 90% of the same.

Values:
- v0 `<number>` - AC for Piercing
- v1 `<number>` - AC for Bashing
- v2 `<number>` - AC for Slashing
- v3 `<number>` - AC for Exotic
- v4 `<strength>` - Armour [strength](oedit-flags-reference#armour-strength-flags) for calculating AC values.

## Potion
As scrolls, but use the `quaff` command. Provides the spells added through `addspell` to their users. Recommend `take` wear flag.

## Furniture
Used as a place to rest or sleep, 

Values:
- v0 `<number>` - Maximum people that can use the furniture at once.
- v1 `<number>` - Maximum weight supported by the furniture.
- v2 `<flags>` - [Furniture flags](oedit-flags-reference#furniture-flags), adjusts how people are shown as using the furniture, position.
- v3 `<number>` - HP regen bonus.
- v4 `<number>` - Mana regen bonus.
- v5 `<number>` - Movement regen bonus.

## Trash
No special handling in OLC. Some mob act flags may pick up and purge these.

## Container
Used to hold objects. Recommend `take` wear flag for carryable containers. Makes use of [lock states](oedit-command-reference#lock-subcommands).

Values:
- v0 `<number>` - Maximum weight the container can hold.
- v1 `<flags>` - [Container flags](oedit-flags-reference#container-flags)
- v2 `<vnum>` - Key vnum. Deprecated by lockstates.
- v3 `<number>` - Maximum number of objects the container can hold.
- v4 `<number>` - Weight multiplier for objects in the container.

## Drink Container
Used for holding drinks! Recommend `take` wear flag
