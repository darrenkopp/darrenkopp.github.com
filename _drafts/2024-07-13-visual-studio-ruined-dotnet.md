---
type: post
layout: post
title: "Visual Studio ruined .NET"
date: 2024-07-13 00:00:00
categories: software
tags: opionion
comments: false
draft: true
---

I can't tell you exactly which release gained the functionality to automatically generate the namespace
for new files, but I'm pretty sure it was in Visual Studio 2005 (possibly Visual Studio 2008). At the
time I was very excited because it previously just put the Project Name as the namespace and you had
to fill out the rest yourself. A wonderful quality-of-life improvement at the time, now not so much.

It's hard to fully blame visual studio here, however. It really was and still is a nice feature.
The problem is that while a lot of the time your namespace hierarchy and folder structure will
align perfectly, once you start building sufficiently complex applications you will likely be dealing
with thousands of files. Having hundreds of files in a single folder isn't very usable, so it's 
fairly natural to start refining the physical file structure to be more manageable.

The logical organization, however, likely should not change. The logical organization is your
namespace structure. This is where the problem creeps in of automatically generating your
logical organization structure from your physical structure, at least if you are lazy. I have
found that almost 100% of the time my physical structure is a subset of my logical structure so
having the IDE generate the namespace for me was a net benefit because I just had to remove portions
of the generated namespace to get the structure I wanted as opposed to having to fill in the structure
I wanted. You can even see the lengths that Microsoft go to work around this in their own projects
by removing the default namespace and [having a hierarchy of folders just to get the defaults to match][1].

Now I'm sure that this sounds like a pedantic difference and you aren't necessarily wrong, but in 
practice there are some practical downsides. Now I'm targeting Visual Studio here but honestly I
feel like things started going downhill the most when ASP.NET MVC hit the scene. I may be wrong here
but MVC was largely convention based and only looked at types in the `Controllers` namespace when
I would argue that likely wasn't important, but maybe it was the best solution when you would have
lots of controllers organized across multiple namespaces in the days before `Areas` hit the scene.

Regardless, we now have `Controllers` as a significant namespace, but due to the pattern set forth
by the team and the project template, you would also have a `Views` and `Models` folder. These also
became namespaces. Now when I've argued this point in the past, the justification I have always 
provided is that when you are designing components to be used by other users (or even yourself), you
should try to ensure that everyone falls into the **pit of success**.

In my opinion here for someone to fall into the pit of success, they should reasonably have everything
they need to be successful when they import your namespace for the majority of their tasks. This
includes just working in the project itself. With the default template in ASP.NET MVC I will always
have to import the `Models` namespace when I am working on a controller (and to a lesser extent a view).
They solved the issue with views by allowing you to import multiple namespaces by default in another file
and to some extent global usings are how people are working around this frustration today.

You can see how this can be a problem for new users working with a project and even more so for users
working with a library. 

[1]: https://github.com/dotnet/runtime/tree/main/src/libraries/System.Text.Json/src/System/Text/Json