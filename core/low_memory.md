# Countermeasures for Insufficient Memory

Errors such as "Cannot allocate memory for Bitmap : ..." may occur.
This seems to happen more frequently due to larger screen resolutions.
While it is less likely to occur in KiriKiri Z compared to KiriKiri 2, it is not impossible.
However, the occurrence can be reduced through settings.

## Cause
In games, this error is more often caused by memory fragmentation rather than an actual lack of memory.
Fragmentation occurs easily when memory allocations for large areas (like images) are mixed with allocations for small classes or strings.
As fragmentation progresses, large memory blocks cannot be allocated, leading to failures when trying to allocate large areas like bitmap memory.

## Countermeasures
Some memory management algorithms are less prone to fragmentation. Windows Vista and later use algorithms that reduce fragmentation.
On XP, enabling LFH (Low Fragmentation Heap) had a similar effect (KiriKiri Z is for Vista and later, so it is always enabled).
Even with this enabled, errors can still occur, so other measures include performing compaction or separating the memory region for images.

### Compaction
Compaction involves connecting adjacent free memory areas into a single large continuous free area.
Since this can be resource-intensive when many allocations/deallocations have occurred, it should be executed occasionally.
In KiriKiri Z, adding the -ghcompact option (specifiable in engine settings) enables global heap compaction whenever System.doCompact is called.
This process adds some load, so it is disabled by default.
If it runs smoothly on your target minimum CPU, it is fine to release with it enabled by default.

### Split Heap
This prevents fragmentation by separating bitmap memory (which allocates relatively large areas) from other allocations.
This can be specified in the engine settings.
By default, it allocates from the process heap, which is fine in most cases.
If errors occur during testing, try this; if it helps, it may be better to set the split heap as the default.

### Calling GC
Calling System.doCompact during scene transitions (such as during a fade to black) in the script is thought to be somewhat effective.

### Not Using Graphic Cache
Not using the graphic cache, or keeping its capacity as small as possible, is effective in preventing memory fragmentation.
If the above measures do not work, it is worth considering disabling the graphic cache.
Without the cache, the game may feel noticeably heavier, especially when skipping, but on modern environments with current CPUs and SSDs, it is not a major concern, so disabling the cache should be fine.

### Using the 64-bit Version
If all the above measures are ineffective, you should consider the ultimate solution: using the 64-bit version.
This allows full utilization of installed memory and resolves complaints about errors occurring despite having plenty of RAM.
Since 64-bit environments are becoming the standard, it is advisable to provide a 64-bit version regardless of memory issues.