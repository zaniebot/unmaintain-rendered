```yaml
number: 15817
title: Include SHA when listing lockfile changes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/gs
created_at: 2025-09-12T16:57:26Z
updated_at: 2025-09-12T17:57:59Z
url: https://github.com/astral-sh/uv/pull/15817
synced_at: 2026-01-10T06:36:15Z
```

# Include SHA when listing lockfile changes

---

_Pull request opened by @charliermarsh on 2025-09-12 16:57_

## Summary

Right now, we only list changes if the _version_ differs. This PR takes the SHA into account. We may want to list changes to _any_ sources, but that gets more complicated (e.g., if the user swaps the index URL, we'd have to show _all_ changes to the index URL).

Closes #15810.


---

_Label `bug` added by @charliermarsh on 2025-09-12 16:57_

---

_Marked ready for review by @charliermarsh on 2025-09-12 16:57_

---

_Comment by @charliermarsh on 2025-09-12 17:11_

(I'll make a separate change to print a line if the lockfile changed but in a way that didn't involve version changes.)

---

_@zanieb approved on 2025-09-12 17:18_

---

_Merged by @charliermarsh on 2025-09-12 17:57_

---

_Closed by @charliermarsh on 2025-09-12 17:57_

---

_Branch deleted on 2025-09-12 17:57_

---
