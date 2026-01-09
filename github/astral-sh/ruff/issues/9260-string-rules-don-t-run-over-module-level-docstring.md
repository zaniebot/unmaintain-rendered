---
number: 9260
title: "String rules don't run over module-level docstring"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-23T13:20:16Z
updated_at: 2023-12-23T15:03:13Z
url: https://github.com/astral-sh/ruff/issues/9260
synced_at: 2026-01-07T13:12:15-06:00
---

# String rules don't run over module-level docstring

---

_Issue opened by @charliermarsh on 2023-12-23 13:20_

E.g., if the file is exactly `u"foo"`, we don't run [UP025](https://docs.astral.sh/ruff/rules/unicode-kind-prefix/) on the string.

---

_Label `bug` added by @charliermarsh on 2023-12-23 13:20_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-23 13:27_

---

_Referenced in [astral-sh/ruff#9261](../../astral-sh/ruff/pulls/9261.md) on 2023-12-23 14:03_

---

_Closed by @charliermarsh on 2023-12-23 15:03_

---
