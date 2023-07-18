---
layout: default
title: TEdit Flags Reference
nav_order: 4
parent: Token Editor
---

# Introduction

### Token Types
```
type [general|quest|affect|skill|spell]
```

#### General
Standard token. Nothing special about it. Can be set multiple times on a given owner.

#### Quest
Standard token. May be used with upcoming 'quest log' command. Can be set multiple times on a given owner.

#### Affect
Not currently used in any special way. May be deprecated soon? Can be set multiple times on a given owner.

#### Skill
Used for scripted skills. Shows up in player skill list. Can only exist once on a given owner. Has preset value names. 

#### Spell
Used for scripted spells. Shows up in player spell list. Can only exist once on a given owner. Has preset value names.

### Token Flags
```
purge_death        purge_idle         purge_quit         purge_reboot       
purge_rift         reverse_timer      no_skill_test      singular           
see_all            permanent          spellbeats         
```

| flag | effect |
|:-----|:-------|
| purge_death | Token is extracted when character dies. |
| purge_idle | Token is extracted if character is sent to limbo due to idling. |
| purge_quit | Token is extracted if character quits. |
| purge_reboot | Token is extracted on reboot. |
| purge_rift | Token is extracted if character rifts. |
| reverse_timer | Token's timer goes up instead of down. Useful for tracking how long a task takes. |
| no_skill_test | Skips skill tests when actions are completed. |
| singular | Can only exist once on a given owner. |
| see_all | Token skips any sight rules for its scripts. |
| permanent | Cannot be removed unless owner is extracted. On players, generally requires pfile edits to remove. |
| spellbeats | Allows token to fire TRIG_SPELLBEATS triggers while casting. |