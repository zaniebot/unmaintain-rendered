```yaml
number: 8360
title: "Add a `uv remove` test where a dependency is repeated"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/add-remove-test
created_at: 2024-10-19T13:19:21Z
updated_at: 2024-10-19T14:33:55Z
url: https://github.com/astral-sh/uv/pull/8360
synced_at: 2026-01-10T12:54:08Z
```

# Add a `uv remove` test where a dependency is repeated

---

_Pull request opened by @zanieb on 2024-10-19 13:19_

_No description provided._

---

_Label `testing` added by @zanieb on 2024-10-19 13:19_

---

_Comment by @zanieb on 2024-10-19 13:23_

cc @j178 — if you're interested, we're removing the sources entry before all the references to the package are gone which seems wrong. We should only remove the source if it's no longer a dependency, right?

---

_Comment by @j178 on 2024-10-19 13:58_

Yes, I agree. I'll take on this.

---

_Merged by @zanieb on 2024-10-19 14:33_

---

_Closed by @zanieb on 2024-10-19 14:33_

---

_Branch deleted on 2024-10-19 14:33_

---
