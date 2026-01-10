```yaml
number: 3468
title: "`--ignore-noqa` isn't respected when `RUF100` is enabled"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-03-12T18:23:10Z
updated_at: 2023-03-16T17:49:38Z
url: https://github.com/astral-sh/ruff/issues/3468
synced_at: 2026-01-10T11:09:46Z
```

# `--ignore-noqa` isn't respected when `RUF100` is enabled

---

_Issue opened by @charliermarsh on 2023-03-12 18:23_

If `RUF100` is enabled, we end up running the `noqa` checker even if the `noqa` flag is not enabled. As a result, `--ignore-noqa` has no effect when `RUF100` is selected.

---

_Label `bug` added by @charliermarsh on 2023-03-12 18:23_

---

_Closed by @charliermarsh on 2023-03-12 18:37_

---

_Comment by @onerandomusername on 2023-03-16 17:49_

This is still an issue with 0.0.256, though not necessarily when RUF100 is enabled.


---

_Comment by @onerandomusername on 2023-03-16 17:49_

Ignore me, I just realised I didn't have my virtual environment enabled.

---
