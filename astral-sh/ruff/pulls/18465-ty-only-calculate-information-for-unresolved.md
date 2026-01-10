```yaml
number: 18465
title: "[ty] Only calculate information for unresolved-reference subdiagnostic if we know we'll emit the diagnostic"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - performance
  - ty
assignees: []
merged: true
base: main
head: alex/cleanup-unresolved-ref
created_at: 2025-06-04T17:08:46Z
updated_at: 2025-06-04T19:41:02Z
url: https://github.com/astral-sh/ruff/pull/18465
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Only calculate information for unresolved-reference subdiagnostic if we know we'll emit the diagnostic

---

_Pull request opened by @AlexWaygood on 2025-06-04 17:08_

## Summary

This optimizes some of the logic added in https://github.com/astral-sh/ruff/pull/18444. In general, we only calculate information for subdiagnostics if we know we'll actually emit the diagnostic. The check to see whether we'll emit the diagnostic is work we'll definitely have to do whereas the the work to gather information for a subdiagnostic isn't work we necessarily have to do if the diagnostic isn't going to be emitted at all.

This PR makes us lazier about gathering the information we need for the subdiagnostic, and moves all the subdiagnostic logic into one function rather than having some `unresolved-reference` subdiagnostic logic in `infer.rs` and some in `diagnostic.rs`.

## Test Plan

`cargo test -p ty_python_semantic`


---

_Review requested from @carljm by @AlexWaygood on 2025-06-04 17:08_

---

_Label `internal` added by @AlexWaygood on 2025-06-04 17:08_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-04 17:08_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-04 17:08_

---

_Label `performance` added by @AlexWaygood on 2025-06-04 17:08_

---

_Label `ty` added by @AlexWaygood on 2025-06-04 17:09_

---

_Comment by @github-actions[bot] on 2025-06-04 17:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-04 17:13_

---

_@carljm approved on 2025-06-04 19:35_

~~I don't see where this is delaying work compared to the prior version from #18444? It seems like in both the old and new version, all the work is in deciding whether an attribute exists in the first place, and in both old and new, that work is done if we are definitely going to emit a diagnostic, and not otherwise.~~ Edit: ah, never mind, I forgot about the early return if `context.report_lint(...)` returns `None`.

But this looks like a nice refactor to collect more of the work in one place, either way!

---

_Merged by @AlexWaygood on 2025-06-04 19:41_

---

_Closed by @AlexWaygood on 2025-06-04 19:41_

---

_Branch deleted on 2025-06-04 19:41_

---
