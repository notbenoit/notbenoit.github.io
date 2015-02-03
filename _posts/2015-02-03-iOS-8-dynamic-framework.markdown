---
layout: post
title:  "CocoaTouch framework for iOS8, dear Xcode..."
date:   2015-02-03 21:01:00
categories: Development
tags: swift framework iOS8 bug xcode
author: "Benoît Layer"
---

With iOS8 release a few months ago, we are now allowed to build CocoaTouch frameworks (.framework like the ones shipped with UIKit) for iOS.

Last week, I started building a small framework to work with colors. I encountered some weird behaviours from Xcode, and I thought it would be useful to share some tips in a short post here.

<!--more-->

When creating a Swift framework for iOS, Xcode generates a boilerplate project with an architecture like the one below :

![boilerplate]

Here you can find a .h file in which you'll have to include all your public headers that will be available to everyone.

Now, you can add any file you want to your project, and add any source file as a member of your target.

After some time working on my project, I encountered a strange compile error.

```
<unknown>:0: error: could not build Objective-C module 'StepColor'
```

![error]

After struggling with module name, product name, schemes, and other parts of Xcode I did not know about, I simply found that the header we talked just before, was part of my target, but at the **Project** scope.

![header_scope]

I resolved this issue, and got a succesful build just by changing the scope of the header to **Public**

Nevertheless, I do not have any idea about how this header scope was moved to **Project** or **Private**.

While writing this post, I created a new CocoaTouch framework to reproduce the issue, but the scope was properly set to **Public**. Xcode, what magic is that ?

[step_color]: https://github.com/notbenoit/StepColor
[boilerplate]: /images/framework-iOS8/boilerplate.png  "Boilerplate"
[error]: /images/framework-iOS8/error.png  "Error"
[header_scope]: /images/framework-iOS8/header_scope.png  "header_scope"
