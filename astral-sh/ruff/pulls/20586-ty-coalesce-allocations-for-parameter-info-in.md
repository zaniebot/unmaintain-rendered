```yaml
number: 20586
title: "[ty] Coalesce allocations for parameter info in ArgumentMatcher"
type: pull_request
state: merged
author: fgiacome
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: coalesce_argmatcher_param_info_allocations
created_at: 2025-09-25T21:47:36Z
updated_at: 2025-09-26T05:38:07Z
url: https://github.com/astral-sh/ruff/pull/20586
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Coalesce allocations for parameter info in ArgumentMatcher

---

_Pull request opened by @fgiacome on 2025-09-25 21:47_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Follow up on #20495. The improvement suggested by @AlexWaygood cannot be applied as-is since the `argument_matches` vector is indexed by argument number, while the two boolean vectors are indexed by parameter number. Still coalescing the latter two saves one allocation.

---

_Review requested from @carljm by @fgiacome on 2025-09-25 21:47_

---

_Review requested from @AlexWaygood by @fgiacome on 2025-09-25 21:47_

---

_Review requested from @sharkdp by @fgiacome on 2025-09-25 21:47_

---

_Review requested from @dcreager by @fgiacome on 2025-09-25 21:47_

---

_Comment by @github-actions[bot] on 2025-09-25 21:50_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-25 21:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@dcreager approved on 2025-09-26 00:56_

---

_Merged by @dcreager on 2025-09-26 00:56_

---

_Closed by @dcreager on 2025-09-26 00:56_

---

_Label `internal` added by @MichaReiser on 2025-09-26 05:38_

---

_Label `ty` added by @MichaReiser on 2025-09-26 05:38_

---
