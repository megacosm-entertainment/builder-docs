---
layout: default
title: AEdit Flags Reference
nav_order: 3
parent: Area Editor
---

# Flags and Other Settings

## Area Flags


| Flag | Settable | Effect |
|:-----|:---------|:-------|
| changed | no | Used to mark that anything in the area has been changed. `asave changed` looks for this. |
| added | no | Used to mark newly added zones. |
| no_map | yes | Prevents showing the minimap. |
| dark | yes | Not currently used. |
| testport | yes | |
| no_recall | yes | Prevents recall, maybe other teleportation magics? |
| no_rooms | yes | Don't save rooms for this zone. |
| newbie | yes? | Reduce damage to players by 75%. Can only be saved on "Realm of Alendith." |
| no_get_random | yes | Prevents get_random_* from targeting anything in this zone. Mostly for questing. |
| no_fading | yes | Prevents use of 'fade' in the zone. |
| blueprint | yes? | Area is used to hold blueprint rooms. Prevents vlinks to zone. |
| locked | yes | Area cannot be entered by normal means.  Also works as no_get_random. |

## AreaWho Flags

These change how your location shows up on the 'who' list.

| AreaWho | Display |
|:--------|:--------|
| blank | |
| abyss | Abyss |
| aerial | Sky |
| arena | Arena |
| at_sea | At Sea |
| battle | Battle |
| castle | Castle |
| cavern | Cavern |
| chat | Rift |
| church | Church |
| cult | Cult |
| dungeon | Dungn |
| eden | Eden |
| forest | Forest |
| fort | Fort |
| home | Home |
| immortal | Cosmos |
| inn | Inn |
| isle | Isle |
| jungle | Jungle |
| keep | Keep |
| limbo | Limbo |
| mountain | Mount |
| netherworld | Nether |
| on_ship | Ship |
| outpost | Outpst |
| palace | Palace |
| pg | PoA |
| planar | Planar |
| pyramid | Pyramd |
| ruins | Ruins |
| swamp | Swamp |
| temple | Temple |
| tomb | Tomb |
| tower | Tower |
| towne | Towne |
| tundra | Tundra |
| undersea | Ocean |
| village | Villa |
| volcano | Vulcan |
| wilderness | Wilder |
| instance | Instce* |
| duty | Duty |


## PlaceType Flag
```
Nowhere            Wilderness         First Continent    Second Continent   
Third Continent    Fourth Continent   Island             Other Plane        
Abyss              Eden               Netherworld        
```

PlaceType is used in a few different ways. 
- Being in "Nowhere" or "Other Plane" prevent issuing or recieving arena challenges. 
- Scrying will only find objects on "First Continent," "Second Continent," "Third Continent," "Fourth Continent," "Island," or "Wilderness."
- Trying to gohall from "Nowhere," "Other Plane," "Island," or a wilderness zone is always considered cross-zone.
- Trying to gocall from a different continent than the location of your church hall is considered cross-zone.
- Used in many places to determine if areas are within the same 'region' of the wilds.
- The spell "earth walk" cannot be used in "Nowhere" or "Other Plane."
- Sages can currently only identify items from "First Continent," "Second Continent," "Island" placetypes, and the Undersea zone specifically.
- Gate cannot target "Nowhere" or "Other Plane" zones.
- In code, "First Continent" is used to check if 'IN_SERALIA' and "Second Continent" for 'IN_ATHEMIA.'
- Place types can be used to filter the 'alist' command to only show areas of a given placetype.
- Invasions can only happen on First/Second/Third/Fourth continent.