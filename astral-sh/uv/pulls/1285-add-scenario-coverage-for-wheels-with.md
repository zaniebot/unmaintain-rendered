```yaml
number: 1285
title: Add scenario coverage for wheels with incompatible ABI and Python tags
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/incompatible-tag-other
created_at: 2024-02-12T18:39:18Z
updated_at: 2024-02-13T04:14:39Z
url: https://github.com/astral-sh/uv/pull/1285
synced_at: 2026-01-12T16:04:33Z
```

# Add scenario coverage for wheels with incompatible ABI and Python tags

---

_@zanieb_

We use

- An arbitrary ABI hash: `MMMMMM` (six base64 characters)
- An unlikely Jython27 Python tag

For cases that are valid but are never going to be available during tests.

See https://github.com/zanieb/packse/pull/109

---

_Label `testing` added by @zanieb on 2024-02-12 19:16_

---

_Review requested from @konstin by @zanieb on 2024-02-12 19:25_

---

_@charliermarsh approved on 2024-02-13 03:17_

---

_Merged by @zanieb on 2024-02-13 04:14_

---

_Closed by @zanieb on 2024-02-13 04:14_

---

_Branch deleted on 2024-02-13 04:14_

---
