```yaml
number: 3678
title: "Allow specification of additional requirements in `uv tool run`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/tool-run-with
created_at: 2024-05-20T23:06:53Z
updated_at: 2024-05-23T01:59:05Z
url: https://github.com/astral-sh/uv/pull/3678
synced_at: 2026-01-10T14:32:20Z
```

# Allow specification of additional requirements in `uv tool run`

---

_Pull request opened by @zanieb on 2024-05-20 23:06_

Allows requesting additional transitive dependencies when running a tool.

e.g. `uv tool run -v --with anyio ruff check example.py` (Why would you want anyio with ruff? Who knows 😄)

The motivation for doing this now is that I think the first implementation of `uv tool install` might just shim into `uv tool run` with pinned dependencies? Regardless this is something we need in the long run and is a trivial addition right now.



---

_Label `preview` added by @zanieb on 2024-05-20 23:06_

---

_Marked ready for review by @zanieb on 2024-05-21 15:24_

---

_@charliermarsh approved on 2024-05-23 00:10_

---

_Merged by @zanieb on 2024-05-23 01:59_

---

_Closed by @zanieb on 2024-05-23 01:59_

---

_Branch deleted on 2024-05-23 01:59_

---
