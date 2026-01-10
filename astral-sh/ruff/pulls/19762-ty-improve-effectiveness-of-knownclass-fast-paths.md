```yaml
number: 19762
title: "[ty] Improve effectiveness of `KnownClass` fast paths in `instance.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/knownclass-fastpath
created_at: 2025-08-05T12:13:29Z
updated_at: 2025-08-05T12:26:15Z
url: https://github.com/astral-sh/ruff/pull/19762
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Improve effectiveness of `KnownClass` fast paths in `instance.rs`

---

_Pull request opened by @AlexWaygood on 2025-08-05 12:13_

## Summary

This may not make much of a perf difference, but the intent of these fast paths is that if a class is a `KnownClass` then we already know if it's a singleton/single-valued. Currently for these methods, we're only taking the fast path if the class is a `KnownClass` _and_ the relevant `KnownClass` method returns `true` -- if the relevant `KnownClass` method returns `false`, we'll currently check the slow path as well even though it's unnecessary.

This PR fixes that so that we use the fast path even if the relevant methods on `KnownClass` return `false`. 

## Test Plan

Existing tests all pass.


---

_Label `internal` added by @AlexWaygood on 2025-08-05 12:13_

---

_Label `ty` added by @AlexWaygood on 2025-08-05 12:13_

---

_Comment by @github-actions[bot] on 2025-08-05 12:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-05 12:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-08-05 12:18_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-05 12:18_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-05 12:18_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-05 12:18_

---

_Comment by @AlexWaygood on 2025-08-05 12:26_

maybe a 1% speedup on pydantic and altair? https://codspeed.io/astral-sh/ruff/branches/alex%2Fknownclass-fastpath?runnerMode=WallTime

---

_Merged by @AlexWaygood on 2025-08-05 12:26_

---

_Closed by @AlexWaygood on 2025-08-05 12:26_

---

_Branch deleted on 2025-08-05 12:26_

---
