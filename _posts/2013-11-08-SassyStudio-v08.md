---
layout: post
title: SassyStudio v0.8
date: 2013-11-08 21:00:00 -06:00
category: SassyStudio
tags: scss intellisense
---

It's been a little while since the last blog talking about what's new in SassyStudio
there has been a lot of new features.

### Nested Property Intellisense

The major new feature in v0.8 is smarter css property intellisense for nested properties.
Consider the following style, you will now get intellisense suggesting values like `family`
and `weight` when in the `font` block.

```scss
body {
	font: {
		size: 14px;
	}
}
```

### Format Document support

There is some basic support for automatically formatting a document.
Right now, it only supports fixing indentation issues, and the indentation options
are configurable in the same place as all other languages 
(`Tools > Options > Text Languages > SCSS`). 
Thanks to [@Unt1tled](https://github.com/Untit1ed) for requesting this.

### Compilation of file when dependent document changed

Now when you modify a document that is imported by another root level document, the root
document will be generated. This was a long overdue feature and I'd like to thank 
[@funzeye](https://github.com/funzeye) for requesting it.

### Basic Compass Support

If you are a compass user, you'll be glad to know that SassyStudio now bootstraps
compass and will use it to generate your css files when you save them. All you need
to do to enable support for compass is to provide the path to your ruby installation
path in the options (`Tools > Options > SassyStudio > Ruby Install Path`). Thanks to
[@okoetter](https://github.com/okoetter) for requesting this.

### Various Intellisense / Parsing Improvements

As usual there have been some various bug fixes.

* Fix CSS intellisense not working when only VS2013 installed
* Fix `@if` directive not parsing correctly when immediately followed by (
* Improve CSS property detection and fix properties declarations not 
followed by a space not being highlighted correctly
* Fix incorrect highlighting after using comment/uncomment selection command

### Moving Forward

As usual, I'll keep enhancing the existing functionality and work on adding more intellisense support.
I'll continue tweaking intellisense to get it to the point where I feel comfortable enough enabling it
by default. If you find anything broken or confusing, please feel free to [reach out to me on
Twitter](https://twitter.com/darrenkopp) or [file an issue on GitHub](https://github.com/darrenkopp/SassyStudio/issues).