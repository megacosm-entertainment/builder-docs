---
layout: default
title: MEdit Fields Reference
nav_order: 4
parent: Mobile Editor
---

# Introduction
---
This is a list of all the flags (and flag-like) settings that can be added to mobs, with a brief explanation of what each does.

### Act Flags
```
sentinel           scavenger          protected          mount              
aggressive         stay_area          wimpy              pet                
train              practice           blacksmith         crew_seller        
no_lore            undead             cleric             mage               
thief              warrior            nopurge            outdoors           
restringer         indoors            questor            healer             
stay_locale        update_always      changer            banker      
```

| Flag | Settable? | Effect |
|:-----|:----------|:-------|
| sentienel | Y | Mob will not move around on its own. |
| scavenger | Y | Periodically takes the highest value item from the ground. |
| protected | Y | Cannot be harmed by normal means. Applied automatically to bankers. Exempt from random mob selection. Cannot be called with 'call familiar'|
| mount | Y | Exempt from random mob selection. Will not assist player unless player is a crusader. Does not steal xp in group. Can kick if set with the 'kick' offense. Must be mount to be ridden. Only mounts get movedice set. Will not randomly wander. |
| aggressive | Y | Can randomly attack players at random in the room. "Peace" command removes this flag from loaded mobs. Removed by 'momentary darkness' spell. Removed by 'charm person' spell. Removed if mob is rescued during quest. |
| stay_area | Y | Exempt from random mob selection. Will wander, but not into other zones.|
| wimpy | Y | May attempt to flee when receiving damage, if under 1/3rd max hp. |
| pet | Y | Purchasable charmed mobs. Players may have one pet. Cannot attack pets. Pets have no carrying capacity. |
| train | Y | Allows players to `train` stats (or skills above 75%). Exempt from random mob selection.  Protected. Cannot be called with 'call familiar'. Required flag if you want players to multiclass at this mob.  Can convert pracs <-> trains|
| practice | Y | Allows players to practice skills up to 75%. Allows bards to `rehearse` songs.  Exempt from random mob selection and 'call familiar'. Protected. |
| blacksmith | Y | Repairs gear. |
| crew_seller | Y | Currently unused. Deprecated by shop stock system. |
| no_lore | Y | Prevents 'mob lore'  |
| undead | Y | Set on animated mobs, cannot be calmed, cannot be slept, immune to plague, targetable by `turn` |
| cleric | Y | Mob WIS +3, STR +1, DEX -1 |
| mage | Y | Mob INT +3, DEX +1, STR -1 |
| thief | Y | Mob DEX +3, INT +1, WIS -1, increased sneak, hide, second attack, disarm, backstab skill |
| warrior | Y | Mob STR +3, INT -1, CON +2, increased second attack, third attack, disarm skill |
| nopurge | Y | Not purged when doing untargeted purge of room. Cannot be purged by scripts. |
| outdoors | Y | Will not wander to rooms flagged as 'indoors' |
| restringer | Y | Allows use of 'restring' and 'unrestring' commands, exempt from random mob selection |
| indoors | Y | Will only wander to rooms flagged as 'indoors' |
| questor | Y | Deprecated by addition of quest_data to mobs. |
| healer | Y | Allows use of 'heal' command - Can be deprecated by scripting, shop_data. Exempt from random mob selection. Protected.  |
| stay_locale | Y | Wanders, but only to rooms with same 'locale' set. |
| update_always | Y | UNUSED |
| changer | Y | Can convert silver <-> gold. Exempt from random mob selection. Protected.  Provides see_all. |
| banker | Y | Can give money. Exempt from random mob selection. Protected. Sees all.  |

### Act2 Flags
```
churchmaster       noquest            plane_tunneler     no_hunt            
airship_seller     wizi_mob           trader             loremaster         
no_resurrect       drop_eq            gq_master          wilds_wanderer     
ship_quest_master  reset_once         see_all            no_chase           
takes_skulls       pirate             player_hunter      invasion_leader    
invasion_mob       see_wizi           soul_deposit       use_skills_only    
can_level          no_xp              renewer            show_in_wilds   
```

