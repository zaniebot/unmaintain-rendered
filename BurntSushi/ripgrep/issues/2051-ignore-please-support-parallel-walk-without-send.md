```yaml
number: 2051
title: "ignore: Please support parallel walk without Send"
type: issue
state: open
author: joshtriplett
labels: []
assignees: []
created_at: 2021-11-07T22:56:46Z
updated_at: 2021-11-08T00:59:19Z
url: https://github.com/BurntSushi/ripgrep/issues/2051
synced_at: 2026-01-12T16:13:24Z
```

# ignore: Please support parallel walk without Send

---

_@joshtriplett_

Based on the documentation of WalkParallel, it *seems* like it should be able to construct the function for each thread *on* that thread, such that it doesn't actually need `Send`. However, both the return value of `mkf` and the `ParallelVisitor` trait require `Send`, which makes it difficult to operate on structures that don't support `Send`. I was hoping to construct separate structures for each thread.

It *does* make sense that the `mkf` function and the `ParallelVisitorBuilder` need to be `Send`, to allow sending them to each thread and calling them. I'd just like to avoid needing `Send` on the function and `ParallelVisitor` (respectively) that they produce.

I looked for an open issue about this, and didn't find one.

---

_Comment by @BurntSushi on 2021-11-08 00:59_

I'm not sure when I would have time to look into this, but the first thing I would try is to remove the bound and see if ripgrep still builds. If it does, then this might be okay. If it doesn't, then we'll have something further to investigate.

---
