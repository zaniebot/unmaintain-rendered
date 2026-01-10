```yaml
number: 16018
title: "[red-knot] Unpacker: Make invariant explicit and directly return a Type"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/unpacker-explicit-invariant
created_at: 2025-02-07T11:18:47Z
updated_at: 2025-02-07T12:00:07Z
url: https://github.com/astral-sh/ruff/pull/16018
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Unpacker: Make invariant explicit and directly return a Type

---

_Pull request opened by @sharkdp on 2025-02-07 11:18_

## Summary

- Do not return `Option<Type<…>>` from `Unpacker::get`, but just `Type`. Panic otherwise.
- Rename `Unpacker::get` to `Unpacker::expression_type`


---

_Label `red-knot` added by @sharkdp on 2025-02-07 11:18_

---

_Review requested from @carljm by @sharkdp on 2025-02-07 11:18_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-07 11:18_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-07 11:18_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-02-07 11:20_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-02-07 11:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/unpacker.rs`:277 on 2025-02-07 11:37_

We call the same method `expression_type` in `TypeInference`. I'm somewhat undecided if I'd value the consistency here or if diverging is fine

---

_@MichaReiser approved on 2025-02-07 11:37_

---

_@sharkdp reviewed on 2025-02-07 11:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/unpacker.rs`:277 on 2025-02-07 11:56_

Let's go with `expression_type` :+1: 

---

_Merged by @sharkdp on 2025-02-07 12:00_

---

_Closed by @sharkdp on 2025-02-07 12:00_

---

_Branch deleted on 2025-02-07 12:00_

---
