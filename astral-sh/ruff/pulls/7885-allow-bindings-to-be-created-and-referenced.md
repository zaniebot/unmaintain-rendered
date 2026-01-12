```yaml
number: 7885
title: Allow bindings to be created and referenced within annotations
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/annotate
created_at: 2023-10-10T03:42:34Z
updated_at: 2023-10-10T03:58:17Z
url: https://github.com/astral-sh/ruff/pull/7885
synced_at: 2026-01-12T02:32:41Z
```

# Allow bindings to be created and referenced within annotations

---

_Pull request opened by @charliermarsh on 2023-10-10 03:42_

## Summary

Given:

```python
baz: Annotated[
    str,
    [qux for qux in foo],
]
```

We treat `baz` as `BindingKind::Annotation`, to ensure that references to `baz` are marked as unbound. However, we were _also_ treating `qux` as `BindingKind::Annotation`, which meant that the load in the comprehension _also_ errored.

Closes https://github.com/astral-sh/ruff/issues/7879.


---

_Label `bug` added by @charliermarsh on 2023-10-10 03:42_

---

_Merged by @charliermarsh on 2023-10-10 03:51_

---

_Closed by @charliermarsh on 2023-10-10 03:51_

---

_Branch deleted on 2023-10-10 03:51_

---

_Comment by @github-actions[bot] on 2023-10-10 03:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
