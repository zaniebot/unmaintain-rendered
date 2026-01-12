```yaml
number: 10908
title: Allow fallback to Python download on non-critical discovery errors
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/find-download
created_at: 2025-01-23T18:10:30Z
updated_at: 2025-01-24T10:04:10Z
url: https://github.com/astral-sh/uv/pull/10908
synced_at: 2026-01-12T16:09:34Z
```

# Allow fallback to Python download on non-critical discovery errors

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/10898

In #10716, I broke fallback to downloading Python versions by throwing a different error kind.

---

_Label `bug` added by @zanieb on 2025-01-23 18:10_

---

_Renamed from "zb/find download" to "Allow fallback to Python download on non-critical discovery errors" by @zanieb on 2025-01-23 18:10_

---

_@zanieb reviewed on 2025-01-23 18:12_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:112 on 2025-01-23 18:12_

This is the key change. The rest is just refactoring so we can match on multiple error types cleanly.

---

_Comment by @zanieb on 2025-01-23 18:12_

I want to add test coverage, but reviews are welcome — this is important to get out today.

---

_Review requested from @charliermarsh by @zanieb on 2025-01-23 18:12_

---

_Review requested from @Gankra by @zanieb on 2025-01-23 18:12_

---

_Marked ready for review by @zanieb on 2025-01-23 18:13_

---

_@zanieb reviewed on 2025-01-23 18:14_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:98 on 2025-01-23 18:14_

#10716 added a throw of `Error::Discovery` instead of a `PythonNotFound` error (which is mapped to `MissingPython` transparently here) which means we no longer fallback.

Ideally we'd implement #10716 higher up, like here? but couldn't think of a clean way to do that. 

---

_@Gankra approved on 2025-01-23 18:41_

---

_@charliermarsh approved on 2025-01-23 19:15_

---

_Merged by @zanieb on 2025-01-23 22:37_

---

_Closed by @zanieb on 2025-01-23 22:37_

---

_Branch deleted on 2025-01-23 22:37_

---

_Comment by @ceejatec on 2025-01-24 01:03_

Thanks for this! We hit this bug just today, and I was literally in the process of writing up a bug report ten minutes ago and couldn't figure out how to reproduce it. Turns out we hit the bug with `uv` 0.5.23 an hour ago, but when I was working on a reproducer it got the fresh-from-the-oven `uv` 0.5.24.

---

_Comment by @zanieb on 2025-01-24 01:48_

That's great to hear! I figured someone else would hit it quickly 😬 

---

_Comment by @jramcast on 2025-01-24 10:04_

Thank you for the quick fix! All is working nicely now.

---
