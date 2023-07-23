---
layout: default
title: OEdit Flags Reference
nav_order: 3
parent: Object Editor
---

# Introduction
---
A list of flags and flag-like settings for objects

### Item Types
```
light              scroll             wand               staff              
weapon             treasure           armour             potion             
furniture          trash              container          drinkcontainer     
key                food               money              boat               
npccorpse          fountain           pill               protect            
map                portal             catalyst           roomkey            
gem                jewelry            jukebox            artifact           
shares             flame_room_object  instrument         seed               
cart               ship               room_darkness_obje ranged_weapon      
sextant            weapon_container   room_roomshield_ob book               
stinking_cloud     smoke_bomb         herb               spell_trap         
withering_cloud    bank               keyring            ice_storm          
flower             trade_type         empty_vial         blank_scroll       
mist               shrine             whistle            shovel             
tattoo             ink                part               telescope          
compass            whetstone          chisel             pick               
tinderbox          drying_cloth       needle             body_part          
```

### Wear Flags
```
take               finger             neck               body               
head               legs               feet               hands              
arms               shield             about              waist              
wrist              wield              hold               nosac              
wearfloat          ring_finger        back               shoulder           
face               eyes               ear                ankle              
conceals           tabard             
```

### Extra Flags
```
glow               hum                no_restring        nokeyring          
evil               invis              magic              nodrop             
bless              antigood           antievil           antineutral        
noremove           nopurge            rotdeath           nosteal            
nolocate           meltdrop           burnproof          freezeproof        
nouncurse          
```

### Extra2 Flags
```
all_remort         locker             true_sight         scare              
sustains           emits_light        float_user         see_hidden         
super-strong       remort_only        no_lore            sell_once          
no_hunt            no_discharge       no_donate          kept               
singular           no_loot            no_enchant         no_container       
no_locker          no_auction         keep_value         
```

### Extra3 Flags
```
exclude_list       no_transfer        always_loot        can_dispel         
keep_equipped      rift_update        show_in_wilds      
```

### Extra4 Flags
```
None at the moment
```

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

### Fragility
```
Solid|Strong|Normal|Weak
```

## Item type specific flags
### Portal exit flags
```
door               closed             nopass             noclose            
nolock             found              broken             nobash             
walkthrough        nobar              aerial             nohunt             
environment        prevfloor          nextfloor          nosearch           
mustsee            
```

### Lock flags
```
locked             magic              snap_key           script             
noremove           broken             jammed             nojam  
```

### Object spells
```
acid blast         acid breath        afterburn          animate dead       
armour             avatar shield      bless              blindness          
burning hands      call familiar      call lightning     calm               
cancellation       cause critical     cause light        cause serious      
chain lightning    channel            charm person       chill touch        
cloak of guile     colour spray       continual light    control weather    
cosmic blast       counterspell       create food        create rose        
create spring      create water       cure blindness     cure critical      
cure disease       cure light         cure poison        cure serious       
cure toxic         curse              dark shroud        death grip         
deathbarbs         deathsight         demonfire          destruction        
detect hidden      detect invis       detect magic       discharge          
dispel evil        dispel good        dispel magic       dispel room        
eagle eye          earth walk         earthquake         electrical barrier 
enchant armour     enchant weapon     energy drain       energy field       
ensnare            entrap             exorcism           faerie fire        
faerie fog         fatigue            fire barrier       fire breath        
fire cloud         fireball           fireproof          flamestrike        
flash              fly                frenzy             frost barrier      
frost breath       gas breath         gate               giant strength     
glacial wave       glorious bolt      harm               haste              
heal               healing aura       holy shield        holy sword         
holy word          ice shards         ice storm          identify           
improved invisibil inferno            infravision        invisibility       
kill               light shroud       lightning bolt     lightning breath   
locate object      magic missile      mass healing       mass invis         
master weather     maze               momentary darkness morphlock          
neurotoxin         nexus              pass door          plague             
poison             raise dead         recharge           refresh            
regeneration       remove curse       room shield        sanctuary          
shield             shocking grasp     shriek             silence            
sleep              slow               soul essence       spell deflection   
spell shield       spell trap         starflare          stone skin         
stone spikes       stone touch        summon             third eye          
toxic fumes        underwater breathi vision             weaken             
web                wind of confusion  withering cloud    word of recall  
```

