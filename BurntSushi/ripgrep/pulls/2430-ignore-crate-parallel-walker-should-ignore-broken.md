```yaml
number: 2430
title: "\"ignore\" crate: Parallel walker should ignore broken symlinks"
type: pull_request
state: closed
author: ruffe972
labels: []
assignees: []
draft: true
base: master
head: wip
created_at: 2023-02-25T09:51:43Z
updated_at: 2023-02-26T11:21:28Z
url: https://github.com/BurntSushi/ripgrep/pull/2430
synced_at: 2026-01-12T18:23:14Z
```

# "ignore" crate: Parallel walker should ignore broken symlinks

---

_@ruffe972_

This fixes https://github.com/sharkdp/fd/issues/746. "fd": uses "ignore" crate from this repo. Steps to reproduce:

1. Create parallel walker with "follow links" option enabled.
2. Create broken symlink with the name broken_symlink, for example.
3. Add "broken_symlink" to ignored files.
4. Get walker dir entry results.

Expected: walker doesn't return dir entry result for the link.
Actual:  walker returns error result for the link.

Note that I didn't fix similar bug in single-threaded walker. I'm new to Rust so it's a bit difficult for me, but if it's required for this PR to be merged, I'll fix single-threaded walker too.

---

_Renamed from "Fix broken symlinks" to ""ignore" crate: Parallel walker should ignore broken symlinks" by @ruffe972 on 2023-02-26 11:16_

---

_Closed by @ruffe972 on 2023-02-26 11:21_

---

_Branch deleted on 2023-02-26 11:21_

---
