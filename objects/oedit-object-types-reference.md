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

Values:
- v0 `<number>` - Maximum liquid.
- v1 `<number>` - Default liquid remaining.
- v2 `<liquid>` - Default [liquid](oedit-flags-reference#liquids).
- v3 `<yes|no>` - Toggles poisoned status for the drink container.

## Key
Keys do not have any special value, but to be used with a lock, the item must be of type 'key'.

## Food
Food is consumed by hungry players.

Values:
- v0 `<number>` - How much 'fullness' is gained.
- v1 `<number>` - How much 'hunger' the food counts for
- v3 `<yes|no>` - Is it poisoned?
- v4 `<number>` - This is just another way to set the timer on the object.

## Money
Used to create money objects. When picked up, these convert to silver and gold that is carried by the character. Can only be selected by Implementors.

Values:
- v0 `<number>` - How much silver the object is worth.
- v1 `<number>` - How much gold the object is worth.

## Boat
Special object type, should not be used at this time.

## NPCCorpse
An npc corpse object.

Values:
- v0 `<corpse type>` - The [corpse type](oedit-flags-reference#corpse-types), for purposes of handling decay.
- v1 `<number 0-100>` - Base resurrection success chance.
- v2 `<number 0-100>` - Base animate dead success chance.
- v3 `<body parts>` - Body [parts](../mobiles/medit-flags-reference.md#parts)
- v5 `<vnum>` - Vnum of the mobile to be used for the zombie created by 'animate dead'

## Fountain
Room based drink containers. For endless fountains, set v0 and v1 to `-1`

Values:
- v0 `<number>` - Liquid total
- v1 `<number>` - Liquid left
- v2 `<liquid>` - The fountain's [liquid](oedit-flags-reference#liquids), from the liquid table.

## Pill
Something like a hybrid of scroll and food. Spells added with `addspell` will apply that spell to the character that eats them.

## Protect
Unused. Do not use.

## Map
Used as part of the boat system, can be used for writing waypoints.

## Portal
Used to transport players from one location to another.

Values:
- v0 `<number>` - Charges before the portal stops working.
- v1 `<exit flags>` - [Exit flags](../rooms/redit-flags-reference.md#exit-types) to apply to the portal.
- v2 `<portal flags>` - [Portal-specific flags](oedit-flags-reference#portal-flags) to apply to the portal.
- v4 `<vnum>` - Key to used if the portal is locked. Deprecated in favour of [lockstates](oedit-command-reference#lock-add).
- v5 `<map uid>` - UID of the wilderness map to send characters to. If set, also add `v6` and `v7`.
- v6 `<map x coord>` - X coordinate of the map destination.
- v7 `<map y coord>` - Y coordinate of the map destination.

## Catalyst
Deprecated in favour of `addcatalyst` command. Only settable by Implementors.

## Roomkey
Unused. 

## Gem
Unused.

## Jewelry
An item type that does not have any values and is mostly unused (except being susceptible to shock damage). Good item type for 'accessories'. Recommend same sorts of wear flags as armour.

## Jukebox
Unused.

## Artifact
No special handling, but cannot be trapped with the 'entrap' spell.

## Shares
Unused. Only settable by Implementors.

## Flame_Room_Object
System object. Used for inferno flames. Only settable by Implementors.

## Instrument
Bards will need these to play songs.

Values:
- v0 `<type>` - The instrument type. Not currently used for anything.
- v1 `<flags>` - Currently, the only flag is 'onehanded'
- v2 `<number>` - Minimum time factor for adjusting song playing time.
- v3 `<number>` - Maximum time factor for adjusting song playing time.

## Seed
Grow into plants!

Values:
- v0 `<number>` - How many ticks before the item grows.
- v1 `<vnum>` - What the seed will become when v0 runs out.

## Cart
Carts are essentially pullable containers. Relics have been moved to this, I think.

Values:
- v0 `<number>` - Maximum weight
- v1 `<number>` - Move delay in combat pulses(?)
- v2 `<number>` - Strength required to pull cart.
- v3 `<number>` - Item capacity.
- v4 `<number>` - Weight multiplier for contained items.

## Ship
Used for partially implemented ship system. Only settable by Implementors.

Values:
- v0 `<number>` - Maximum weight in cargo.
- v1 `<number>` - Move delay in pulses.
- v2 `<number>` - Minimum crew required for ship to function.
- v3 `<number>` - Cargo capacity.
- v4 `<number>` - Maximum crew supported by this ship.
- v5 `<vnum>`   - Room entered when boarding.
- v6 `<number>` - Hit points for the ship.
- v7 `<number>` - Max cannons supported (not currently used)

## Room_Darkness_Object
System object. Used for momentary darkness. Only settable by Implementors.

## Ranged_Weapon
Ranged weapons (bows, crossbows, harpoons, etc). Recommend `take back` for wear flags.

Values:
- v0 `<class>`  - [Class](oedit-flags-reference#ranged-weapon-types) of ranged weapon.
- v1 `<number>` - Number of dice to roll for damage.
- v2 `<number>` - Type of dice to roll.
- v3 `<number>` - Maximum projectile distance.

## Sextant
Used to get current coordinates in the wilderness. Uses navigation/survey skills for accuracy.

Values:
- v0 `<number 0-100>` - Base percentage chance of working.
## Weapon_Container
Containers specifically for weapons. Originally intended for use with scabbards, etc.

Values:
- v0 `<number>` - Maximum weight.
- v1 `<weapon type>` - Weapon type held by this container.
- v3 `<number>` - Maximum capacity.
- v4 `<number>` - Weight multiplier.

## Room_Roomshield_Object
System object. Used for room shield. Only settable by Implementors.

## Book
Intended for giving players things to read.

Values:
- v1 `<flags>` - [Container](oedit-flags-reference#container-flags) flags.
- v2 `<vnum>` - Key vnum (maybe better to use lockstates?)

## Stinking_Cloud
System object. Used for stinking clouds generated by smoke bombs.

## Smoke_Bomb
System object for smoke bombs. Only settable by Implementors.

## Herb
Currently unused.

## Spell_Trap
System object for spell trap.

## Withering_Cloud
System object. Used for withering clouds. Only settable by Implementors.

## Bank
Used for portable bank quest item. Only settable by Implementors.

## Keyring
Used for keyring quest item. Only settable by Implementors.

## Ice_Storm
System object. Used for ice storm.

## Flower
No special handling.

## Trade_Type
Used for commodities in trade system, which is part of old ship code. Not currently used. Do not set.

Values:
- v0 `<trade type>` - The type of trade good.

## Empty_Vial
System object. Used for brewing potions.

## Blank_Scroll
System object. Used for scribing scrolls.

## Mist
Mist objects have a chance to obscure items and mobs in the room. Spooky!

Values:
- v0 `<number 0-100>` - Percentage chance to hide items on ground.
- v1 `<number 0-100>` - Percentage chance to hide npcs/players.
## Shrine
Used for relics. Only settable by Implementors.

## Whistle
Used for 'blow' command. At present, only used for personal mount summon.

## Shovel
Enables 'dig' command to unearth buried items in the room.

## Tattoo
Enables 'touch' command. Tattoos function as worn scrolls, essentially. Cast spells added with `addspell` command.

Values:
- v0 `<number>` - Maximum charges
- v1 `<number 0-100>` - Chance of fading away with each touch.

## Ink
Enables 'ink' command for adding tattoos. Inks can have up to three catalyst types. This system is unfinished.

Values:
- v0 `<catalyst type>` - First catalyst the ink provides.
- v1 `<catalyst type>` - Second catalyst ink provides.
- v2 `<catalyst type>` - Third catalyst ink provides.

## Part
Unused.

## Telescope

## Compass
Used for navigation. Requires navigation skill on a ship, survey otherwise. On ships, helps to get ship heading.

Values:
- v0 `<number 0-100>` - Accuracy percentage
- v1 `<wilderness map>` - Which map the compass is for.

## Whetstone
Part of an unfinished crafting/gathering system. Unused.

## Chisel
Part of an unfinished crafting/gathering system. Unused.

## Pick
Part of an unfinished crafting/gathering system. Unused.

## Tinderbox
Part of an unfinished crafting/gathering system. Unused.

## Drying_Cloth
Part of an unfinished crafting/gathering system. Unused.

## Needle
Part of an unfinished crafting/gathering system. Unused.

## Body_Part
Used for making body part objects on corpses.

Values:
- v0 `<parts>` - Body [parts](../mobiles/medit-flags-reference.md#parts)
- v1 `<race>` - Unsure