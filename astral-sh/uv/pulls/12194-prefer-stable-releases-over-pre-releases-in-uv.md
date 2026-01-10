```yaml
number: 12194
title: "Prefer stable releases over pre-releases in `uv python install`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/install-pre
created_at: 2025-03-15T20:43:48Z
updated_at: 2025-04-21T21:16:08Z
url: https://github.com/astral-sh/uv/pull/12194
synced_at: 2026-01-10T11:10:39Z
```

# Prefer stable releases over pre-releases in `uv python install`

---

_Pull request opened by @zanieb on 2025-03-15 20:43_

e.g., `uv python install 3` should not install the 3.14 alpha

Closes #12184 

---

_Marked ready for review by @zanieb on 2025-03-16 14:14_

---

_Comment by @zanieb on 2025-03-16 14:14_

I'll make sure there's test coverage for the discovery to ensure there are no regressions before merging.

---

_@charliermarsh approved on 2025-03-16 17:39_

Nice, thanks.

---

_Comment by @zanieb on 2025-03-18 18:17_

This revealed some confusing behavior where we consider any Python pre-releases on the search path as "opt-in". There shouldn't be regressions here though.

---

_Merged by @zanieb on 2025-04-21 21:16_

---

_Closed by @zanieb on 2025-04-21 21:16_

---

_Branch deleted on 2025-04-21 21:16_

---
