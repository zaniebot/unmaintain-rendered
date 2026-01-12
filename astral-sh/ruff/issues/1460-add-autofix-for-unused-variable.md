```yaml
number: 1460
title: "Add autofix for \"unused variable\""
type: issue
state: closed
author: charliermarsh
labels:
  - fixes
assignees: []
created_at: 2022-12-30T02:44:21Z
updated_at: 2023-01-07T03:06:05Z
url: https://github.com/astral-sh/ruff/issues/1460
synced_at: 2026-01-12T15:54:41Z
```

# Add autofix for "unused variable"

---

_@charliermarsh_

To start, we can just mimic `autoflake`, whose logic is relatively simple: https://github.com/PyCQA/autoflake/blob/60eeb31082bf5290341e84a008d5c832fb04fd13/autoflake.py#L664.

---

_Label `enhancement` added by @charliermarsh on 2022-12-30 02:44_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-30 02:56_

---

_Unassigned @charliermarsh by @charliermarsh on 2022-12-30 04:02_

---

_Label `good first issue` added by @charliermarsh on 2022-12-30 04:03_

---

_Comment by @charliermarsh on 2022-12-30 04:04_

(Not "trivial" but could be a good first issue if anyone's interested given that the `autoflake` logic can mostly be ported over.)

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:12_

---

_Label `autofix` added by @charliermarsh on 2022-12-31 18:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-06 04:36_

---

_Label `good first issue` removed by @charliermarsh on 2023-01-06 04:36_

---

_Closed by @charliermarsh on 2023-01-07 03:06_

---
