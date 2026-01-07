---
layout: default
title: Features where KiriKiri Z is advanced compared to KiriKiri 2
---

KiriKiri Z has continued to evolve compared to its initial release version, offering several advantages over KiriKiri 2.

## Advantages of KiriKiri Z

### MPEG4 AVC (H.264) Video Playback
KiriKiri Z enables H.264 playback.
Due to licensing issues, a license fee is required if the total video length exceeds 12 minutes, but it allows you to incorporate high-quality, high-compression ratio videos.

### 64-bit Support
KiriKiri Z also has a 64-bit version, which is effective when using large amounts of memory.
In Full HD games using 32-bit, the application may stop due to insufficient memory after playing for a while, but with 64-bit, you can develop without worrying about such issues.

### Improved Memory Utilization Efficiency
In KiriKiri 2, errors such as "Cannot allocate memory for bitmap/..." could occur even at HD sizes after playing for a while. KiriKiri Z has been enhanced to prevent memory fragmentation as much as possible, making this error less likely to occur.
Furthermore, if the situation does not improve, you can also use the 64-bit version.

### Higher Quality Scaling Functions
KiriKiri 2 had partial support up to Bicubic, but KiriKiri Z additionally supports Lanczos2, Lanczos3, Spline16, Spline36, Area Average (downscaling only), Gaussian, and BlackmanSinc.
Also, Bicubic has been significantly accelerated compared to KiriKiri 2. In KiriKiri 2, it was difficult to use scaling higher than fast Bilinear due to speed constraints, but in KiriKiri Z, the scaling process has been completely rewritten and optimized to operate at practical speeds.
For image quality, please refer to [Image Scaling Filters](https://krkrz.github.io/documents/TJS2/imagescaling.html).

### SSE2/AVX2 Support
In KiriKiri 2, image processing was supported up to MMX, but KiriKiri Z supports SSE2 and partially AVX2, allowing for faster operation on modern CPUs.

### Optimization Benefits from Compiler Evolution
While KiriKiri 2 was compiled with a compiler from 18 years ago, KiriKiri Z is sequentially compiled with newer compilers, resulting in generally faster operation.
In particular, some functions such as JPEG loading have become dramatically faster.

### Opus Audio Support
KiriKiri Z supports Opus audio via the kropus.dll plugin.
Compared to Ogg Vorbis, Opus offers advantages such as higher sound quality, higher compression, and lower latency, contributing to the compression of audio data that occupies a large capacity.
Strictly speaking, kropus.dll likely works on KiriKiri 2 as well, but it is not officially supported.

### Text Rendering via FreeType
It is now possible to select text rendering using FreeType, allowing for beautiful character drawing.
(In some cases, the difference may not be noticeable.)

### Image File Saving Functionality
In KiriKiri 2, images could not be saved in formats other than Bitmap, but in KiriKiri Z, Layer images can be saved in readable image file formats such as PNG/JPEG/TLG.

### Asynchronous Image Loading
KiriKiri Z allows image data to be loaded in the background. By pre-loading image data, you can create a system that allows for more stress-free play than before.

### High-DPI Display Support
KiriKiri 2 does not support high-DPI displays, and when launched on a high-DPI display, it may start at a larger size than intended. KiriKiri Z always launches at the specified size.

### Touch Panel / Multi-touch Support
KiriKiri Z supports touch panels and can process multi-touch input.
By creating a system compatible with touch panels, you can create an environment where players can play without stress on tablets and similar devices.

### Bug Fixes
Development of KiriKiri 2 has stopped, and several functions malfunction when used on new operating systems.
Additionally, issues such as errors occurring when XP3 files exceed 2GB remain.
KiriKiri Z is under continuous development, and fixes are constantly being added to ensure it runs without problems on new OS versions.
Furthermore, bugs that existed in KiriKiri 2 are also being continuously fixed.

### Multi-language Support
KiriKiri Z is multi-language compatible and operates in non-Japanese environments.
Additionally, error messages and other texts support Japanese, English, and Chinese.

### Theora Video Playback
Ogg Theora videos can also be played.
Since WMV is available, there are not many occasions for it to be used, but it can be used if desired.

### Other Useful Features
When writing TJS2 scripts directly, various convenient features have been added in detail, such as 9patch functionality, Array/Dictionary pack/unpack, avoiding script execution in save data via the Array/Dictionary loadStruct method, flexible script writing using ImageFunction and the Bitmap class, addition of the Rect class, support for mouse X1/X2 (Forward/Back) buttons, and UTF-8 support.