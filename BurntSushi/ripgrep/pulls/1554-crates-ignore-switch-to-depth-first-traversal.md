```yaml
number: 1554
title: "crates/ignore: switch to depth first traversal"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/debug-leak
created_at: 2020-04-18T15:21:36Z
updated_at: 2020-07-06T07:45:24Z
url: https://github.com/BurntSushi/ripgrep/pull/1554
synced_at: 2026-01-12T18:23:14Z
```

# crates/ignore: switch to depth first traversal

---

_@BurntSushi_

This replaces the use of channels in the parallel directory traversal
with a simple stack. The primary motivation for this change is to reduce
peak memory usage. In particular, when using a channel (which is a
queue), we wind up visiting files in a breadth first fashion. Using a
stack switches us to a depth first traversal. While there are no real
intrinsic differences, depth first traversal generally tends to use less
memory because directory trees are more commonly wide than they are
deep.

In particular, the queue/stack size itself is not the only concern. In
one recent case documented in #1550, a user wanted to search all Rust
crates. The directory structure was shallow but extremely wide, with a
single directory containing all crates. This in turn results is in
descending into each of those directories and building a gitignore
matcher for each (since most crates have `.gitignore` files) before ever
searching a single file. This means that ripgrep has all such matchers
in memory simultaneously, which winds up using quite a bit of memory.

In a depth first traversal, peak memory usage is much lower because
gitignore matches are built and discarded more quickly. In the case of
searching all crates, the peak memory usage decrease is dramatic. On my
system, it shrinks by an order magnitude, from almost 1GB to 50MB. The
decline in peak memory usage is consistent across other use cases as
well, but is typically more modest. For example, searching the Linux
repo has a 50% decrease in peak memory usage and searching the Chromium
repo has a 25% decrease in peak memory usage.

Search times generally remain unchanged, although some ad hoc benchmarks
that I typically run have gotten a bit slower. As far as I can tell,
this appears to be result of scheduling changes. Namely, the depth first
traversal seems to result in searching some very large files towards the
end of the search, which reduces the effectiveness of parallelism and
makes the overall search take longer. This seems to suggest that a stack
isn't optimal. It would instead perhaps be better to prioritize
searching larger files first, but it's not quite clear how to do this
without introducing more overhead (getting the file size for each file
requires a stat call).

Fixes #1550 

cc @bmalehorn who attempted a similar change many moons ago!

---

_Merged by @BurntSushi on 2020-04-18 15:33_

---

_Closed by @BurntSushi on 2020-04-18 15:33_

---

_Branch deleted on 2020-04-18 15:33_

---

_Comment by @sharkdp on 2020-05-24 21:59_

A few days ago, I released a new version of `fd` and `cargo update`d all dependencies without noticing (shame on me) that there was a new patch version of the `ignore` crate, which includes this change. The `ignore` crate is really the heart of the `fd` application, so I usually follow `ignore` releases very closely, but somehow I missed this.

We have two open tickets in `fd` that concern high peak memory usage (https://github.com/sharkdp/fd/issues/378, https://github.com/sharkdp/fd/issues/471), so I'm really excited to see some work on this front (thanks!). I haven't checked if those issues would be resolved by this change, but it could very well be the case.

I'm a bit "concerned" about the change to a depth-first traversal though. Sure, if users of `rg` or `fd` perform a full search, it doesn't really matter (except for the output order, which is non-deterministic anyway - especially if the search is not limited to a single thread). But how about the case where someone is just waiting for "that one" result in a longer search? Wouldn't we expect the breadth first search to result in a better experience?

I found this PR after reading the report of a `fd` user who uses `fd` in combination with `fzf` to interactively search for files. With the previous breadth first search behavior, file system entries close to the start directory would show up very quickly. With the new depth first search, some of these entries appear at the very end of the (long running) search. I know that a lot of `ripgrep` users also use it in combination with `fzf`. Could this change also result in worse behavior for them? How about those users who perform a search for "that single keyword that only appears once somewhere in my home folder"? Isn't that file likely to be located at shallower depths?

Sorry for this long post - I'd be glad to hear your thoughts!

> Search times generally remain unchanged, although some ad hoc benchmarks
> that I typically run have gotten a bit slower. As far as I can tell,
> this appears to be result of scheduling changes.

The same user also reports significant performance regressions in the latest `fd` version that includes this change. I wasn't able to reproduce this myself, but I thought I would mention it.

---

_Comment by @BurntSushi on 2020-05-24 22:07_

@sharkdp Sure, this change is invariably going to make some cases worse. Not really sure if there's much to do about it though. The peak memory usage of breadth first traversal was really not acceptable, and was also significantly slower in some cases.

---

_Comment by @tmccombs on 2020-07-06 07:45_

> The same user also reports significant performance regressions in the latest fd version that includes this change.

I wonder if this has to do with the CPU cache. Specifically, if with breadth-first traversal, the entries for a directory were more likely to be in a nearby cache-line, but with depth-first traversal, the directory data might be flushed from the cache and has to be read from main memory on each iteration.

---
