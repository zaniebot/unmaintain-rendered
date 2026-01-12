```yaml
number: 5061
title: Change W605 autofix to use raw strings if possible
type: issue
state: closed
author: hauntsaninja
labels:
  - fixes
assignees: []
created_at: 2023-06-13T18:34:13Z
updated_at: 2023-06-25T21:35:09Z
url: https://github.com/astral-sh/ruff/issues/5061
synced_at: 2026-01-12T15:54:45Z
```

# Change W605 autofix to use raw strings if possible

---

_@hauntsaninja_

If a string contains no valid escape sequences, it would be nice to fix by turning the string into a raw string, instead of escaping the backslash. For the common cases of regular expressions and latex, this improves readability.

---

_Label `autofix` added by @charliermarsh on 2023-06-13 18:48_

---

_Closed by @charliermarsh on 2023-06-25 21:35_

---
