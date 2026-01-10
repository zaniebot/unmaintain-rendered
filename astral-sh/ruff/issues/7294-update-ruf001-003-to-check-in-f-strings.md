```yaml
number: 7294
title: "Update `RUF001-003` to check in f-strings"
type: issue
state: closed
author: dhruvmanila
labels:
  - rule
  - python312
assignees: []
created_at: 2023-09-12T09:43:10Z
updated_at: 2023-09-19T06:29:24Z
url: https://github.com/astral-sh/ruff/issues/7294
synced_at: 2026-01-10T11:09:49Z
```

# Update `RUF001-003` to check in f-strings

---

_Issue opened by @dhruvmanila on 2023-09-12 09:43_

`RUF001`, `RUF002`, and `RUF003` checks for ambiguous unicode character in strings, comments and docstrings respectively.

The rule needs to be updated to check for the same inside a f-string. Here, we only need to check within a `FStringMiddle` token which contains the non-expression part of a f-string.

---

_Label `rule` added by @dhruvmanila on 2023-09-12 09:46_

---

_Label `python312` added by @dhruvmanila on 2023-09-12 09:46_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-17 05:59_

---

_Closed by @dhruvmanila on 2023-09-19 06:29_

---
