---
layout: default
title: REdit Flags Reference
nav_order: 3
parent: Room Editor
---

# Room Flags Reference
{: .no_toc }

<details open markdown="block">
<summary>Table of Contents</summary>
{: .text-delta }
- TOC
{:toc}
</details>

---

## Room Flags

Set with `room <flag>` in the room editor. Multiple flags can be combined.

| Flag | Description |
|:-----|:------------|
| `dark` | Room is permanently dark. Characters need a light source to see. |
| `gods_only` | Only immortals/gods can enter. |
| `imp_only` | Only implementors can enter. |
| `indoors` | Room is treated as inside (affects weather, some spells, etc.). |
| `newbies_only` | Only new characters can enter (level-gated). |
| `no_map` | Room does not appear on the area map. |
| `no_mob` | NPCs cannot enter this room (they won't wander in). |
| `no_recall` | Characters cannot use the `recall` command here. |
| `no_wander` | Wandering mobs won't wander into this room. |
| `noview` | Characters cannot look through windows or `peek` into this room. |
| `pet_shop` | Enables the pet shop functionality. NPCs present become purchasable pets. |
| `private` | Only the room owner can enter (plus immortals). Requires `owner` to be set. |
| `safe` | No combat is allowed in this room. NPCs won't attack. |
| `solitary` | Only one character can be in this room at once. |
| `arena` | Designates the room as part of the arena. PK combat is freely allowed. |
| `bank` | Enables banker NPC functionality in this room. |
| `chaotic` | Marks the room as chaotic aligned. |
| `dark_attack` | Attackers strike more effectively in the dark. |
| `death_trap` | Characters who enter this room die instantly. |
| `helm` | Marks this room as a ship's helm (for ship navigation). |
| `locker` | Enables the locker system in this room. |
| `nocomm` | Communication commands (say, shout, etc.) are blocked here. |
| `nomagic` | Spellcasting is disabled in this room. |
| `nowhere` | Room is outside normal world geography (instances, limbo, etc.). |
| `pk` | PK combat is allowed in this room (without full arena status). |
| `real_estate` | Room can be purchased as player housing. |
| `rocks` | Movement through this room costs extra move points (rocky terrain). |
| `ship_shop` | Enables ship purchasing functionality. |
| `underwater` | Room is underwater. Characters need water-breathing to survive. |
| `view_wilds` | Room has a view of the wilderness map. |

---

## Room2 Flags

Set with `room2 <flag>` in the room editor.

| Flag | Description |
|:-----|:------------|
| `alchemy` | Enables alchemy crafting in this room. |
| `bar` | Marks the room as a bar — affects drunk mechanics and ambient flavor. |
| `briars` | Movement causes scratch damage. |
| `citymove` | Room counts as city terrain for city-movement bonuses. |
| `drain_mana` | Mana drains slowly while in this room. |
| `hard_magic` | Spells are more difficult to cast here (harder spell checks). |
| `multiplay` | Multiple characters from the same account can be in this room. |
| `slow_magic` | Spells take longer to cast in this room. |
| `toxic_bog` | Characters take periodic toxic damage while in this room. |
| `virtual_room` | Marks the room as a virtual room (used in scripted instances). |
| `fire` | Room contains fire — periodic fire damage. |
| `icy` | Room is icy — movement penalty, possible slip effects. |
| `no_quest` | Quest givers will not assign quests that bring players to this room. |
| `no_quit` | Players cannot quit the game in this room. |
| `post_office` | Enables mailing system in this room. |
| `underground` | Room is underground (affects sunlight, some spells). |
| `vis_on_map` | Room appears on the in-game map display. |
| `no_floor` | Room has no floor — falling damage may apply, or special movement required. |
| `clone_persist` | Cloned rooms (instances) keep their state between resets. |
| `blueprint` | Room was created as part of a blueprint instance. |
| `no_clone` | This room cannot be cloned by blueprint systems. |
| `no_get_random` | Random item drops do not appear in this room. |
| `always_update` | Room always runs its update tick regardless of occupancy. |
| `safe_harbor` | Ships can anchor safely at this room. |
| `vault` | Room functions as a secure vault (player housing/org storage). |

---

## Sector Types

Set with `sector <type>`. Affects movement cost, map display, and terrain-related gameplay.

| Sector | Description |
|:-------|:------------|
| `inside` | Indoor room of a building. Low movement cost. |
| `city` | City streets or paved urban areas. Low movement cost. |
| `field` | Open fields and plains. Standard movement cost. |
| `forest` | Wooded areas. Moderate movement cost. |
| `hills` | Rolling hills. Moderate-high movement cost. |
| `mountain` | Rocky mountains. High movement cost. |
| `swim` | Shallow water — can be entered without swimming ability. |
| `noswim` | Deep water — requires swimming or water-breathing to survive. |
| `tundra` | Frozen tundra. Moderate movement cost, cold effects. |
| `air` | Open sky. Requires flying ability. |
| `desert` | Hot desert. Higher thirst drain, moderate movement cost. |
| `netherworld` | Astral/otherworldly space. Special movement rules. |
| `dock` | Port/dock area. Standard movement. Transition point for ships. |
| `enchanted_forest` | Magical forest. May have spell effects. Moderate movement cost. |
| `toxic_bog` | Swampy toxic terrain. Periodic toxin damage. |
| `cursed_sanctum` | Cursed sacred ground. Special effects. |
| `bramble` | Dense thorns. High movement cost, possible scratch damage. |
| `swamp` | Swamp terrain. High movement cost, slow movement. |
| `acid` | Acid terrain. Periodic acid damage. |
| `lava` | Molten lava. High fire damage, very high movement cost. |
| `snow` | Snow-covered ground. Moderate movement cost, cold effects. |
| `ice` | Icy surface. Slip chance, cold effects. |
| `cave` | Underground cavern. Moderate movement cost. |
| `underwater` | Fully submerged underwater. Requires water-breathing. |
| `deep_underwater` | Deep water. Higher difficulty than `underwater`. |
| `jungle` | Dense jungle. High movement cost. |
| `dirt_road` | Dirt path. Low movement cost. |
| `paved_road` | Paved road. Minimum movement cost. |

---

## Exit Flags

Applied per-direction when creating exits. E.g. `north <vnum> door closed`.

| Flag | Description |
|:-----|:------------|
| `door` | Exit has a door that can be opened/closed. |
| `closed` | Door starts closed (requires `door`). |
| `nopass` | Door cannot be passed through magically (no `pass door` spell). |
| `noclose` | Door cannot be closed once open. |
| `nolock` | Door cannot be locked. |
| `hidden` | Exit is hidden — not shown in room description. |
| `found` | Hidden exit that has been found (shows after search). |
| `broken` | Door is broken — stuck open or closed. |
| `nobash` | Door cannot be bashed open. |
| `walkthrough` | Characters can walk through this exit despite it appearing closed. |
| `nobar` | Door cannot be barricaded. |
| `aerial` | Exit is an aerial passage — requires flying ability. |
| `nohunt` | Mobs cannot pathfind through this exit. |
| `environment` | Exit is an environmental boundary (used for instancing). |
| `nounlink` | Exit cannot be unlinked by scripts or events. |
| `nosearch` | This exit cannot be found by the `search` command. |
| `mustsee` | Character must look at the exit specifically to notice it. |

---

## Lock Flags

Set on door exits to configure locking behavior.

| Flag | Description |
|:-----|:------------|
| `locked` | Door starts locked. Requires a key to open. |
| `magic` | Door requires magical means to unlock. |
| `snap_key` | Key breaks after one use. |
| `script` | Lock is controlled by a script (not standard key). |
| `noremove` | Key cannot be removed from lock once inserted. |
| `broken` | Lock is broken — cannot be locked or unlocked normally. |
| `jammed` | Lock is jammed closed. Requires bashing to break. |
| `nojam` | Lock cannot be jammed. |


### Room Flags
```
dark               gods_only          imp_only           indoors            
newbies_only       no_map             no_mob             no_recall          
no_wander          noview             pet_shop           private            
safe               solitary           arena              bank               
cpk                dark_attack        death_trap         helm               
locker             nocomm             nomagic            nowhere            
pk                 real_estate        rocks              ship_shop          
underwater         view_wilds
```

### Room2 Flags
```
alchemy            bar                briars             citymove           
drain_mana         hard_magic         multiplay          slow_magic         
toxic_bog          fire               icy                no_quest           
no_quit            post_office        underground        vis_on_map         
no_floor           clone_persist      blueprint          no_clone           
no_get_random      always_update      safe_harbor        
```

### Sector Types
```
inside             city               field              forest             
hills              mountain           swim               noswim             
tundra             air                desert             netherworld        
dock               enchanted_forest   toxic_bog          cursed_sanctum     
bramble            swamp              acid               lava               
snow               ice                cave               underwater         
deep_underwater    jungle             
```

### Exit Types
```
door               closed             nopass             noclose            
nolock             hidden             found              broken             
nobash             walkthrough        nobar              aerial             
nohunt             environment        nounlink           nosearch           
mustsee            
```

### Lock Flags
```
locked             magic              snap_key           script             
noremove           broken             jammed             nojam   
```