| Flag | Settable? | Effect |
|:-----|:----------|:-------|
| churchmaster | Y | Allows various `church` administration commands. |
| noquest | Y | Exempt from random mob selection. |
| plane_tunneler | Y | Deprecated. Exempt from random mob selection. |
| no_hunt | Y | Prevents mob from being `hunt`ed. |
| airship_seller | Y | Deprecated(?) by shop rework. Exempt from random mob selection. |
| wizi_mob | Y | Hides mob from normal player interaction. Exempt from random mob selection. |
| trader | Y | Used for trade goods. Partially disabled? |
| loremaster | Y | Enables `lore` command. Exempt from random mob selection. |
| no_resurrect | Y | Sets mob's corpse to no_resurrect, no_animate |
| drop_eq | Y | Mob will drop all inv into the room on death. |
| gq_master | Y | Enables `deposit` command for gq items. |
| wilds_wanderr | Y | Mob will wander the wilds |
| ship_quest_master | Y | Part of invasion system. Flag currently does nothing. |
| reset_once | Y | Sets mob limit to max of 1 in its area. |
| see_all | Y | Sees through all mortal forms of invisibility. |
| no_chase | Y | Will not attempt to hunt fleeing players. |
| takes_skulls | Y | Will attempt to take player skulls in the room, if possible. |
| pirate | Y | Currently unused. |
| player_hunter | Y | Currently unused. |
| invasion_leader | Y | Prevents mob head from decaying, part of Invasions/Ship Quests. |
| invasion_mob | Y | Part of Invasion system, chance to give quest point on death. |
| see_wizi | Y | Can see through all immortal forms of invisibility. |
| soul_deposit | Y | Currently unused |
| use_skills_only | Y | Currently unused. |
| can_level | Y | Capable of gaining experience and leveling. |
| no_xp | Y | Awards 0 experience. |
| renewer | Y | Enables `renew` command. |
| show_in_wilds | Y | Appears in wilds as <span style="color:yellow">@</span> |

### AffectedBy
```
blind              invisible          detect_evil        detect_invis       
detect_magic       detect_hidden      detect_good        sanctuary          
faerie_fire        infrared           curse              death_grip         
poison             sneak              hide               sleep              
charm              flying             pass_door          haste              
calm               plague             weaken             frenzy             
berserk            swim               regeneration       slow               
web
```

| Flag | Settable? | Effect |
|:-----|:----------|:-------|
| blind | Y | Affected by the blind spell|
| invisible | Y | Affected by 'invisible' |
| detect_evil | Y | |
| detect_invis | Y | |
| detect_magic | Y | |
| detect_hidden | Y | |
| detect_good | Y | |
| sanctuary | Y | |
| faerie_fire | Y | |
| infrared | Y | |
| curse | Y | |
| death_grip | Y | |
| poison | Y | |
| sneak | | |
| hide | | |
| sleep | | |
| charm | | |
| flying | | |
| pass_door | | |
| haste | | |
| calm | | |
| plague | | | 
| weaken | | |
| frenzy | | |
| berserk | | |
| swim | | |
| regeneration | | |
| slow | | |
| web | | |

### Affect2
```
silence            evasion            cloak_guile        warcry             
light_shroud       healing_aura       energy_field       spell_shield       
spell_deflection   avatar_shield      fatigue            paralysis          
neurotoxin         toxin              electrical_barrier fire_barrier       
frost_barrier      improved_invis     ensnare            see_cloak          
stone_skin         morphlock          deathsight         immobile           
protected
```

| Flag | Settable? | Effect |
|:-----|:----------|:-------|
| silence | Y | Affected by 'silence'. Cannot recite scrolls, communicate verbally, use `warcry` or `cast`. |
| evasion | Y | Affected by 'evasion'. Increased chance to dodge attacks. Chance to avoid aggro from mobs. |
| cloak_guile | Y | Further chance to avoid aggro mobs.|
| warcry | Y | Increased skill in many combat abilities. |
| light_shroud | Y | Reduced damage from undead by 10%. Blocks kill spell.|
| healing_aura | Y | Another form of regeneration. Mutually exclusive with regeneration. |
| energy_field | Y | Affected by 'energy field' - Reduces incoming energy/lightning damage by 10%. |
| spell_shield | Y | Affected by 'spell shield'. Unused..? |
| spell_deflection | Y | Affected by 'spell deflection'. Chance to bounce spells back at random targets. |
| avatar_shield | Y | Reduces damage from evil by 10%. |
| fatigue | Y | Lowers victim movement. |
| paralysis | Y | Prevents fleeing. |
| neurotoxin | Y | Affected by neurotoxin, lowers int. |
| toxin | Y | Affected by toxins, lowers strength. |
| electrical_barrier | Y | Affected by 'electrical barrier'. Chance to bounce damage back at enemies. |
| fire_barrier | Y | Affected by 'fire barrier'. Chance to bounce damage back at enemies. |
| frost_barrier | Y | Chance to bounce damage back at enemies. |
| improved_invis | Y | Affected by 'improved invis'. Invis isn't broken by combat. |
| ensnare | Y | Affected by 'ensnare'. Cannot move. |
| see_cloak | Y | Can see through 'cloak of guile' spell. |
| stone_skin | Y | Affected by 'stone skin'. Harder to bite. Improves AC. |
| morphlock | Y | Prevents use of `shift` or `shape`. |
| deathsight | Y | Affected by 'deathsight' spell. Can see ghosts. |
| immobile | Y | Currently just means unable to flee. |
| protected | Y | Protected from magic or damage. |

