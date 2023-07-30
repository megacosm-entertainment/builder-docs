---
layout: default
title: REdit Command Reference
nav_order: 2
parent: Room Editor
---

```
?              addcdesc       addrprog       commands       comments       
coords         create         delcdesc       delrprog       description    
dislink        down           east           ed             editcdesc      
heal           locale         mana           move           mreset         
name           north          northeast      northwest      oreset         
owner          persist        room           room2          sector         
show           south          southeast      southwest      up             
west           varset         varclear       
```

### ?
This is used to access the list of available flag banks, but is not limited to room flags. You can do things like `? token` to get a list of token types that can be set, for example.

### addcdesc `<type> <phrase>`
Used to add conditional descriptions to the room, based on specific criteria.

### addrprog `<vnum> <trigger> <phrase>`

### commands
Shows the list of commands available in the room editor

### comments
Opens the string editor to write builder comments. This field is like writing any description, but is only visible to builders. Leave notes here about plans, things you need help with, the purpose of the room, related vnums, etc.

### coords `<x> <y> <z> [uid]`
Sets the room's coordinates on a wilderness map. Meant for use with the show_wilds room2 flag.

### create `[vnum]`
Creates a room using the next available vnum in the zone. If `[vnum]` is specified, it will create a room at that vnum instead.

### delcdesc

### delrprog `<number>`
Removes the selected room prog from the room.

### description
Enters the string editor to set a description for the room.

### dislink

### down

### east

### ed

### editcdesc

### heal `<number>`
Sets the health regen rate of the room.

### locale `<number>`
Sets a locale for the room. Locales are used for the `stay_locale` mob flag.

### mana `<number>`
Sets the mana regen rate of the room.

### move `<number>`
Sets the movement regen rate of the room.

### mreset `<vnum> <maximum number> <minimum number>`
Sets a mob reset for the room.

### name `<string>`
Sets the name of the room.

### north

### northeast

### northwest

### oreset

### owner `<string>`
Sets an owner for the room. Owners are able to ignore the 'private' room flag.

### persist `<yes|no>`

### room `<flag1> [flag2] [flag3] ...`
Sets one or more [`room` flags](redit-flags-reference#room-flags)

### room2 `<flag1> [flag2] [flag3] ...`

### sector `<sector>`
Sets the [sector](redit-flags-reference#sector-types) of the room. Used for movement loss, among other things.

### show
Shows the current state of the room being edited. Can also be accessed by pressing enter.

### south

### southeast

### southwest

### up

### west

### varset `<name> <number|string|room> <yes|no> <value>`
Sets the given value on the room index.

### varclear