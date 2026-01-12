```yaml
number: 20428
title: "[ty] Make TypeIs invariant in its type argument"
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - ty
assignees: []
merged: true
base: main
head: make-type-is-invariant
created_at: 2025-09-16T02:33:28Z
updated_at: 2025-09-18T14:53:13Z
url: https://github.com/astral-sh/ruff/pull/20428
synced_at: 2026-01-12T15:57:01Z
```

# [ty] Make TypeIs invariant in its type argument

---

_@ericmarkmartin_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
What it says on the tin. See the [typing spec](https://docs.python.org/3/library/typing.html#typing.TypeIs) for justification.

## Test Plan

Add more tests to PEP 695 `variance.md` suite.
<!-- How was it tested? -->

---

_Review requested from @carljm by @ericmarkmartin on 2025-09-16 02:33_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-09-16 02:33_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-09-16 02:33_

---

_Review requested from @dcreager by @ericmarkmartin on 2025-09-16 02:33_

---

_Comment by @github-actions[bot] on 2025-09-16 02:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-16 02:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:783 on 2025-09-16 03:16_

Isn't `TypeIs`'s variance already tested [in `is_subtype_of.md`](https://github.com/astral-sh/ruff/blob/2a6dde4acb6cb6a8ef4426ca3d792bdb1c477f21/crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md#typeguard-and-typeis)?

---

_@InSyncWithFoo reviewed on 2025-09-16 03:16_

---

_Label `ty` added by @AlexWaygood on 2025-09-16 06:14_

---

_Renamed from "Make TypeIs invariant in its type argument" to "[ty] Make TypeIs invariant in its type argument" by @AlexWaygood on 2025-09-16 06:15_

---

_@ericmarkmartin reviewed on 2025-09-16 15:00_

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:783 on 2025-09-16 15:00_

Oh this is interesting. The pre-existence of these tests implies the invariance was already being enforced? I'll have to look further

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:783 on 2025-09-16 15:19_

Do the tests you added here fail before the code change in this PR?

It seems to me that what you are testing for here is assignability/subtyping of `TypeIs` types themselves (which I think was already correctly invariant), but the code change you made would not impact that, it would instead affect variance inference when you have e.g. `TypeIs[T]` as part of a method signature or as an attribute on a PEP695 generic class with typevar `T`. Which seems like a rare/unusual case, but still worth getting right.

---

_@carljm reviewed on 2025-09-16 15:19_

---

_@ericmarkmartin reviewed on 2025-09-18 04:03_

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:783 on 2025-09-18 04:03_

Yeah, I missed that there was existing logic for the in variance of `TypeIs`. Changed the tests, and also fixed `assert` -> `static_assert` 🙈 

---

_Review request for @sharkdp removed by @sharkdp on 2025-09-18 06:51_

---

_@carljm approved on 2025-09-18 14:52_

Thank you!

---

_Merged by @carljm on 2025-09-18 14:53_

---

_Closed by @carljm on 2025-09-18 14:53_

---
