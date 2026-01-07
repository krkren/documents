---
layout: default
title: Anti-cracking measures specific to Kirikiri Z
---

Due to its high extensibility, Kirikiri 2/Z has multiple points that can serve as entry points for cracking attempts.
This section describes measures specific to Kirikiri.

## Measures against executable replacement
When performing authentication, if the executable can be replaced with one that has no checks, it is equivalent to having no protection at all.
It is necessary to prevent the game from running if replaced with a publicly available executable.
Be careful about operation when replacing with executables released via patches or trial versions.
Ensure that target game data, DLLs, etc., do not function unless used with a specific executable.
By embedding the decryption process for encrypted xp3 files, it becomes impossible to start with other executables.
However, since this becomes meaningless if a decrypted archive is created, it is better to add other unique features as well.
Continuous development is required to ensure that previously cracked executables cannot be used.

## Prevention of auto-loading plugins
There is a feature that automatically loads DLLs with the .tpm extension, but since auto-loading allows for replacing built-in APIs, it must be disabled.
However, implementing this measure will make it impossible to use encryption that relies on built-in plugin features.

## Prevention of auto-loading scripts
Since scripts loaded at startup, such as startup.tjs in data.xp3, are fixed, replacing them allows for replacing built-in APIs, so this must be prevented.
By fixing the location of the initial script, performing script tampering detection, or encrypting the xp3 and decrypting it upon every load, insertion of scripts other than specific ones can be avoided to some extent.
By compiling with an arbitrary string set in TVP_START_UP_SCRIPT_NAME, an arbitrary script can be executed at startup, preventing the execution of arbitrary scripts via the -startup option.
It is also possible to specify settings such as forcing loading from data.xp3 using TVPSystemSecurityOptions.

## Measures against plugin replacement
By renaming the original plugin and placing a plugin with the same name, arbitrary plugins can be loaded, allowing for hijacking of APIs exposed by that plugin or replacement of built-in APIs.
Replacing DLLs that perform file loading, such as krmovie.dll, makes it easy to obtain the data passed to that DLL (in version 1.3 and later, krmovie.dll is built into the main body, so this issue no longer exists for krmovie.dll).
In the case of krmovie.dll, it is video files; for KAGParser.dll series, it is KAG scripts.
To avoid this, it is necessary to perform tampering detection on DLLs.
This includes checking the hash of the DLL to be loaded or verifying digital signatures.
However, since time-consuming checks affect execution, it must be something that can perform high-speed checks.
If plugins are built into the main executable and the use of external plugins is disabled, these concerns are eliminated.
Functionality can be added via the [Extension](http://www.kaede-software.com/2014/01/extension.html) feature, but currently, this cannot be done without modifying the plugin source code.
The goal is to make it possible to build static libraries by changing the build method without modifying the source code, and then link them to the main body for integration.

## Prevention of Controller, Script Editor, Watch Expressions, and Console via hotkeys
In Kirikiri Z, the "Controller, Script Editor, and Watch Expressions" have been removed, and the console has become a command line.
[Deleted Features](../TJS2/deleted.html)
Disable these features.
In Kirikiri Z, console output can be obtained via plugins, so that should also be suppressed.
Disabling console output itself is expected to slightly improve speed.
Building without TVP_LOG_TO_COMMANDLINE_CONSOLE prevents logs from being output to the command line (public binaries have this ON and will output logs).
If disabling console output, note that some plugins output OSS licenses to the log; when using such plugins, it is necessary to attach a separate text file containing the license description.

## Bytecode conversion
Since TJS2 scripts are easy to analyze if stored as-is, the scripts should be converted to bytecode.
Bytecode conversion is possible via Scripts.compileStorage.
Even if the archive is encrypted, it can still be breached, so it is meaningful to make script analysis difficult.
Also, bytecode can be loaded faster than scripts (since script compilation is performed in advance).

## Bytecode obfuscation
Obfuscate the bytecode to make analysis difficult.
Obfuscation primarily makes it difficult to infer processing by replacing human-readable strings.
It is even more powerful if built-in functions can be obfuscated at the same time.
Currently, no tools for TJS2 bytecode obfuscation are provided.

## Archive encryption
xp3 archives can be encrypted by passing an encryption DLL to the releaser.
Decryption is performed by loading a tpm plugin.