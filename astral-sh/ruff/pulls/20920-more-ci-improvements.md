```yaml
number: 20920
title: More CI improvements
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/ci-improvements
created_at: 2025-10-16T13:18:54Z
updated_at: 2025-10-16T13:35:06Z
url: https://github.com/astral-sh/ruff/pull/20920
synced_at: 2026-01-12T15:57:12Z
```

# More CI improvements

---

_@AlexWaygood_

## Summary

- Just skip the `--release` job entirely on forks. It times out on GitHub's Linux runner...
- Move `shell: bash` and the `NEXTEST_PROFILE` environment variables to workflow-level defaults, to make the workflows less repetitive

## Test Plan

CI on this PR


---

_Label `ci` added by @AlexWaygood on 2025-10-16 13:18_

---

_@MichaReiser approved on 2025-10-16 13:21_

---

_Comment by @github-actions[bot] on 2025-10-16 13:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @AlexWaygood on 2025-10-16 13:25_

---

_Closed by @AlexWaygood on 2025-10-16 13:25_

---

_Branch deleted on 2025-10-16 13:25_

---

_Comment by @github-actions[bot] on 2025-10-16 13:35_

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
