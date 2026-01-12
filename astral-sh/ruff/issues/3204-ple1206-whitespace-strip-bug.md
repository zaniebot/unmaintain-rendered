```yaml
number: 3204
title: "`PLE1206`: whitespace strip bug"
type: issue
state: closed
author: sasanjac
labels:
  - bug
assignees: []
created_at: 2023-02-24T11:36:57Z
updated_at: 2023-02-25T02:57:50Z
url: https://github.com/astral-sh/ruff/issues/3204
synced_at: 2026-01-12T15:54:43Z
```

# `PLE1206`: whitespace strip bug

---

_@sasanjac_

When logging certain strings like `"100% dynamic"`, i get a false positive for `PLE1206`. I assume when checking for this, whitespace is stripped, so its interpreted as `"100%dynamic"`.

---

_Label `bug` added by @charliermarsh on 2023-02-24 15:42_

---

_Comment by @charliermarsh on 2023-02-24 15:54_

Yeah this looks like a bug!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-24 22:57_

---

_Closed by @charliermarsh on 2023-02-25 02:57_

---
