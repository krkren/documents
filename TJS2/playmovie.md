---
layout: default
title: How to Play Videos
---

KiriKiriZ TJS2 sample script.
Plays a video in mixer (VMR9) mode.
This is the standard method for playing videos in the foreground.

{% highlight javascript %}
class MainWindow extends Window {
	var video;
	function MainWindow( width, height ) {
		super.Window();
		video = new VideoOverlay(this);
		video.mode = vomMixer;
		video.open("test.mpg");
		setInnerSize( video.originalWidth, video.originalHeight );
		video.play();
		video.width = video.originalWidth;
		video.height = video.originalHeight;
		video.visible = true;
		add( video );
	}
};
var win = new MainWindow(640,480);
win.visible = true;
{% endhighlight %}