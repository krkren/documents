---
layout: default
title: Countermeasures for Out of Memory
---

Errors such as "Cannot allocate memory for Bitmap : ..." may occur.
This seems to happen more frequently due to larger screen resolutions and similar factors.
While it is less likely to occur in Kirikiri Z compared to Kirikiri 2, it is not impossible.
However, occurrences can be reduced through settings.

## Cause
In games, this error is more often caused by memory fragmentation rather than an actual lack of physical memory.
Fragmentation is likely to occur when memory allocations requiring large areas (such as images) are mixed with allocations for small classes or strings.
As fragmentation progresses, large memory blocks can no longer be allocated, leading to failures when trying to secure large areas like bitmap memory.

## Countermeasures
Some memory management algorithms are less prone to fragmentation. Windows Vista and later use algorithms that reduce fragmentation.
On XP, enabling [LFH](https://msdn.microsoft.com/ja-jp/library/windows/desktop/aa366750%28v=vs.85%29.aspx) has a similar effect (Kirikiri Z is for Vista and later, so it is always enabled).
However, since errors can still occur even with this feature enabled, other measures include performing compaction or separating the memory region for images.

### Compaction
Compaction involves joining adjacent free memory areas into a single large continuous free area.
Since the load can be somewhat high in situations where memory has been allocated and released many times, it should be executed occasionally.
In Kirikiri Z, by adding the -ghcompact option (specifiable in the engine settings), global heap compaction is performed when System.doCompact is called.
Because this process imposes some load, it is disabled by default.
If it runs without stress on the minimum target CPU, it is fine to release the game with it enabled by default.

### Split Heap
This prevents fragmentation by separating bitmap memory (which allocates relatively large areas) from other allocations.
This can be specified in the engine settings.
By default, it allocates from the process heap, which is fine in most cases.
If errors occur during testing, try this; if it reduces the issue, it is better to set the split heap as the default.

### GC Calls
In scripts, calling System.doCompact during scene transitions (such as during a fade to black) is thought to be somewhat effective.

### Disabling Graphic Cache
Not using the graphic cache, or reducing its capacity as much as possible, is effective in preventing memory fragmentation.
If the above measures are not effective, it is worth considering not using the graphic cache.
Without the graphic cache, the game may feel noticeably heavy, especially when skipping, but on modern environments with current CPUs and SSDs, it is not that noticeable, so it should be fine to disable the cache.

### Using the 64-bit Version
If all the above measures are ineffective, you should consider the ultimate countermeasure: using the 64-bit version.
This allows full utilization of the installed memory and resolves complaints about errors occurring despite having plenty of RAM.
As 64-bit environments are becoming the standard, it is likely better to provide a 64-bit version regardless of memory issues.