### Container Flags
```
closeable          closed             puton              pushopen           
closelock          
```

### Armour Class Types
```
pierce             bash               slash              exotic  
```

### Armour Strength Flags

| Strength | Calculation |
|:---------|:------------|
| Light | level/10 |
| Medium | level/5 |
| Strong | level * 3/10 |
| Heavy | level * 2/5 |
| None | 0 |

### Apply Flags (stat modifiers)
```
none               strength           dexterity          intelligence       
wisdom             constitution       sex                mana               
hp                 move               gold               ac                 
hitroll            damroll            
```

### Weapon classes
```
exotic             sword              dagger             spear              
mace               axe                flail              whip               
polearm            stake              quarterstaff       arrow              
bolt               dart               harpoon            
```

### Damage types
| Name | Attack String | Damage Type |
|:-----|:--------------|:------------|
| none | hit | untyped |
| slice | slice | slash |
| stab | stab | pierce |
| slash | slash | slash |
| whip | whip | slash |
| claw | claw | slash |
| blast | blast | bash |
| pound | pound | bash |
| crush | crush | bash |
| grep | grep | slash |
| bite | bite | pierce |
| pierce | pierce | pierce |
| suction | suction | bash |
| beating | beating | bash |
| digestion | digestion | acid |
| charge | charge | bash |
| slap | slap | bash |
| punch | punch | bash |
| wrath | wrath | energy |
| magic | magic | magic |
| divine | divine power | holy |
| holy | holy fire | holy |
| cleave | cleave | slash |
| scratch | scratch | pierce |
| peck | peck | pierce |
| peckb | peck | bash |
| chop | chop | slash |
| sting | sting | pierce |
| smash | smash | bash |
| shbite | shocking bite | lightning |
| flbite | flaming bite | fire |
| frbite | freezing bite | cold |
| acbite | acidic bite | acid |
| chomp | chomp | pierce |
| drain | life drain | negative |
| decay | decaying touch | negative |
| thrust | thrust | pierce |
| slime | slime | acid |
| shock | shock | lightning |
| thwack | thwack | bash |
| flame | flame | fire |
| chill | chill | cold |
| vorpal | slash | vorpal |
| purify | purifying light | holy |
| crblow | crippling blow | negative |
| acrid | acrid spray | acid |
| blight | blighting touch | disease |
| boiling | boiling spray | water |
| corrode | corrosion | acid |
| discharge | discharge | lightning |
| flog | flog | slash |
| fumes | caustic fumes | acid |
| lacerate | laceration | slash |
| lash | lash | slash |
| lbolt | lightning bolt | lightning |
| pain | pain | mental |
| pestilence | pestilence | disease |
| scorch | scorching | fire |
| scourge | scourge | slash |
| scream | scream | sound |
| shining | shining light | light |
| shriek | deafening shriek | sound |
| spike | spike | pierce |
| spores | spores | disease |
| surge | surge | lightning |
| torrent | watery torrent | water |
| toxblast | toxic blast | poison |
| venom | venom | poison |
| winbrth | wintery breath | cold |

### Special weapon flags
```
acidic             annealed           blaze              dull               
offhand            onehand            poison             resonate           
shocking           suckle             throwable          flaming            
frost              sharp              twohands           vampiric           
vorpal             
```

### Ranged weapon types
```
exotic             crossbow           bow                blowgun            
harpoon            
```

### Catalyst Types
```
none               acid               air                astral             
blood              body               chaos              cosmic             
darkness           death              earth              energy             
fire               holy               ice                law                
light              mana               metallic           mind               
nature             shock              soul               sound              
toxin              water              
```

### Instrument types
```
any                reed               flute              brass              
drum               percussion         chorded            string    
```