```yaml
number: 21818
title: "[ty] Improve typevar solving when encountering unions of generic types"
type: pull_request
state: open
author: suleymanozkeskin
labels:
  - ty
assignees: []
base: main
head: main
created_at: 2025-12-05T20:19:59Z
updated_at: 2025-12-18T16:31:58Z
url: https://github.com/astral-sh/ruff/pull/21818
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Improve typevar solving when encountering unions of generic types

---

_Pull request opened by @suleymanozkeskin on 2025-12-05 20:19_

Improves type inference for union types with multiple generic parameters like
Ok[T] | Err[E] by adding union-pairing logic to the constraint solver.

Changes:
 - Added union element pairing with typevar confinement checks in generics.rs
 - Added 3 tests to type_guards.md

## Summary

Following the explanatory [comment](https://github.com/astral-sh/ruff/pull/21739#issuecomment-3615023976), made by @carljm, I made some refactoring which I hope addresses the issue on the correct place.

I am not sure if this is still a **right** or **suitable** fix that matches the further plans of the maintainers on this topic.  

Fixes https://github.com/astral-sh/ty/issues/1703

## Test Plan
I used the existing markdown test suite with additional test cases to cover the issue I opened + the case mentioned [here](https://github.com/astral-sh/ty/issues/1703#issuecomment-3599797957)


---

_Review requested from @carljm by @suleymanozkeskin on 2025-12-05 20:20_

---

_Review requested from @AlexWaygood by @suleymanozkeskin on 2025-12-05 20:20_

---

_Review requested from @sharkdp by @suleymanozkeskin on 2025-12-05 20:20_

---

_Review requested from @dcreager by @suleymanozkeskin on 2025-12-05 20:20_

---

_Label `ty` added by @AlexWaygood on 2025-12-05 20:22_

---

_Renamed from "  Fixes https://github.com/astral-sh/ty/issues/1703" to "[ty] Improve typevar solving when encountering unions of generic types" by @AlexWaygood on 2025-12-05 20:23_

---

_Comment by @astral-sh-bot[bot] on 2025-12-05 20:24_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-05 20:26_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 492 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 39 diagnostics
+ Found 42 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5519 diagnostics
+ Found 5518 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Assigned to @sharkdp by @sharkdp on 2025-12-08 15:39_

---

_Comment by @sharkdp on 2025-12-10 19:39_

Thank you for your contribution. I'm planning to review this soon.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:473 on 2025-12-11 09:50_

Can you go into details why this would be unsound? It seems like this should be supported in some scenarios? If `A` and `B` are covariant (or bivariant, like here) in `T`, `int | str` seems like a valid solution?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:478 on 2025-12-11 09:51_

Why?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:488 on 2025-12-11 09:51_

Ok, but there is a valid solution here (`T = int`). This test makes it sound like we should never handle this case.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/generics.rs`:1606 on 2025-12-11 09:52_

If this is handled by the code below, we should add tests for it.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/generics.rs`:1520 on 2025-12-11 09:59_

This (together with even more assumptions below) looks like the code here is tailored for *exactly* the one use case of the `Ok[Something] | Error[SomethingElse]` vs. `Ok[T] | Error[E]` matching described in the issue. The ecosystem report also shows that this change doesn't seem to have a broader impact. This makes me wonder if it is worth adding this, given that we'll soon want to replace this with a much more general solution based on constraint sets.

I think I might be in favor of implementing something like this now (without constraint sets), *if* we can generalize it to more cases, i.e. without explicitly matching on `Type::NominalInstance` below, for example. But in its current form, I'm not sure if it's worth the additional complexity and review+maintenance effort.

I would also value @dcreager's opinion here.

---

_@sharkdp reviewed on 2025-12-11 10:02_

Thank you for your contribution. As laid out in one of the inline comments, I'm not sure if it's worth integrating this in its current form, given that it's rather limited in applicability(?).

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-18 16:31_

---
