---
layout: default
title: MEdit Fields Reference
nav_order: 3
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

### Act2 Flags (First bank filled up)
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
| plane_tunneler | Y | Partially deprecated, only allows Olaria, Goblin Fort, and Reza. Reworking with scripts. |
| no_hunt | a | |
| airship_seller | | |
| wizi_mob | | |
| trader | | |
| loremaster | | |
| no_resurrect | | |
| drop_eq | | |
| gq_master | | |
| wilds_wanderr | | |
| ship_quest_master | | |
| reset_once | | |
| see_all | | |
| no_chase | | |
| takes_skulls | | |
| pirate | | |
| player_hunter | | |
| invasion_leader | | |
| invasion_mob | | |
| see_wizi | | |
| soul_deposit | | |
| use_skills_only | | |
| can_level | | |
| no_xp | | |
| renewer | | |
| show_in_wilds | | |

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
| blind | | |
| invisible | | |
| detect_evil | | |
| detect_invis | | |
| detect_magic | | |
| detect_hidden | | |
| detect_good | | |
| sanctuary | | |
| faerie_fire | | |
| infrared | | |
| curse | | |
| death_grip | | |
| poison | | |
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
| silence | | |
| evasion | | |
| cloak_guile | | |
| warcry | | |
| light_shroud | | |
| healing_aura | | |
| energy_field | | |
| spell_shield | | |
| spell_deflection | | |
| neurotoxin | | |
| toxin | | |
| electrical_barrier | | |
| fire_barrier | | |
| improved_invis | | |
| ensnare | | |
| see_cloak | | |
| stone_skin | | |
| morphlock | | |
| deathsight | | |
| immobile | | |
| protected | | |

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
| breath_any | | |
| breath_acid | | |
| breath_fire | | |
| breath_frost | | |
| breath_gas | | |
| breath_lightning | | |
| cast_adept | | |
| cast_cleric | | |
| cast_judge | | |
| cast_mage | | |
| cast_undead | | |
| executioner | | |
| fido | | |
| guard | | |
| janitor | | |
| mayor | | |
| poison | | |
| thief | | |
| nasty | | |
| patrolman | | |
| questmaster | | |
| protector | | |
| dark_magic | | |
| fight_bot | | |
| pirate | | |
| pirate_hunter | | |
| invasion | | |
| invasion_leader | | |

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