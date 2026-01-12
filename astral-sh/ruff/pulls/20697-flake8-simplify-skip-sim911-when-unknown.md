```yaml
number: 20697
title: "[`flake8-simplify`] Skip `SIM911` when unknown arguments are present"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
assignees: []
merged: true
base: main
head: sim911-guard-non-empty-args
created_at: 2025-10-04T07:41:01Z
updated_at: 2025-10-21T20:58:49Z
url: https://github.com/astral-sh/ruff/pull/20697
synced_at: 2026-01-12T15:57:07Z
```

# [`flake8-simplify`] Skip `SIM911` when unknown arguments are present

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #18778

Prevent SIM911 from triggering when zip() is called on .keys()/.values() that take any positional or keyword arguments, so Ruff
never suggests the lossy rewrite.

## Test Plan

<!-- How was it tested? -->

Added a test case to SIM911.py.


---

_@TaKO8Ki reviewed on 2025-10-04 07:48_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/flake8_simplify/rules/zip_dict_keys_and_values.rs`:92 on 2025-10-04 07:48_

I initially thought I have to modify SIM911  to skip only the autofix, but I found a similar implementation here and aligned with that instead. If you’d prefer we go with it, I can update the implementation accordingly.

https://github.com/astral-sh/ruff/blob/00f702e9fff0837b3e1aa41ea65ec300665f9ace/crates/ruff_linter/src/rules/flake8_simplify/rules/key_in_dict.rs#L68

---

_Comment by @github-actions[bot] on 2025-10-04 07:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-10-07 19:54_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/zip_dict_keys_and_values.rs`:92 on 2025-10-21 20:56_

Either way works, I guess it makes sense to be consistent with the other SIM rule.

---

_@ntBre approved on 2025-10-21 20:57_

Thank you! Sorry for the delay

---

_Renamed from "[`ruff`] Skip SIM911 when keys/values take arguments" to "[`flake8-simplify`] Skip `SIM911` when unknown arguments are present" by @ntBre on 2025-10-21 20:58_

---

_Merged by @ntBre on 2025-10-21 20:58_

---

_Closed by @ntBre on 2025-10-21 20:58_

---
