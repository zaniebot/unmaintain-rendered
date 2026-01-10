```yaml
number: 15058
title: "[red-knot] Add support for unpacking `for` target"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/unpack-for-target
created_at: 2024-12-19T11:37:12Z
updated_at: 2024-12-23T06:15:41Z
url: https://github.com/astral-sh/ruff/pull/15058
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Add support for unpacking `for` target

---

_Pull request opened by @dhruvmanila on 2024-12-19 11:37_

## Summary

Related to #13773 

This PR adds support for unpacking `for` statement targets.

This involves updating the `value` field in the `Unpack` target to use an enum which specifies the "where did the value expression came from?". This is because for an iterable expression, we need to unpack the iterator type while for assignment statement we need to unpack the value type itself. And, this needs to be done in the unpack query.

### Question

One of the ways unpacking works in `for` statement is by looking at the union of the types because if the iterable expression is a tuple then the iterator type will be union of all the types in the tuple. This means that the test cases that will test the unpacking in `for` statement will also implicitly test the unpacking union logic. I was wondering if it makes sense to merge these cases and only add the ones that are specific to the union unpacking or for statement unpacking logic.

## Test Plan

Add test cases involving iterating over a tuple type. I've intentionally left out certain cases for now and I'm curious to know any thoughts on the above query.


---

_Label `red-knot` added by @dhruvmanila on 2024-12-19 11:37_

---

_Renamed from "[red-knot] Add support for unpacking union types" to "[red-knot] Add support for unpacking `for` target" by @dhruvmanila on 2024-12-19 11:37_

---

_Comment by @github-actions[bot] on 2024-12-20 04:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:476 on 2024-12-20 12:12_

I've intentionally left out certain test cases, refer to the "Question" in PR description.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1034 on 2024-12-20 12:12_

This is basically the same logic as in the assignment statement, I'm going to wait until I implement unpacking support in other places to get an overview of what can be simplified to avoid repetition.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:209 on 2024-12-20 12:13_

Inferring the value type is moved in the `Unpacker`

---

_Review comment by @dhruvmanila on `crates/red_knot_workspace/tests/check.rs`:217 on 2024-12-20 12:14_

This is actually a bug, calling `visit_expr` on the expression panics in the following case:
```py
[] = *data
() = *data
```

This is because in `infer_target`, we completely skip the inference when there are no elements in the list / tuple target.

I'm going to fix this in a follow-up.

---

_@dhruvmanila reviewed on 2024-12-20 12:15_

---

_Marked ready for review by @dhruvmanila on 2024-12-20 12:15_

---

_Review requested from @carljm by @dhruvmanila on 2024-12-20 12:15_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-12-20 12:15_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-12-20 12:15_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-12-20 12:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:476 on 2024-12-20 21:00_

I think the tests you have here look good!

---

_@carljm approved on 2024-12-20 21:00_

Awesome!

---

_Merged by @dhruvmanila on 2024-12-23 06:13_

---

_Closed by @dhruvmanila on 2024-12-23 06:13_

---

_Branch deleted on 2024-12-23 06:13_

---
