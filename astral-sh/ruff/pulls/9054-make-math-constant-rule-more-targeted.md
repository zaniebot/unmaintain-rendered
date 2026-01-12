```yaml
number: 9054
title: "Make `math-constant` rule more targeted"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/furb
created_at: 2023-12-08T16:48:32Z
updated_at: 2023-12-08T17:42:19Z
url: https://github.com/astral-sh/ruff/pull/9054
synced_at: 2026-01-12T15:55:27Z
```

# Make `math-constant` rule more targeted

---

_@charliermarsh_

## Summary

We now only flag `math.pi` if the value is in `[3.14, 3.15)`, and apply similar rules to the other constants.

Closes https://github.com/astral-sh/ruff/issues/9049.


---

_Label `bug` added by @charliermarsh on 2023-12-08 16:48_

---

_Review requested from @zanieb by @charliermarsh on 2023-12-08 16:58_

---

_Marked ready for review by @charliermarsh on 2023-12-08 16:58_

---

_Comment by @github-actions[bot] on 2023-12-08 17:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@konstin approved on 2023-12-08 17:10_

So i can safely continue with `π=3` :P

---

_@zanieb approved on 2023-12-08 17:41_

---

_Merged by @charliermarsh on 2023-12-08 17:42_

---

_Closed by @charliermarsh on 2023-12-08 17:42_

---

_Branch deleted on 2023-12-08 17:42_

---
