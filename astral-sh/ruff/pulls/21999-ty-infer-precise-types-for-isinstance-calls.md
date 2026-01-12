```yaml
number: 21999
title: "[ty] Infer precise types for `isinstance(…)` calls involving typevars"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-1895
created_at: 2025-12-16T09:23:12Z
updated_at: 2025-12-16T09:36:58Z
url: https://github.com/astral-sh/ruff/pull/21999
synced_at: 2026-01-12T15:57:38Z
```

# [ty] Infer precise types for `isinstance(…)` calls involving typevars

---

_@sharkdp_

## Summary

Infer `Literal[True]` for `isinstance(x, C)` calls when `x: T` and `T` has a bound `B` that satisfies the `isinstance` check against `C`. Similar for constrained typevars.

closes https://github.com/astral-sh/ty/issues/1895

## Test Plan

* New Markdown tests
* Verified the the example in the linked ticket checks without errors

---

_Review requested from @carljm by @sharkdp on 2025-12-16 09:23_

---

_Label `ty` added by @sharkdp on 2025-12-16 09:23_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-16 09:23_

---

_Review requested from @dcreager by @sharkdp on 2025-12-16 09:23_

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 09:25_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-16 09:27_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 41 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_@AlexWaygood approved on 2025-12-16 09:29_

---

_Comment by @sharkdp on 2025-12-16 09:32_

> ```diff
> - src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
> ```

Hmm.. This should not go away, right?

```py
def _get_packages(
    *,
    packages: Sequence[str] | Mapping[str, str] | None,
    name: str,
) -> dict[str, str]:
    if packages is not None:
        if isinstance(packages, Mapping):
            return dict(packages)
        return {str(Path(p).name): p for p in packages}
```

The type of `packages` in the `if isinstance` branch is `(Sequence[str] & Top[Mapping[Unknown, object]]) | Mapping[str, str]`, i.e. an instance of [this problem](https://docs.astral.sh/ty/features/type-checking/#top-and-bottom-materializations). 

---

_Comment by @AlexWaygood on 2025-12-16 09:33_

> Hmm.. This should not go away, right?

Isn't that one of our regular flakes?

---

_Comment by @sharkdp on 2025-12-16 09:33_

> > Hmm.. This should not go away, right?
> 
> Isn't that one of our regular flakes?

Oh, thanks! (Also: wow, those are so annoying)

---

_Merged by @sharkdp on 2025-12-16 09:34_

---

_Closed by @sharkdp on 2025-12-16 09:34_

---

_Branch deleted on 2025-12-16 09:34_

---

_Comment by @AlexWaygood on 2025-12-16 09:34_

Yeah, here's it flaking on another PR a few days ago: https://github.com/astral-sh/ruff/pull/21938#issuecomment-3645700462

---
