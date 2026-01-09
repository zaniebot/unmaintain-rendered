---
number: 9452
title: "D413 rule doesn't work correctly"
type: issue
state: closed
author: stinodego
labels:
  - bug
  - docstring
assignees: []
created_at: 2024-01-10T00:42:52Z
updated_at: 2024-08-07T01:50:17Z
url: https://github.com/astral-sh/ruff/issues/9452
synced_at: 2026-01-07T13:12:15-06:00
---

# D413 rule doesn't work correctly

---

_Issue opened by @stinodego on 2024-01-10 00:42_

If I take the code from the example section in the docs:
https://docs.astral.sh/ruff/rules/blank-line-after-last-section/

And run `ruff format` on it, the blank line after the last section is not added.

My ruff version is `0.1.11` and the config is simply `select = ["D413"]`.


---

_Label `bug` added by @charliermarsh on 2024-01-10 16:24_

---

_Label `docstring` added by @charliermarsh on 2024-01-10 16:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-26 19:12_

---

_Referenced in [astral-sh/ruff#9654](../../astral-sh/ruff/pulls/9654.md) on 2024-01-26 19:22_

---

_Closed by @charliermarsh on 2024-01-26 19:31_

---
