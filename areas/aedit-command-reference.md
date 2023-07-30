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
landx          landy          name           open           placetype      
postoffice     recall         removetrade    repop          security       
settrade       show           varclear       varset         viewtrade      
vnum           wilds          x              y              levels
notes
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