```yaml
number: 21210
title: "[ty] Improve generic call expression inference"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/generic-call-inference
created_at: 2025-11-03T00:32:05Z
updated_at: 2025-11-10T21:54:51Z
url: https://github.com/astral-sh/ruff/pull/21210
synced_at: 2026-01-12T15:57:19Z
```

# [ty] Improve generic call expression inference

---

_@ibraheemdev_

## Summary

Implements https://github.com/astral-sh/ty/issues/1356 and https://github.com/astral-sh/ty/issues/136#issuecomment-3413669994.


---

_Label `ty` added by @ibraheemdev on 2025-11-03 00:32_

---

_Comment by @github-actions[bot] on 2025-11-03 00:34_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @github-actions[bot] on 2025-11-03 00:48_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dulwich (https://github.com/dulwich/dulwich)
- dulwich/pack.py:1702:83: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 175 diagnostics
+ Found 174 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_validators.py:161:16: error[invalid-return-type] Return type does not match returned value: expected `Pattern[bytes]`, found `Pattern[str | bytes]`
+ pydantic/_internal/_validators.py:161:16: error[invalid-return-type] Return type does not match returned value: expected `Pattern[bytes]`, found `Pattern[bytes | str]`

pandera (https://github.com/pandera-dev/pandera)
+ tests/mypy/pandas_modules/pandas_dataframe.py:35:12: error[no-matching-overload] No overload of bound method `pipe` matches arguments
- tests/mypy/pandas_modules/pandas_dataframe.py:41:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[Schema] | DataFrame[SchemaOut]`
+ tests/mypy/pandas_modules/pandas_dataframe.py:41:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[SchemaOut] | DataFrame[Schema]`
- Found 1637 diagnostics
+ Found 1638 diagnostics

Expression (https://github.com/cognitedata/Expression)
- tests/test_result.py:491:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Literal[42], Any]`, found `Result[Literal[0], Any]`
- tests/test_result.py:509:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Any, Literal["original error"]]`, found `Result[Any, Literal["new error"]]`
- Found 200 diagnostics
+ Found 198 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/domains/python/__init__.py:672:42: error[index-out-of-bounds] Index 0 is out of bounds for string `Literal[""]` with length 0
- Found 515 diagnostics
+ Found 514 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/prototype/transforms/_misc.py:34:13: error[invalid-assignment] Object of type `dict[Any, Sequence[int] & ~Top[dict[Unknown, Unknown]]]` is not assignable to `Sequence[int] | dict[type, Sequence[int] | None]`
- Found 1503 diagnostics
+ Found 1502 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- tests/unittests/sources/test_lxd.py:40:5: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/unittests/test_net.py:5337:41: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `Unknown | list[Unknown | str] | dict[Unknown | str, Unknown | str | None] | dict[Unknown, Unknown]`
- tests/unittests/test_net.py:5494:41: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `Unknown | list[Unknown | str] | dict[Unknown | str, Unknown | str | None] | dict[Unknown | str, Unknown | str]`
- Found 1134 diagnostics
+ Found 1131 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/build.py:1293:9: error[invalid-assignment] Object of type `list[Sequence[str | Literal[False]] | Literal[False]]` is not assignable to attribute `install_dir` of type `list[str | Literal[False]]`
- Found 1664 diagnostics
+ Found 1665 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/format.py:74:16: error[invalid-return-type] Return type does not match returned value: expected `PyprojectFormatter | dict[str, str]`, found `dict[str, Literal["*"]]`
- Found 49 diagnostics
+ Found 48 diagnostics

colour (https://github.com/colour-science/colour)
- colour/recovery/otsu2018.py:1606:21: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]) | (bound method dict[Unknown, Unknown].__setitem__(key: Unknown, value: Unknown, /) -> None)` cannot be called with a key of type `Node_Otsu2018` and a value of type `Unknown | list[Unknown] | dict[Unknown, Unknown] | int` on object of type `Unknown | list[Unknown] | dict[Unknown, Unknown] | int`
- colour/recovery/otsu2018.py:1607:21: error[unsupported-operator] Operator `+=` is unsupported between objects of type `list[Unknown]` and `Literal[1]`
- colour/recovery/otsu2018.py:1607:21: error[unsupported-operator] Operator `+=` is unsupported between objects of type `dict[Unknown, Unknown]` and `Literal[1]`
- colour/recovery/otsu2018.py:1610:17: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]) | (bound method dict[Unknown, Unknown].__setitem__(key: Unknown, value: Unknown, /) -> None)` cannot be called with a key of type `Node_Otsu2018` and a value of type `int` on object of type `Unknown | list[Unknown] | dict[Unknown, Unknown] | int`
- colour/recovery/otsu2018.py:1610:54: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | list[Unknown] | dict[Unknown, Unknown] | int`
- colour/recovery/otsu2018.py:1611:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | list[Unknown] | dict[Unknown, Unknown] | int`
- Found 567 diagnostics
+ Found 561 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/test/unit/test_series.py:4489:37: error[no-matching-overload] No overload of function `round` matches arguments
- Found 2004 diagnostics
+ Found 2005 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/homekit/config_flow.py:502:9: error[invalid-assignment] Object of type `Any` is not assignable to `EntityFilterDict`
+ homeassistant/components/homekit/config_flow.py:546:9: error[invalid-assignment] Object of type `Any` is not assignable to `EntityFilterDict`
+ homeassistant/components/homekit/config_flow.py:649:9: error[invalid-assignment] Object of type `Any` is not assignable to `EntityFilterDict`
- Found 14455 diagnostics
+ Found 14458 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-03 00:51_

---

_Comment by @github-actions[bot] on 2025-11-03 00:57_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 4 | 3 | 0 |
| `invalid-argument-type` | 0 | 3 | 0 |
| `invalid-return-type` | 0 | 1 | 2 |
| `possibly-missing-attribute` | 0 | 3 | 0 |
| `no-matching-overload` | 2 | 0 | 0 |
| `unsupported-operator` | 0 | 2 | 0 |
| `index-out-of-bounds` | 0 | 1 | 0 |
| `non-subscriptable` | 0 | 1 | 0 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **6** | **15** | **2** |

**[Full report with detailed diff](https://ibraheem-generic-call-infere.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-generic-call-infere.ecosystem-663.pages.dev/timing))


---

_Comment by @codspeed-hq[bot] on 2025-11-03 01:23_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fgeneric-call-inference?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21210 will **degrade performances by 7.82%**

<sub>Comparing <code>ibraheem/generic-call-inference</code> (4aa5f3a) with <code>main</code> (d258302)</sub>



### Summary

`❌ 4` regressions  
`✅ 18` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fgeneric-call-inference?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fgeneric-call-inference?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 861.8 ms | 927.5 ms | -7.08% |
| ❌ | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fgeneric-call-inference?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 15.1 s | 16.3 s | -7.82% |
| ❌ | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fgeneric-call-inference?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 43.1 s | 45.4 s | -5.08% |
| ❌ | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fgeneric-call-inference?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 13 s | 13.6 s | -4.63% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fgeneric-call-inference?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-11-03 18:23_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-03 18:23_

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-11-03 20:36_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-03 20:36_

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-11-03 21:11_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-03 21:11_

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-11-04 16:55_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-04 16:55_

---

_Marked ready for review by @ibraheemdev on 2025-11-04 20:25_

---

_Review requested from @carljm by @ibraheemdev on 2025-11-04 20:25_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-11-04 20:25_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-11-04 20:25_

---

_Review requested from @dcreager by @ibraheemdev on 2025-11-04 20:25_

---

_Comment by @ibraheemdev on 2025-11-04 20:27_

I imagine we could avoid the performance regression by special casing `x: ... | None = f()` to avoid the inference attempt using only `None` as the type context, but I'm a little hesitant to make that change, as there are (rare) cases where we might want to narrow to `None`. Most of the regressions seem relatively reasonable, other than `colour`.

Also note that a lot of the diagnostic improvements won't be seen in this PR directly, but https://github.com/astral-sh/ruff/pull/20933 (which is currently stacked on top of this PR).

---

_@MichaReiser reviewed on 2025-11-06 12:16_

Do we understand what's causing the performance regression? Is it that we need to clone bindings or is it something else?

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-11-06 14:48_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-06 14:48_

---

_Comment by @ibraheemdev on 2025-11-06 15:43_

Most of the performance regressions have been resolved. There is an unavoidable regression when assigning the result of a generic call to a union, as we have to attempt inference multiple times for each element of the union.

---

_Comment by @carljm on 2025-11-06 15:53_

Can you take a look at the new diagnostics in the ecosystem report? Just taking a quick glance, I see in HomeAssistant a few occurrences of "object of type `Any` is not assignable ..." which definitely doesn't look right.

---

_Comment by @ibraheemdev on 2025-11-06 17:01_

It looks like those failures are due to `TypedDict` being assignable to `None`, which confuses overload evaluation. The weird diagnostic should be fixed by https://github.com/astral-sh/ruff/pull/21267, the actual type we infer (with type context) is `Any | None` there, not `Any`.

I added a failing test to track those.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:420 on 2025-11-07 17:30_

Nit: should these just be another heading layer? Our usual approach is to reserve filenames for tests that actually need to be multi-file (because they are testing cross-module behavior / imports) and use headings when we are just trying to group related tests together.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:551 on 2025-11-07 17:34_

We should probably do literal promotion here?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:531 on 2025-11-10 19:39_

is this for all generic classes? or just invariant ones? All the examples in this section use invariant ones

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:459 on 2025-11-10 19:40_

```suggestion
A function's arguments are also inferred using the type context:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:584 on 2025-11-10 19:59_

nit: there's a builtin function `id()` in Python which you're shadowing here; maybe call this `identity` to avoid confusion?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:887 on 2025-11-10 20:02_

nit

```suggestion

    /// If the type is a specialized instance of the given `KnownClass`, returns the specialization.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:897 on 2025-11-10 20:03_

```suggestion
    /// If this type is a class instance, returns its specialization.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:902 on 2025-11-10 20:03_

```suggestion
    /// If the type is a specialized instance of the given class, returns the specialization.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:2759 on 2025-11-10 20:12_

TIL about `Option::zip()`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6046 on 2025-11-10 20:22_

can we skip the re-inference if `was_in_multi_inference == true`? Because that means that we wouldn't actually have diagnostics enabled for this later `infer_all_argument_types()` call either. No tests fail if I apply this edit to your branch:

```suggestion
            // Successfully narrowed to an element of the union.
            self.context.set_multi_inference(was_in_multi_inference);

            // If necessary, infer the argument types again with diagnostics enabled.
            if !was_in_multi_inference {
                self.infer_all_argument_types(
                    ast_arguments,
                    argument_types,
                    bindings,
                    narrowed_tcx,
                    MultiInferenceState::Intersect,
                );
            }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6114 on 2025-11-10 20:27_

nit: I'd prefer to have this as two `debug_assert_eq!()` calls so it's easy to identify which condition is failing (if one of them fails)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:10928 on 2025-11-10 20:29_

```suggestion
    /// Note that `Overwrite` does not interact well with nested inferences: it
    /// overwrites values that were written with `MultiInferenceState::Intersect`.
```

---

_@AlexWaygood approved on 2025-11-10 20:31_

I'm not too familiar with this machinery, but from what I can tell this all looks reasonable to me, and I definitely approve of all of the semantics changes I see in the tests! This is, as ever, very cool work

---

_@ibraheemdev reviewed on 2025-11-10 21:25_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:531 on 2025-11-10 21:25_

This currently applies to all generic classes. I'm open to changing it, but see https://github.com/astral-sh/ruff/pull/21070#issuecomment-3445659355 for my reasoning there.

---

_@ibraheemdev reviewed on 2025-11-10 21:26_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:6046 on 2025-11-10 21:26_

Great idea!

---

_Merged by @ibraheemdev on 2025-11-10 21:29_

---

_Closed by @ibraheemdev on 2025-11-10 21:29_

---

_Branch deleted on 2025-11-10 21:29_

---

_@carljm reviewed on 2025-11-10 21:43_

Oops, I had a couple draft comments from a review I started on Friday but hadn't completed yet. Nothing important, just things to consider for future.

---

_@AlexWaygood reviewed on 2025-11-10 21:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:551 on 2025-11-10 21:54_

that might be fixed in https://github.com/astral-sh/ruff/pull/21320 :-)

---
