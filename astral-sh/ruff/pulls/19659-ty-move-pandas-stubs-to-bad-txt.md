```yaml
number: 19659
title: "[ty] Move `pandas-stubs` to bad.txt"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/pandas-stubs-primer
created_at: 2025-07-31T11:27:52Z
updated_at: 2025-07-31T11:33:26Z
url: https://github.com/astral-sh/ruff/pull/19659
synced_at: 2026-01-12T15:56:44Z
```

# [ty] Move `pandas-stubs` to bad.txt

---

_@AlexWaygood_

## Summary

Following https://github.com/pandas-dev/pandas-stubs/commit/bf1221eb7ea0e582c30fe233d1f4f5713fce376b, we panic when trying to check the pandas-stubs `main` branch (we're entering code that's supposed to be unreachable, which is exciting!). This is causing every mypy_primer run to fail and meaning that we can't get diffs on PRs, so for now I'm just moving the project to bad.txt.

## Test Plan

Hopefully primer will not fail on this PR


---

_Review requested from @carljm by @AlexWaygood on 2025-07-31 11:27_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-31 11:27_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-31 11:27_

---

_Label `ci` added by @AlexWaygood on 2025-07-31 11:27_

---

_Label `ty` added by @AlexWaygood on 2025-07-31 11:27_

---

_Comment by @github-actions[bot] on 2025-07-31 11:29_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-31 11:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @AlexWaygood on 2025-07-31 11:33_

---

_Closed by @AlexWaygood on 2025-07-31 11:33_

---

_Branch deleted on 2025-07-31 11:33_

---
