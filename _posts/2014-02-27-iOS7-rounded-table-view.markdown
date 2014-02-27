---
layout: post
title:  "Missing the rounded UITableView ?"
date:   2014-02-27 17:29:45
categories: jekyll update
author: "Benoît Layer"
---
Everyone knows iOS7 introduced a huge earthquake in UIKit look and feel. I mean, every component changed, from the simple rounded rect button to the style of a whole UITableView. If you are a bit nostalgic of your old rounded grouped UITableView on iOS6, this is made for you. 

For the demo, you will need to have a simple controller with a UITableView and a few items in it. I will go straight to what we are here for : mimic iOS6 Grouped UITableView, so I won't spend time on all the tableView delegate/dataSource stuff.

![Starting point][id]

###Okay, let's do this !

First, you will want to get rid of the extra rows that live at the bottom of the UITableView (we have those rows because our UITableView content is not long enough to fill the whole space).

{% highlight objc%}
_tableView.tableFooterView = [[UIView alloc] initWithFrame:CGRectZero];
{% endhighlight %}

Then the separators, we want them to stick to the left :

{% highlight objc%}
_tableView.separatorInset = UIEdgeInsetsZero;
{% endhighlight %}

You can also do this modification with the appearance protocol wherever you want.

Then you have to set an inset to avoid the view being stuck to the borders of the screen (the view can also be resized in a nib for instance...). Remember, we want to mimic iOS6 style, so we set a margin on each side of the UITableView.

{% highlight objc%}
_tableView.frame = CGRectInset(_tableView.frame, 20.f, 0.f);
{% endhighlight %}

If you have set your tableview style to Grouped, then you should have something similar to this.
Now the tricky part is to round the top corners of the first cell of each section, and the bottom corners of the bottom cell of each section.

To do this, we are gonna subclass UITableView to modify its drawing in the layoutSubviews method.
{% highlight objc%}
#define IS_IOS7_AND_UP() ([[UIDevice currentDevice].systemVersion floatValue] >= 7.f)
{% endhighlight %}
{% highlight objc%}
- (void)layoutSubviews
{
    [super layoutSubviews];
    
    // Apply our modification only on iOS7 and above
    if (self.style == UITableViewStyleGrouped && IS_IOS7_AND_UP())
    {
        // For each section, we round the first and last cell
        int numberOfSections = [self.dataSource numberOfSectionsInTableView:self];
        for (int i = 0 ; i < numberOfSections ; i++)
        {
            static CGFloat cornerRadius = 8.f;
            
            // Get first and last cell
            int numberOfRows = [self.dataSource tableView:self numberOfRowsInSection:i];
            UITableViewCell *topCell = [self cellForRowAtIndexPath:[NSIndexPath indexPathForRow:0 inSection:i]];
            UITableViewCell *bottomCell = [self cellForRowAtIndexPath:[NSIndexPath indexPathForRow:numberOfRows-1 inSection:i]];
            
            // If there is a single row, we round the cell by each corner
            if (topCell == bottomCell)
            {
                CAShapeLayer *shape = [[CAShapeLayer alloc] init];
                shape.path = [UIBezierPath bezierPathWithRoundedRect:topCell.bounds
                                                        cornerRadius:cornerRadius].CGPath;
                topCell.layer.mask = shape;
                topCell.layer.masksToBounds = YES;
            }
            else
            {
                // Create the top mask we will apply on the cell to round it.
                CAShapeLayer *topShape = [[CAShapeLayer alloc] init];
                topShape.path = [UIBezierPath bezierPathWithRoundedRect:topCell.bounds
                                                      byRoundingCorners:UIRectCornerTopLeft|UIRectCornerTopRight
                                                            cornerRadii:CGSizeMake(cornerRadius, cornerRadius)].CGPath;
                topCell.layer.mask = topShape;
                topCell.layer.masksToBounds = YES;
                
                // Create the bottom mask we will apply on the cell to round it.
                CAShapeLayer *bottomShape = [[CAShapeLayer alloc] init];
                bottomShape.path = [UIBezierPath bezierPathWithRoundedRect:bottomCell.bounds
                                                         byRoundingCorners:UIRectCornerBottomLeft|UIRectCornerBottomRight
                                                               cornerRadii:CGSizeMake(cornerRadius, cornerRadius)].CGPath;
                bottomCell.layer.mask = bottomShape;
                bottomCell.layer.masksToBounds = YES;
            }
            
            // For performance issue, and to avoid offscreen rendering, we rasterize our layers.
            topCell.layer.shouldRasterize = YES;
            topCell.layer.rasterizationScale = [[UIScreen mainScreen] scale];
            bottomCell.layer.shouldRasterize = YES;
            bottomCell.layer.rasterizationScale = [[UIScreen mainScreen] scale];
            
            // Because the TableView reuses some cells, we don't want our middle cells to be rounded,
            // so we force their mask to be nil (not a random rounded one).
            for (int j = 1 ; j < numberOfRows-1 ; j++)
            {
                UITableViewCell *cell = [self cellForRowAtIndexPath:[NSIndexPath indexPathForRow:j inSection:i]];
                cell.layer.mask = nil;
                cell.layer.shouldRasterize = NO;
            }
        }
    }
}
{% endhighlight %}

By the way, since you have subclassed UITableView, we can apply here some tricks we used just before. You can for instance set the inset stuff and the footer trick in the willMoveToSuperview: method. Like this : 

{% highlight objc%}
- (void)willMoveToSuperview:(UIView *)newSuperview
{
    [super willMoveToSuperview:newSuperview];
    if (newSuperview && self.style == UITableViewStyleGrouped && IS_IOS7_AND_UP())
    {
        // Side margin
        CGFloat sideOffset = 8.f;
        self.tableFooterView = [[UIView alloc] initWithFrame:CGRectZero];
        self.separatorInset = UIEdgeInsetsZero;
        self.frame = CGRectInset(self.frame, sideOffset, 0.f);
    }
}
{% endhighlight %}

The final result should look like this :

![Result][result]

Of course, you can adjust the colors as you want, as for the corner radius or the frame inset.
These modifications do not cost a lot in terms of performance. The rasterization avoid offscreen rendering, and the mask is not that complicated. I've tested this implementation on UITableView with thousands of items, and just noticed a little drop in performance when pushing a UIViewController with such modifications. Feel free to use this trick, and even more to improve it!


[id]: /images/rounded-table-view/startingpoint.png  "Starting point"
[result]: /images/rounded-table-view/result.png  "Result"