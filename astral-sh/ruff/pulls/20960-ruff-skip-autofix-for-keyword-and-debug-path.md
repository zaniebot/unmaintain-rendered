```yaml
number: 20960
title: "[`ruff`] Skip autofix for keyword and `__debug__` path params"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fastapi-fast003-skip-keyword-fixes
created_at: 2025-10-18T15:58:45Z
updated_at: 2025-10-20T23:44:15Z
url: https://github.com/astral-sh/ruff/pull/20960
synced_at: 2026-01-10T17:34:34Z
```

# [`ruff`] Skip autofix for keyword and `__debug__` path params

---

_Pull request opened by @TaKO8Ki on 2025-10-18 15:58_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20941

Skip autofix for keyword and __debug__ path params

## Test Plan

<!-- How was it tested? -->

I added two test cases to crates/ruff_linter/resources/test/fixtures/fastapi/FAST003.py.


---

_Comment by @github-actions[bot] on 2025-10-18 16:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@amyreese approved on 2025-10-20 23:36_

---

_Renamed from "[`ruff`] Skip autofix for keyword and __debug__ path params" to "[`ruff`] Skip autofix for keyword and `__debug__` path params" by @amyreese on 2025-10-20 23:36_

---

_Merged by @amyreese on 2025-10-20 23:37_

---

_Closed by @amyreese on 2025-10-20 23:37_

---

_Label `bug` added by @amyreese on 2025-10-20 23:44_

---

_Label `fixes` added by @amyreese on 2025-10-20 23:44_

---
