```yaml
number: 1545
title: fix OS detection for Alpine Linux
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/fix-i1427
created_at: 2024-02-16T21:25:10Z
updated_at: 2024-02-16T21:37:23Z
url: https://github.com/astral-sh/uv/pull/1545
synced_at: 2026-01-10T15:33:24Z
```

# fix OS detection for Alpine Linux

---

_Pull request opened by @BurntSushi on 2024-02-16 21:25_

This PR fixes the OS detection for Alpine Linux such that the version
of musl available is correctly determined. The issue boiled down to
a regex that required 2 digits for each version component. But a
valid musl version is 1.2.4, which only has a single digit for each
component.

It's unclear how this was working for musl before this change. My
theory is that our other methods of OS detection were somehow working.

The first commit in this PR cleans up our Linux detection logic and adds
lots of tracing calls to make debugging issues like this easier in the
future. To do so, one can run:

    $ RUST_LOG=trace uv pip install -v whatever

The second commit has the actual fix.

Fixes #1427


---

_Review requested from @charliermarsh by @BurntSushi on 2024-02-16 21:25_

---

_Review requested from @zanieb by @BurntSushi on 2024-02-16 21:25_

---

_@charliermarsh approved on 2024-02-16 21:29_

---

_Merged by @BurntSushi on 2024-02-16 21:37_

---

_Closed by @BurntSushi on 2024-02-16 21:37_

---

_Branch deleted on 2024-02-16 21:37_

---

_Label `bug` added by @BurntSushi on 2024-02-16 21:37_

---
