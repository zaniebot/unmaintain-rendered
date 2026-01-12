```yaml
number: 8707
title: Treat display as a builtin in IPython
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/display
created_at: 2023-11-15T23:16:09Z
updated_at: 2023-11-16T02:05:50Z
url: https://github.com/astral-sh/ruff/pull/8707
synced_at: 2026-01-10T23:40:55Z
```

# Treat display as a builtin in IPython

---

_Pull request opened by @charliermarsh on 2023-11-15 23:16_

## Summary

`display` is a special-cased builtin in IPython. This PR adds it to the builtin namespace when analyzing IPython notebooks.

Closes https://github.com/astral-sh/ruff/issues/8702.

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-11-15 23:16_

---

_@charliermarsh reviewed on 2023-11-15 23:16_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/test.rs`:58 on 2023-11-15 23:16_

I renamed this to avoid some confusion.

---

_Label `bug` added by @charliermarsh on 2023-11-15 23:16_

---

_Comment by @github-actions[bot] on 2023-11-15 23:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_python_stdlib/src/builtins.rs`:183 on 2023-11-16 01:43_

```suggestion
    // Constructed by converting the `PYTHON_BUILTINS` slice to a `match` expression.
```

---

_@dhruvmanila approved on 2023-11-16 01:43_

---

_Merged by @charliermarsh on 2023-11-16 01:58_

---

_Closed by @charliermarsh on 2023-11-16 01:58_

---

_Branch deleted on 2023-11-16 01:58_

---
