```yaml
number: 18015
title: "[ty] Remove unused variants from various `Known*` enums"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/unused-variants
created_at: 2025-05-11T09:03:22Z
updated_at: 2025-05-12T17:19:11Z
url: https://github.com/astral-sh/ruff/pull/18015
synced_at: 2026-01-12T15:56:10Z
```

# [ty] Remove unused variants from various `Known*` enums

---

_@AlexWaygood_

## Summary

`KnownClass::Range`, `KnownInstanceType::Any` and `ClassBase::any()` are no longer used or useful: all our tests pass with them removed. `KnownModule::Abc` _is_ now used outside of tests, however, so I removed the `#[allow(dead_code)]` branch above that variant.

## Test Plan

`cargo test -p ty_python_semantic`


---

_Review requested from @carljm by @AlexWaygood on 2025-05-11 09:03_

---

_Label `internal` added by @AlexWaygood on 2025-05-11 09:03_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-11 09:03_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-11 09:03_

---

_Label `ty` added by @AlexWaygood on 2025-05-11 09:03_

---

_Comment by @github-actions[bot] on 2025-05-11 09:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-05-11 09:10_

An unexpected -- but welcome! -- 1% speedup on the `many_string_assignments` benchmark: https://codspeed.io/astral-sh/ruff/branches/alex%2Funused-variants. (Neutral on the other two ty benchmarks.)

---

_@sharkdp reviewed on 2025-05-11 10:16_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:2453 on 2025-05-11 10:16_

Nice!

---

_@sharkdp approved on 2025-05-11 10:16_

Thank you!

---

_Merged by @AlexWaygood on 2025-05-11 10:18_

---

_Closed by @AlexWaygood on 2025-05-11 10:18_

---

_Branch deleted on 2025-05-11 10:18_

---

_Comment by @carljm on 2025-05-12 17:19_

Removing `KnownInstanceType::Any` means we are no longer compatible with a custom typeshed from prior to the change to represent Any as a class. But I think that's fine -- not clear to me that we provide any warranties at all when it comes to using any typeshed other than the vendored one.

---
