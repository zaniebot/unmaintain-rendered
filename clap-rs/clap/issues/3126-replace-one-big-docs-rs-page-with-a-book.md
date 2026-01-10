---
number: 3126
title: Replace one big docs.rs page with a book
type: issue
state: closed
author: epage
labels:
  - A-derive
assignees: []
created_at: 2021-12-09T16:17:24Z
updated_at: 2021-12-09T16:40:50Z
url: https://github.com/clap-rs/clap/issues/3126
synced_at: 2026-01-10T01:27:33Z
---

# Replace one big docs.rs page with a book

---

_Issue opened by @epage on 2021-12-09 16:17_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Tuesday Dec 31, 2019 at 04:05 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/319_

----

Right now, `structopt`'s documentation is nothing more than just a one big page on docs.rs. 

While this approach has some advantages, namely:

* One big page makes it easier to search via `Ctrl+F`.
* Code snippets can be tested with simple `cargo test`.

It also has a great number of disadvantages:

* Such a huge page isn't really easy to navigate. `Ctrl+F` will help you only if you know what you need *exactly*, otherwise you are doomed to scroll up and down until you happen to stumble on the right topic.
* Our artificial TOC isn't really helpful since it's placed only at the beginning of the page, once you hit a tittle you have no easy way back, you're going to have to scroll. 
* `rustdoc` does not verify links.
* We can't tweak the theme ("Important" blocks look ugly and don't really attract attention).
* We can't fix typos without publishing a new version (this is arguable).

Thus, I propose to make a book via [`mdbook`](https://github.com/rust-lang/mdBook) and publish it via github pages. Tt addresses all the disadvantages above:

* Built-in search, works *great*.
* Built-in TOC, looks awesome.
* We can verify links via [mdbook-linkcheck](https://github.com/Michael-F-Bryan/mdbook-linkcheck), integration is trivial.
* We can inject any `.css` styles we please.
* We can update it whenever we wish (the question is, should we?).
* Testing is possible via [`skeptic`](https://github.com/budziq/rust-skeptic), integration is pretty easy.

In fact, I already made a [test repo](https://github.com/CreepySkeleton/structopt-book-test.github.io), and published it on [github pages](https://creepyskeleton.github.io/structopt-book-test.github.io/).

What do you think?


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Tuesday Jan 07, 2020 at 20:29 GMT_

----

Having the doc on docs.rs is easier to find, and needed. We must be careful to have a good doc here whatever we do around.

A mdbook can be a plus, but it can't replace completely the doccomments.


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Saturday Jan 11, 2020 at 16:34 GMT_

----

> but it can't replace completely the doccomments.

It can't, and it won't. 

Doc comments are superb at documenting *items*: the `StructOpt` trait, modules, the crate itself. They are so wonderful at this task because they offer crosslinks out of box, you can just look at the function signature, click on some type within it, and, congrats!, you are reading the docs for this type now. 

But the situation is completely different for proc-macros and their attributes in particular. They don't have any builtin links, they have no logical structure and the only way to document them is the plain text with manually crafted links. `rustdoc` doesn't offer anything for proc-macros.

So, every single proc-macro's documentation on docs.rs is doomed to grow and grow and grow, until it's so large that it's basically unnavigatable. 

What I'm proposing is pretty much the same, but logically separated into different pages, which is much easier to both read and navigate. 

If you are concerned that the book would be undiscoverable - well `serde` has the same [summary on docs.rs + book](https://docs.rs/serde) solution; the link to the book is present in the first paragraph. We could render it bold so it's more attractive.


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Saturday Feb 01, 2020 at 10:24 GMT_

----

OK, but if clap v3 will be out in a "short" time, maibe that's better to focus on it.


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Saturday Feb 01, 2020 at 10:42 GMT_

----

Hmm, to focus on releasing clap 3 or making a book?


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Saturday Feb 01, 2020 at 10:58 GMT_

----

To write the book for clap and not structopt.


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Saturday Feb 01, 2020 at 10:59 GMT_

----

Agreed. I'm transferring the issue.


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Saturday Feb 01, 2020 at 11:00 GMT_

----

No, I'm not transferring the issue because I simply can't :( I'll figure it out later.


---

_Comment by @epage on 2021-12-09 16:27_

The new docs are now split between bulder API reference, derive ref, and tutorial.  I'm assuming this resolves this issue.

---

_Closed by @epage on 2021-12-09 16:27_

---

_Label `A-derive` added by @epage on 2021-12-09 16:40_

---
