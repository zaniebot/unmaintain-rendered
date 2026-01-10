```yaml
number: 8155
title: "`invalid-escape-sequence` (`W605`) quick fix has incorrect description"
type: issue
state: closed
author: DetachHead
labels:
  - bug
assignees: []
created_at: 2023-10-24T05:27:10Z
updated_at: 2023-10-26T16:23:22Z
url: https://github.com/astral-sh/ruff/issues/8155
synced_at: 2026-01-10T11:09:50Z
```

# `invalid-escape-sequence` (`W605`) quick fix has incorrect description

---

_Issue opened by @DetachHead on 2023-10-24 05:27_

the quick fix says "Add backslash to escape sequence":
![image](https://github.com/astral-sh/ruff/assets/57028336/6864fa02-5aa1-44d1-a6ed-440cdf2cb68a)

but it actually turns it into a `r` string instead:
```py
r"\d"
```
https://play.ruff.rs/0aa99ac0-9dff-4f90-affb-36f66b885661

---

_Label `bug` added by @dhruvmanila on 2023-10-24 05:54_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-10-24 05:54_

---

_Closed by @dhruvmanila on 2023-10-26 16:23_

---
