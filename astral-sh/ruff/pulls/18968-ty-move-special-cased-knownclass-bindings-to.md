```yaml
number: 18968
title: "[ty] move special-cased `KnownClass` bindings to `class.rs`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
draft: true
base: main
head: alex/knownclass-bindings
created_at: 2025-06-26T18:10:27Z
updated_at: 2025-06-26T18:35:54Z
url: https://github.com/astral-sh/ruff/pull/18968
synced_at: 2026-01-10T18:39:09Z
```

# [ty] move special-cased `KnownClass` bindings to `class.rs`

---

_Pull request opened by @AlexWaygood on 2025-06-26 18:10_

## Summary

Currently if you want to add special handling for a call to a class constructor, you have to both add a branch to `Type::bindings()` _and_ remember to update this `matches!()` call:

https://github.com/astral-sh/ruff/blob/b85c219283dcdae474642e9174352da5d9aee132/crates/ty_python_semantic/src/types/infer.rs#L5341-L5359

This PR adds a new `KnownClass::bindings()` method and uses it to ensure that there's now only a single source of truth for whether calls to a constructor need special casing or not.

Ideally we wouldn't hardcode so many class constructor signatures at all. I looked into deriving some of them from typeshed, but it seems surprisingly hard? I couldn't figure out quickly how to do it, and it didn't seem worth spending that much time on it.

## Test Plan

Existing tests all pass


---

_Review requested from @carljm by @AlexWaygood on 2025-06-26 18:10_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-26 18:10_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-26 18:10_

---

_Label `internal` added by @AlexWaygood on 2025-06-26 18:10_

---

_Label `ty` added by @AlexWaygood on 2025-06-26 18:10_

---

_Renamed from "move special-cased `KnownClass` bindings to `class.rs`" to "[ty] move special-cased `KnownClass` bindings to `class.rs`" by @AlexWaygood on 2025-06-26 18:10_

---

_Comment by @github-actions[bot] on 2025-06-26 18:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-06-26 18:20_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fknownclass-bindings?runnerMode=Instrumentation)

### Merging #18968 will **degrade performances by 5.51%**

<sub>Comparing <code>alex/knownclass-bindings</code> (9ea95e9) with <code>main</code> (b85c219)</sub>



### Summary

`❌ 1` regressions  
`✅ 37` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Fknownclass-bindings?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_micro[many_attribute_assignments] `` | 56.7 ms | 60 ms | -5.51% |


---

_Converted to draft by @AlexWaygood on 2025-06-26 18:23_

---

_Comment by @AlexWaygood on 2025-06-26 18:35_

Don't really understand why this would cause such a big perf regression, but I can repro it locally. Probably not worth spending time on right now, though I do find the duplication in the current code pretty confusing.

---

_Closed by @AlexWaygood on 2025-06-26 18:35_

---
