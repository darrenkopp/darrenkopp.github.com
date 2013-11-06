---
layout: post
title: "SassyStudio Feature #1: Xml? No, better intellisense of course!"
date: 2013-10-12 21:00:00 -06:00
category: SassyStudio
tags: scss intellisense
---

<p class="jumbotron">
	The first <a href="https://github.com/darrenkopp/SassyStudio">SassyStudio</a> specific feature is 
	<a href="https://github.com/darrenkopp/SassyStudio/issues/10">support for the functionality to get intellisense in other files</a> 
	without the using the <code>@import</code> directive.
</p>

<div class="alert alert-warning">
	<strong>Update:</strong> Looks like the <a href="http://blogs.msdn.com/b/webdev/archive/2013/11/06/a-high-value-undocumented-less-editor-feature-in-visual-studio.aspx">LESS functionality in visual studio supports
	the same syntax</a> as this, which is awesome! Originally I was using the attribute <strong>file</strong>
	but have since updated it to use the attribute <strong>path</strong> to match.
</div>

With large applications you'll typically find yourself breaking your SCSS files into logical groups,
typically centered around functional components. This is clean and efficient, but you lose intellisense
when you do... until now.

To accomplish this, I have started support for the `reference` tag in a triple slash (`///`) comment.
An example of this would be

    /// <reference path="variables" /> 

To get intellisense for multiple files, just add multiple references

    /// <reference path="variables" />
    /// <reference path="mixins" />

The `reference` tag works the same as a `@import` statement, and it can serve
as documentation for what files the current file you are working on depends on.

If you have any other ideas for what you would like to see supported in SassyStudio, please feel free
to [submit a feature](https://github.com/darrenkopp/SassyStudio/issues) request on GitHub