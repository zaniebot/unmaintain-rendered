```yaml
number: 17078
title: "Fix handling of `extra != ...` markers"
type: pull_request
state: closed
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: zb/marker-inequal
created_at: 2025-12-10T18:46:09Z
updated_at: 2025-12-13T19:21:51Z
url: https://github.com/astral-sh/uv/pull/17078
synced_at: 2026-01-10T05:49:14Z
```

# Fix handling of `extra != ...` markers

---

_Pull request opened by @zanieb on 2025-12-10 18:46_

This fix by Claude feels extremely dubious but it fixed a trivial example.

Investigating #17065 

---

_Label `bug` added by @zanieb on 2025-12-10 18:46_

---

_Comment by @konstin on 2025-12-10 19:07_

There's an assumption in the resolver that extras are additive, otherwise adding a package could remove a dependency, which breaks our model of insatisfiability. That may or may not be the case for the bug report, but it's the reason why we don't support the general case of negative extras.

---

_Comment by @zanieb on 2025-12-10 19:12_

Yeah that makes sense. I believe this is the case in the bug report.

---

_Closed by @zanieb on 2025-12-13 19:21_

---
