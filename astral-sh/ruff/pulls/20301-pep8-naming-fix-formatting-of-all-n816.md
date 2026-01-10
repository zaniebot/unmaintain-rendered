```yaml
number: 20301
title: "[`pep8-naming`] Fix formatting of `__all__` (`N816`)"
type: pull_request
state: merged
author: onerandomusername
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-09-08T03:29:49Z
updated_at: 2025-09-09T14:43:43Z
url: https://github.com/astral-sh/ruff/pull/20301
synced_at: 2026-01-10T17:46:22Z
```

# [`pep8-naming`] Fix formatting of `__all__` (`N816`)

---

_Pull request opened by @onerandomusername on 2025-09-08 03:29_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Noticed this was not escaped when writing a project that parses the result of `ruff rule --outputformat json`. This is visible here: <https://docs.astral.sh/ruff/rules/mixed-case-variable-in-global-scope/#why-is-this-bad>

## Test Plan

documentation only


---

_Label `documentation` added by @ntBre on 2025-09-08 13:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pep8_naming/rules/mixed_case_variable_in_global_scope.rs`:22 on 2025-09-08 13:58_

Should we add code formatting to `from M import *` in the line above while we're here?

---

_@ntBre approved on 2025-09-08 13:59_

Thank you!

---

_Comment by @github-actions[bot] on 2025-09-08 14:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Fix formatting of __all__ in N816" to "[`pep8-naming`] Fix formatting of `__all__` (`N816`)" by @ntBre on 2025-09-08 14:37_

---

_Merged by @ntBre on 2025-09-08 14:40_

---

_Closed by @ntBre on 2025-09-08 14:40_

---

_Branch deleted on 2025-09-09 14:43_

---
