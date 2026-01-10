```yaml
number: 8462
title: Add basic testing of managed Python installs
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/test-managed
created_at: 2024-10-22T14:58:32Z
updated_at: 2024-10-24T16:18:22Z
url: https://github.com/astral-sh/uv/pull/8462
synced_at: 2026-01-10T12:54:10Z
```

# Add basic testing of managed Python installs

---

_Pull request opened by @zanieb on 2024-10-22 14:58_

With a change like https://github.com/astral-sh/uv/pull/8458, we really need tests for these.

I'm just going to take the possible performance hit of these slow tests and deal with optimizing them separately.

---

_Label `testing` added by @zanieb on 2024-10-22 14:58_

---

_Marked ready for review by @zanieb on 2024-10-22 17:02_

---

_Review requested from @charliermarsh by @zanieb on 2024-10-22 19:00_

---

_Comment by @zanieb on 2024-10-22 19:02_

For context, these take ~7s on my machine and the free-threaded one makes the slow test list in Linux CI at 22s. I believe that one is slow because there's not an "install only" variant.



---

_@charliermarsh approved on 2024-10-22 23:54_

Trust you on this one.

---

_Merged by @zanieb on 2024-10-24 16:18_

---

_Closed by @zanieb on 2024-10-24 16:18_

---

_Branch deleted on 2024-10-24 16:18_

---