### Special Functions
```
breath_any         breath_acid        breath_fire        breath_frost       
breath_gas         breath_lightning   cast_adept         cast_cleric        
cast_judge         cast_mage          cast_undead        executioner        
fido               guard              janitor            mayor              
poison             thief              nasty              patrolman          
questmaster        protector          dark_magic         magic_master       
fight_bot          pirate             pirate_hunter      invasion           
invasion_leader
```

These are mostly deprecated.. they provide some functions, but will likely be replaced by utility scripts you can attach to mobs.

| Flag | Settable? | Effect |
|:-----|:----------|:-------|
| breath_any | Y | Will attempt to use any of the breath affects at random. |
| breath_acid | Y | Periodically attempts to use acid breath. |
| breath_fire | Y | Periodically attempts to use fire breath. |
| breath_frost | Y | Periodically attempts to use frost breath. |
| breath_gas | Y | Periodically attempts to use gas breath. |
| breath_lightning | Y | Periodically attempts to use lightning breath. |
| cast_adept | Y | Periodically casts one of armour, bless, cure blindness, heal, cure poison, refresh, or cure disease on players level 10 or under. |
| cast_cleric | Y | Uses blindness, cause serious, earthquake, cause critical, dispel evil, curse, flamestrike, harm, plague, or dispel magic in combat, based on caster level. |
| cast_judge | Y | Uses 'high explosive' in combat. |
| cast_mage | Y | Depending on caster level, can use fireball, chill touch, weaken, colour spray, change sex, energy drain, plague, or acid blast in combat. |
| cast_undead | Y | Depending on caster level, can use curse, weaken, chill touch, blindness, poison, energy drain, harm, teleport, or plague in combat. |
| executioner | Y | Used to begin combat with 'criminals' - There are no crimes currently defined, so this is functionally disabled. |
| fido | Y | Consumes mob corpses in the room, leaving items on the floor. |
| guard | Y | Tells players to put pants on if male. If female, may tell them to put clothes on or grossly hit on them. Don't use this, please. |
| janitor | Y | Picks up and purges low value (under 10 silver) trash or drink containers in the room. |
| mayor | Y | Does absolutely nothing. Entire function commented out. Used to do pathfinding and speech for the Olarian Mayor. |
| poison | Y | Attempts to bite enemies in combat, injecting them with poison. |
| thief | Y | Will attempt to steal coins from players. |
| nasty | Y | Will attempt to backstab players that are slightly above (within 10 levels) the mob's level, then attempt to flee. |
| patrolman | Y | Attempts to break up fights in the room.. by joining them. |
| questmaster | Y | If fighting, acts as the 'cast_mage' special function. |
| protector | Y | Unused. Used to make Sir Albert Steiner throw people over level 30 in gaol if they attacked mobs in Plith. |
| dark_magic | Y | Like some of the other caster specs, but needlessly edgy in its speech lines. Completely commented out. |
| magic_master | Y | If blinded, attempts to cure blind. If cursed, remove curse. If not affected by sanctuary, attempts to sanc itself. Will also attempt to faerie fire itself for some reason. May attempt to cure itself, dispel magic on enemies, and otherwise cast a large variety of spells. |
| fight_bot | Y | Attempts to web, silence, blind, poison, plague enemies if not affected by these. Will cure blind itself, will attempt to remove/rewear all if not sanced. Can counterspell if enemy casts. Will otherwise cast magic missile. |
| pirate | Y | Same as fight_bot. Used in some invasion/ship quest code. Do not use. |
| pirate_hunter | Y | Limited form of fight_bot, used for going after pirate players. |
| invasion | Y | Just does various emotes. Used in invasions |
| invasion_leader | Y | Limited fight_bot, cannot be attacked by players outside of invasion quest level range. |

### Sex
```
male               female             neutral            random             
none
``` 

### Parts
```
arms               brains             claws              ear                
eye                eyestalks          fangs              feet               
fingers            fins               gills              guts               
hands              head               heart              hide               
horns              legs               long_tongue        scales             
tail               tentacles          tusks              wings              
```

| Flag | Settable? | Effect |
|:-----|:----------|:-------|

### Races
```
Available races are:
 none            human           elf            
 dwarf           titan           vampire        
 drow            sith            draconian      
 slayer          minotaur        angel          
 mystic          demon           lich           
 avatar          seraph          berserker      
 colossus        fiend           specter        
 naga            dragon          changeling     
 hell baron      wraith          shaper         
 were_changed    mob_vampire     bat            
 werewolf        bear            bugbear        
 cat             centipede       dog            
 doll            fido            fox            
 goblin          hobgoblin       kobold         
 lizard          doxian          orc            
 pig             rabbit          school monster 
 snake           song bird       golem          
 unicorn         griffon         troll          
 water fowl      giant           wolf           
 wyvern          nileshian       skeleton       
 zombie          wisp            insect         
 gnome           angel_mob       demon_mob      
 rodent          treant          horse          
 bird            fungus          unique  
```

