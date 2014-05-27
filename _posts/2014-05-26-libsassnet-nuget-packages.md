---
layout: post
title: libsassnet nuget packages
date: 2014-05-26 19:00:00
category: coding
tags: libsassnet nuget libsass System.Web.Optimization
---
I just released two new nuget packages: 
[libsassnet](http://www.nuget.org/packages/libsassnet/) and 
[libsassnet.Web](http://www.nuget.org/packages/libsassnet.Web/).

A while ago [I blogged about the state of Sass support](/posts/2013/07/21/ASPNET-MVC-plus-Sass-equals-love.html)
in ASP.NET. Since then, a lot has changed: Sass hit version 3.3, libsass has become extremely popular,
and Visual Studio 2013 now has support for Sass built in.

Unfortunately in the case of NSass, it hasn't been updated in over a year. For that reason, I've
been porting most of the functionality over to libsass-net, and the nuget packages and ASP.NET
integration has been a glaring whole in that project for a while.

### Pre-requisites

Since libsass is a C++ project, you'll need to make sure you have the Visual C++ Redistributable
installed. I'm not a C++ expert, so I'm not sure if the latest version works or if you need
the 2012 version.

### Integrating with System.Web.Optimization

Integrating with the System.Web.Optimization project is very simple with libsass.
Below is an example configuration:

```csharp
var sass = new SassBundle("~/content/css", basePath: "~/sass")
    .Include("~/sass/app.scss")
    .Include("~/sass/fonts.scss");

bundle.Add(sass);
```
