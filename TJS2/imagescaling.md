---
layout: default
title: Image Scaling Filters
---

In Kirikiri Z, various filters have been added for image scaling in addition to the nearest neighbor, bilinear, and bicubic methods used in version 2.

## Scaling Filters
* Nearest Neighbor
* Bilinear
* Bicubic
* Lanczos2
* Lanczos3
* Spline16
* Spline36
* Area Average (Downscaling only)
* Gaussian
* BlackmanSinc

In Kirikiri 2, bicubic was quite slow and difficult to use, but in the current Kirikiri Z, it has been optimized and is fast enough to be used with any filter.

* Reference Image: Image from another site
![Reference Image](http://kaede-software.com/krkrz/resample_20140404.png "Execution Result Sample")