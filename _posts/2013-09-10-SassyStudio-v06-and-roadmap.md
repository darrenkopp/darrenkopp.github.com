---
layout: post
title: SassyStudio v0.6 and roadmap
date: 2013-09-10 05:27:00 -0600
category: SassyStudio
tags: SassyStudio sass scss
---

<p class="jumbotron">
	<a href="https://github.com/darrenkopp/SassyStudio/releases/tag/0.6">I just released <strong>v0.6</strong> 
	of SassyStudio</a> and wanted to give a progress update on what is coming up next and where I plan on taking things.
</p>

The latest version of SassyStudio ended up being a lot more work than I had anticipated, but now that
it is done I'm extremely excited about what is coming up next for the project. For the next few releases
I will be <strong>focuessing primarily on intellisense</strong> and expanding on what was added in v0.6.

Intellisense falls into three categories in Visual Studio:

* Completion - The list of possible values
* Quick Info - Information about a specific completion value
* Signatures - Information about a method call (such as arguments and the order they appear)

In v0.6 I just shipped the initial support for completion which adds completion support for
keywords, variables, functions, and mixins.  In future releases I will continue to expand the
scope of completions that are supported, but the core of what SCSS offers should have decent completion
support at this time. Quick Info and Signatures will be coming, but
likely not for a few releases. 

Now, lets lay out what you can expect to see in the next few releases.

### v0.7 - CSS Intellisense

Ultimately, v0.6 took so long because I ended up not being able to extend the current CSS editor and
had to recreate it on my own and that means I'll end up having to add the CSS intellisense on my own
as well. However, I can use the same source information that the regular CSS editor uses since it's
all stored in some <abbr title="Extensible Markup Language">XML</abbr> files on the file system.

Since I don't have to compile a list of all CSS properties and elements and should be able to use
the schema files already included in Visual Studio, I'm cautiosly optimistic that v0.7 may be able
to be released before the end of the month.

### v0.8 - SCSS Intellisense

Once we have support for basic CSS intellisense, I will be able to introduce intellisense into SCSS
specific contexts like nested properties. For instance, when we have some SCSS code like
`.panel { font: { family: Helvetica, Arial, sans-serif; } }`, we can limit completion values in the
`font` property block to the `font-` namespace like `family`, `weight`, etc.

As of right now, I'm not sure how difficult this would be, but I am thinking it should be simple and
we can expect that v0.8 will quickly follow v0.7.

### v0.9 - Classic SASS support

By the time that v0.9 rolls around, I am hoping that I will have definitively determined how to easily
add support for .sass files for those who are still using them. Now, SASS and SCSS are so similar that
(as far as I am aware of right now), I should be able to just have my new tokenizer emit fake curly brace
tokens when the indentation level changes. If not, then I will just end up having to split the tokenizer
into two and share what I can, but the unique parts should be pretty small and thus I am again 
**cautiously optimistic** that this should be a fairly simple and quick release.

### v1.0 - Advanced Function Intellisense

For the 1.0 release I want to finally get the intellisense around functions to be on par with what you
see in other first class languages in Visual Studio. For 1.0 I will add support for viewing the function "signature"
so that you can see the arguments that the function is expecting for both system functiona and custom functions. 
With this will also come general completion support for "named arguments" when calling a function (i.e. `rgb($r: 255, $g: 255, $b: 255)`)
and likely some warnings if your function call is invalid.

## Infinity and beyond

That gives you an overview of what to expect out of the next few releases, but beyond that is somewhat in
flux at this point. There's so much more to do with intellisense in addition to what I have laid out here
but they are much more complex (i.e. type inference on variables) that I felt that what I laid out in the
roadmap will be much more beneficial to the user.

If you have any feedback on what you would like to see in SassyStudio, feel free to leave a comment here
or [enter it as an issue on GitHub](https://github.com/darrenkopp/SassyStudio/issues).