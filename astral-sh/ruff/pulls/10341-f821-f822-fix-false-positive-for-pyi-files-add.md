```yaml
number: 10341
title: "F821, F822: fix false positive for `.pyi` files; add more test coverage for `.pyi` files"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
assignees: []
merged: true
base: main
head: pyi-annassign-bindings
created_at: 2024-03-11T14:17:29Z
updated_at: 2024-03-11T22:15:28Z
url: https://github.com/astral-sh/ruff/pull/10341
synced_at: 2026-01-10T22:47:01Z
```

# F821, F822: fix false positive for `.pyi` files; add more test coverage for `.pyi` files

---

_Pull request opened by @AlexWaygood on 2024-03-11 14:17_

## Summary

This PR fixes the following false positive in a `.pyi` stub file:

```py
x: int
y = x  # F821 currently emitted here, but shouldn't be in a stub file
```

In a `.py` file, this is invalid regardless of whether `from __future__ import annotations` is enabled or not. In a `.pyi` stub file, however, it's always valid, as an annotation counts as a binding in a stub file even if no value is assigned to the variable.

I also added more test coverage for `.pyi` stub files in various edge cases where ruff's behaviour is currently correct, but where `.pyi` stub files do slightly different things to `.py` files. In the process of doing this, I discovered https://github.com/astral-sh/ruff/issues/10340.

## Test Plan

`cargo test`


---

_Label `bug` added by @AlexWaygood on 2024-03-11 14:17_

---

_Comment by @github-actions[bot] on 2024-03-11 14:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-03-11 14:34_

> ✅ ecosystem check detected no linter changes.

Unfortunately typeshed already has F821 switched off in its config file due to this bug, or we'd likely see some changes here

---

_@charliermarsh approved on 2024-03-11 20:48_

---

_Merged by @AlexWaygood on 2024-03-11 22:15_

---

_Closed by @AlexWaygood on 2024-03-11 22:15_

---

_Branch deleted on 2024-03-11 22:15_

---
