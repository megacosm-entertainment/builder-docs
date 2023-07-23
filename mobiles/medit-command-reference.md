---
layout: default
title: MEdit Command Reference
nav_order: 3
parent: Mobile Editor
---

```
?              act            act2           addmprog       affect         
affect2        alignment      armour         attacks        commands       
comments       create         damdice        damtype        delmprog       
description    hitdice        hitroll        immune         level          
long           manadice       material       movedice       name           
next           off            owner          part           persist        
position       prev           questor        crew           boss           
race           res            sex            shop           short          
show           sign           size           spec           vuln           
wealth         scriptkwd      varset         varclear       corpsetype     
corpsevnum     zombievnum 
```

### ?
This is used to access the list of available flag banks, but is not limited to mobile flags. You can do things like `? token` to get a list of token types that can be set, for example.

### act `<flag1> [flag2] [flag3] ...`
Used to set act flags for mobs. Refer to [act flags reference](medit-flags-reference#act-flags) for more details.

### act2 `<flag1> [flag2] [flag3] ...`
Used to set act2 flags for mobs. Refer to [act2 flags reference](medit-flags-reference#act2-flags) for more details.

### addmprog `<vnum> <trigger> <phrase>`
Adds a [mob prog](scripting/mobile-programs) to the mob, set to fire on the given trigger, with the provided phrase.

`Example: addmprog 11001 sayto hello` - This would cause mob prog 11001 to fire when a player used `sayto <mob> hello`.

### affect `<affect1> [affect2] [affect3] ...`
Used to set affect flags for mobs. Refer to [affect flags reference](medit-flags-reference#affectedby) for more details.

### affect2 `<affect2 1> [affect2 2] [affect2 3] ...`
Used to set affect2 flags for mobs. Refer to [affect2 flags reference](medit-flags-reference#affect2) for more details.

### alignment `<number>`
Used to set alignment for the mob. Used for some damage and experience checks, or by scripting.

### armour `[pierce] [bash] [slash] [exotic]`
Sets the mob's base armour class values. Lower is better.

### attacks `<number>`
How many attacks the mob does per round. Set to 0 to have it function like normal combat, or a negative to do scripted attacks only.

### commands
Lists all of the commands available in the mobile editor. You're here!

### comments
Opens the string editor to write builder comments. This field is like writing any description, but is only visible to builders. Leave notes here about plans, things you need help with, the purpose of the mob, relevant other vnums, etc.

### create `[vnum]`
Creates a brand new mobile. If vnum is omitted, it will use the next available in your current zone.

### damdice `<number> <sides> <bonus>`
Sets base damage dice for the mob (if not using a weapon.) Works out to `<number>d<sides>+bonus`

### damtype `<name>`
Used to set the damage verb used for the mob, such as 'wintery breath' or 'boiling spray'. Impacts damage type used in functions.

### delmprog `<number>`
Removes the listed mob prog from the mob.

### description
Enters the string editor to set a description that will be seen when players `look <mob>`. Make it a good one!

### hitdice `<number> <type> <bonus>`
As with damdice, sets the hit dice used to determine this mob's health pool.

### hitroll `<number>`
Not used anymore outside of scripting.

### immune `<flag1> [flag2] [flag3] ...`
Sets [immunity flags](medit-flags-reference#immresvuln) for the mob. These damage types will do nothing to it.

### level `<number>`
Set's the mob's level. Used to set defaults for many values.

### long `<string>`
Sets the description of the mobile as seen to players in the room.

Ex: long An old dwarf sits at a desk piled high with papers.
### manadice `<number> <type> <bonus>`
Sets the mana dice given to the mob. Automatically set when assigning a level. Defaults to 

### material `<material>`
Sets the mob's [material](../objects/oedit-flags-reference#material). May be used in some damage calculations?

### movedice
Sets the movement given to the mob. Automatically set when giving the mob a level. Defaults to `10 + 13 * mob level`

### name `<keywords>`
Sets the keywords used to interact with the mob.

Ex: name guard plith

### next
Begins editing the next mob in the area.

### off `<flag1> [flag2] [flag3] ...`
Sets [offensive behaviours](medit-flags-reference#offenses) for the mob.

### owner `<string>`
Sets an 'owner' on the mob. May be useful for having pets for npcs?

### part `<part1> [part2] [part3] ...`
Sets (or removes) additional body parts on the mob.

### persist `<on|off>`
Marks the mob as persistent. Copies of this mob will save more like players, and can be used to have an npc that remembers specific events across reboots, etc. Use sparingly.

### position `<start|default> <position>`
Sets the starting or default [position](medit-flags-reference#position) of the mob.

### prev
Switches to editing the previous mob in the area.

### questor `<many commands>`
Sets questmaster data on the mob. See [questor](medit-additional-data#questor) for more details.

### crew `<many commands>`
Sets crew data on the mob. See [crew](medit-additional-data#crew) for more details.

### boss `<on|off>`
Marks the mob as a boss. Currently unused outside of dungeons.

### race `<race>`
Sets the mob's [race](medit-flags-reference#races). This also sets default parts for the mob.

### res `<flag1> [flag2] [flag3] ...`
Sets [resist flags](medit-flags-reference#immresvuln) for the mob. These damage types will do 25% less damage.

### sex `<sex>`
Sets the [sex](medit-flags-reference#sex) of the mob. 

### shop `<many commands>`
Sets shop data on the mob. See [shop](medit-additional-data#shop) for more details.

### short `<string>`
Sets the short desc of the mob. This is what players see when interacting with the mob.

Ex: short the passport officer

### show
Shows the current state of the mob. Can also be viewed by pressing enter with no commands.

### sign
Used by Implementors to 'sign' the mob. This allows the mob to bypass point restrictions and ignore balance safeguards.

### size `<size>`
Sets the mob's [size](medit-flags-reference#size). Used in some damage calculations, as well as scripting.

### spec `<spec1> [spec2] [spec3] ...`
Sets [special function(s)](medit-flags-reference#special-functions) on the mob. This may be getting deprecated by scripting. Beware!

### vuln `<flag1> [flag2] [flag3]`
Sets [vulnerability flags](medit-flags-reference#immresvuln) for the mob. These damage types will do 25% more damage.

### wealth `<number>`
Sets the mob's carried coins, in silver.

### scriptkwd `<keywords>`
Used to set keywords on the mob that can only be used by scripting. Somewhat deprecated, deprecates old style of mob naming `mmVNUMmm` for use by scripts.

### varset `<name> <number|string|room> <yes|no> <value>`
Used to set a variable that will load on every instance of the mob. This can be a number, a string, or reference to a room. The Yes/No value is to mark if the variable should be saved on the mob.

Ex: `varset home room yes 4000` - This would set a variable on the mob, 'home', as a reference to room 4000.

### varclear `<variable name>`
Clears the given variable on the mob.

Ex: `varclear home`

### corpsetype `<corpse type>`
Sets the default [corpse type](medit-flags-reference#corpse-type) for the mob.

### corpsevnum `<vnum>`
Sets a custom vnum to be left behind as the mob's corpse.

### zombievnum `<vnum>`
Sets a custom 'zombie' vnum for the mob (for use with animate dead)