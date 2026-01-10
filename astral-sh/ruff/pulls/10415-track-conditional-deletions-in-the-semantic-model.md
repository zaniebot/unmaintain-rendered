```yaml
number: 10415
title: Track conditional deletions in the semantic model
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/del
created_at: 2024-03-15T00:23:12Z
updated_at: 2024-03-15T00:45:47Z
url: https://github.com/astral-sh/ruff/pull/10415
synced_at: 2026-01-10T22:47:02Z
```

# Track conditional deletions in the semantic model

---

_Pull request opened by @charliermarsh on 2024-03-15 00:23_

## Summary

Given `del X`, we'll typically add a `BindingKind::Deletion` to `X` to shadow the current binding. However, if the deletion is inside of a conditional operation, we _won't_, as in:

```python
def f():
    global X

    if X > 0:
        del X
```

We will, however, track it as a reference to the binding. This PR adds the expression context to those resolved references, so that we can detect that the `X` in `global X` was "assigned to".

Closes https://github.com/astral-sh/ruff/issues/10397.


---

_Label `bug` added by @charliermarsh on 2024-03-15 00:23_

---

_Marked ready for review by @charliermarsh on 2024-03-15 00:23_

---

_Comment by @github-actions[bot] on 2024-03-15 00:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-03-15 00:45_

---

_Closed by @charliermarsh on 2024-03-15 00:45_

---

_Branch deleted on 2024-03-15 00:45_

---
