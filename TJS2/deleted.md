# Features removed in Kirikiri Z
Features removed from the Kirikiri Z core can still be used by adding plugins.

## KagParser Class
Available by linking KAGParser.dll included in the Kirikiri Z repository.

## Menu Class
Available by linking menu.dll included in the Kirikiri Z repository.

## Pad Class
Available by loading Krkr2Compat included in the Kirikiri Z repository.

## Console Window (Debug.console)
Available by loading Krkr2Compat included in the Kirikiri Z repository.

## Script Editor
Available by loading Krkr2Compat included in the Kirikiri Z repository.

## Font Selection Dialog (Font.doUserSelect)
Available by loading Krkr2Compat included in the Kirikiri Z repository.

## Single-line Input Dialog (System.inputString)
Available by loading Krkr2Compat included in the Kirikiri Z repository.

## Layer.hint
Can be implemented in TJS. See [How to display tooltips](./tooltip.md) for details.

## Obsolete methods of the Layer class
The affineBlend/affinePile/blendRect/pileRect/stretchBlend/stretchPile methods can be implemented in TJS as follows.

```
Layer.affineBlend = function(src, sleft, stop, swidth, sheight, affine, A, B, C, D, E, F, opa=255, type=stNearest) {
    this.operateAffine(src, sleft, stop, swidth, sheight, affine, A, B, C, D, E, F, omOpaque, opa, type);
};
Layer.affinePile = function(src, sleft, stop, swidth, sheight, affine, A, B, C, D, E, F, opa=255, type=stNearest) {
    this.operateAffine(src, sleft, stop, swidth, sheight, affine, A, B, C, D, E, F, omAuto, opa, type);
};
Layer.blendRect = function(dleft, dtop, src, sleft, stop, swidth, sheight, opa=255) {
    this.operateRect(dleft, dtop, src, sleft, stop, swidth, sheight, omOpaque, opa);
};
Layer.pileRect = function(dleft, dtop, src, sleft, stop, swidth, sheight, opa=255) {
    this.operateRect(dleft, dtop, src, sleft, stop, swidth, sheight, omAuto, opa);
};
Layer.stretchBlend = function(dleft, dtop, dwidth, dheight, src, sleft, stop, swidth, sheight, opa=255, type=stNearest) {
    this.operateStretch(dleft, dtop, dwidth, dheight, src, sleft, stop, swidth, sheight, omOpaque, opa, type);
};
Layer.stretchPile = function(dleft, dtop, dwidth, dheight, src, sleft, stop, swidth, sheight, opa=255, type=stNearest) {
    this.operateStretch(dleft, dtop, dwidth, dheight, src, sleft, stop, swidth, sheight, omAuto, opa, type);
};
```