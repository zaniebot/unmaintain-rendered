```yaml
number: 11075
title: "uv-resolver: fix conflicting extra bug during `uv sync`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/fix-double-torch
created_at: 2025-01-29T18:36:36Z
updated_at: 2025-01-29T22:21:13Z
url: https://github.com/astral-sh/uv/pull/11075
synced_at: 2026-01-12T16:09:39Z
```

# uv-resolver: fix conflicting extra bug during `uv sync`

---

_@BurntSushi_

In #10875, I relaxed the error checking during resolution to permit
dependencies like `foo[x1]`, where `x1` was defined to be conflicting.
In exchange, the error was, roughly speaking, moved to installation
time. This was achieved by looking at the full set of enabled extras
and checking whether any conflicts occurred. If so, an error was
reported. This ends up being more expressive and permits more valid
configurations.

However, in so doing, there was a bug in how the accumulated extras
were being passed to conflict marker evaluation. Namely, we weren't
accounting for the fact that if `foo[x1]` was enabled, then that fact
should be carried through to all conflict marker evaluations. This is
because some of those will use things like `extra != 'x1'` to indicate
that it should only be included if an extra *isn't* enabled.

In #10985, this manifested with PyTorch where `torch==2.4.1` and
`torch==2.4.1+cpu` were being installed simultaneously. Namely, the
choice to install `torch==2.4.1` was not taking into account that
the `cpu` extra has been enabled. If it did, then it's conflict
marker would evaluate to `false`. Since it didn't, and since
`torch==2.4.1+cpu` was also being included, we ended up installing both
versions.

The approach I took in this PR was to add a second breadth first
traversal (which comes first) over the dependency tree to accumulate all
of the activated extras. Then, only in the second traversal do we
actually build up the resolution graph.

Unfortunately, I have no automatic regression test to include here. The
regression test we _ought_ to include involves `torch`. And while we are
generally find to use those in tests that only generate a lock file, the
regression test here actually requires running installation. And
downloading and installing `torch` in tests is bad juju. So adding a
regression test for this is blocked on better infrastructure for PyTorch
tests. With that said, I did manually verify that the test case in #10985
no longer installs multiple versions of `torch`.

Fixes #10985


---

_Label `bug` added by @BurntSushi on 2025-01-29 18:36_

---

_Review requested from @charliermarsh by @BurntSushi on 2025-01-29 18:55_

---

_@charliermarsh approved on 2025-01-29 20:32_

---

_Merged by @BurntSushi on 2025-01-29 22:21_

---

_Closed by @BurntSushi on 2025-01-29 22:21_

---

_Branch deleted on 2025-01-29 22:21_

---
