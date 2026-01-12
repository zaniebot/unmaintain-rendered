```yaml
number: 13327
title: "[`pyupgrade`] Fix broken doc link and clarify that deprecated aliases were removed in Python 3.12 (`UP005`)"
type: pull_request
state: merged
author: dizzy57
labels:
  - documentation
assignees: []
merged: true
base: main
head: deprecated-unittest-alias-docs-fix
created_at: 2024-09-11T15:50:35Z
updated_at: 2024-09-11T18:27:08Z
url: https://github.com/astral-sh/ruff/pull/13327
synced_at: 2026-01-12T15:55:43Z
```

# [`pyupgrade`] Fix broken doc link and clarify that deprecated aliases were removed in Python 3.12 (`UP005`)

---

_@dizzy57_


## Summary

Deprecated `unittest` aliases [were removed in Python 3.12](https://docs.python.org/3.12/whatsnew/3.12.html#id3).
As they were removed, the deprecation notice was also removed from the official documentation, and now the link pointing to the latest Python 3 documentation is broken, so I replaced it with a link to Python 3.11 documentation,

## Test Plan

`cargo dev generate-docs`

---

_Comment by @github-actions[bot] on 2024-09-11 16:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-09-11 18:26_

Thanks!

---

_Label `documentation` added by @AlexWaygood on 2024-09-11 18:27_

---

_Merged by @AlexWaygood on 2024-09-11 18:27_

---

_Closed by @AlexWaygood on 2024-09-11 18:27_

---
