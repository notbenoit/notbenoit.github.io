---
layout: post
title:  "Twist and Shim !"
date:   2015-02-01 00:15:00
categories: Development
tags: swift shimming shim
author: "Benoît Layer"
---

Ever heard of Shimming ?

According to [Wikipedia][shim_wikipedia], a shim is :

{% highlight text %}
[...] a small library that transparently intercepts API calls and changes the arguments passed, handles the operation itself, or redirects the operation elsewhere [...]
{% endhighlight %}


At the WWDC 2014 ([233][233]), Elizabeth Reid showed what they called in the iWork team **Shimming**. It is simply conditional compilation in order to use the same code on both iOS and OSX.

{% highlight objc%}
#if TARGET_OS_IPHONE@interface AmazingView : UIView#else@interface AmazingView : NSView#endif{} @end
{% endhighlight %}

Because **UIView** and **NSView** serve the same purpose on both platforms, using shimming is a nice solution to present yout library interface to any platform.
But then, internally, you'll have to be careful about what you do with this AmazingView. Don't try do to mouse handling on iOS. That means you'll need to use the conditional compilation `#if TARGET_OS_IPHONE` at several places in your code, and that could quickly be a mess.

But when you have a small library, working with simple classes available on both platforms, shimming is really useful. Let's look at the UIColor/NSColor pair. If you want to design a library that handles colors, shimming might help.

{% highlight objc%}#if os(iOS)
    import UIKit
    public typealias Color = UIColor
    #else
    import AppKit
    public typealias Color = NSColor
#endif

[...]

public init(colors: Color...) {
    self.init(colors: colors, nil)
}
{% endhighlight %}

Notice that with Swift, you identify the os you're building for with the os() macro.

By making this typealias, you provide your own **Color** that will adapt on each platform. However, when working with your library we will be forced to use this **Color** type.

One thing that could be possible is to keep your typealias contained to your module, and provide two (or more) distinct initializers. This way, we do not have to concern about the type of the color object to use when calling your library.

{% highlight objc%}#if os(iOS)
    import UIKit
    typealias Color = UIColor
    #else
    import AppKit
    typealias Color = NSColor
#endif

[...]

#if os(iOS)
	// Put any initializers here for iOS
	public init(colors: UIColor...) {
    	self.init(colors: colors, nil)
	}
#else
	// Put any initializers here for OSX
	public init(colors: NSColor...) {
	    self.init(colors: colors, nil)
	}
#endif
{% endhighlight %}

We saw that for simple classes like **UIColor**/**NSColor** which have a lot in common on iOS and OSX, shimming will let you work for both platforms without rewriting a lot of code. It becomes more complicated when the class you encapsulate differs, even slightly, on each platform. You'll have to write a bit more code to handle specific cases.

For a complete tour of how you could share code between iOS and OSX, watch the talk Elizabeth Reid gave at the [wwdc_2014][WWDC] 2014.

[shim_wikipedia]: http://en.wikipedia.org/wiki/Shim_(computing)
[233]: http://asciiwwdc.com/2014/sessions/233
[wwdc_2014]:https://developer.apple.com/videos/wwdc/2014/
