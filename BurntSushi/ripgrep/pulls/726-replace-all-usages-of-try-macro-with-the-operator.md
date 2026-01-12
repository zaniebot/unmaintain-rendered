```yaml
number: 726
title: "Replace all usages of try! macro with the ? operator"
type: pull_request
state: merged
author: balajisivaraman
labels: []
assignees: []
merged: true
base: master
head: replace_try_with_qmark
created_at: 2018-01-01T09:14:34Z
updated_at: 2018-01-01T14:34:55Z
url: https://github.com/BurntSushi/ripgrep/pull/726
synced_at: 2026-01-12T18:23:13Z
```

# Replace all usages of try! macro with the ? operator

---

_@balajisivaraman_

So as I was learning and going through the codebase, I came across `try!` a lot. Thinking it was a new macro/syntax I had to learn, I searched the Internet for it only to [learn](https://doc.rust-lang.org/std/macro.try.html) that it was an earlier alternative to the newer `?` operator, which should be preferred nowadays.

As a result of this, I have changed all usages of the `try!` macro to the newer `?` operator in the codebase.

My reasoning is as follows:
- The latest edition (2nd) of the Rust Programming language book teaches newcomers about the `?` as the shortcut for [propagating errors](https://doc.rust-lang.org/book/second-edition/ch09-02-recoverable-errors-with-result.html#a-shortcut-for-propagating-errors-).
- I also find that `?` is easier to parse and read than `try!` and is much less of an overhead on my brain. (Note: Subject to personal taste.)

I did not want to create a new issue for this since I assume we only want to do that for actual issues with RipGrep and not changes we want to make to the codebase. So I went ahead and created a PR instead. 

Please feel free to close this PR if you all feel otherwise and are OK with letting `try!` remain as such. Thanks!

---

_Merged by @BurntSushi on 2018-01-01 14:22_

---

_Closed by @BurntSushi on 2018-01-01 14:22_

---

_Comment by @BurntSushi on 2018-01-01 14:23_

Awesome, thanks! I did indeed want to do this change at some point. For some history, ripgrep was written before `?` was stabilized (or somewhere around then). Even after `?` was stabilized, I wanted to continue supporting older versions of Rust. But enough time has passed, and ripgrep's minimum version is well above Rust 1.13 now anyway.

There are dependencies of ripgrep (like the regex crate) that still aim to support Rust 1.12, and therefore can't use `?` yet.

---

_Branch deleted on 2018-01-01 14:34_

---
