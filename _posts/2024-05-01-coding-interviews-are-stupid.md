---
type: post
layout: post
title: "Coding interviews are stupid (ish)"
date: 2024-05-01 00:00:00
categories: software
tags: career interview
comments: false
---

I have, once again, [failed an interview][1] (probably).
I don't actually know yet but if I were the interviewer I probably wouldn't recommend moving the
candidate forward. Based on the hyperbolic title, you are probably guessing that I'm pretty salty about failing, but I'm not. I write [one of these reflection blog posts][3] each time I fail an interview I care
about (fun fact: both were about the same company, enjoy the bonus round at the end). Now I'm writing one after experiencing a few years at a company that didn't have any coding interviews (fun fact: I almost didn't pass their interview process either).

Now, I'm just as guilty about being on the coding interview bandwagon as everyone else. I've given
probably about a hundred of the same formulaic[^1] interview that I just took. I've passed on most
of those candidates and I would have passed on me. I've also spent hours fixing code that had issues
directly stemming from not using the correct data structure/algorithm that I may not have had to
if we had been better at screening the employees during the interview process.

So which one works better? From my small sample size and general mediocre statistics abilities, 
I'd say they are about the same. What I do know, however, is that for every 1-hour interview where
I evaluated if someone knew their data structures, I could have just taught them. Maybe they did 
know them already and just forgot because they haven't used them recently. Math was one of my strongest
subjects in school but if you asked me right now to take a derivative of something I wouldn't be able to.

For some reason we think that someone needs to be able to whip out the solution to any random
problem that is space and time optimal within 40 minutes. Ok, that's not necessarily fair (more
hyperbole, I know). I'm well aware that it's often said that what's more important is how you solve
the problem, not that you solve the problem, but in practice human biases will work against you
if you don't get close to it working.

If you've stuck around this far, you are probably curious about the question. Honestly, the question
is not interesting at all and I thought it to be perfectly reasonable (and was probably the first of two).
The reason I'm writing about all of this is because I woke up to the epiphany that the problem I failed
was a problem I solved a year ago.

> Q: Assume that you have an infinite stream of data that is coming in from multiple threads in
> an unordered fashion. Write the stream items to the console in order.

My actual problem, which took about a month, was that I needed to ingest all of the changes
from one or more tables in SQL Server as fast as possible. What I chose to do was hook into
the Change Data Capture functionality and simultaneously stream the data from CDC while scanning
the table for snapshots of the data. Now, while each of those go in sequential order, we need to
reconcile the two together depending on which we read first (ie if we get updates before snapshot,
we need to store them until we get the full row data and then send them). On top of that I then built
an in-memory transaction layer sitting on top of SQLite to optimize memory/disk usage to hit the
performance benchmarks I wanted.

This literally checked off every box they were looking for (even the ones I missed) in the interview question.
**Thread safety**: check. 
**Lock primitives**: check.
**Optimal Concurrency**: check.
**Producer-consumer**: check.
**Fault tolerant**: check.
**Hashing**: check.
The code I wrote is ok at best, honestly. The greatest lie we have ever told ourselves is that we want greenfield projects because we won't have to deal with legacy code, but legacy code is just greenfield code that is written under the duress of trying to solve the problem at the same time.

So now, just as when I'm in an interview, I'm at a loss for the answer to the question at hand:
what should we do instead? I honestly don't know but I do feel like we are stuck in a cargo-cult
mentality where we are just doing things becaues that's what the big companies do, and if it works
for them it must be what we need to do. I understand the problems associated with hiring the wrong
people as well, which may well be the actual reason we are rightfully stuck with such fearful/timid
hiring practices.

For now, I'll just keep practicing for interviews until I successfully trick someone into thinking
that I know how to code and then secretely become one of the best employees they have ever had.

---

**Bonus round!** I guess enough time has gone by (11 years!) that I can say that both of my failed
interview posts were from when I applied to StackOverflow. The first time I bombed out just as hard
as this interview. The second time, however, I went through 2 - 3 coding interviews (which I believe
that I passed), 1 interview with the product manager, and then finally an interview with David Fullerton
and Joel Spolsky. I completely bombed the interview with David Fullerton and I'm pretty sure that's
what killed my application (but I can't say one way or another).

That interview, however, is the only interview that actually bothers me to this day. The question, 
which I've heard is a Facebook favorite, was "convert a decimal number to base negative 2". 
This question, which is possible/doable in 40 minutes and more about the problem solving process than anything,
is the dumbest fucking question I have ever had and I don't care what anybody else thinks. Every other
question I've had I've never had problems with because, like I've shown in this post, most everything
you'll actually run into at some point. And yes converting numbers to other things (roman numerals, etc)
is a valid thing, but fuck that question and just waiting for you to eventually arrive at the
little trick to make it work.

Because of my schedule, the interviews were stacked back-to-back, where they normally would not be
and I wouldn't have actually ended up having the interview with Joel, which was so awkward because he
was a little late and so I was just sitting there on a spinning google hangouts waiting while my head
was still trying to process the question while knowing that I had completely bombed that interview
and the next interview didn't matter, not actually knowing if it would happen or not.

No hostility to any of those involved, but to this day I hate that question and it's the only
question I've received that I would actually categorize as "gimicky." RIP StackOverflow Careers.

---

[^1]: 5 minutes on introductions, 40 minutes on the question, 10-15 minutes for questions.

[1]: /posts/2013/04/19/Post-mortem-of-my-failed-interview
[2]: https://www.amazon.com/Cracking-Coding-Interview-Programming-Questions/dp/0984782850
[3]: /posts/2013/05/28/Always-finish
[4]: /posts/2016/02/25/always-learn-something-from-an-interview