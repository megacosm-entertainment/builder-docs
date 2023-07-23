---
layout: default
title: OEdit Command Reference
nav_order: 2
parent: Object Editor
---

```
?              addaffect      addcatalyst    addimmune      addoprog       
addskill       addspell       allowedfixed   commands       comments       
condition      cost           create         delaffect      delcatalyst    
delimmune      deloprog       delspell       description    ed             
extra          extra2         extra3         extra4         fragility      
level          lock           long           material       name           
next           oupdate        persist        prev           scriptkwd      
short          show           sign           timer          type           
v0             v1             v2             v3             v4             
v5             v6             v7             varclear       varset         
waypoints      wear           weight         
```

### ?
This is used to access the list of available flag banks, but is not limited to object flags. You can do things like `? token` to get a list of token types that can be set, for example.

### addaffect `<apply location> <#modifier> <#rand>`
Adds affects to the object. See ['apply'](oedit-flags-reference#apply-flags-stat-modifiers) flags for more details.

### addcatalyst `<type> <strength> <charges> <chance> <active> [name]`
Adds [catalyst](oedit-flags-reference#catalyst-types) data to the object. Catalysts are a work in progress.

### addimmune `<imm|res|vuln> <flag> <#rand>`
Allows adding an [imm/res/vuln flag](../mobiles/medit-flags-reference.md#immresvuln) as an affect on the object.

### addoprog `<vnum> <trigger> <phrase>`
Adds an object script to an object, with the given trigger and phrase

### addskill `<skill name> <#modifier> <#rand>`
Adds skill modifier affects to an object.

### addspell `<spell name> <spell level> <#rand>`
Adds a spell to an object that will be cast on the wearer (or used for zap, brandish, etc, for wands, staves, scrolls, and so on).

### allowedfixed `<max repairs>`
Sets the maximum number of times an object can be repaired.

### commands
Lists all available commands in the object editor.

### comments
Opens the string editor to write builder comments. This field is like writing any description, but is only visible to builders. Leave notes here about plans, things you need help with, the purpose of the object, related vnums, etc.

### condition `<0-100>`
Sets the starting condition for the object.

### cost `<silver>`
Sets the default cost of the object, in silver.

### create `[vnum]`
Creates a new object. Will use next available vnum in the current zone, unless a vnum is specified.

### delaffect `<affect number>`
Removes the specified affect from the object.

### delcatalyst `<catalyst data number>`
Removes the specified catalyst from the object.

### delimmune `<imm/res/vuln affect number>`
Removes the specified imm/res/vuln flag from the object.

### deloprog `<obj prog number>`
Removes the specified object prog from the object.

### delspell `<spell number>`
Removes the specified spell from the object.

### description
Enters a string editor to set the description used when players `look <object>`

### ed `<add | delete | edit | format | environment> <keyword>` -or- `copy <old keyword> <new keyword>`
Used to add extra descriptions to the object, which can be used to set additional keywords players can look at. This can be used for things like clues, or simply additional flavour.

### extra `<flag1> [flag2] [flag3] ...`
Sets ['extra'](oedit-flags-reference#extra-flags) flags on the object.

### extra2 `<flag1> [flag2] [flag3] ...`
Sets ['extra2'](oedit-flags-reference#extra2-flags) flags on the object.

### extra3 `<flag1> [flag2] [flag3] ...`
Sets ['extra3'](oedit-flags-reference#extra3-flags) flags on the object.

### extra4 `<flag1> [flag2] [flag3] ...`
Sets ['extra4'](oedit-flags-reference#extra4-flags) flags on the object.

### fragility

### level `<level>`
Sets the object level. For gear, this is required for being able to wear/wield objects. Will also assign a number of 'points' for changing things on the object.

### lock

### long `<string>`
Sets the long description (shown in the room) for the object.

### material

### name `<keywords>`
Sets keywords used for players to interact with the object.

### next
Moves to editing the next object in the zone.

### oupdate

### persist

### prev
Moves to editing the previous object in the zone.

### scriptkwd `<keywords>`
As name, but only accessible to scripting.

### short `<string>`
A short line that is used to describe the object in inventory, equipment list, etc.

### show
Shows the current state of the object being edited. Can be accessed by pressing enter.

### sign
Allows Implementors to 'sign' an object, mostly freeing it from point constraints.

### timer `<number>`
Sets the default time until the object destroys itself.

### type

### v0

### v1

### v2

### v3

### v4

### v5

### v6

### v7

### varclear `<variable name>`

### varset

### waypoints

### wear `<flag1> [flag2] [flag3] ...`
Sets [wear locations](oedit-flags-reference#wear-flags) for the object. `take` is mandatory for anything players need to be able to pick up.

### weight