| Flag | Settable? | Effect |
|:-----|:----------|:-------|

### Imm/Res/Vuln
```
summon             charm              magic              weapon             
bash               pierce             slash              fire               
cold               light              lightning          acid               
poison             negative           holy               energy             
mental             disease            drowning           sound              
wood               silver             iron               kill               
water              air                earth              plant     
```

These are used to set damage types that mobs may be resistant, vulnerable, or immune to. This is fairly straightforward in Sentience.
- Resist means that damage is reduced by 25% from that type.
- Vulnerable means that damage is increased by 25% from that type.
- Immune means no damage is taken from that type.

### Offenses
```
area_attack        backstab           bash               berserk            
disarm             dodge              fade               kick               
dirt_kick          parry              rescue             tail               
trip               crush              assist_all         assist_align       
assist_race        assist_players     assist_npc         assist_guard       
assist_vnum        magic              
```

| Flag | Settable? | Effect |
|:-----|:----------|:-------|
| area_attack | | |
| backstab | | |
| bash | | |
| berserk | | |
| disarm | | |
| dodge | | |
| fade | | |
| kick | | |
| dirt_kick | | |
| parry | | |
| rescue | | |
| tail | | |
| trip | | |
| crush | | |
| assist_all | | |
| assist_align | | |
| assist_race | | |
| assist_players | | |
| assist_npc | | |
| assist_guard | | |
| assist_vnum | | |
| magic | | |

### Size
```
tiny               small              medium             large              
huge               giant              
```

| Flag | Settable? | Effect |
|:-----|:----------|:-------|

### Material
```
air                dirt               dust               paper              
organic            fruit              meat               moss               
leaves             skin               ink                flesh              
clay               mud                cloth              cotton             
fur                light              wool               energy             
fire               water              glass              feathers           
intestines         muscle             parchment          rubber             
slime              bark               wicker             reed               
sand               salt               phosphorus         sulfur             
hemp               leather            porcelain          crystal            
ceramic            wood               ice                tin                
brass              copper             zinc               bone               
shell              metal              bronze             iron               
steel              lead               onyx               ivory              
silk               silver             nickel             quartz             
obsidian           gemstone           spidersilk         ceramic            
ruby               sapphire           emerald            amethyst           
opal               pearl              topaz              gold               
granite            concrete           stone              marble             
platinum           moonstone          dragonscale        diamond            
mithril            papyrus            plant              unknown  
```

| Flag | Settable? | Effect |
|:-----|:----------|:-------|

### Position
```
sleeping           resting            sitting            standing           
feigned            
```

| Flag | Settable? | Effect |
|:-----|:----------|:-------|

### Damtype (for unarmed)
```
Name                 Noun
none                 hit                 
slice                slice               
stab                 stab                
slash                slash               
whip                 whip                
claw                 claw                
blast                blast               
pound                pound               
crush                crush               
grep                 grep                
bite                 bite                
pierce               pierce              
suction              suction             
beating              beating             
digestion            digestion           
charge               charge              
slap                 slap                
punch                punch               
wrath                wrath               
magic                magic               
divine               divine power        

Name                 Noun
holy                 holy fire           
cleave               cleave              
scratch              scratch             
peck                 peck                
peckb                peck                
chop                 chop                
sting                sting               
smash                smash               
shbite               shocking bite       
flbite               flaming bite        
frbite               freezing bite       
acbite               acidic bite         
chomp                chomp               
drain                life drain          
decay                decaying touch      
thrust               thrust              
slime                slime               
shock                shock               
thwack               thwack              
flame                flame               
chill                chill               

Name                 Noun
vorpal               slash               
purify               purifying light     
crblow               crippling blow      
acrid                acrid spray         
blight               blighting touch     
boiling              boiling spray       
corrode              corrosion           
discharge            discharge           
flog                 flog                
fumes                caustic fumes       
lacerate             laceration          
lash                 lash                
lbolt                lightning bolt      
pain                 pain                
pestilence           pestilence          
scorch               scorching           
scourge              scourge             
scream               scream              
shining              shining light       
shriek               deafening shriek    
spike                spike               

Name                 Noun
spores               spores              
surge                surge               
torrent              watery torrent      
toxblast             toxic blast         
venom                venom               
winbrth              wintery breath
```

| Flag | Settable? | Effect |
|:-----|:----------|:-------|

### Corpse Type
```
charred            dissolve           explode            flay               
frozen             iceblock           incinerate         melted             
nocorpse           normal             shatter            skeletal           
stone              withered           
```

| Flag | Settable? | Effect |
|:-----|:----------|:-------|