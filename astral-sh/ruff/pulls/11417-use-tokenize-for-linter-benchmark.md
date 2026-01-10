```yaml
number: 11417
title: "Use `tokenize` for linter benchmark"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/bench-linter
created_at: 2024-05-14T03:11:53Z
updated_at: 2024-05-14T14:28:41Z
url: https://github.com/astral-sh/ruff/pull/11417
synced_at: 2026-01-10T22:05:26Z
```

# Use `tokenize` for linter benchmark

---

_Pull request opened by @dhruvmanila on 2024-05-14 03:11_

## Summary

This PR updates the linter benchmark to use the `tokenize` function instead of the lexer.

The linter expects the token list to be up to and including the first error which is what the `ruff_python_parser::tokenize` function returns.

This was not a problem before because the benchmarks only uses valid Python code.


---

_Label `internal` added by @dhruvmanila on 2024-05-14 03:11_

---

_Comment by @github-actions[bot] on 2024-05-14 03:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-05-14 14:28_

---

_Merged by @charliermarsh on 2024-05-14 14:28_

---

_Closed by @charliermarsh on 2024-05-14 14:28_

---

_Branch deleted on 2024-05-14 14:28_

---
