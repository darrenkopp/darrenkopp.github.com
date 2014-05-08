---
layout: post
title: Ignoring SignalR in NewRelic
date: 2014-05-07 19:00:00
category: coding
tags: signalr newrelic
---

We just started using [NewRelic](http://newrelic.com) at work and I've really been digging it. 
We also use [SignalR](http://signalr.net/) which (by design) can use long-polling connections. 
Unfortunately, those tend connections to skew the metrics in NewRelic.
Fortunately, [NewRelic provides an API](https://www.nuget.org/packages/NewRelic.Agent.Api/) to ignore certain transactions, 
so I thought I'd be able to tell it to just ignore signalr.

At first, I tried [this answer on StackOverflow](http://stackoverflow.com/a/13499306/77), 
but it ended up ignoring *all* requests, not just the ones for SignalR. 
In the end I had to use an Owin module to get the job done.

Here is the code I ended up with

``` csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Data;
using System.Diagnostics;
using AppFunc = Func<IDictionary<string,object>, Task>;

public class NewRelicIgnoreTransactionOwinModule
{
	private AppFunc _nextAppFunc;
	public NewRelicIgnoreTransactionOwinModule(AppFunc nextAppFunc)
	{
		_nextAppFunc = nextAppFunc;
	}

	public Task Invoke(IDictionary<string, object> environment)
	{
		object request = null;
		if (environment.TryGetValue("owin.RequestPath", out request)) {
			if (((string)request).IndexOf("signalr", StringComparison.OrdinalIgnoreCase) > -1) {
				NewRelic.Api.Agent.NewRelic.IgnoreTransaction();
			}
		}

		return _nextAppFunc(environment);
	}
}
```

To use this in your owin startup code call the following

```csharp
app.Use(typeof(NewRelicIgnoreTransactionOwinModule));
```