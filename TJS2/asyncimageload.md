# Asynchronous Image Loading
Standard image loading waits for the completion of the load when requested, but asynchronous loading allows other processes to run while the image is being loaded in the background.  
This makes it possible to design the application so that slow processing, such as reading from a disk, is less noticeable.  

## Specifications
* Asynchronous loading is performed using Bitmap.loadAsync(filename) and Bitmap.onLoaded(meta,async,error,message).
* Request loading with loadAsync and receive completion with onLoaded.
* You can determine if an asynchronous load is in progress using Bitmap.loading.
* Accessing other members of the Bitmap during asynchronous loading will cause an exception.
* Note that the filename argument for loadAsync must include the extension, which is different from Layer.loadImages where it can be omitted.

### The values received in onLoaded (meta,async,error,message) are as follows:
* meta : Same as what is returned by Layer.loadImages. A dictionary array of tag information.
* async : Whether it was loaded asynchronously. If it was in the cache, it returns via onLoaded immediately after the load request.
* error : Whether a loading error occurred.
* message : Error message. If an error occurs, the error message is passed.

Loaded images can be copied from the Bitmap to the Layer using Layer.copyFromBitmapToMainImage(Bitmap).  
Even though it is called a "copy," it is shared until modified, so it finishes instantly.  
Since the loading process is asynchronous, there is a possibility that the Layer to which the image is being passed has already been invalidated by the time the load is complete.  
When accessing other objects in onLoaded, it is recommended to check if they have been invalidated.  
Alternatively, you must ensure they are not invalidated until onLoaded is complete.  

## Sample Script
```
/*
Load image0.png - image9.png asynchronously 10 times
*/
System.setArgument("-contfreq", 60);
System.graphicCacheLimit = 0;
// Disable image cache because it is meaningless if the cache is enabled

class Bitmap2Layer extends Bitmap {
	var target;
	var tag;
	function Bitmap2Layer(layer,filename) {
		super.Bitmap();
		this.target = layer;
		this.tag = filename;
	}
	function onLoaded(meta,async,error,message) {
		Debug.message("Exit load async:"+async+", error:"+error+", message:"+message);
		if( !error && isvalid(target) ) { // Since it is asynchronous, check that it has not already been invalidated
			target.copyFromBitmapToMainImage(this);
			target.setSizeToImageSize();
		}
	}
};

class MainWindow extends Window {
	var base;
	var layer;
	var layermove;
	var addval;
	var bmps;

	function MainWindow( width, height ) {
		super.Window();
		setSize( width, height );
		setInnerSize( width, height );

		base = new Layer(this, null);
		base.setSize(width,height);
		base.setSizeToImageSize();
		base.name = "base";
		base.visible = true;
		add( base );

		layer = new Layer(this,base);
		layer.setSize(100,100);
		layer.setSizeToImageSize();
		layer.colorRect(0,0,100,100,0x00ff00,128);
		layer.visible = true;
		add( layer );

		layermove = new Layer(this,base);
		layermove.setSize(100,100);
		layermove.setPos(0,height-100);
		layermove.setSizeToImageSize();
		layermove.colorRect(0,0,100,100,0xff0000,128);
		layermove.drawText( 0, 40, "Test string rendering", 0xffffff );
		layermove.visible = true;
		add( layermove );

		bmps = new Array();
		for( var j = 0; j < 10; j++ ) {
		for( var i = 0; i < 10; i++ ) {
			var filename = "image"+i+".png";
			var bmp = new Bitmap2Layer(layer,filename);
			bmp.loadAsync(filename);
			bmps.add(bmp);
		}
		}
		add( bmps );

		addval = 1;
		System.addContinuousHandler(onMoveImage);
	}
	function finalize() {
		System.removeContinuousHandler(onMoveImage);
		super.finalize();
	}
	function onMoveImage() {
		layermove.left += addval;
		if( addval > 0 ) {
			if( layermove.left >= (this.width-layermove.width) ) {
				addval = -1;
			}
		} else {
			if( layermove.left <= 0 ) {
				addval = 1;
			}
		}
	}
};

var win = new MainWindow(640,480);
win.visible = true;
```

[Same as the one in GitHub](https://github.com/krkrz/krkrz/blob/master/script/Sample/asyncimageload/startup.tjs)