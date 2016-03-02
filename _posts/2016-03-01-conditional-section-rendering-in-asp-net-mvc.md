---
 layout: post 
 title: Conditional section rendering in ASP.NET MVC
 comments: true
 date: 2016-03-01 12:30:00
---

Recently I was working on reducing some redundancy in an ASP.NET MVC application and ran into an error when trying to conditionally render a section.
ASP.NET was not happy and gave me the following error:

> The following sections have been defined but have not yet been rendered for the layout page "~/Views/Shared/_Layout.cshtml": "ProductionOnlyScripts".

On this layout page we have the following code.

``` csharp
@if (ApplicationConfiguration.IsProduction)
{
    @RenderSection("ProductionOnlyScripts", false)
}
```

I never expected that if I defined a section that MVC would require me to render it.
I can kind of understand why, but I disagree with this design decision.
Lets run down the options we have with sections in general.

1. Calling `@RenderSection(name)` when the section has not been defined will throw an error. This is the **right thing to do**.
2. Calling `@RenderSection(name, required: false)` when the section has not been defined will not throw an error. This is the **right thing to do**.
3. Not calling `@RenderSection(name)` or `@RenderSection(name, required: false)` will throw an error if that section has been defined (as is what we have observed).

So, the work around to this is simple: we render the section to nothing.

``` csharp
@if (ApplicationConfiguration.IsProduction)
{
    @RenderSection("ProductionOnlyScripts", false)
}
else
{
    RenderSection("ProductionOnlyScripts", false)?.WriteTo(TextWriter.Null);
}
```

In the end, ASP.NET is tracking the section fragment and ensuring that we are writing out it's contents.
What it doesn't know is that we are just writing that to /dev/null effectively so that the content will
never make it's way to the browser.

This can be extended as well into an extension method if you find yourself doing this often.
