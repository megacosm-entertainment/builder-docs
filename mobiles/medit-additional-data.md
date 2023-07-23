---
layout: default
title: Additional MEdit Data
nav_order: 2
parent: Mobile Editor
---

## Questor
Questor data is used primarily with scripting to create additional quest masters. More information on this to follow as I document the scripting engine. All commands are in the mobile editor and begin with `questor`.


### Add

Enables questor data on the mob.

### Remove

Removes questor data from the mob.

### Scroll `<vnum>`

Sets the scroll object the questor will hand out to the specified vnum.

### Keywords `<string>`

Sets keywords for the scroll.

Ex: `questor scroll keywords quest scroll` - This will set the keywords of the scroll given to players to `quest scroll`.

### Short `<string>`

Sets the short desc for the scroll.

### Long `<long>`

Sets the long desc for the scroll.

### Header

Opens a string editor to set the header for the scroll. See `mshow 6777` for an example quest scroll.

### Footer

Opens a string editor to set the footer for the scroll. See `mshow 6777` for an example.

### Prefix `<string>`

Sets the prefix used for each line of the quest scroll after the header.

### Suffix `<string>`

Sets the suffix used for each line of the quest scroll after the header.

### Width `<number>`

Sets a maximum line width for the scroll lines.


## Shop
Shop data is used to create mobile run shops. This was previously done with resets. All commands are in the mobile editor and begin with `shop`.


### Assign

Assigns a shop to this mobile.

### Remove

Removes shop data from the mobile.

### Discount `<0-100|reset>`

Sets the discount rate for the shop.

### Flags `<flag1> [flag2] [flag3] ...`

Sets [shop flags](medit-flags-reference#shop) on this mobile's shop.

### Hours `<opening> <closing>`

Sets the hours the shop is operational, from 0-23

### Profit `<buying> <selling>`

Sets the markup (or mark down) for buying from or selling to this shop.

### Restock `<minutes>`

How often the shop restocks.

### Shipyard clear

Clears shipyard data from the shop

### Shipyard `<wilds uid> <x1> <y1> <x2> <y2> <description>`

Designates a square area on the given wilderness map as a shipyard to place new vessels.

### Stock add `<type> <value>`

Adds a new stock entry to the shop. Type/value combinations below:

| type | value |
|:-----|:------|
| object | vnum |
| pet | vnum |
| mount | vnum |
| guard | vnum |
| crew | vnum |
| ship | vnum |
| custom | keyword |

### Stock `<#> discount <0-100>`

Sets a discount percentage for the given stock entry.

### Stock `<#> description <string>`

A short description of what the stock entry is. Appears on a new line in the shop list.

### Stock `<#> level <level|0>`

Sets the level of this stock entry. Used for loading items or mobs. 0 will use the object/mob's assigned level.

### Stock `<#> price <currency> <value>`

Sets the price for the stock entry. Can set multiple prices, all will be charged. Use 0 for any currency to remove it.

Ex: `shop stock 1 price silver 20`, `shop stock 1 price qp 20` - This will result in an item that costs 20 silver AND 20 quest points.

| currency | value |
|:---------|:------|
| silver | number |
| qp | number |
| dp | number |
| pneuma | number |
| custom | string |

### Stock `<#> quantity unlimited`

Sets this stock entry to have unlimited quantity.

### Stock `<#> quantity <number> [reset rate]`

Sets this stock entry to have the specified quantity, with an optional refresh rate per restock interval.

### Stock `<#> singular`

Sets a stock entry as singular. Cannot buy more than 1 at a time.

### Stock `<#> remove`

Removes the specified stock entry.

## Crew
Used for setting crew data. Used with the ship system. All commands are used in the mobile editor, prefaced with `crew`

Details about each crew rating to follow.


### Assign

Adds crew data to the mobile.

### Remove

Removes crew data from the mobile.

### Minrank `<NYI>`

Not yet implemented. Meant to be minimum rank to hire this crew member.

### Scouting `<rating>`

Sets this crew member's 'scouting' skill rating.

### Gunning `<rating>`

Sets this crew member's 'gunning' skill rating.

### Oarring `<rating>`
Sets this crew member's 'oarring' skill rating.

### Mechanics `<rating>`

Sets this crew member's 'mechanics' skill rating.

### Navigation `<rating>`

Sets this crew member's 'navigation' skill rating.

### Leadership `<rating>`

Sets this crew member's 'leadership' skill rating.