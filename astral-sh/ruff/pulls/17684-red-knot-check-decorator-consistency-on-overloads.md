```yaml
number: 17684
title: "[red-knot] Check decorator consistency on overloads"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: dhruv/overload-decorator-consistency
created_at: 2025-04-28T15:40:57Z
updated_at: 2025-04-30T15:04:23Z
url: https://github.com/astral-sh/ruff/pull/17684
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Check decorator consistency on overloads

---

_Pull request opened by @dhruvmanila on 2025-04-28 15:40_

## Summary

Part of #15383.

As per the spec (https://typing.python.org/en/latest/spec/overload.html#invalid-overload-definitions):

For `@staticmethod` and `@classmethod`:

> If one overload signature is decorated with `@staticmethod` or `@classmethod`, all overload signatures must be similarly decorated. The implementation, if present, must also have a consistent decorator. Type checkers should report an error if these conditions are not met.

For `@final` and `@override`:

> If a `@final` or `@override` decorator is supplied for a function with overloads, the decorator should be applied only to the overload implementation if it is present. If an overload implementation isn’t present (for example, in a stub file), the `@final` or `@override` decorator should be applied only to the first overload. Type checkers should enforce these rules and generate an error when they are violated. If a `@final` or `@override` decorator follows these rules, a type checker should treat the decorator as if it is present on all overloads.

## Test Plan

Update existing tests; add snapshots.


---

_Label `red-knot` added by @dhruvmanila on 2025-04-28 15:40_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-28 15:40_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-28 15:40_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-28 15:40_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-28 15:40_

---

_Comment by @github-actions[bot] on 2025-04-28 15:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-04-29 22:29_

Excellent!

---

_Label `diagnostics` added by @dhruvmanila on 2025-04-30 14:10_

---

_@AlexWaygood reviewed on 2025-04-30 14:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:6499 on 2025-04-30 14:37_

I don't know if it makes much difference, but you could consider returning owned `FunctionType`s here, since they're just `u32`s under the hood

```suggestion
    fn all(&self) -> impl Iterator<Item = FunctionType<'db>> + '_ {
        self.overloads.iter().copied().chain(self.implementation)
```

---

_@dhruvmanila reviewed on 2025-04-30 14:48_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:6499 on 2025-04-30 14:48_

I don't think it should but this is better, thanks for the suggestion!

---

_Merged by @dhruvmanila on 2025-04-30 15:04_

---

_Closed by @dhruvmanila on 2025-04-30 15:04_

---

_Branch deleted on 2025-04-30 15:04_

---
