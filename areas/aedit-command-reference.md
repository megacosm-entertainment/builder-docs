---
layout: default
title: AEdit Commands Reference
nav_order: 2
parent: Area Editor
---

# AEdit Command Reference
---

```
?              addaprog       addtrade       age            airshipland    
areawho        builder        commands       comments       create         
credits        delaprog       description    filename       flags          
landx          landy          name           notes          open           
placetype      postoffice     recall         regions        removetrade    
repop          security       settrade       show           tags           
topic          varclear       varset         viewtrade      vnum           
wilds          x              y              levels
```

### ?
Shows the list of flag banks. You can use `? <flag_type>` to view a list of available flags. This command is common to all editors.

### addaprog `<area prog vnum> <trigger> <phrase>`
Adds an area script to the zone, with the given arguments.

### addtrade
Not currently used, needs further research.

### age `<number>`
Used to determine if the area is due to repop soon. Repop is based on age being higher than repop.

### airshipland `<vnum>`
Room where the Goblin Airship will land. Airship is currently disabled.

### areawho `<flag>`
How do players in this zone show up on who? See [areawho](area-flags-reference#areawho-flags) for more details.

### builder `<name>`
Marks the named immortal as a builder for this zone. This is necessary for some OLC commands.

### commands
Lists the available commands in the area editor. You're here!

### comments
Opens the string editor to write builder comments. This field is like writing any description, but is only visible to builders. Leave notes here about plans, things you need help with, the purpose of the area, important vnums, etc.

### create
Creates a brand new zone, with an area# filename. Change this, then start setting the other things for your zone.

### credits `<string>`
A list of credits for the zone. Visible to players.

### delaprog `<number>`
Removes an area prog from the list.

### description
Enters the string editor to set a description for the area. This should give some background lore, and try to entice players into exploring. This is a player visible field.

### filename `<file>`
The filename used to store the area on the server.

### flags `<flag1> [flag2] [flag3] ...`
Sets various [area flags](aedit-flags-reference#area-flags).

### landx `<x coord>`
Landing x coordinate on the area's set wilderness map. Unused at present.

### landy `<y coord>`
Landing y coordinate on the area's set wilderness map. Unused at present.

### levels `<min level> <max level>`
Sets the suggested level range for the zone. Visible to players.

### name `<string>`
Sets the name of the area, seen by players in multiple commands. Used for area lookups.

### notes 
Enters the string editor to make area-wide building notes. Not visible to players.

### open `<yes|no>`

### placetype `<continent>`
Sets the [continent](aedit-flags-reference#placetype-flag) for the area. Used for some teleportation and selection of rooms for quests, etc.

### postoffice `<vnum>`
Sets the vnum for the 'post office' room for the zone. Informational for players.

### recall `<vnum>`
Sets the recall room for the zone.

### regions `<subcommand> [args]`

Manages named sub-regions within an area. Each area has a default region (applying to all rooms that aren't assigned to a custom region) and any number of named custom regions. Custom regions can override the area's `areawho`, `placetype`, `topic`, and flags on a per-region basis. Use `redit region` to assign rooms.

When sub-commands take `<#|name|default>`, the region can be identified by its list number, a prefix of its name (once set with `regions name`), or the keyword `default` for the area-wide default region.

Sub-commands:

##### regions list
Shows the default region settings and all custom regions.

##### regions add `<name>`
Creates a new named region. Returns its number for use in other sub-commands.

##### regions remove `<#>`
Deletes a custom region. All rooms in that region revert to the default region.

##### regions name `<#|name|default> <name>`
Renames a region.

##### regions topic `<#|name|default> <topic|clear>`
Sets the topic string for the region. Overrides the area-level topic for rooms in this region. Use `clear` to remove.

##### regions description `<#|name|default>`
Opens the string editor to write a lore description for the region.

##### regions comments `<#|name|default>`
Opens the string editor to write builder notes for the region.

##### regions flags `<#|name|default> <flag>`
Toggles a [region flag](aedit-flags-reference#region-flags) on the specified region.

##### regions who `<#|name|default> <title|blank>`
Overrides the [areawho](aedit-flags-reference#areawho-flags) display for rooms in this region.

##### regions place `<#|name|default> <placetype|none>`
Overrides the [placetype](aedit-flags-reference#placetype-flag) for rooms in this region.

### removetrade `<number>`
Currently unused.

### repop `<minutes>`
How often the area will reset.

### security `<0-9>`
Minimum security setting required to build in the zone.

### settrade
Unused.

### show
Shows the current state of the area being edited. Same as pressing enter.

### tags `<string>`
Sets searchable tags for the area. Auto-populated from the area name; use this to add additional keywords (e.g. builder names, alternate names, themes) that help with `alist` lookups.

### topic `<topic|clear>`
Sets the area-wide topic string. Topics appear in room descriptions and are used by quests and events to identify content themes. Use `clear` to remove. Can be overridden per-region with `regions topic`.

### varclear `<name>`
Clears the named variable.

### varset `<name> <number|string|room> <yes|no> <value>`
Sets a variable of the given type on the area index.

### viewtrade
Unused.

### vnum `<lower vnum> <upper vnum>`
Sets the vnum range for the area.

### wilds `<map uid>`
Sets the wilderness map that this area is 'on'

### x `<x coord>`
Sets the approximate location of this zone on its wilderness map.

### y `<y coord>`
Sets the approximate location of this zone on its wilderness map.