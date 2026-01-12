```yaml
number: 2431
title: "\"ignore\" crate: Parallel walker should ignore broken symlinks"
type: pull_request
state: closed
author: ruffe972
labels: []
assignees: []
base: master
head: symlinks
created_at: 2023-02-26T11:20:57Z
updated_at: 2023-07-07T18:18:09Z
url: https://github.com/BurntSushi/ripgrep/pull/2431
synced_at: 2026-01-12T18:23:14Z
```

# "ignore" crate: Parallel walker should ignore broken symlinks

---

_@ruffe972_

This fixes https://github.com/sharkdp/fd/issues/746. "fd": uses "ignore" crate from this repo. Steps to reproduce:

- Create parallel walker with "follow links" option enabled.
- Create broken symlink with the name broken_symlink, for example.
- Add "broken_symlink" to ignored files.
- Get walker dir entry results.

Expected: walker doesn't return dir entry result for the link.
Actual: walker returns error result for the link.

Note that I didn't fix similar bug in single-threaded walker. I'm new to Rust so it's a bit difficult for me, but if it's required for this PR to be merged, I'll fix single-threaded walker too.

---

_Marked ready for review by @ruffe972 on 2023-02-26 11:22_

---

_Comment by @BurntSushi on 2023-07-07 18:18_

I've opened #2552 for this. I'm not sure this is the right way to fix this issue, and the fact that the single threaded case isn't fixed means I don't want to bring this in as-is. I looked and fixing the single threaded case appears tricky.

I'm going to close this for now in favor of tracking #2552 as a bug. Since I don't consider this to be a high priority bug, I'm going to defer fixing this until I've had a chance to overhaul `ignore`'s internals.

---

_Closed by @BurntSushi on 2023-07-07 18:18_

---
