---
layout: post
title: Making posh-git more humane by improving the color scheme
date: 2014-07-12 19:00:00
category: coding
tags: git posh-git
---

<p class="jumbotron">
	Out of the box, posh-git uses a red color that's a bit hard to read against most background colors.
	All hope is not lost, because it's very easy to fix.
</p>

The console on windows has some named colors built-in and posh-git is configured to use `DarkRed`. While we can't
change the named color that PoshGit uses, we can change the color that the console displays. To do this, simply **right-click
on the powershell icon**, then **select properties** from the drop down menu.

It should be pretty clear which color in the row of colors is dark red, but if it isn't then it is the **fifth color 
from the left**.

<div class="text-center">
	<img src="/assets/2014-07-13-console-properties-dark-red-position.png" title="Position of the dark red color in the console properties window" width="665" height="337">
</div>

Once you select the dark red color, you can the selected color values to a more humane color. As can be seen in the
screenshot, I've chosen a salmon color (`R: 255`, `G: 154`, `B: 154`) which goes well with my black color. 
Now, likely you're still on `Screen Background` and the console background has changed to a salmon color, so just 
remember to **re-select the previous background color before you hit Ok**.

That's it. Now hopefully you'll have a more humane posh-git experience. 