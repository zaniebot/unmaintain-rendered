```yaml
number: 7296
title: "Update `PLE2510`, `PLE2512-2515` to check in f-strings"
type: issue
state: closed
author: dhruvmanila
labels:
  - rule
  - python312
assignees: []
created_at: 2023-09-12T09:43:19Z
updated_at: 2023-09-16T08:12:51Z
url: https://github.com/astral-sh/ruff/issues/7296
synced_at: 2026-01-12T15:54:47Z
```

# Update `PLE2510`, `PLE2512-2515` to check in f-strings

---

_@dhruvmanila_

`PLE2510`, `PLE2512`, `PLE2513`, `PLE2514`, and `PLE2515` checks for invalid string characters.

The rule needs to be updated to check within the f-string as well. Here, we only need to check within the `FStringMiddle` token which contains the non-expression part of a f-string.

---

_Label `python312` added by @dhruvmanila on 2023-09-12 09:47_

---

_Label `rule` added by @dhruvmanila on 2023-09-12 09:48_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-14 07:28_

---

_Closed by @dhruvmanila on 2023-09-16 08:12_

---
