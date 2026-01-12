```yaml
number: 1987
title: Walk(Parallel) should give some indication of whether any paths were ignored and why
type: issue
state: open
author: tavianator
labels: []
assignees: []
created_at: 2021-09-09T19:50:17Z
updated_at: 2021-09-09T19:50:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1987
synced_at: 2026-01-12T16:13:24Z
```

# Walk(Parallel) should give some indication of whether any paths were ignored and why

---

_@tavianator_

To implement https://github.com/sharkdp/fd/issues/612#issuecomment-896049390, I'd need some way for WalkParallel to report that some files were ignored during the search.  I can think of a few ways to do it.  My suggestion would be to add a new method to `ParallelVisitor` that will be passed any ignored entries (with a default no-op implementation).  If you think that's a good strategy, I can write the patch.

---
