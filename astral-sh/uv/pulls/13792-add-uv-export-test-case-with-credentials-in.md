```yaml
number: 13792
title: "Add `uv export` test case with credentials in `pyproject.toml`"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/export-git-token
created_at: 2025-06-02T22:24:20Z
updated_at: 2025-06-03T12:47:06Z
url: https://github.com/astral-sh/uv/pull/13792
synced_at: 2026-01-10T11:10:42Z
```

# Add `uv export` test case with credentials in `pyproject.toml`

---

_Pull request opened by @zanieb on 2025-06-02 22:24_

Testing reported case in https://github.com/astral-sh/uv/issues/13791

I cherry-picked this commit onto 0.7.0, and saw no change.

---

_Label `testing` added by @zanieb on 2025-06-02 22:24_

---

_Marked ready for review by @zanieb on 2025-06-02 22:31_

---

_Review requested from @charliermarsh by @zanieb on 2025-06-02 23:23_

---

_Assigned to @oconnor663 by @zanieb on 2025-06-02 23:24_

---

_Review requested from @oconnor663 by @zanieb on 2025-06-02 23:24_

---

_Comment by @konstin on 2025-06-03 09:49_

Is it different with `git+ssh` over `git+https`?

---

_Comment by @zanieb on 2025-06-03 12:46_

Apparently that's the issue, yeah. We should still merge this.


---

_Merged by @zanieb on 2025-06-03 12:47_

---

_Closed by @zanieb on 2025-06-03 12:47_

---

_Branch deleted on 2025-06-03 12:47_

---
