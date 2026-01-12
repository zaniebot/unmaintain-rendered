```yaml
number: 8369
title: Fix invalid E231 error with f-strings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/f
created_at: 2023-10-30T23:27:33Z
updated_at: 2023-10-31T11:59:17Z
url: https://github.com/astral-sh/ruff/pull/8369
synced_at: 2026-01-12T15:55:26Z
```

# Fix invalid E231 error with f-strings

---

_@charliermarsh_

## Summary

We were considering the `{` within an f-string to be a left brace, which caused the "space-after-colon" rule to trigger incorrectly.

Closes https://github.com/astral-sh/ruff/issues/8299.

---

_Label `bug` added by @charliermarsh on 2023-10-30 23:27_

---

_Merged by @charliermarsh on 2023-10-30 23:38_

---

_Closed by @charliermarsh on 2023-10-30 23:38_

---

_Branch deleted on 2023-10-30 23:38_

---

_Comment by @github-actions[bot] on 2023-10-30 23:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2023-10-31 01:52_

Thanks!

---

_Comment by @bigluck on 2023-10-31 11:59_

Thanks so much @charliermarsh !

---
