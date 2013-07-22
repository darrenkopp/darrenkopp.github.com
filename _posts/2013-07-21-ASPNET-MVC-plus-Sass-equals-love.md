---
layout: post
title: ASP.NET MVC + Sass = &hearts;
date: 2013-07-21 18:38:00-0600
categories:
- sass
tags:
- aspnetmvc
- sass
---

<p class="jumbotron">
	Sass makes CSS much easier to work with and since many of the popular front-end frameworks are 
	<a href="http://foundation.zurb.com/">developing their CSS using SASS</a>, it becomes
	essential to have friction-free support for it with ASP.NET MVC.
<p>

I just spent a few hours trying to figure out how to get ASP.NET MVC and Sass to work well together.
There are [so many packages on NuGet](http://www.nuget.org/packages?q=sass) that **claim** to support sass, 
you would think it would be easy, but it was more challenging than I had expected.

Ultimately, I was able to get [NSass](http://www.nuget.org/packages/NSass.Optimization/) to work the way I wanted it to
and went with that, but here is what I found out along the way.

### The ideal configuration ###
Before setting out to find a runtime solution, I currently was using the 
[System.Web.Optimization](http://www.nuget.org/packages/Microsoft.AspNet.Web.Optimization/) packages
and build / save time compilation of the sass files.

While this worked out reasonably well for me, it wasn't working for our designer who just wanted
to edit the sass files and refresh the browser to see the changes. This meant that we needed a solution
that would catch file modifications and then regenerate the css which was being minified by the 
`System.Web.Optimization` packages.

### SassAndCofee ###

The [SassAndCofee.AspNet](http://www.nuget.org/packages/SassAndCoffee.AspNet/) package is by 
[Paul Betts](http://paulbetts.org/) and it uses the [SassAndCoffee.Ruby](http://www.nuget.org/packages/SassAndCoffee.Ruby/) package.
It seems to be primarily convention based and does minification, but doesn't integrate with the 
`System.Web.Optimization` classes.

While I did get this to work, it didn't work out for me because this library doesn't
seem to support some newer functionality (specifically the interpolation stuff) so
it threw an exception while parsing and then went into an infinite loop while trying to handle that exception. 
It likely will work once the ruby parser (embedded in the assembluy) is updated.

Unfortunately, this flaw basically makes many other packages not work.

- [Web.Optimization.Bundles.Sass](http://www.nuget.org/packages/Web.Optimization.Bundles.Sass/)
- [Cassette.Sass](http://www.nuget.org/packages/Cassette.Sass/)

Project Site - [https://github.com/xpaulbettsx/SassAndCoffee](https://github.com/xpaulbettsx/SassAndCoffee)

### BundleTransformer.SassAndScss ###
The [BundleTransformer.SassAndScss](http://www.nuget.org/packages/BundleTransformer.SassAndScss/) packages 
is very similar to SassAndCoffee.Ruby. It uses IronRuby and uses a ruby file to parse
and compile the sass, but it can integrate with `System.Web.Optimization` to support bundling and minification.
This package had the same problem as SassAndCoffee in that it ran into a parsing exception when
it hit the interpolation operator, but it didn't go into a loop.

Project Site - [http://bundletransformer.codeplex.com/](http://bundletransformer.codeplex.com/)

### Bundler ###
The [Bundler](http://www.nuget.org/packages/Bundler/) package is a member of the [ServiceStack](http://www.servicestack.net/)
family. With this you create a `.bundle` file in your content directory which has the sass files you
want in that bundle, then you run `bundler.cmd` (or is run automatically if you have a visual
studio extension installed) and it will generate the css files and optionally minify them.

It also comes with some extensions in for MVC that give you functionality similar to those in `System.Web.Optimization`.
Ultimately, I decided against using this because it wasn't that different than what I was already
doing, but it does work and could be the ideal solution for many other people.

Project Site - [https://github.com/ServiceStack/Bundler](https://github.com/ServiceStack/Bundler)

### FubuMVC.Sass ###
I didn't evaluate the [FubuMVC.Sass package](http://www.nuget.org/packages/FubuMVC.Sass/) since
I am not using FubuMVC, but I will assume that it works.

### NSass ###
There are a few packages to NSass:

- [NSass.Core](http://www.nuget.org/packages/NSass.Core/) is the core functionality to parse sass code and emit css
- [NSass.Optimization](http://www.nuget.org/packages/NSass.Optimization/) gives you a few classes that
let you hook into `System.Web.Optimization`
- [NSass.Handler](http://www.nuget.org/packages/NSass.Handler/) provides an `IHttpHandler` implementation
that will parse requests to .scss files, which is what the `System.Web.Optimization` bundle will
emit when in debug mode, so it's necessary when using it.

This project just wraps [libsass](https://github.com/hcatlin/libsass) and didn't run into the interpolation problem. 
This was simultaneously this simplest project to get running and the one that took the longest to get **really running**.
It works absolutely fantastic out of the box, but you need to be sure that the machine you run it on
has the Microsoft Visual C++ redistributable runtime installed. It took me forever to figure out
that was the reason it wouldn't work on our web servers.

Once that problem is resolved, it is amazing.

Project Site - [https://github.com/TBAPI-0KA/NSass](https://github.com/TBAPI-0KA/NSass)

### Notable Mentions ###
Well, before trying to get all of this working, I was using [MindScape's Web Workbench](http://www.mindscapehq.com/products/web-workbench), 
but even that had problems which is why I began looking for a runtime solution rather than a 
build / save time solution. Once I had found NSass, all I needed next was the Visual Studio support 
back which is why I ended up creating [SassyStudio](/posts/2013/07/21/Announcing-SassyStudio.html).