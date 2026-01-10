```yaml
number: 2932
title: Skip scenario tests on Windows
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/windows-scenarios
created_at: 2024-04-09T14:22:20Z
updated_at: 2024-04-12T06:56:42Z
url: https://github.com/astral-sh/uv/pull/2932
synced_at: 2026-01-10T14:43:31Z
```

# Skip scenario tests on Windows

---

_Pull request opened by @zanieb on 2024-04-09 14:22_

These tests are about resolver correctness, which should not be platform dependent and Windows CI is horribly slow.


---

_Label `internal` added by @zanieb on 2024-04-09 14:22_

---

_Label `testing` added by @zanieb on 2024-04-09 14:22_

---

_@charliermarsh approved on 2024-04-09 14:22_

---

_Marked ready for review by @zanieb on 2024-04-09 14:27_

---

_Comment by @zanieb on 2024-04-09 14:33_

Meh such marginal impact.

---

_Comment by @zanieb on 2024-04-09 14:57_

Impossibly, it's slower. Going to merge and we can revert later if there are problems — these runners are so inconsistent.

---

_Merged by @zanieb on 2024-04-09 14:57_

---

_Closed by @zanieb on 2024-04-09 14:57_

---

_Branch deleted on 2024-04-09 14:57_

---

_Comment by @konstin on 2024-04-12 06:56_

With the perf issue fixed in packse, do we want to revert?

---
