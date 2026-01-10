```yaml
number: 18551
title: "`ruff/__main__.py`: Remove unnecessary `os.fsdecode`"
type: pull_request
state: merged
author: RazerM
labels:
  - internal
assignees: []
merged: true
base: main
head: feature/unnecessary-fsdecode
created_at: 2025-06-08T17:56:40Z
updated_at: 2025-06-09T10:38:53Z
url: https://github.com/astral-sh/ruff/pull/18551
synced_at: 2026-01-10T18:45:04Z
```

# `ruff/__main__.py`: Remove unnecessary `os.fsdecode`

---

_Pull request opened by @RazerM on 2025-06-08 17:56_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This is really minor, but `find_ruff_bin()` returns `str`, so calling `os.fsdecode` is a no-op. This was missed when `find_ruff_bin()` was refactored to no longer return `Path`.

## Test Plan

N/A

---

_@AlexWaygood approved on 2025-06-09 10:32_

---

_Label `internal` added by @AlexWaygood on 2025-06-09 10:32_

---

_Renamed from "Remove unnecessary os.fsdecode" to "`ruff/__main__.py`: Remove unnecessary `os.fsdecode`" by @AlexWaygood on 2025-06-09 10:32_

---

_Merged by @AlexWaygood on 2025-06-09 10:34_

---

_Closed by @AlexWaygood on 2025-06-09 10:34_

---

_Comment by @github-actions[bot] on 2025-06-09 10:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
