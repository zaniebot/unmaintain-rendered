---
number: 1635
title: Performance of YML vs performance of code?
type: issue
state: closed
author: dominikbraun
labels: []
assignees: []
created_at: 2020-01-17T10:10:52Z
updated_at: 2020-01-17T16:12:08Z
url: https://github.com/clap-rs/clap/issues/1635
synced_at: 2026-01-10T01:27:00Z
---

# Performance of YML vs performance of code?

---

_Issue opened by @dominikbraun on 2020-01-17 10:10_

This is just a quick question concerning the performance of parsing the CLI app structure:

How does building the app from a YML file compare to building the app in the source code in terms of performance? I'd assume that loading and parsing the file is slower, but are there some benchmarks showing the differences? Can a large YML file even slow down the CLI app?

Thanks in advance!

---

_Comment by @CreepySkeleton on 2020-01-17 11:41_

**Those are my subjective conclusions based on my personal experience!**

Most certainly the parsing does have it's own performance impact, **but this impact is relative**.

We don't have a benchmark for YAML parsing in this repo so I can't give any determinate numbers, but let's just make a couple of educated guesses.

* The CLI parsing is done *only once per program run*, we can call it O(1). Your program, in turn, is going to process some data, perform some calculations, render something etc... those are tend to be O(n) with respect to the input data. 
* Large YAML files, you say? How many of your yaml files have the size of megabytes? I bet none 😄 . Just so you could feel the scale, it would take ~250 ebook pages (text only). How many of your CLI descriptions take the size of novella?
    
    I'm fairly certain that few kilobytes will be processed in no time [citation needed].

With all that said, even if the YAML parser is slow, the difference will be negligible - and effectively imperceptible - for end user. 

99% of CLI apps have no need to care about it.

I would also like to recommend [this article](https://www.doxsey.net/blog/a-response-to-hello-world) to anyone who likes premature optimization. Optimize something only when your debugger told you so.



---

_Comment by @BurntSushi on 2020-01-17 11:47_

@CreepySkeleton That's a fine analysis. But to add to that, there is a fairly common case where process overhead can be observed, and it's actually one of the reasons why I use clap (because it's fast). Namely, when running processes many times in a short period of time. This can occur in a loop, or perhaps more commonly, with `xargs`.

Definitely agree with your conclusion though! Just wanted to mention a case where startup overhead (even a small amount) can be a factor.

---

_Comment by @CreepySkeleton on 2020-01-17 11:56_

@BurntSushi Good point with `xargs`, I forgot about it!

Just wanted to note here that, even when a program is executed in a loop repeatedly, the CLI parsing is probably the least significant part of it; it's very likely that  process creation + data processing + IO will likely take the most of the time.

---

_Comment by @BurntSushi on 2020-01-17 12:22_

Sometimes. Really depends on what you're doing. It certainly matters for tools like grep, where even spawning a few threads has noticeable overhead in an `xargs` pipeline. Turns out you can do a lot in 10-30ms of overhead!

---

_Comment by @dominikbraun on 2020-01-17 16:12_

@CreepySkeleton Thanks for your opinion - that is pretty much what I've expected. Your O(1) consideration is interesting as well.

I merely wanted to make sure that there isn't a certain risk of getting a performance penalty when using a YML file. At least for most cases. :)

Thanks to @BurntSushi, too!

---

_Closed by @dominikbraun on 2020-01-17 16:12_

---
