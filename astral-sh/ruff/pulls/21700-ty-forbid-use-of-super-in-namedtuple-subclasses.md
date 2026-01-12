```yaml
number: 21700
title: "[ty] Forbid use of `super()` in `NamedTuple` subclasses"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/named
created_at: 2025-11-30T03:02:11Z
updated_at: 2025-11-30T16:03:05Z
url: https://github.com/astral-sh/ruff/pull/21700
synced_at: 2026-01-12T15:57:31Z
```

# [ty] Forbid use of `super()` in `NamedTuple` subclasses

---

_@charliermarsh_

## Summary

The exact behavior around what's allowed vs. disallowed was partly detected through trial and error in the runtime.

I was a little confused by [this comment](https://github.com/python/cpython/pull/129352) that says "`NamedTuple` subclasses cannot be inherited from" because in practice that doesn't appear to error at runtime.

Closes [#1683](https://github.com/astral-sh/ty/issues/1683).


---

_Review requested from @carljm by @charliermarsh on 2025-11-30 03:02_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-11-30 03:02_

---

_Review requested from @sharkdp by @charliermarsh on 2025-11-30 03:02_

---

_Review requested from @dcreager by @charliermarsh on 2025-11-30 03:02_

---

_Label `ty` added by @charliermarsh on 2025-11-30 03:03_

---

_Renamed from "Forbid use of `super()` in `NamedTuple` subclasses" to "[ty] Forbid use of `super()` in `NamedTuple` subclasses" by @charliermarsh on 2025-11-30 03:03_

---

_Comment by @astral-sh-bot[bot] on 2025-11-30 03:04_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-30 03:06_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 44 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Review requested from @MichaReiser by @charliermarsh on 2025-11-30 03:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:414 on 2025-11-30 12:41_

extremely nitpicky comment: my preferred term is "`NamedTuple` class" rather than "`NamedTuple` subclass". The generated class doesn't actually have `NamedTuple` in its MRO (because `NamedTuple` is actually a function rather than a class!) -- the generated class actually inherits directly from `tuple`. The runtime does some magic to swap out `NamedTuple` for `tuple` in the class's bases, so it feels _slightly_ incorrect to refer to it as a "subclass of" `NamedTuple`, even if `NamedTuple` is included in the class's bases.

```suggestion
Using `super()` in a method of a `NamedTuple` class will raise an exception at runtime. In Python
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1 on 2025-11-30 12:42_

You could also add a test to verify that using `super()` on a `NamedTuple` class works fine if it occurs outside the class, e.g.

```py
class F(NamedTuple):
    x: int

super(F, F(42))  # fine
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:5555 on 2025-11-30 12:43_

```suggestion
                                    "Cannot use `super()` in a method of NamedTuple class `{}`",
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:5609 on 2025-11-30 12:43_

```suggestion
                                        "Cannot use `super()` in a method of NamedTuple class `{}`",
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1766 on 2025-11-30 12:43_

```suggestion
    /// Checks for calls to `super()` inside methods of `NamedTuple` classes.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1769 on 2025-11-30 12:44_

```suggestion
    /// Using `super()` in a method of a `NamedTuple` class will raise an exception at runtime.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1786 on 2025-11-30 12:45_

```suggestion
        summary: "detects `super()` calls in methods of `NamedTuple` classes",
        status: LintStatus::preview("0.0.1-alpha.30"),
```

---

_@AlexWaygood approved on 2025-11-30 12:45_

Thank you, this is excellent! Just a few nitpicks really

---

_Comment by @AlexWaygood on 2025-11-30 12:49_

> I was a little confused by [this comment](https://github.com/python/cpython/pull/129352) that says "`NamedTuple` subclasses cannot be inherited from" because in practice that doesn't appear to error at runtime.

yeah, I think that comment was just incorrect

---

_Merged by @charliermarsh on 2025-11-30 15:49_

---

_Closed by @charliermarsh on 2025-11-30 15:49_

---

_Branch deleted on 2025-11-30 15:49_

---

_Comment by @codspeed-hq[bot] on 2025-11-30 16:03_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fnamed?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21700 will **improve performances by 4.45%**

<sub>Comparing <code>charlie/named</code> (664df36) with <code>main</code> (b02e821)</sub>



### Summary

`⚡ 1` improvement  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fnamed?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 207.5 s | 198.7 s | +4.45% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fnamed?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---
