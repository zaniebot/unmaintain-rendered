```yaml
number: 16501
title: "[red-knot] Add rule `invalid-type-checking-constant`"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: new-rule-invalid-type-checking-constant
created_at: 2025-03-04T15:53:21Z
updated_at: 2025-03-05T02:24:06Z
url: https://github.com/astral-sh/ruff/pull/16501
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Add rule `invalid-type-checking-constant`

---

_Pull request opened by @mtshiba on 2025-03-04 15:53_

## Summary

This PR adds more features to #16468.

* Adds a new error rule `invalid-type-checking-constant`, which occurs when we try to assign a value other than `False` to a user-defined `TYPE_CHECKING` variable (it is possible to assign `...` in a stub file).
* Allows annotated assignment to `TYPE_CHECKING`. Only types that `False` can be assigned to are allowed. However, the type of `TYPE_CHECKING` will be inferred to be `Literal[True]` regardless of what the type is specified.

## Test plan

I ran the tests with `cargo test -p red_knot_python_semantic` and confirmed that all tests passed.


---

_Review requested from @carljm by @mtshiba on 2025-03-04 15:53_

---

_Review requested from @MichaReiser by @mtshiba on 2025-03-04 15:53_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-03-04 15:53_

---

_Review requested from @sharkdp by @mtshiba on 2025-03-04 15:53_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-04 16:20_

---

_Comment by @carljm on 2025-03-04 19:41_

Thank you!!

Made some adjustments while reviewing. I decided that in practice a generic `invalid-assignment` error is too confusing for a line like `TYPE_CHECKING: str = "foo"`, which would be valid normally. We need to emit a `TYPE_CHECKING` specific diagnostic for all cases related to special handling of `TYPE_CHECKING`. There was also an inconsistency between the tests (which said that the annotation must be assignable from `False`) and the implementation (which checked that the annotation is assignable from `True`); I think the best resolution here is to require that it is assignable from `bool`, since in a certain sense it is both `False` and `True`.

I went ahead and pushed these adjustments, and will land once CI is done.

---

_Merged by @carljm on 2025-03-04 19:49_

---

_Closed by @carljm on 2025-03-04 19:49_

---

_Branch deleted on 2025-03-05 02:24_

---
