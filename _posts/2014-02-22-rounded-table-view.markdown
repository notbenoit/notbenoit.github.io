---
layout: post
title:  "Missing the rounded UITableView ?"
date:   2014-02-22 17:29:45
categories: jekyll update
author: "Benoît Layer"
---
Everyone knows iOS7 introduced a huge earthquake in UIKit look and feel. I mean, every component changed, from the simple rounded rect button to the style of a whole UITableView. If you are a bit nostalgic of your old rounded grouped UITableView on iOS6, this is made for you. 

For the demo, you will need to have a simple controller with a UITableView and a few items in it. I will go straight to what we are here for : mimic iOS6 Grouped UITableView, so I won't spend time on all the tableView delegate/dataSource stuff.

![Starting point][id]

###Okay, let's do this !

First, you will want to get rid of the extra rows that live at the bottom of the UITableView (we have those rows because our UITableView content is not long enough to fill the whole space).

{% highlight c %}
_tableView.tableFooterView = [[UIView alloc] initWithFrame:CGRectZero];
{% endhighlight %}

Then the separators, we want them to stick to the left :

{% highlight c %}
_tableView.separatorInset = UIEdgeInsetsZero;
{% endhighlight %}

You can do this modification with the appearance protocol wherever you want.

Then you have to set an inset to avoid the view being stuck to the borders of the screen (the view can also be resized in a nib for instance...). Remember, we want to mimic iOS6 style, so we set a margin on each side of the UITableView.

{% highlight c %}
_tableView.frame = CGRectInset(_tableView.frame, 20.f, 20.f);
{% endhighlight %}

If you have set your tableview style to Grouped, then you should have something similar to this.
Now the tricky part is to round the top corners of the first cell of each section, and the bottom corners of the bottom cell of each section.

To do this, we are gonna subclass UITableView to modify its drawing in the layoutSubviews method.


[id]: /images/rounded-table-view/startingpoint.png  "Starting point"
