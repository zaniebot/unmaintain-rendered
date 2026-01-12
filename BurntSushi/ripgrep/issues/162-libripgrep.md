```yaml
number: 162
title: libripgrep
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
  - libripgrep
assignees: []
created_at: 2016-10-11T00:42:59Z
updated_at: 2018-08-20T11:10:20Z
url: https://github.com/BurntSushi/ripgrep/issues/162
synced_at: 2026-01-12T16:13:21Z
```

# libripgrep

---

_@BurntSushi_

This is a tracking issue for the concept known as "libripgrep."

In fact, "libripgrep" probably won't ever be a thing unto itself. Instead, there will be several independent crates that together make up the thing one might think of as "libripgrep." Most of this work is already done. Here's a list:
- [memchr](https://crates.io/crates/memchr) - Fast single byte search.
- [walkdir](https://crates.io/crates/walkdir) - Recursive directory iterator.
- [utf8-ranges](https://crates.io/crates/utf8-ranges) - Generate utf8 automata.
- [regex-syntax](https://crates.io/crates/regex-syntax) - A regex parser (including literal extraction helpers).
- [regex](https://crates.io/crates/regex) - The regex engine itself.
- [globset](https://crates.io/crates/globset) - Regex based glob matching, including glob-set matching.
- [grep](https://crates.io/crates/grep) - Blazing fast line-by-line search. This is where all of the inner literal optimizations happen. (INCOMPLETE.)
- [ignore](https://crates.io/crates/ignore) - Recursively traverse a directory while respecting ignore files and file types.
- parallel directory iterator - Iterate over files in parallel, applying ignore rules. (**This is now done and part of the `ignore` crate.**)

`grep` is incomplete because it exposes only the most basic line-by-line
search. For example, it doesn't handle inverted matching, context handling,
line counts or anything other than "which line matches this regex." Still,
there is considerable regex-specific work in here around literal matching and
other optimizations. The rest of the aforementioned work is done in `ripgrep`
proper. Ideally, all of that should get moved out into the `grep` crate. This
is predominantly a _design_ task, because searching is inextricably tied to
printing, which means the API for all of these features is quite complex.

`gitignore` is a reasonably light wrapper around the `globset` crate. The hope
is that a future `hgignore` crate would be too. Nevertheless, the semantics are
tricky enough that it's worth it to put it into its own crate.

A parallel directory iterator is something I want because so much time is spent
in handling all of the ignore rules. If we can apply parallelism to that, I
kind of expect that we'll get a nice win. Whether a parallel directory iterator
can be design out-of-tree or not still isn't clear. In particular, it's not
clear how coupled this needs to be with respecting ignore rules.

---

In any case, I don't really have the bandwidth to do the design work required to manage contributions from others toward this goal quite yet. I kind of think they're big enough that I would like to at least do the initial work on them.


---

_Label `enhancement` added by @BurntSushi on 2016-10-11 00:42_

---

_Assigned to @BurntSushi by @BurntSushi on 2016-10-11 00:43_

---

_Comment by @BurntSushi on 2016-10-11 00:47_

With that said, if someone wanted to get started on a `sed` replacement, it'd be faster to fork `ripgrep` and try to start from there by ripping out pieces you no longer need and adding what you do need. I'd still recommend waiting for more progress on "libripgrep" though. :-)


---

_Comment by @BurntSushi on 2016-10-12 00:30_

I'm currently working on moving ignore pattern handling out into a separate crate, with an eye toward designing an abstraction that is amenable to parallel directory iteration.


---

_Comment by @BurntSushi on 2016-10-30 02:38_

Update on this: The [`ignore`](https://github.com/BurntSushi/ripgrep/tree/master/ignore) crate is now a thing. We don't have a parallel iterator yet, but I designed a persistent data structure that should be amenable to it. Other than that, the only other big thing left in ripgrep core is the search code.


---

_Comment by @BurntSushi on 2016-11-04 00:44_

I finally have an implementation of a parallel recursive directory iterator and it is a beautiful sight to see. On the Chromium repository, it can actually scan the entire repo while respecting ignores faster than GNU find can print every path(!).

No data races. No unsafe. Rust is awesome.


---

_Comment by @BurntSushi on 2016-11-05 01:46_

Utterly massive improvements. On 20GB of various git checkouts:

```
[andrew@Cheetah clones] du -csh
20G     .
20G     total
[andrew@Cheetah clones] time ag Openbox | wc -l
5

real    0m4.931s
user    0m5.863s
sys     0m12.790s
[andrew@Cheetah clones] time rg-master -n Openbox | wc -l
5

real    0m2.791s
user    0m19.297s
sys     0m2.760s
[andrew@Cheetah clones] time rg -n Openbox | wc -l
5

real    0m0.903s
user    0m8.203s
sys     0m4.880s
```

We even benefit from the parallel iterator when we don't respect `.gitignore`:

```
[andrew@Cheetah clones] time rg-master -nu Openbox | wc -l
5

real    0m0.865s
user    0m3.480s
sys     0m3.093s
[andrew@Cheetah clones] time rg -nu Openbox | wc -l
5

real    0m0.631s
user    0m4.123s
sys     0m5.057s
```

And this is why:

```
[andrew@Cheetah clones] time rg-master --files | wc -l
631778

real    0m2.522s
user    0m2.043s
sys     0m0.563s
[andrew@Cheetah clones] time rg --files | wc -l
631778

real    0m0.552s
user    0m5.157s
sys     0m1.083s
[andrew@Cheetah clones] time rg-master -u --files | wc -l
611552

real    0m0.797s
user    0m0.397s
sys     0m0.460s
[andrew@Cheetah clones] time rg -u --files | wc -l
611552

real    0m0.377s
user    0m1.447s
sys     0m0.903s
```


---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Label `libripgrep` added by @BurntSushi on 2017-01-11 03:04_

---

_Comment by @acornejo on 2017-01-20 17:00_

As a potential use of the hypothetical grep crate, wondering if I should wait or fork-and-ripout as you had suggested.

I do need to keep track of line numbers and inverted matching, I am hoping the naive way of doing this is good enough (or perhaps ripgrep employs a sophisticated datastructure for these?)

---

_Comment by @BurntSushi on 2017-01-20 18:12_

Note that there is already a [`grep`](https://docs.rs/grep/0.1.5/grep/) crate, but I intend to do a near complete overhaul of it when I move the (rest of the) search code out of ripgrep into that crate.

> I do need to keep track of line numbers and inverted matching, I am hoping the naive way of doing this is good enough (or perhaps ripgrep employs a sophisticated datastructure for these?)

Not really. These things aren't really stored anywhere, they're just computed as they are needed to be printed for performance reasons.

There is a lot of complexity in ripgrep's `search_stream` module for handling inverted matches, line numbers, contexts and a menagerie of other features. My plan to expose this in the `grep` crate is to define a trait with a visitor like pattern that the searcher calls. There can then be implementations of that trait that, say, print the results directly to an output or there could be "sink" implementations that collect the results as structured data in memory.

@acornejo What problem are you trying to solve? Without telling me that, I don't really know how to give you any advice.

---

_Comment by @acornejo on 2017-01-20 18:20_

Let me try to describe the problem I want solved (and btw, if you know of a utility that already does this, please let me know since if I can avoid writing it it will be a big plus ;)

At work I often have to examine log files which are millions of lines long, and usually my workflow looks like this:

```
cat somelog | grep module
# read and examine output
cat somelog | grep module2
# read and examine output
cat somelog | grep module2 | grep -v feature1
# read and examine output
cat somelog | grep module2 | grep -v feature1 | grep -v feature2
# readn and examine output
cat somelog | grep module2 | grep -v feature1 | grep -v feature2 | grep keyword
# etc...
```

This is a lot more tedious/slower than it could be. Hence, I now want to write an interactive ncurses program, lets call it `loggrep`

Then I would do:

`loggrep somelog`

There I am envisioning to be able to interactively add more greps to the pipeline, remove them from the pipeline, and see how the results are being updated interactively as I type. I also want to support a few features which are not grep related (something like, cut, sort, and uniq, which are often in my command pipeline too).



---

_Comment by @BurntSushi on 2017-01-20 18:27_

@acornejo I see. That definitely seems like something that could be built on top of what I think the `grep` crate will look like.

My feeling though is that you might get better mileage out of reusing existing command line tools. It sounds like your key problem is that you're rerunning your entire pipeline for each invocation. But if you saved your results to an intermediate file (which I imagine you'd have to do anyway in your `loggrep` tool) and continued your pipeline on that, then that seems like it could work well. Maybe there's a simple wrapper shell script you could write instead?

---

_Comment by @acornejo on 2017-01-20 18:40_

yea, i have a nasty bash script that speeds up the aforementioned workflow, but this is something that I use frequently enough that I feel that having something more ergonomic would pay off.

I guess I'll take a look at the current grep crate when I start this and take it from there.

---

_Comment by @theduke on 2017-09-18 09:58_

@BurntSushi so there is no plan to provide an easy to use `ripgrep` library?

I think that could be quite useful!

```rust
extern crate ripgrep;

use ripgrep::{run, Options};
...

run("some/path", Options{...}, |result| {
  // Do something with a match...
})?;
```

---

_Comment by @BurntSushi on 2017-09-18 10:19_

@theduke Could you explain why you think [this](https://github.com/BurntSushi/ripgrep/milestone/1) doesn't qualify as a plan?

---

_Comment by @theduke on 2017-09-18 10:33_

Maybe I understood the second sentence in your root comment on this issue:

> In fact, "libripgrep" probably won't ever be a thing unto itself. 

I interpreted that as "there won't be an actual library, just all the functionality provided by different crates that you have to tie together yourself.

---

_Comment by @BurntSushi on 2017-09-18 10:44_

@theduke Yes, that's right. You'll have to tie it together. There's currently no plan to move *all* of ripgrep into a library. I'm not saying it won't happen or that it's not useful, it's just not my goal right now.

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---
