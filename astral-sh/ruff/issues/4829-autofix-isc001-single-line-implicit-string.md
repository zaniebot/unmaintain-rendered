```yaml
number: 4829
title: Autofix ISC001 (single-line-implicit-string-concatenation)
type: issue
state: closed
author: tkukushkin
labels:
  - fixes
assignees: []
created_at: 2023-06-03T09:57:16Z
updated_at: 2023-06-12T23:28:59Z
url: https://github.com/astral-sh/ruff/issues/4829
synced_at: 2026-01-10T11:09:47Z
```

# Autofix ISC001 (single-line-implicit-string-concatenation)

---

_Issue opened by @tkukushkin on 2023-06-03 09:57_

ISC001 could have an autofixer. It can be difficult to combine strings of different kinds, but it shouldn't be difficult to combine strings of the same kind, so, IMHO, it would be ok to fix only such cases.

---

_Label `autofix` added by @charliermarsh on 2023-06-04 02:04_

---

_Comment by @saippuakauppias on 2023-06-04 09:11_

Totally agree, autofix could make it easier to fix this kind of code

---

_Comment by @tkukushkin on 2023-06-04 11:42_

Trying to write autofixer by myself

---

_Assigned to @tkukushkin by @MichaReiser on 2023-06-05 06:18_

---

_Closed by @charliermarsh on 2023-06-12 23:28_

---
