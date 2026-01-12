```yaml
number: 8495
title: "Avoid `D301` autofix for `u` prefixed strings"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/d301
created_at: 2023-11-05T11:07:55Z
updated_at: 2023-11-05T14:45:50Z
url: https://github.com/astral-sh/ruff/pull/8495
synced_at: 2026-01-10T23:40:55Z
```

# Avoid `D301` autofix for `u` prefixed strings

---

_Pull request opened by @dhruvmanila on 2023-11-05 11:07_

This PR avoids creating the fix for `D301` if the string is prefixed with `u` i.e., it's a unicode string. The reason being that `u` and `r` cannot be used together as it's a syntax error.

Refer: https://github.com/astral-sh/ruff/issues/8402#issuecomment-1788783287

---

_Label `bug` added by @dhruvmanila on 2023-11-05 11:07_

---

_Comment by @github-actions[bot] on 2023-11-05 11:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2023-11-05 14:45_

Seems reasonable, though better I think we could be to remove the `u` since it's unnecessary in Python 3 IIRC.

---

_Merged by @charliermarsh on 2023-11-05 14:45_

---

_Closed by @charliermarsh on 2023-11-05 14:45_

---

_Branch deleted on 2023-11-05 14:45_

---
