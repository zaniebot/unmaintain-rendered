```yaml
number: 4902
title: "uv-resolver: partially revert Requires-Python version narrowing"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/revert-narrow
created_at: 2024-07-08T16:44:06Z
updated_at: 2024-07-08T16:57:00Z
url: https://github.com/astral-sh/uv/pull/4902
synced_at: 2026-01-10T13:42:52Z
```

# uv-resolver: partially revert Requires-Python version narrowing

---

_Pull request opened by @BurntSushi on 2024-07-08 16:44_

The PR #4707 introduced the notion of "version narrowing," where a
Requires-Python constraint was _possibly_ narrowed whenever the
universal resolver created a fork. The version narrowing would occur
when the fork was a result of a marker expression on `python_version`
that is *stricter* than the configured `Requires-Python` (via, say,
`pyproject.toml`).

The crucial conceptual change made by #4707 is therefore that
`Requires-Python` is no longer an invariant configuration of resolution,
but rather a mutable constraint that can vary from fork to fork. This in
turn can result in some cases, such as in #4885, where different
versions of dependencies are selected. We aren't sure whether we can fix
those or not, with version narrowing, so for now, we do this revert to
restore the previous behavior and we'll try to address the version
narrowing some other time.

This also adds the case from #4885 as a regression test, ensuring that
we don't break that in the future. I confirmed that with version
narrowing, this test outputs duplicate distributions. Without narrowing,
there are no duplicates.

Ref #4707, Fixes #4885


---

_Review requested from @charliermarsh by @BurntSushi on 2024-07-08 16:44_

---

_Review requested from @zanieb by @BurntSushi on 2024-07-08 16:44_

---

_@charliermarsh approved on 2024-07-08 16:45_

---

_Label `bug` added by @zanieb on 2024-07-08 16:50_

---

_Comment by @charliermarsh on 2024-07-08 16:51_

Do you mind reopening the linked issue too that was closed by the PR?

---

_Merged by @BurntSushi on 2024-07-08 16:56_

---

_Closed by @BurntSushi on 2024-07-08 16:56_

---

_Branch deleted on 2024-07-08 16:57_

---
