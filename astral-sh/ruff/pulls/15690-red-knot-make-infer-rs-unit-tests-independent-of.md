```yaml
number: 15690
title: "[red-knot] Make `infer.rs` unit tests independent of public symbol inference"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/infer-unit-tests-no-public-symbol-dependence
created_at: 2025-01-23T13:22:20Z
updated_at: 2025-01-23T13:30:21Z
url: https://github.com/astral-sh/ruff/pull/15690
synced_at: 2026-01-10T20:05:43Z
```

# [red-knot] Make `infer.rs` unit tests independent of public symbol inference

---

_Pull request opened by @sharkdp on 2025-01-23 13:22_

## Summary

Make the remaining `infer.rs` unit tests independent from public symbol type inference decisions (see upcoming change in #15674).

## Test Plan

- Made sure that the unit tests actually fail if one of the `assert_type` assertions is changed.


---

_Label `red-knot` added by @sharkdp on 2025-01-23 13:22_

---

_Review requested from @carljm by @sharkdp on 2025-01-23 13:22_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-23 13:22_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-23 13:22_

---

_@sharkdp reviewed on 2025-01-23 13:22_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:6056 on 2025-01-23 13:22_

This just brings back two functions that I previously deleted.

---

_Renamed from "[red-knot] Make `infer.rs` unit tests independent from public symbol inference" to "[red-knot] Make `infer.rs` unit tests independent of public symbol inference" by @sharkdp on 2025-01-23 13:28_

---

_@AlexWaygood approved on 2025-01-23 13:29_

---

_Merged by @sharkdp on 2025-01-23 13:30_

---

_Closed by @sharkdp on 2025-01-23 13:30_

---

_Branch deleted on 2025-01-23 13:30_

---
