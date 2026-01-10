```yaml
number: 19938
title: "[ty] Use debug builds for conformance tests and run them single threaded"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/conformance-debug-build
created_at: 2025-08-16T14:27:44Z
updated_at: 2025-08-18T07:20:50Z
url: https://github.com/astral-sh/ruff/pull/19938
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Use debug builds for conformance tests and run them single threaded

---

_Pull request opened by @MichaReiser on 2025-08-16 14:27_

## Summary

Use debug builds that are faster to build (ty's own runtime is very short). Debug builds have the added advantage that they run with `debug_assertion`s enabled.

Use `TY_MAX_PARALLELISM=1` for more deterministic execution (which hopefully fixes the `fatal[panic] Panicked at` on every PR)

## Test plan

The CI job now completes in about the same time as before, but this is without any cached Rust artifacts (which are only available once this is merged to main)


---

_Label `testing` added by @MichaReiser on 2025-08-16 14:27_

---

_Label `ty` added by @MichaReiser on 2025-08-16 14:27_

---

_Comment by @github-actions[bot] on 2025-08-16 14:30_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Review requested from @dhruvmanila by @MichaReiser on 2025-08-16 14:34_

---

_@dhruvmanila approved on 2025-08-18 07:02_

---

_Merged by @MichaReiser on 2025-08-18 07:20_

---

_Closed by @MichaReiser on 2025-08-18 07:20_

---

_Branch deleted on 2025-08-18 07:20_

---
