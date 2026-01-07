# How to distinguish between Kirikiri 2 and Kirikiri Z
## Static Determination
In the TJS2 preprocessor, kirikiriz is set to 1, so static switching can be done using this.
However, in the case of compiled bytecode binaries, switching is not possible because the bytecode is generated according to the preprocessor at the time of compilation.
There is no problem if the scripts are stored as text.

## Dynamic Determination
The System.versionInformation property is "Kirikiri [kirikiri] 2 Execution Core..." in Kirikiri 2, but "Kirikiri [kirikiri] Z Execution Core..." in Kirikiri Z, making it possible to distinguish them.
Also, as a change in the version string, System.versionString returns 1.0.0.001.
Since the version has been reset with Kirikiri Z, caution is required if you are expecting 2.X.X.XXX etc.