```yaml
number: 21886
title: "[ty] support `NewType`s of `float` and `complex`"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: newtype_float
created_at: 2025-12-10T04:28:51Z
updated_at: 2025-12-12T00:52:31Z
url: https://github.com/astral-sh/ruff/pull/21886
synced_at: 2026-01-12T15:57:36Z
```

# [ty] support `NewType`s of `float` and `complex`

---

_@oconnor663_

https://github.com/astral-sh/ty/issues/1818

~~There is a known-failing test case in this first draft, which is related to a very hacky bit that I'm pretty sure is wrong. Feedback needed :) Comments below.~~

Second pass: allow the two special-cased unions as `NewTypeBase` variants, and remove the presumption that the concrete base type is a `ClassType`.

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 04:30_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@oconnor663 reviewed on 2025-12-10 04:31_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder.rs`:5626 on 2025-12-10 04:31_

This feels very dirty to me, and I'd love suggestions for a better way to do it. The obvious alternative would be hardcoding the names `"float"` and `"complex"`, but that seems excessively brittle?

This is also responsible for the failing test in `new_types.md`, because the lookup fails for a forward reference in a stub file.

---

_@oconnor663 reviewed on 2025-12-10 04:31_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:444 on 2025-12-10 04:31_

See https://github.com/astral-sh/ruff/pull/21886#pullrequestreview-3560658200

---

_Label `ty` added by @oconnor663 on 2025-12-10 04:31_

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 04:32_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5119 diagnostics
+ Found 5118 diagnostics

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

_@oconnor663 reviewed on 2025-12-10 04:33_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:185 on 2025-12-10 04:33_

Would it be valuable to expand the happy-path tests here with e.g. assertions about `__class__`, subtyping, etc.? Speaking of which, it's not actually clear to me whether `Foo` here should be considered a subtype of `float`...

---

_Renamed from "[ty] first pass at supporting `NewType`s of `float` and `complex`" to "[ty] support `NewType`s of `float` and `complex`" by @oconnor663 on 2025-12-11 01:22_

---

_@oconnor663 reviewed on 2025-12-11 01:26_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/newtype.rs`:108 on 2025-12-11 01:26_

🚲 shedding requested for the name of this method. Maybe "concrete" has a different technical meaning here that I don't want to stomp on?

---

_Marked ready for review by @oconnor663 on 2025-12-11 01:36_

---

_Review requested from @carljm by @oconnor663 on 2025-12-11 01:36_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-12-11 01:36_

---

_Review requested from @sharkdp by @oconnor663 on 2025-12-11 01:36_

---

_Review requested from @dcreager by @oconnor663 on 2025-12-11 01:36_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-12-11 01:36_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 01:46_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://newtype-float.ecosystem-663.pages.dev/diff)** ([timing results](https://newtype-float.ecosystem-663.pages.dev/timing))




---

_@AlexWaygood approved on 2025-12-11 17:38_

Nice, I like the semantics and the implementation here!

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:238 on 2025-12-11 20:20_

Glad we're getting this important fix in 😆 

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/newtype.rs`:108 on 2025-12-11 20:22_

This name seems fine to me!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/newtype.rs`:198 on 2025-12-11 20:25_

It might be worth explicitly commenting here that mapping base class types is used for normalization and applying type mappings, and neither of these apply to int/float/complex (they are already fully normalized and not generic), so it's fine to ignore them here.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:13965 on 2025-12-11 20:28_

This could be a single method which returns an option of an enum, since these are mutually exclusive possibilities that require effectively the same work to check... that would be slightly less redundant and slightly more efficient? But I think it doesn't really matter, this way is totally fine.

---

_@carljm approved on 2025-12-11 20:28_

I also like these semantics and this implementation; nice work!

---

_@oconnor663 reviewed on 2025-12-11 21:00_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:238 on 2025-12-11 21:00_

I don't know why `blacken` suddenly insisted on changing this but it did :shrug: 

---

_@AlexWaygood reviewed on 2025-12-11 21:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:238 on 2025-12-11 21:03_

I think `mdformat` rather than `blacken-docs` in this case -- `blacken-docs` only cares about the bits in ```` ```py ```` codefences 😄

---

_Label `ecosystem-analyzer` removed by @oconnor663 on 2025-12-11 21:10_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-12-11 21:10_

---

_Merged by @oconnor663 on 2025-12-12 00:43_

---

_Closed by @oconnor663 on 2025-12-12 00:43_

---

_Branch deleted on 2025-12-12 00:43_

---

_Comment by @oconnor663 on 2025-12-12 00:47_

This auto-merge was ultimately correct, but I'm concerned that it happened before CI was finished running on the last "clippy fix" commit. I guess I don't understand the rules for auto-merge?

Edit: Ah Brent set me straight, this is a settings thing:

<img width="1480" height="1190" alt="image" src="https://github.com/user-attachments/assets/50bc2296-1e07-46c8-9da5-723de59070ea" />


---
