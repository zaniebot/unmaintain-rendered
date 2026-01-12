```yaml
number: 636
title: Order of search results is not predictable
type: issue
state: closed
author: kumar303
labels:
  - duplicate
assignees: []
created_at: 2017-10-11T21:13:58Z
updated_at: 2017-10-11T21:18:13Z
url: https://github.com/BurntSushi/ripgrep/issues/636
synced_at: 2026-01-12T16:13:22Z
```

# Order of search results is not predictable

---

_@kumar303_

If I re-run the same search on the same codebase, sometimes the results are returned in a different order. 

For example, this makes it hard to do a refactoring where I'm renaming a function and I need to re-run `rg` after fixing each file -- i.e. I can lose my place in the sequence.

```
$ rg --version
ripgrep 0.6.0
-AVX -SIMD
```

---

_Comment by @BurntSushi on 2017-10-11 21:17_

Duplicate of #152 

Summary: Technically, this bug exists in any recursive directory searchers that doesn't explicitly sort, but if traversal is single threaded, then in practice, you get deterministic results (but it's not guaranteed). However, ripgrep parallelizes traversal itself, which makes the output far less deterministic than it would be otherwise.

TL;DR - Pick one: speed or determinism. Speed is the default. If you want determinism, then use the `--sort-files` flag.

---

_Label `duplicate` added by @BurntSushi on 2017-10-11 21:17_

---

_Closed by @BurntSushi on 2017-10-11 21:17_

---
