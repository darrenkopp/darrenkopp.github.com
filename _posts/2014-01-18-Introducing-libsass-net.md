---
layout: post
title: Introducing libsass-net
date: 2014-01-18 00:00:00
category: opensource
tags: sass libsass
---
<p class="jumbotron">
	Although there are already <a href="https://libsassnet.codeplex.com/">other</a> <a href="https://github.com/TBAPI-0KA/NSass">projects</a>
	that wrap <a href="https://github.com/hcatlin/libsass">libsass</a>, they don't appear to be maintained anymore, so I fork'd NSass and fixed some of 
	the maintainability problems with it. From that, <a href="https://github.com/darrenkopp/libsass-net">libsass-net was born</a>.
</p>

### Introduction
Recently there have been a few bugs that Sassy Studio users have been running into related to libsass that
have been fixed for a while, but the wrapper I had been using (NSass) had not been updated in over 6 months.
Since NSass was just a fork of libsass and then moved the files, getting the updates from libsass would not
be an easy process.

To solve this issue, I created [a new project on GitHub](https://github.com/darrenkopp/libsass-net) and 
brought in the code using a [git submodule](http://git-scm.com/docs/git-submodule) which should allow me to
very easily grab updates as the roll out and become stable.

### Roadmap
Currently, I'm a bit bogged down with a new job and a basement project so right now libsass-net is the bare
minimum amount of work that I needed to do to get it working for Sassy Studio. In the somewhat near future
I plan on adding support for the following (pull requests welcome):

- NuGet packages
- x64 support (currently it works, must a manual process)
- ASP.NET handler
- System.Web.Optimization integration