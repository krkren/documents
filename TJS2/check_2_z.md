---
layout: default
title: How to distinguish between KiriKiri 2 and KiriKiri Z
---

## Static Determination
In the TJS2 preprocessor, kirikiriz is set to 1, so static switching can be handled this way.
However, in the case of bytecode binaries, the bytecode is generated according to the preprocessor at the time of compilation, so it cannot be switched.
There is no problem if the script is stored as text.

## Dynamic Determination
The System.versionInformation property is "KiriKiri [kirikiri] 2 Execution Core..." in KiriKiri 2, but "KiriKiri [kirikiri] Z Execution Core..." in KiriKiri Z, making it possible to distinguish them.
Additionally, as a change in the version string, System.versionString returns 1.0.0.001.
Since the version has been reset with KiriKiri Z, caution is needed if you are expecting 2.X.X.XXX or similar.