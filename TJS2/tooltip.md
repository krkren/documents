---
layout: default
title: How to Display Tooltips
---

A tooltip is a simple help message displayed on mouseover.
Specifically, it is what is displayed via Layer.hint.
In KiriKiri, it is referred to as a "hint".
In KiriKiri 2, it was displayed using OS functions (strictly speaking, VCL), but in KiriKiri Z, the specification has changed to allow displaying an arbitrary layer.
Note that since mouseover cannot be triggered on tablet devices with touch panels, it is necessary to provide an alternative method for display.

## Specifications
* When a hint is set and the cursor stops for a certain period, the Window.onHintChanged(text:string,x:int,y:int,isshow:bool) event occurs at the hint display timing.
* The cursor stop time can be specified with property:Window.hintDelay.
Default is 500 (msec).
This was previously a fixed value but has been made configurable.
* Enable property:Layer.ignoreHintSensing on the layer used to draw the hint to make it ignore hint sensing.
* Hide the hint when the event is received with isshow set to false.

By drawing the hint on the frontmost layer when isshow is true in onHintChanged, and configuring the hint layer to ignore mouse messages, you can achieve a display similar to the legacy behavior.

Previously, only the standard plain display was available, but by allowing arbitrary drawing on a layer, it is now possible to display hints using images and other elements instead of just text.

## Sample
{% highlight javascript linenos %}
class MainWindow extends Window {
	var base;
	var layer;
	var hintlayer;

	function MainWindow( width, height ) {
		super.Window();
		setSize( width, height );
		setInnerSize( width, height );

		base = new Layer(this, null);
		base.setSize(width,height);
		base.setSizeToImageSize();
		base.name = "base";
		base.visible = true;

		layer = new Layer(this,base);
		layer.setPos(100,200);
		layer.setSize(100,100);
		layer.setSizeToImageSize();
		layer.colorRect(0,0,100,100,0xff0000,128);
		layer.drawText( 0, 40, "Draw Sample Text", 0xffffff );
		layer.hint = "Show Hint";
		layer.visible = true;

		// Hint Layer
		hintlayer = new Layer(this,base);
		hintlayer.visible = false;
		hintlayer.ignoreHintSensing = true;
		hintlayer.font.height = 14;
		hintlayer.hitThreshold = 256;

		// hintDelay = 500; // default
		// hintDelay = 0; // immediate
		// hintDelay = -1; // never
		// hintDelay = 1000; // slow
	}
	function onHintChanged(text,x,y,isshow) {
		if( isshow ) {
			var w = hintlayer.font.getTextWidth( text ) + 6;
			var h = hintlayer.font.getTextHeight( text ) + 4 + 4;
			hintlayer.setImageSize( w, h );
			hintlayer.setSizeToImageSize();

			if( (x+w) > innerWidth ) { x = innerWidth - w; }
			if( (y+h) > innerHeight ) { y = innerHeight - h; }
			hintlayer.setPos( x, y );

			hintlayer.fillRect( 0, 0, w, h, 0 );
			hintlayer.colorRect( 0, 0, w, h, clInfoBk, 196 );

			hintlayer.colorRect( 0, 0, 1, h, 0xFFFFFF );
			hintlayer.colorRect( 0, 0, w, 1, 0xFFFFFF );
			hintlayer.colorRect( w-1, 0, 1, h, 0xFFFFFF );
			hintlayer.colorRect( 0, h-1, w, 1, 0xFFFFFF );

			hintlayer.drawText( 2, 2, text, clInfoText, 220 );
			hintlayer.visible = true;
		} else {
			hintlayer.visible = false;
		}
	}
}
var win = new MainWindow(640,480);
win.visible = true;
{% endhighlight %}

[The same sample is also available on GitHub.
](https://github.com/krkrz/krkrz/blob/master/script/Sample/tooltip/startup.tjs)