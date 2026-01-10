```yaml
number: 16468
title: "[red-knot] respect `TYPE_CHECKING` even if not imported from typing"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-15722-type-checking
created_at: 2025-03-03T04:14:01Z
updated_at: 2025-03-04T16:16:58Z
url: https://github.com/astral-sh/ruff/pull/16468
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] respect `TYPE_CHECKING` even if not imported from typing

---

_Pull request opened by @mtshiba on 2025-03-03 04:14_

## Summary

This PR closes #15722.

The change is that if the variable `TYPE_CHECKING` is defined/imported, the type of the variable is interpreted as `Literal[True]` regardless of what the value is.
This is compatible with the behavior of other type checkers (e.g. mypy, pyright).

## Test Plan

I ran the tests with `cargo test -p red_knot_python_semantic` and confirmed that all tests passed.

---

_Review requested from @carljm by @mtshiba on 2025-03-03 04:14_

---

_Review requested from @MichaReiser by @mtshiba on 2025-03-03 04:14_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-03-03 04:14_

---

_Review requested from @sharkdp by @mtshiba on 2025-03-03 04:14_

---

_Label `red-knot` added by @MichaReiser on 2025-03-03 07:47_

---

_@MichaReiser reviewed on 2025-03-03 07:48_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2155 on 2025-03-03 07:48_

```suggestion
                if &name.id == "TYPE_CHECKING" {
```

---

_@carljm reviewed on 2025-03-03 18:17_

Thank you! This looks really good. A few additional wrinkles occurred to me in reviewing it.

1. Should we also support `TYPE_CHECKING: bool = False`? (That is, apply this special case also to an annotated assignment statement.)
2. What should we do if something other than `False` is assigned to `TYPE_CHECKING`?

After thinking about this for a bit, here's the behavior I would propose. The aim here is to avoid confusion by being very explicit up-front that the name `TYPE_CHECKING` is fully reserved for this purpose, rather than making it seem like it can be used as a normal variable, but then giving it unusual behavior in some contexts.

1. The right-hand-side of any assignment to `TYPE_CHECKING` must be a literal `False`; anything else results in a diagnostic. We can use a new diagnostic code `invalid-type-checking-constant`, and a message like "The name `TYPE_CHECKING` is reserved for use as a flag; only `False` can be assigned to it."
2. We should also support `TYPE_CHECKING: bool = False`, or using `object` or `Any` as annotation; the rule should be that `bool` must be assignable to whatever the annotation is. But we should record both the declared and inferred type as `Literal[True]`.
3. In a stub file, we should support `TYPE_CHECKING: bool` with no RHS, and still infer it as both a declaration and binding to `Literal[True]`.

This will require adding support to `infer_annotated_assignment_definition`, in addition to the support you've already added to `infer_assignment_definition`. It will also require adding a new diagnostic code and emitting it.

I think as a result of implementing these rules there (in particular rule 3), it will also mean we can eliminate the special case for `typing.TYPE_CHECKING` that currently exists in `symbol_impl`; we will already infer `typing.TYPE_CHECKING` and `type_extensions.TYPE_CHECKING` as `Literal[True]`, so we won't need a special case on symbol resolution.

Let me know if this doesn't make sense, or if you disagree with any of the suggestions!

---

_Comment by @mtshiba on 2025-03-03 19:58_

Thank you for your review. I've implemented the additional feature you suggested.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/known_constants.md`:50 on 2025-03-03 21:02_

I think it would be clearer if we separate these lines (and the explanation paragraph above) into a separate test, so we can keep this test focused on the correct usage, without the reader wondering if there might be some interaction between the erroring lines and the correct line.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/known_constants.md`:52 on 2025-03-03 21:03_

Similarly, let's have a separate test that does this, so we can show that both `TYPE_CHECKING = False` and `TYPE_CHECKING: object = False` result in a "functional" `TYPE_CHECKING` constant.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/known_constants.md`:61 on 2025-03-03 21:14_

Similarly I think this would be clearer if split into two separate tests.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2154 on 2025-03-03 21:15_

```suggestion
                // `TYPE_CHECKING` is a special variable that should only be assigned `False`
                // at runtime, but is always considered `True` in type checking.
                // See mdtest/known_constants.md#user-defined-type_checking for details.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2234 on 2025-03-03 21:19_

I think we should report this diagnostic also in a stub file, if we see e.g. `TYPE_CHECKING: object = "str"` (and add a test for this.) So I think we should check the RHS (if any) regardless of whether we are in a stub or not, but we should also allow a `...` RHS in a stub.

---

_@carljm reviewed on 2025-03-03 21:20_

I think this looks pretty close! Let's break up the tests, and I have one question about the logic for annotated assignments.

---

_Comment by @MichaReiser on 2025-03-04 09:42_

I'd recommend to make the extension for a new rule and supporting annotated assignments as a separate PR. It doesn't seem that supporting either drastically changes this PR. 

---

_Comment by @mtshiba on 2025-03-04 13:19_

I understand. I will complete this PR with commits up to 574253720367e10f5dc24be2d58fdb3268ef6764 for now, and create a new PR for the new rule.

---

_Comment by @carljm on 2025-03-04 15:51_

Thank you! Merging.

---

_Merged by @carljm on 2025-03-04 15:58_

---

_Closed by @carljm on 2025-03-04 15:58_

---

_Branch deleted on 2025-03-04 16:16_

---
