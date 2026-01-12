```yaml
number: 11346
title: Simplify arithmetic operation in logical lines checker
type: pull_request
state: merged
author: augustelalande
labels:
  - internal
assignees: []
merged: true
base: main
head: simplify-arithmetic
created_at: 2024-05-08T22:40:20Z
updated_at: 2024-05-09T01:57:29Z
url: https://github.com/astral-sh/ruff/pull/11346
synced_at: 2026-01-12T15:55:37Z
```

# Simplify arithmetic operation in logical lines checker

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Simplify arithmetic operation in logical lines checker


---

_Comment by @github-actions[bot] on 2024-05-08 22:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb approved on 2024-05-08 23:09_

Huh. It'd be good to get the eyes of the original author on this too.

---

_@charliermarsh approved on 2024-05-09 00:59_

---

_Label `internal` added by @charliermarsh on 2024-05-09 00:59_

---

_Merged by @charliermarsh on 2024-05-09 00:59_

---

_Closed by @charliermarsh on 2024-05-09 00:59_

---

_Comment by @charliermarsh on 2024-05-09 01:00_

Yeah that's interesting, hmm... Maybe it evolved over time? Maybe this was just the way they thought about the logic?

---

_@charliermarsh reviewed on 2024-05-09 01:01_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/logical_lines.rs`:27 on 2024-05-09 01:01_

Wait sorry, _is_ this the same? What if `indent` is 4 and `tab_size` is 3?

`indent += tab_size` would yield 7, but `indent = (indent / tab_size) * tab_size + tab_size` would yield 6, right?

---

_@charliermarsh reviewed on 2024-05-09 01:02_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/logical_lines.rs`:27 on 2024-05-09 01:02_

I.e., is the goal here perhaps that `indent` ends up as a multiple of `tab_size`?

---

_@zanieb reviewed on 2024-05-09 01:36_

---

_Review comment by @zanieb on `crates/ruff_linter/src/checkers/logical_lines.rs`:27 on 2024-05-09 01:36_

Is the deal here that there's implicit integer rounding?

---

_Branch deleted on 2024-05-09 01:50_

---

_@zanieb reviewed on 2024-05-09 01:57_

---

_Review comment by @zanieb on `crates/ruff_linter/src/checkers/logical_lines.rs`:27 on 2024-05-09 01:57_

If so, can we add a comment explaining the deal? @charliermarsh

---
