---
layout: default
title: Engine Configuration Tool
---

## In KiriKiri Z, the independent engine configuration tool has been removed.  
Starting the executable with the -userconf argument will launch it as the user-facing engine configuration tool.  
By creating a shortcut with the -userconf argument in an installer or similar, it can be handled the same way as before.

## A developer-specific engine configuration tool is not provided.
Items you do not want to display in the user settings can be hidden by setting "user":false in option_desc_ja.json or by deleting the item itself.  
If settings do not display correctly, there may be a JSON syntax error in the log. It is recommended to launch via console and check the log.  
To apply these changes, you must either rebuild the core or use a tool capable of modifying resources.  

See also [Adding/Editing Engine Settings](../core/engine_setting.html).