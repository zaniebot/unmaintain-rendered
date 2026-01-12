```yaml
number: 14230
title: "Avoid using `typing.Self` in stub files pre-Python 3.11"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/self
created_at: 2024-11-09T17:49:17Z
updated_at: 2024-11-09T18:17:41Z
url: https://github.com/astral-sh/ruff/pull/14230
synced_at: 2026-01-12T15:55:47Z
```

# Avoid using `typing.Self` in stub files pre-Python 3.11

---

_@charliermarsh_

## Summary

See: https://github.com/astral-sh/ruff/pull/14217#discussion_r1835340869.

This means we're recommending `typing_extensions` in non-stubs pre-3.11, which may not be a valid project dependency, but that's a separate issue (https://github.com/astral-sh/ruff/issues/9761).


---

_Review requested from @AlexWaygood by @charliermarsh on 2024-11-09 17:49_

---

_Label `bug` added by @charliermarsh on 2024-11-09 17:49_

---

_Review request for @AlexWaygood removed by @charliermarsh on 2024-11-09 17:49_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-11-09 17:49_

---

_Comment by @github-actions[bot] on 2024-11-09 18:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-11-09 18:15_

---

_Comment by @AlexWaygood on 2024-11-09 18:15_

Thanks!

---

_Merged by @charliermarsh on 2024-11-09 18:17_

---

_Closed by @charliermarsh on 2024-11-09 18:17_

---

_Branch deleted on 2024-11-09 18:17_

---

_Comment by @charliermarsh on 2024-11-09 18:17_

Thank you!

---
