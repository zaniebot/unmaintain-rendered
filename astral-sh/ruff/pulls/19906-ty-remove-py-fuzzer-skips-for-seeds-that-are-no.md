```yaml
number: 19906
title: "[ty] Remove py-fuzzer skips for seeds that are no longer slow"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/pyfuzz-noskip
created_at: 2025-08-13T23:04:45Z
updated_at: 2025-08-13T23:26:57Z
url: https://github.com/astral-sh/ruff/pull/19906
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Remove py-fuzzer skips for seeds that are no longer slow

---

_Pull request opened by @AlexWaygood on 2025-08-13 23:04_

## Summary

These used to take a very long time for ty to check, but it doesn't seem like that's the case anymore!

## Test Plan

- Ran `uvx --force-reinstall --from=./python/py-fuzzer fuzz --bin=ty 0-500`
- Cd'd into `python/py-fuzzer` and ran `uv run --with=mypy mypy`

---

_Review requested from @carljm by @AlexWaygood on 2025-08-13 23:04_

---

_Label `ci` added by @AlexWaygood on 2025-08-13 23:04_

---

_Label `ty` added by @AlexWaygood on 2025-08-13 23:04_

---

_Comment by @AlexWaygood on 2025-08-13 23:06_

oh do we not run the fuzzer CI job on PRs that touch the fuzzer?

---

_Comment by @AlexWaygood on 2025-08-13 23:21_

> oh do we not run the fuzzer CI job on PRs that touch the fuzzer?

Fixed this. The run successfully completed in 1m25s 🎉 https://github.com/astral-sh/ruff/actions/runs/16951581527/job/48045472265?pr=19906

---

_@carljm approved on 2025-08-13 23:22_

---

_Merged by @AlexWaygood on 2025-08-13 23:23_

---

_Closed by @AlexWaygood on 2025-08-13 23:23_

---

_Branch deleted on 2025-08-13 23:23_

---

_Comment by @github-actions[bot] on 2025-08-13 23:26_

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
