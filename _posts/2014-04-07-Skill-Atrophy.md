---
layout: post
title: Skill Atrophy
date: 2014-04-7 12:00:00
category: coding
tags: coding life
---

<p class="jumbotron">
  Just like a muscle technical skills can and will diminish over time
  if you don't take the time to regularly practice them.
</p>

About 8 years ago I took a C++ course at Weber State University and everything
was great. I had just finished my C course, so C++ was *fairly straightforward*
and I didn't have any problems with the language. I was even able to teach
myself C# using what I learned from C and C++ and have been using C# ever since.

Since that time, I have not written any C/C++ code. Zilch. That is, until just
recently when I needed to write some C++ code for
[libsass-net](https://github.com/darrenkopp/libsass-net) that would add support
for generating sourcemap files. I didn't even need to really write any code,
just hook into the existing code. Needless to say, it wasn't that simple.

### Problem after problem

Before I was even able to get started coding, I had already hit a problem after
pulling the latest changes from [libsass](https://github.com/hcatlin/libsass).
The compiler kept complaining about a method named UNICODE, but I couldn't
figure out what the problem was. The only hint I had was that Visual Studio was
highlighting this method differently than all the other methods.

After a while I finally figured out that a configuration setting in my project
file was telling Visual C++ that I wanted to use Unicode strings, so there was a
`#define UNICODE` being emitted by the compiler. I believe I may have found the
solution quicker if I had been more familiar with the toolset.

Throughout the process of trying to figure out **why things weren't working** I
found myself having a hard time understanding what most of the code was actually
doing. C++ is a very powerful and very terse language; I found myself struggling
to keep track of all the symbols that were in front of me. I was lost in a world
of template methods, `void *` pointers, and lots of other features I vaguely
remember.

### Keeping up to date

For me, I have decided that I should spend a little time to maintain at least
a reading level of the various languages that I have learned over the years.
I know that I won't be able to easily maintain a fluency level in all the
languages I have learned because I won't be writing in them daily, but I can
at least retain some of my skill set by reading other people's code.

In fact, I think by maintaining the libsass-net project, I am in an ideal
situation: I am required to write a minimal amount of C++ and need to have at
least a minimal reading comprehension of the language. Hopefully I'll be able to
keep my skills somewhat up-to-date without having to go overboard and force
myself to make my own project to practice.

Regardless of how you approach the problem, **be aware of the skills that you are
letting atrophy and take the time to practice them** if those skills are important
to you. If you do not, you may find yourself in a similar situation to my own,
and it's not the most pleasant place to be.
