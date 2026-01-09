---
number: 2422
title: PLR1722 use-sys-exit autofix should add sys import
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-01-31T21:56:22Z
updated_at: 2023-01-31T22:00:03Z
url: https://github.com/astral-sh/ruff/issues/2422
synced_at: 2026-01-07T13:12:14-06:00
---

# PLR1722 use-sys-exit autofix should add sys import

---

_Issue opened by @spaceone on 2023-01-31 21:56_

`PLR1722` transform `exit()` → `sys.exit()`.

if `sys` is not imported the autofix is skipped.
→ Instead `sys` should be imported.

---

_Comment by @charliermarsh on 2023-01-31 22:00_

Yeah I see this as tracked via #835.

---

_Closed by @charliermarsh on 2023-01-31 22:00_

---
