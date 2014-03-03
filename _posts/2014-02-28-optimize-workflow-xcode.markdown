---
layout: post
title:  "Optimize your workflow in Xcode"
date:   2014-02-27 17:29:45
categories: Xcode Workflow
author: "Benoît Layer"
---
Xcode is the IDE you'll be always using when developing MacOS apps or iOS apps. It's sometimes frustrating, and sometimes brillant, here are some hints to optimize your workflow and keep your brain fresh when using it.

I can already hear people saying that we do not have to use Xcode all the time. It's true. For all text based files, you can use whatever your want. SublimeText, Vim, or AppCode from JetBrains. But when it comes to editing nibs and storyboard, the choice is all made, you'll have to use Xcode (as far as I know).

After years working with Xcode, you learn to overpass the first feeling you had with it. Of course, the time of Xcode 3 is way behind us, and Xcode 4 made the things a lot better, believe me.

###My eyes are bleeding!

Lets's begin with the visual part. This is not specific to Xcode or any IDE, but you must use the colorscheme that better fits your eyes. As a personnal hint, prefer darker themes to avoid tears after just two hours behind a bright blank screen. For instance, I use [zenburn][zenburn] most of the time.

![Xcode-zenburn][xcode-zenburn-img]

All colors are dimmed, and the background is not a full black color, but rather a dimmed pastel black. You can find the colorscheme [here][xcode-zenburn], with the instructions to change you colorscheme in Xcode.

###Shortcut time
There are lots of shortcuts in Xcode, some you can't do without, and some that you'll forget very soon.

#### Build and run `⌘ R`
This is alot quicker than clicking on the top left 'play' arrow. This shortcut is the one you'll use the most often.

#### Stop `⌘ .`
Kills the current running app. With the previous shortcut, this one a must use.

#### Clean `⇧ ⌘ K`
Perform a clean action on the current project.

#### Organizer `⌘ 2`
Show the organizer. Useful if you want to check your provisioning profiles, or some documentation.

#### Search everywhere `⇧ ⌘ F`
Perform a search on the whole project.

#### Jump to next counterpart `^ ⌘ ↑` / `^ ⌘ ↓`
This is also a very useful one. Switch easily between a .m file and its .h. You can also do a three fingers vertical swipe.

#### Open Quickly `⇧ ⌘ O`
Okay, this is THE shortcut. It allows you to open any file in your workspace, or in any linked framework directly by tapping its name, or part of its name.

![Open Quickly][open-quickly]

###Usefull little hints
A useful tip you'll have to remember when working on huge projects, is the filter field in the left pane. It allows you to filter any tab you are currently displaying. For instance, on the project navigator tab, you can filter your classes with their name. You'll use this filter when finding a given set of files. Imagine all you model classes contain the "Model" word. If you want to display only your models, just filter your project with the word "model". Below is an example on the word "ViewController" to get all classes containing this word.

![Filter][filter]

###Snippets, I love you !
Last but not least, the code snippets. On the right pane, you have the snippet library. Xcode contains a lot of snippets out of the box. Have a look there and find the snippets you that can be useful to you.

![Snippets][xcode-snippets]

But the best part of it, is creating your own snippets. For instance, I built a snippet to generate appledoc comments (used for good documentation). To create this kind of snippet, just type in the editor the text you want to become a snippet. The interesting part is when you want placeholder texts for your snippet. Just surround your text with `<#` and `#>`. The hard part is here, the drag and drop of your snippet in the library. With a mouse you have to do it with both clicks pressed (whereas it will do another selection). On a trackpad, honestly I don't remember... Once your snippet is saved in your library, edit its name, its shortcut, its use cases... Then try it !

![Snippet description][xcode-snippet-desc]

![Snippet edition][xcode-snippet-lib]

![Snippet usage][xcode-appledoc]

***
This was a preview of all the possibilities Xcode has to offer. Now it's your turn to build your own snippet or find your favorite shortcuts. Remember that you will learn a key combination once, or take three minutes to build a snippet, and you will be rewarded by earning a lot of time in the future.

[zenburn]: http://slinky.imukuppi.org/zenburnpage/
[xcode-zenburn]: https://github.com/an0/Zenburn-for-Xcode
[xcode-zenburn-img]:/images/workflow-xcode/xcode-zenburn.png
[open-quickly]:/images/workflow-xcode/open_quickly.gif
[filter]:/images/workflow-xcode/xcode_filter.gif
[xcode-snippets]:/images/workflow-xcode/xcode_snippets.jpg
[xcode-snippet-desc]: /images/workflow-xcode/xcode_user_snippet.gif
[xcode-snippet-lib]: /images/workflow-xcode/xcode_snippet_lib.jpg
[xcode-appledoc]: /images/workflow-xcode/xcode_appledoc.gif

