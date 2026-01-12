```yaml
number: 7804
title: "Bug: `ruff rule` shows `PLR6301` as non-preview"
type: issue
state: closed
author: jamesbraza
labels:
  - bug
  - cli
assignees: []
created_at: 2023-10-04T07:03:57Z
updated_at: 2023-10-04T15:12:45Z
url: https://github.com/astral-sh/ruff/issues/7804
synced_at: 2026-01-12T15:54:47Z
```

# Bug: `ruff rule` shows `PLR6301` as non-preview

---

_@jamesbraza_

With `ruff==0.0.292`, it seems `PLR6301` from `ruff rule --all --format=json` has `"preview": False`.  I think it should be `True` per https://docs.astral.sh/ruff/rules/no-self-use/

> This rule is unstable and in [preview](https://docs.astral.sh/ruff/preview/).

---

_Label `bug` added by @charliermarsh on 2023-10-04 13:43_

---

_Label `cli` added by @charliermarsh on 2023-10-04 13:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-04 13:43_

---

_Closed by @charliermarsh on 2023-10-04 15:12_

---
