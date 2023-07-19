---
layout: default
title: TEdit Command Reference
nav_order: 2
parent: Token Editor
---

# Token editor commands
---

```
commands       ?              comments       create         show           
name           type           flags          timer          ed             
desc           value          valuename      addtprog       deltprog       
varset         varclear       
```

### Commands
Lists the commands available in the token editor

### ?
This allows you to call the 'list of flags' help, as in any editor. For example, `? token` brings up the list of token flags.

### Comments
Allows you to set a "Builder's Comments" field. Use of this is encouraged to explain any particulars of the token, or any troubles you're having with it so other imms can help out, or expand on it.

### create <vnum>
Creates a new token with the specified vnum.

### show
Also called by hitting `<enter>` while in the token editor. Shows the token you're working on.

### name
Sets the token's name. Only visible to staff.

### type
Sets the token type. See the [token flags reference](tedit-flags-reference) for more details.

### flags
Sets various flags on the token. See the [token flags reference](tedit-flags-reference) for more details.

### timer
Sets how long the token will last before extracting itself. Can be affected by the `reverse_timer` token flag.

### ed
**Not Yet Implemented**

### desc
Allows you to set a description for the token. This is only visible to staff.

### value <0-7> <value>
Sets one of the 7 numerical values on the token.

### valuename <0-7> `<string>`
Sets the name of the given value. Some of these are set automatically depending on token type.

### addtprog `<vnum> <trigger> <phrase>`
Attaches a token prog to the token with the given trigger and phrase.

*Example*: `addtprog 11001 speech hello` - This would attached token prog 11001 to this token, and make it fire whenever anyone says 'hello'.

### deltprog <tprog number>
Removes the given tprog from the list assigned to this token.

### varset
varset <name> <number|string|room> <yes|no> <value>
### varclear
varclear <name>