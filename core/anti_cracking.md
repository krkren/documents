# KiriKiri Z Specific Anti-Cracking Measures
KiriKiri 2/Z has multiple points that can serve as entry points for cracking due to its high extensibility.
This document describes measures specific to KiriKiri.

## Executable Replacement Measures
When performing authentication, if the executable can be replaced with one that has no checks, it is equivalent to having no protection at all.
It is necessary to prevent the game from running if replaced with other available executables.
Be careful about replacement with executables released via patches or trial versions.
Ensure that game data, DLLs, etc., only work with a specific executable.
By embedding the decryption process for encrypted xp3 files, it becomes impossible to start with other executables.
However, since this becomes meaningless if a decrypted archive is created, it is better to add other unique features.
Continuous development is required to prevent the use of previously cracked executables.

## Prevention of Auto-loading Plugins
There is a feature to automatically load DLLs with the .tpm extension, but since auto-loading allows for the replacement of built-in APIs, it must be disabled.
However, implementing this measure will make it impossible to use encryption that relies on built-in plugin features.

## Prevention of Auto-loading Scripts
Since scripts loaded at startup, such as startup.tjs in data.xp3, are fixed, replacing them allows for the replacement of built-in APIs, so this must be prevented.
By fixing the location of the initial script, performing script tampering detection, or encrypting the xp3 and decrypting it upon every load, insertion of scripts other than the specific ones can be avoided to some extent.
By setting an arbitrary string to TVP_START_UP_SCRIPT_NAME and compiling, you can execute a specific script at startup and prevent the execution of arbitrary scripts via the -startup option.
It is also possible to specify settings such as forcing loading from data.xp3 using TVPSystemSecurityOptions.

## Plugin Replacement Measures
By renaming the original plugin and placing a plugin with the same name, arbitrary plugins can be loaded, allowing for the hijacking of APIs exposed by that plugin or the replacement of built-in APIs.
Replacing DLLs that perform file loading, such as krmovie.dll, allows for easy acquisition of data passed to that DLL (from version 1.3 onwards, krmovie.dll is integrated into the core, so this issue no longer applies to krmovie.dll).
In the case of krmovie.dll, it is video files; for KAGParser.dll series, it is KAG scripts.
To avoid this, it is necessary to perform tampering detection on DLLs.
Such as checking the hash of the DLL to be loaded or verifying digital signatures.
However, since time-consuming checks affect execution, it must be something that can perform high-speed checks.
If plugins are integrated into the core and the use of external plugins is disabled, these concerns are eliminated.
While features can be added via the [Extension](http://www.kaede-software.com/2014/01/extension.html) functionality, it currently cannot be done without modifying the plugin's source code.
The goal is to create a static library by changing the build method without modifying the source code, and then link it to the core to make it embeddable.

## Prevention of Controller, Script Editor, Watch Expressions, and Console via Hotkeys
In KiriKiri Z, the "Controller, Script Editor, and Watch Expressions" have been removed, and the console has become a command line.
[Deleted Features](../TJS2/deleted.md)
Disable these features.
In KiriKiri Z, console output can be obtained via plugins, so that should also be suppressed.
Disabling console output itself is expected to slightly improve performance.
Building without TVP_LOG_TO_COMMANDLINE_CONSOLE prevents logs from being output to the command line (public binaries have this ON and output logs).
If you disable console output, note that some plugins output OSS licenses to the log; if using such plugins, you must separately attach a text file containing the license.

## Bytecode Conversion
Since TJS2 scripts are easy to analyze if stored as-is, convert the scripts to bytecode.
Bytecode conversion is possible via Scripts.compileStorage.
Even if the archive is encrypted, it can be bypassed, so there is value in making script analysis difficult.
Also, bytecode can be loaded faster than scripts (since script compilation is done in advance).

## Bytecode Obfuscation
Obfuscate the bytecode to make analysis difficult.
Obfuscation primarily makes it difficult to infer processing by replacing readable strings.
It is even more powerful if built-in functions can be obfuscated at the same time.
Currently, no obfuscation tools for TJS2 bytecode are provided.

## Archive Encryption
xp3 archives can be encrypted by passing an encryption DLL to the releaser.
Decryption is performed by loading a tpm plugin.