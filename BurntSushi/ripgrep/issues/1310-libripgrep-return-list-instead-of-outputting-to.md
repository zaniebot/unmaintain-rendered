```yaml
number: 1310
title: Libripgrep return list instead of outputting to terminal or returning JSON
type: issue
state: closed
author: mrageh
labels:
  - question
assignees: []
created_at: 2019-06-22T13:48:29Z
updated_at: 2020-05-08T11:39:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1310
synced_at: 2026-01-12T16:13:23Z
```

# Libripgrep return list instead of outputting to terminal or returning JSON

---

_@mrageh_

Thanks for creating ripgrep and extracting libripgrep.

#### What version of ripgrep are you using?
https://github.com/mrageh/simplegrep

#### How did you install ripgrep?
I'm using libripgrep

#### What operating system are you using ripgrep on?
Mac

#### Question

I'm new to Rust and I'm trying to build a toy project that searches through directories and then instead of outputting the result to the terminal or returning JSON. It gives me back a simple list 
that matches my certain criteria, where I can continue modifying it in Rust. 

https://github.com/mrageh/simplegrep/blob/master/src/main.rs#L55

The `SummaryBuilder` returns the exact results I want but then when I call `build` on it and pass it the `cli::stdout` it prints to the terminal. Which is expected behavior. 
But I'm wondering if I could pass something else to `build` like a vector and return a populated list from the `SummaryBuilder`.

---

_Comment by @BurntSushi on 2019-06-24 11:19_

@mrageh [`SummaryBuilder::build`](https://docs.rs/grep-printer/0.1.2/grep_printer/struct.SummaryBuilder.html#method.build) requires a `WriteColor`. You can't pass something else to it. Indeed, the entire point of the `grep-printer` crate is specifically to craft an output format that is readable by end users in a style they are familiar with. If you want to handle matches as structured data, then the `grep-printer` crate is the wrong tool for that.

You are on the right track with respect to "giving something else" to `build`. That is, this is the code you'll want to change: https://github.com/mrageh/simplegrep/blob/223bf40a3507785b90529180d0846367a08cba77/src/main.rs#L71-L75

In particular, that corresponds to [`Searcher::search_path`](https://docs.rs/grep-searcher/0.1.4/grep_searcher/struct.Searcher.html#method.search_path), which a matcher, a file path to search and [something that implements `Sink`](https://docs.rs/grep-searcher/0.1.4/grep_searcher/trait.Sink.html). So all you need to do is create a new type and implement the `Sink` trait. Then pass a value of that type to `search_path` and get rid of `grep_printer` entirely.

Note that there are some pre-built types that implement `Sink` for you, which you might find convenient instead of defining your own types: https://docs.rs/grep-searcher/0.1.4/grep_searcher/sinks/index.html

There's a short example using the `sinks::UTF8` sink here: https://docs.rs/grep-searcher/0.1.4/grep_searcher/index.html#example

---

_Label `question` added by @BurntSushi on 2019-06-24 11:19_

---

_Comment by @swr1bm86 on 2019-09-29 05:41_

@BurntSushi your comment here really saved my day!

---

_Closed by @BurntSushi on 2020-05-08 11:39_

---
