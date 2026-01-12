```yaml
number: 21910
title: "[ty] avoid fixpoint unioning of types containing current-cycle Divergent"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/nodiv
created_at: 2025-12-11T00:32:52Z
updated_at: 2025-12-12T03:52:36Z
url: https://github.com/astral-sh/ruff/pull/21910
synced_at: 2026-01-12T15:57:36Z
```

# [ty] avoid fixpoint unioning of types containing current-cycle Divergent

---

_@carljm_

Partially addresses https://github.com/astral-sh/ty/issues/1732

## Summary

Don't  union the previous type in fixpoint iteration if the previous type contains a `Divergent` from the current cycle and the latest type does not. The theory here, as outlined by @mtshiba at https://github.com/astral-sh/ty/issues/1732#issuecomment-3609937420, is that oscillation can't occur by removing and then reintroducing a `Divergent` type repeatedly, since `Divergent` types are only introduced at the start of fixpoint iteration.

## Test Plan

Removes a `Divergent` type from the added mdtest, doesn't otherwise regress any tests.


---

_Label `ty` added by @carljm on 2025-12-11 00:32_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 00:34_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-11 00:46_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
- lib/spack/spack/mirrors/mirror.py:114:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Literal[True] | Unknown | str | Divergent`
+ lib/spack/spack/mirrors/mirror.py:114:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Literal[True] | Unknown | str`
- lib/spack/spack/mirrors/mirror.py:120:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Unknown | str | Divergent`
+ lib/spack/spack/mirrors/mirror.py:120:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Unknown | str`
- lib/spack/spack/mirrors/mirror.py:263:45: error[invalid-argument-type] Argument to bound method `_update_connection_dict` is incorrect: Expected `dict[Unknown, Unknown]`, found `@Todo | str | Divergent`
+ lib/spack/spack/mirrors/mirror.py:263:45: error[invalid-argument-type] Argument to bound method `_update_connection_dict` is incorrect: Expected `dict[Unknown, Unknown]`, found `@Todo | str`

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/command/build_ext.py:270:23: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `(Unknown & ~AlwaysFalsy) | (list[tuple[Unknown, str] | Unknown] & ~AlwaysFalsy) | (list[Divergent] & ~AlwaysFalsy)`
+ setuptools/_distutils/command/build_ext.py:270:23: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `(Unknown & ~AlwaysFalsy) | (list[tuple[Unknown, str] | Unknown] & ~AlwaysFalsy)`
- setuptools/_distutils/command/sdist.py:372:58: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | Literal[0] | None | list[Unknown | int | None] | list[Divergent]`
+ setuptools/_distutils/command/sdist.py:372:58: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | Literal[0] | None | list[Unknown | int | None]`
- setuptools/_distutils/text_file.py:232:44: error[unsupported-operator] Operator `+` is not supported between objects of type `@Todo | int | None | Divergent` and `Literal[1]`
+ setuptools/_distutils/text_file.py:232:44: error[unsupported-operator] Operator `+` is not supported between objects of type `@Todo | int | None` and `Literal[1]`
- setuptools/_distutils/text_file.py:242:41: error[unsupported-operator] Operator `+` is not supported between objects of type `@Todo | int | None | Divergent` and `Literal[1]`
+ setuptools/_distutils/text_file.py:242:41: error[unsupported-operator] Operator `+` is not supported between objects of type `@Todo | int | None` and `Literal[1]`
- setuptools/_vendor/autocommand/autoparse.py:306:5: error[invalid-assignment] Object of type `Unknown & ~None` is not assignable to attribute `func` on type `_Wrapped[(...), Unknown, (argv=None), Unknown] | _Wrapped[(...), Unknown, Divergent, Unknown]`
- setuptools/_vendor/autocommand/autoparse.py:307:5: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `parser` on type `_Wrapped[(...), Unknown, (argv=None), Unknown] | _Wrapped[(...), Unknown, Divergent, Unknown]`
+ setuptools/_vendor/autocommand/autoparse.py:306:5: error[unresolved-attribute] Unresolved attribute `func` on type `_Wrapped[(...), Unknown, (argv=None), Unknown]`.
+ setuptools/_vendor/autocommand/autoparse.py:307:5: error[unresolved-attribute] Unresolved attribute `parser` on type `_Wrapped[(...), Unknown, (argv=None), Unknown]`.

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-aws/tests/test_s3.py:442:21: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_s3.py:449:21: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-docker/tests/test-project/flow.py:31:9: error[no-matching-overload] No overload matches arguments
- Found 5533 diagnostics
+ Found 5530 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5118 diagnostics
+ Found 5119 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/stats/_axis_nan_policy.py:713:9: error[invalid-assignment] Object of type `Signature` is not assignable to attribute `__signature__` on type `_Wrapped[(...), Unknown, (*args, *, _no_deco=Literal[False], **kwds), Unknown] | _Wrapped[(...), Unknown, Divergent, Unknown]`
+ scipy/stats/_axis_nan_policy.py:713:9: error[unresolved-attribute] Unresolved attribute `__signature__` on type `_Wrapped[(...), Unknown, (*args, *, _no_deco=Literal[False], **kwds), Unknown]`.


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct metadata = ~11MB
+     struct metadata = ~10MB


