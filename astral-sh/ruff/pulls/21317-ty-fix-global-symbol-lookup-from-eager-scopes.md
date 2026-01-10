```yaml
number: 21317
title: "[ty] fix global symbol lookup from eager scopes"
type: pull_request
state: merged
author: mtshiba
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: fix-comprehension-symbol-lookup
created_at: 2025-11-07T15:37:52Z
updated_at: 2025-11-13T15:51:39Z
url: https://github.com/astral-sh/ruff/pull/21317
synced_at: 2026-01-10T16:53:55Z
```

# [ty] fix global symbol lookup from eager scopes

---

_Pull request opened by @mtshiba on 2025-11-07 15:37_

## Summary

cf. https://github.com/astral-sh/ruff/pull/20962

In the following code, `foo` in the comprehension was not reported as unresolved:

```python
# error: [unresolved-reference] "Name `foo` used when not defined"
foo
foo = [
    # no error!
    # revealed: Divergent
    reveal_type(x) for _ in () for x in [foo]
]

baz = [
    # error: [unresolved-reference] "Name `baz` used when not defined"
    # revealed: Unknown
    reveal_type(x) for _ in () for x in [baz]
]
```

In fact, this is a more serious bug than it looks: for `foo`, [`explicit_global_symbol` is called](https://github.com/astral-sh/ruff/blob/6cc3393ccd9059439d9c1325e0e041db1d7481af/crates/ty_python_semantic/src/types/infer/builder.rs#L8052), causing a symbol that should actually be `Undefined` to be reported as being of type `Divergent`.

This PR fixes this bug. As a result, the code in `mdtest/regression/pr_20962_comprehension_panics.md` no longer panics.

## Test Plan

`corpus\cyclic_symbol_in_comprehension.py` is added.
New tests are added in `mdtest/comprehensions/basic.md`.


---

_Comment by @github-actions[bot] on 2025-11-07 15:41_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @github-actions[bot] on 2025-11-07 15:41_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandera (https://github.com/pandera-dev/pandera)
- pandera/api/dataframe/container.py:1273:31: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `Any | None`
- pandera/api/dataframe/container.py:1274:33: warning[possibly-missing-attribute] Attribute `parsers` may be missing on object of type `Any | None`
- pandera/api/dataframe/container.py:1275:32: warning[possibly-missing-attribute] Attribute `checks` may be missing on object of type `Any | None`
- pandera/api/dataframe/container.py:1276:34: warning[possibly-missing-attribute] Attribute `nullable` may be missing on object of type `Any | None`
- pandera/api/dataframe/container.py:1277:32: warning[possibly-missing-attribute] Attribute `unique` may be missing on object of type `Any | None`
- pandera/api/dataframe/container.py:1278:32: warning[possibly-missing-attribute] Attribute `coerce` may be missing on object of type `Any | None`
- pandera/api/dataframe/container.py:1279:30: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Any | None`
- Found 1636 diagnostics
+ Found 1629 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 44 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @mtshiba on 2025-11-07 16:19_

`ty_server::e2e pull_diagnostics::document_diagnostic_caching_changed` seems to hang non-deterministically.

https://github.com/astral-sh/ruff/actions/runs/19173434027/job/54811898939?pr=21317

---

_Marked ready for review by @mtshiba on 2025-11-07 16:21_

---

_Review requested from @carljm by @mtshiba on 2025-11-07 16:21_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-11-07 16:21_

---

_Review requested from @sharkdp by @mtshiba on 2025-11-07 16:21_

---

_Review requested from @dcreager by @mtshiba on 2025-11-07 16:21_

---

_Label `bug` added by @AlexWaygood on 2025-11-07 16:25_

---

_Label `ty` added by @AlexWaygood on 2025-11-07 16:25_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:8494 on 2025-11-07 21:16_

If returning here is correct, then pushing the constraint into `constraint_keys` immediately above this cannot have any effect, so should we remove that as well? (Removing it doesn't seem to fail any test.)

Why do we hit the `FoundConstraint` case in this situation in the first place? It's not clear to me where the "constraint" comes from.

---

_@carljm reviewed on 2025-11-07 21:17_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer/builder.rs`:8494 on 2025-11-08 11:17_

Since place includes member as well as symbol, narrowing constraints must still be tracked even if there are no bindings, for example:

```python
from typing import Literal

class Foo:
    bar: Literal[1, 2]

foo = Foo()
if foo.bar == 1:
    # `foo.bar` has no explicit binding, but the constraint is `foo.bar == 1`
    [reveal_type(foo.bar) for _ in ()]  # Literal[1]
```

But as you pointed out, there was no need to add the constraint to `constraint_keys` here, because we already added it when iterating over `ancestor_scopes` above. Fixed.

---

_@mtshiba reviewed on 2025-11-08 11:17_

---

_Comment by @mtshiba on 2025-11-09 08:01_

I found another example of a too-many-cycle-iteration panic.

```python
if C:
    class C[_T](C): ...
```

The panic occurred because `C` in the base class list was being resolved circularly.
It should evaluate to `Undefined` rather than throwing a cyclic-class-definition error.

---

_Comment by @mtshiba on 2025-11-09 08:08_

I don't have time right now (I originally found this bug while working on #20566), but I think the logic in `infer_place_load` is clearly too complicated and could be a breeding ground for bugs like this one.
I think it should be refactored at some point.

---

_@mtshiba reviewed on 2025-11-09 08:24_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer/builder.rs`:8494 on 2025-11-09 08:24_

A more fundamental issue is that the global scope should be excluded from the scopes of enclosing scope lookup.
See the comments below.

---

_@MichaReiser reviewed on 2025-11-10 08:36_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/corpus/cyclic_symbol_in_comprehension.py`:1 on 2025-11-10 08:36_

Can you say more about why you converted this file to a corpus test? It's not clear to me why that would be preferred or required.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:7871 on 2025-11-10 08:38_

Nit
```suggestion
                if enclosing_scope_file_id.is_global() {
```

---

_@MichaReiser reviewed on 2025-11-10 08:38_

---

_@AlexWaygood reviewed on 2025-11-10 08:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/corpus/cyclic_symbol_in_comprehension.py`:1 on 2025-11-10 08:42_

The previous test asserted that we panicked; the new test asserts that the snippet does not cause us to panic

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/corpus/cyclic_symbol_in_comprehension.py`:1 on 2025-11-10 08:44_

I guess it could have stayed as an mdtest, but corpus tests are what we usually use when all we care about is checking that we didn't panic; it makes sense to me

---

_@AlexWaygood reviewed on 2025-11-10 08:44_

---

_@carljm reviewed on 2025-11-10 23:12_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:8494 on 2025-11-10 23:12_

> Fixed.

It looks like the push to `constraint_keys` is still there?

---

_@mtshiba reviewed on 2025-11-11 02:12_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer/builder.rs`:8494 on 2025-11-11 02:12_

Pushing to `constraint_keys` is now necessary because iteration over `ancestor_scopes` now skips the global scope lookup.
For example in the following case:

```python
class C:
    x: int | None = None

c = C()

if c.x is not None:
    class _:
        reveal_type(c.x)  # revealed: int
```

---

_Comment by @carljm on 2025-11-11 19:59_

The new diagnostic on the conformance suite looks incorrect. We are now throwing an error on the reference to `Private` at https://github.com/python/typing/blob/main/conformance/tests/generics_syntax_scoping.py#L74 but we should not.

---

_Comment by @carljm on 2025-11-12 15:48_

This is looking great, thanks! Just as a heads-up: I am making some (minor: code organization and naming) changes to this PR and then I will merge it.

---

_Merged by @carljm on 2025-11-12 18:15_

---

_Closed by @carljm on 2025-11-12 18:15_

---

_Branch deleted on 2025-11-13 15:51_

---
