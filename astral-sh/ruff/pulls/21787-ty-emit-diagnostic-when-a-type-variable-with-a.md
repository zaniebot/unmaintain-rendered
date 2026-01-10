```yaml
number: 21787
title: "[ty] Emit diagnostic when a type variable with a default is followed by one without a default"
type: pull_request
state: merged
author: 11happy
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ty_typevar_invalid_order
created_at: 2025-12-04T10:16:57Z
updated_at: 2025-12-14T19:49:30Z
url: https://github.com/astral-sh/ruff/pull/21787
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Emit diagnostic when a type variable with a default is followed by one without a default

---

_Pull request opened by @11happy on 2025-12-04 10:16_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR fixes https://github.com/astral-sh/ty/issues/1651

## Test Plan

<!-- How was it tested? -->

I have added mdtest

---

_Review requested from @carljm by @11happy on 2025-12-04 10:16_

---

_Review requested from @AlexWaygood by @11happy on 2025-12-04 10:16_

---

_Review requested from @sharkdp by @11happy on 2025-12-04 10:16_

---

_Review requested from @dcreager by @11happy on 2025-12-04 10:16_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 10:21_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-14 19:31:44.194785139 +0000
+++ new-output.txt	2025-12-14 19:31:47.826806321 +0000
@@ -439,6 +439,7 @@
 generics_basic.py:171:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
 generics_basic.py:172:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
 generics_basic.py:199:5: error[type-assertion-failure] Type `Iterator[Any]` does not match asserted type `Unknown`
+generics_defaults.py:24:40: error[invalid-generic-class] Type parameter `T` without a default cannot follow earlier parameter `DefaultStrT` with a default
 generics_defaults.py:30:1: error[type-assertion-failure] Type `type[NoNonDefaults[str, int]]` does not match asserted type `<class 'NoNonDefaults'>`
 generics_defaults.py:31:1: error[type-assertion-failure] Type `type[NoNonDefaults[str, int]]` does not match asserted type `<class 'NoNonDefaults[str, int]'>`
 generics_defaults.py:32:1: error[type-assertion-failure] Type `type[NoNonDefaults[str, int]]` does not match asserted type `<class 'NoNonDefaults[str, int]'>`
@@ -1025,4 +1026,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1027 diagnostics
+Found 1028 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-04 10:26_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics

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

_Review requested from @MichaReiser by @11happy on 2025-12-04 10:33_

---

_Label `ecosystem-analyzer` added by @MichaReiser on 2025-12-04 11:06_

---

_Renamed from "[ty]: raise diagnostic when a type variable with a default is followed by one without a default" to "[ty] Emit diagnostic when a type variable with a default is followed by one without a default" by @AlexWaygood on 2025-12-04 11:11_

---

_Label `ty` added by @AlexWaygood on 2025-12-04 11:12_

---

_@AlexWaygood reviewed on 2025-12-04 11:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1011 on 2025-12-04 11:12_

```suggestion
    /// - [PEP 696: Type defaults for type parameters](https://peps.python.org/pep-0696/)
    pub(crate) static INVALID_TYPE_PARAM_ORDER = {
```

---

_@MichaReiser reviewed on 2025-12-04 11:24_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:930 on 2025-12-04 11:24_

It probably makes sense to skip this check if the rule is disabled for this file, similar to what we do for `INVALID_GENERIC_CLASS`:

```rust
if self.context.is_lint_enabled(&INVALID_GENERIC_CLASS) {
	...
}

```


---

_Review comment by @11happy on `crates/ty_python_semantic/src/types/diagnostic.rs`:1011 on 2025-12-04 12:02_

done

---

_@11happy reviewed on 2025-12-04 12:02_

---

_@11happy reviewed on 2025-12-04 12:02_

---

_Review comment by @11happy on `crates/ty_python_semantic/src/types/infer/builder.rs`:930 on 2025-12-04 12:02_

done

---

_Comment by @carljm on 2025-12-05 01:47_

Thanks for the PR!

This is emitting two new diagnostics on the conformance suite. The one in `generics_defaults.py` is correct: we should emit this diagnostic there.

The one at https://github.com/python/typing/blob/main/conformance/tests/generics_defaults_specialization.py#L42 looks wrong -- there should not be any diagnostic there, and the one we emit is clearly not right, since it references `T1`, a type variable from the superclass which we are specializing away with `int`. And it claims `T1` is following some other type variable.

I haven't looked at the code here yet, but it looks like in that subclassing scenario we are checking the wrong set of type variables somehow.

There are a lot of new diagnostics in the ecosystem here. I would recommend first fixing the above issue, and then seeing if the ecosystem impact is reduced.

---

_Review request for @sharkdp removed by @sharkdp on 2025-12-05 08:36_

---

_Comment by @11happy on 2025-12-10 07:05_

> Thanks for the PR!
> 
> This is emitting two new diagnostics on the conformance suite. The one in `generics_defaults.py` is correct: we should emit this diagnostic there.
> 
> The one at https://github.com/python/typing/blob/main/conformance/tests/generics_defaults_specialization.py#L42 looks wrong -- there should not be any diagnostic there, and the one we emit is clearly not right, since it references `T1`, a type variable from the superclass which we are specializing away with `int`. And it claims `T1` is following some other type variable.
> 
> I haven't looked at the code here yet, but it looks like in that subclassing scenario we are checking the wrong set of type variables somehow.
> 
> There are a lot of new diagnostics in the ecosystem here. I would recommend first fixing the above issue, and then seeing if the ecosystem impact is reduced.

Thank you for the pointers! , I have updated the approach & added the new test as well.
Thank you : )

---

_Comment by @11happy on 2025-12-14 08:32_

ping! have resolved the merged conflict
would appreciate a review
Thank  you : )

---

_@AlexWaygood approved on 2025-12-14 19:34_

thank you

---

_Merged by @AlexWaygood on 2025-12-14 19:35_

---

_Closed by @AlexWaygood on 2025-12-14 19:35_

---

_Comment by @codspeed-hq[bot] on 2025-12-14 19:49_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/11happy%3Aty_typevar_invalid_order?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21787 will **degrade performances by 5.02%**

<sub>Comparing <code>11happy:ty_typevar_invalid_order</code> (ceea5c5) with <code>main</code> (8bc753b)</sub>



### Summary

`❌ 1` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/11happy%3Aty_typevar_invalid_order?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/11happy%3Aty_typevar_invalid_order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 201.8 s | 212.5 s | -5.02% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/11happy%3Aty_typevar_invalid_order?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---