```

</details>




---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-11 00:48_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 00:57_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 0 | 3 | 0 |
| `unresolved-attribute` | 3 | 0 | 0 |
| `invalid-argument-type` | 0 | 0 | 2 |
| `invalid-return-type` | 0 | 0 | 2 |
| `unsupported-operator` | 0 | 0 | 2 |
| `possibly-missing-attribute` | 0 | 0 | 1 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **4** | **3** | **7** |

**[Full report with detailed diff](https://cjm-nodiv.ecosystem-663.pages.dev/diff)** ([timing results](https://cjm-nodiv.ecosystem-663.pages.dev/timing))




---

_Comment by @codspeed-hq[bot] on 2025-12-11 01:02_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fnodiv?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21910 will **not alter performance**

<sub>Comparing <code>cjm/nodiv</code> (060a617) with <code>main</code> (3f63ea4)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fnodiv?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @carljm on 2025-12-11 20:15_

With #21906, this no longer has any impact on our mdtests. Pushing a rebase to see if it has any ecosystem impact worth pursuing.

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-12-11 20:23_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-11 20:23_

---

_Comment by @carljm on 2025-12-11 21:33_

Ok, this doesn't seem to hurt performance and there are cases in the ecosystem where it helps, so I think it's worth going ahead with. I'll add at least one example from the ecosystem as an mdtest and then put it up for review.

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-12-12 00:52_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-12 00:53_

---

_Marked ready for review by @carljm on 2025-12-12 00:55_

---

_Review requested from @AlexWaygood by @carljm on 2025-12-12 00:55_

---

_Review requested from @sharkdp by @carljm on 2025-12-12 00:55_

---

_Review requested from @dcreager by @carljm on 2025-12-12 00:55_

---

_@carljm reviewed on 2025-12-12 00:57_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:943 on 2025-12-12 00:57_

If I remove the `!has_divergent_type_in_cycle(self)` requirement, we do get an oscillation in `mdtest/regression/1377_iteration_count_mismatch.md`. So it is possible to oscillate between types containing a `Divergent` from the current cycle. But it should not be possible to reintroduce a `Divergent` from the current cycle once it has been eliminated.

---

_Comment by @carljm on 2025-12-12 00:58_

@mtshiba please take a look and let me know what you think.

---

_Comment by @mtshiba on 2025-12-12 03:50_

Looks good!

---

_Merged by @carljm on 2025-12-12 03:52_

---

_Closed by @carljm on 2025-12-12 03:52_

---

_Branch deleted on 2025-12-12 03:52_

---
