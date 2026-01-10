```yaml
number: 20842
title: "[ty] Treat functions, methods, and dynamic types as function-like `Callable`s"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-1342-attempt-2
created_at: 2025-10-13T12:21:17Z
updated_at: 2025-10-13T13:21:57Z
url: https://github.com/astral-sh/ruff/pull/20842
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Treat functions, methods, and dynamic types as function-like `Callable`s

---

_Pull request opened by @sharkdp on 2025-10-13 12:21_

## Summary

Treat functions, methods, and dynamic types as function-like `Callable`s

closes https://github.com/astral-sh/ty/issues/1342
closes https://github.com/astral-sh/ty/issues/1344

## Ecosystem analysis

All removed diagnostics look like cases of https://github.com/astral-sh/ty/issues/1344

## Test Plan

Added regression test


---

_Label `ty` added by @sharkdp on 2025-10-13 12:21_

---

_Renamed from "[ty] Treat functions, methods, and dynamic types as function-like Cal…" to "[ty] Treat functions, methods, and dynamic types as function-like `Callable`s" by @sharkdp on 2025-10-13 12:21_

---

_Comment by @github-actions[bot] on 2025-10-13 12:23_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-13 12:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- src/attr/validators.py:175:31: error[unresolved-attribute] Type `(Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = int) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo, flags: Unknown = int) -> Match[bytes] | None]) & ~AlwaysFalsy` has no attribute `__name__`
- tests/test_validators.py:870:13: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((val: _T@lt) -> @Todo) | ((val: _T@le) -> @Todo) | ((val: _T@ge) -> @Todo) | ((val: _T@gt) -> @Todo)` may be missing
- Found 561 diagnostics
+ Found 559 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/attr/validators.py:186:31: error[unresolved-attribute] Type `(Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = int) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo, flags: Unknown = int) -> Match[bytes] | None]) & ~AlwaysFalsy` has no attribute `__name__`
- Found 7521 diagnostics
+ Found 7520 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- examples/contrib/block_dns_over_https.py:357:19: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((flow) -> Unknown)` may be missing
- Found 1843 diagnostics
+ Found 1842 diagnostics

vision (https://github.com/pytorch/vision)
- test/test_transforms_v2.py:6756:40: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((inpt: Unknown) -> Unknown) | ((inpt: Unknown) -> int) | ((pic, mode=None) -> Unknown) | ((inpt: Unknown, displacement: Unknown, interpolation: InterpolationMode | int = InterpolationMode, fill: @Todo = None) -> Unknown)` may be missing
- Found 1482 diagnostics
+ Found 1481 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/records/driver.py:61:36: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((params) -> Unknown)` may be missing
- Found 849 diagnostics
+ Found 848 diagnostics

jax (https://github.com/google/jax)
- jax/experimental/jax2tf/tests/primitives_test.py:146:30: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((x: @Todo, y: @Todo, /) -> Array)` may be missing
- Found 2427 diagnostics
+ Found 2426 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/benchmarks/bench_symbench.py:134:31: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | (() -> Unknown)` may be missing
- Found 14028 diagnostics
+ Found 14027 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- test/aws/mzcompose.py:109:34: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((c: Unknown, ctx: TestContext) -> Unknown)` may be missing
- Found 3387 diagnostics
+ Found 3386 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/optimize/tests/test_zeros.py:539:43: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((f, a, b, args=tuple[()], xtol=float, rtol=Unknown, maxiter=int, full_output=bool, disp=bool) -> Unknown)` may be missing
- scipy/signal/tests/test_filter_design.py:4131:16: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((N, Wn, btype=str, analog=bool, output=str, norm=str, fs=None) -> Unknown) | ((N, Wn, btype=str, analog=bool, output=str, fs=None) -> Unknown) | ... omitted 3 union elements` may be missing
- scipy/signal/tests/test_windows.py:798:25: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((M, *, sym=bool, xp=None, device=None) -> Unknown) | ((M, at, sym=bool, *, xp=None, device=None) -> Unknown) | ... omitted 6 union elements` may be missing
- scipy/sparse/_base.py:676:55: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((a: @Todo, b: @Todo, /) -> Any)` may be missing
- scipy/special/_precompute/gammainc_data.py:117:36: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((a, x, dps=int, maxterms=int) -> Unknown)` may be missing
- Found 6861 diagnostics
+ Found 6856 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-13 12:26_

---

_Comment by @github-actions[bot] on 2025-10-13 12:31_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-missing-attribute` | 0 | 12 | 0 |
| `unresolved-attribute` | 0 | 2 | 0 |
| **Total** | **0** | **14** | **0** |

**[Full report with detailed diff](https://david-fix-1342-attempt-2.ecosystem-663.pages.dev/diff)** ([timing results](https://david-fix-1342-attempt-2.ecosystem-663.pages.dev/timing))


---

_Marked ready for review by @sharkdp on 2025-10-13 12:57_

---

_Review requested from @carljm by @sharkdp on 2025-10-13 12:57_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-13 12:57_

---

_Review requested from @dcreager by @sharkdp on 2025-10-13 12:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:1 on 2025-10-13 13:00_

do we really not already have tests like these somewhere...?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:479 on 2025-10-13 13:00_

```suggestion
A function with no explicit return type should be gradually equivalent to a function-like callable
```

or

```suggestion
A function with no explicit return type should be gradual-equivalent to a function-like callable
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:535 on 2025-10-13 13:02_

👍

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9774 on 2025-10-13 13:05_

strictly speaking, if we're doing it this way rather than with intersections, I suppose we should _also_ have a `method_like: bool` flag for `BoundMethodType`s -- methods not only have all the attributes that functions have, but they also have attributes such as `__closure__` that functions do not have

(Not blocking for this PR, though, of course)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9883 on 2025-10-13 13:06_

This looks identical to the `CallableType::function_like` method immediately below it?

---

_@AlexWaygood approved on 2025-10-13 13:07_

---

_@sharkdp reviewed on 2025-10-13 13:09_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:1 on 2025-10-13 13:09_

:man_shrugging: 

---

_@sharkdp reviewed on 2025-10-13 13:10_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:9774 on 2025-10-13 13:10_

I very much like the `Callable[…] & FunctionType` / `Callable[…] & BoundMethodType` approach, but I'd like to move on with this for now, to get to [what I'm actually trying to achieve](https://github.com/astral-sh/ruff/pull/20802) :smile: 

---

_@sharkdp reviewed on 2025-10-13 13:10_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:9883 on 2025-10-13 13:10_

Oh, haha. Thanks!

---

_@AlexWaygood reviewed on 2025-10-13 13:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9774 on 2025-10-13 13:12_

yeah ikik, that's why I said this is non-blocking 😄

---

_Merged by @sharkdp on 2025-10-13 13:21_

---

_Closed by @sharkdp on 2025-10-13 13:21_

---

_Branch deleted on 2025-10-13 13:21_

---
