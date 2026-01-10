---
number: 7366
title: "Preview: Consider erroring if direct code is used when mode is disabled"
type: issue
state: open
author: zanieb
labels:
  - needs-decision
  - preview
assignees: []
created_at: 2023-09-13T20:08:23Z
updated_at: 2023-09-13T20:08:31Z
url: https://github.com/astral-sh/ruff/issues/7366
synced_at: 2026-01-10T01:22:47Z
---

# Preview: Consider erroring if direct code is used when mode is disabled

---

_Issue opened by @zanieb on 2023-09-13 20:08_

In https://github.com/astral-sh/ruff/pull/7210/commits/167aeeefe23e18c251cf67065cc6e5d55c24f060 we demonstrate that a warning is displayed when a preview rule is selected with a direct (exact) code but preview mode is disabled. We may want make this an error instead (and exit with a non-zero code).

---

_Label `needs-decision` added by @zanieb on 2023-09-13 20:08_

---

_Label `preview` added by @zanieb on 2023-09-13 20:08_

---
