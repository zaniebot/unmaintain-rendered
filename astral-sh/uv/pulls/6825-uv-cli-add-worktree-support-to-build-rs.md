```yaml
number: 6825
title: "uv-cli: add worktree support to build.rs"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/fix-rebuilds
created_at: 2024-08-29T17:36:32Z
updated_at: 2024-08-29T18:11:52Z
url: https://github.com/astral-sh/uv/pull/6825
synced_at: 2026-01-12T16:07:32Z
```

# uv-cli: add worktree support to build.rs

---

_@BurntSushi_

Previously, we were always asking Cargo to rebuild `uv-cli` if
`.git/HEAD` had changed. But in a worktree, `.git` is a file, not a
directory. And the file contains the path to git's internal worktree
state, which also has its own `HEAD` file. So in the case of a worktree,
we read the file and tell Cargo to watch the worktree-specific `HEAD`
file instead of `.git/head`.

The main thing this fixes is that, previously, in a worktree, `cargo
build` would *always* re-compile `uv` even if nothing changed.

This doesn't impact or fix anything in "typical" clones of uv though.
Only in worktrees.

Closes #6196, Closes #6197 


---

_Review requested from @zanieb by @BurntSushi on 2024-08-29 17:36_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-08-29 17:36_

---

_@charliermarsh approved on 2024-08-29 17:38_

---

_Merged by @BurntSushi on 2024-08-29 18:11_

---

_Closed by @BurntSushi on 2024-08-29 18:11_

---

_Branch deleted on 2024-08-29 18:11_

---
