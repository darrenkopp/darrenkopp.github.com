---
layout: post
title: Announcing SassyStudio
date: 2013-07-21 19:36:00 -0600
categories: projects
tags: SassyStudio sass scss
---
<div class="jumbotron">
	<p>
		TL;DR: I got tired of MindScape's Web Workbench extension being slow and popping up modal
		dialogs telling me to buy the pro version, so I decided to add support for scss into Visual Studio
		myself. 
	</p>
	<p>
		You can <a href="http://visualstudiogallery.msdn.microsoft.com/85fa99a6-e4c6-4a1c-9f00-e6a8129b6f4d">install it from the Visual Studio Gallery</a>
		and you can <a href="https://github.com/darrenkopp/SassyStudio">view the code on GitHub</a>
	</p>
</div>

Today I am announcing my first Visual Studio extension: [SassyStudio](https://github.com/darrenkopp/SassyStudio). 
Right now, there is not a lot of support for sass/scss in Visual Studio (though I believe the web team is working on it), 
so I decided to do something about it. 

Originally, I had been using [MindScape's Web Workbench](http://www.mindscapehq.com/products/web-workbench) 
for working with sass files which worked fine for the most part, but I disliked a few parts about it:

- It was slow opening files that had many imports (in my case [Zurb Foundation](http://foundation.zurb.com/))
- It pops up modal dialogs telling me that what I'm doing is a _pro_ feature only. That messes up your flow greatly.

The only reason I was using it was so that I could generate a css file from the scss file, but that was never
how I _really_ wanted to work. I wanted to have the file generated automatically by ASP.NET MVC. Once
I was able to do that, Web Workbench didn't really give me a lot, so I started on SassyStudio.

### Minimum Viable Weekend Project ###
Right now what you see is what I could get done in a couple of days. This is my first extension, so I
had to do a lot of tinkering to figure this out and why there are very few features it supports. For **v0.1**
you get the following:

- Syntax Highlighting
- Region Outlining

Yup, that's it. Until I know how to look up your current css color settings, you will need to go configure
the colors yourself. It will look like plain text by default, but once you have them configured in settings
you are good to go.

Don't worry, I am planning to add support for the following:

- Integrating with CSS Editor to provide CSS intellisense
- Support for generating CSS file when saving
- Intellisense support for sass (variable / mixin / function completion)
- Multiline comments (this one is amusing)
- Keyboard Shortcuts (the irony of this one is **astounding**, right?)
- Support old sass style (indentation based)

Now, some of these are stupid easy to add, I just need to figure out the visual studio
integration points. If you are interested in one of these getting sooner than another, let me know
and I will see what I can do.

### Acknowledgements ###
I should say that most of this wouldn't have been possible (or would have taken much longer) 
without [Web Essentials](http://vswebessentials.com/). I browsed their source code quite heavily
to figure out how to do this stuff and there's parts of their code in a lot of my files as
I used their implementations as a starting point for mine.

### Going Forward ###
I'm hoping to add more features to this extension in my spare time, but I don't know when that will be
since this was basically a weekend distraction while I transition from one project to the next. Since
most of the features aren't that tough, I would expect those to get done soon at a minimum, but the
harder things might be a while.

You should still go [download and install it](http://visualstudiogallery.msdn.microsoft.com/85fa99a6-e4c6-4a1c-9f00-e6a8129b6f4d) though.
If you find any problems, please [make an issue on GitHub](https://github.com/darrenkopp/SassyStudio/issues) 
or [berate me on twitter](https://twitter.com/darrenkopp).