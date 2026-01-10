```yaml
number: 4863
title: "F523: Regression: index out-of-bounds panic on invalid parameter indices"
type: issue
state: closed
author: addisoncrump
labels:
  - bug
assignees: []
created_at: 2023-06-05T12:57:19Z
updated_at: 2023-06-05T18:31:49Z
url: https://github.com/astral-sh/ruff/issues/4863
synced_at: 2026-01-10T11:09:47Z
```

# F523: Regression: index out-of-bounds panic on invalid parameter indices

---

_Issue opened by @addisoncrump on 2023-06-05 12:57_

Potentially, a `.format`'d string specifies a parameter outside of the number of real parameters:

```py
"{8}".format(0, 1)
```

If a) the index specified in the template exceeds the number of parameters and b) there are unused format args, the F523 fix will panic (regression from: https://github.com/charliermarsh/ruff/pull/4837)

This was detected by #4822.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-05 12:58_

---

_Label `bug` added by @charliermarsh on 2023-06-05 12:58_

---

_Renamed from "F523: Regression: index out-of-bounds panic on invalid indices" to "F523: Regression: index out-of-bounds panic on invalid parameter indices" by @addisoncrump on 2023-06-05 13:00_

---

_Comment by @charliermarsh on 2023-06-05 16:38_

Will fix this today.

---

_Comment by @charliermarsh on 2023-06-05 18:22_

I had to modify the snippet a bit to reproduce: `"{1} {8}".format(0, 1)`

---

_Closed by @charliermarsh on 2023-06-05 18:31_

---
