```yaml
number: 310
title: "Don't search stdout redirected file."
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: fix-286
created_at: 2017-01-09T02:46:22Z
updated_at: 2017-01-09T21:12:12Z
url: https://github.com/BurntSushi/ripgrep/pull/310
synced_at: 2026-01-12T18:23:12Z
```

# Don't search stdout redirected file.

---

_@BurntSushi_

When running ripgrep like this:

    rg foo > output

we must be careful not to search `output` since ripgrep is actively writing
to it. Searching it can cause massive blowups where the file grows without
bound.

While this is conceptually easy to fix (check the inode of the redirection
and the inode of the file you're about to search), there are a few problems
with it.

First, inodes are a Unix thing, so we need a Windows specific solution to
this as well. To resolve this concern, I created a new crate, `same-file`,
which provides a cross platform abstraction.

Second, stat'ing every file is costly. This is not avoidable on Windows,
but on Unix, we can get the inode number directly from directory traversal.
However, this information wasn't exposed, but now it is (through both the
ignore and walkdir crates).

Fixes #286

---

_Comment by @BurntSushi on 2017-01-09 02:46_

The new crate, `same-file`, can be found here: https://docs.rs/same-file

cc @retep998

---

_Merged by @BurntSushi on 2017-01-09 21:12_

---

_Closed by @BurntSushi on 2017-01-09 21:12_

---

_Branch deleted on 2017-01-09 21:12_

---
