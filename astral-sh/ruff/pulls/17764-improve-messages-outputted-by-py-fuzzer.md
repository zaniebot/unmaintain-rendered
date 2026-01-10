```yaml
number: 17764
title: Improve messages outputted by py-fuzzer
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: alex/py-fuzzer-output
created_at: 2025-05-01T12:29:30Z
updated_at: 2025-05-01T12:39:03Z
url: https://github.com/astral-sh/ruff/pull/17764
synced_at: 2026-01-10T18:57:03Z
```

# Improve messages outputted by py-fuzzer

---

_Pull request opened by @AlexWaygood on 2025-05-01 12:29_

## Summary

- When reporting a panic/bug, clearly state that it is a _new_ panic/bug if `--only-new-bugs` has been passed on the CLI. When this flag has been passed, we won't report the panic/bug unless it does not exist with the baseline executable.
- A couple of small grammar fixes ("file" instead of "files" if only 1 seed was selected, etc.)

## Test Plan

- Ran mypy and Ruff on the code
- Ran the fuzzer locally both with and without `--only-new-bugs`, with a single seed specified, and with multiple seeds specified.


---

_Label `internal` added by @AlexWaygood on 2025-05-01 12:29_

---

_Label `testing` added by @AlexWaygood on 2025-05-01 12:29_

---

_Merged by @AlexWaygood on 2025-05-01 12:32_

---

_Closed by @AlexWaygood on 2025-05-01 12:32_

---

_Branch deleted on 2025-05-01 12:32_

---

_Comment by @github-actions[bot] on 2025-05-01 12:39_

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
