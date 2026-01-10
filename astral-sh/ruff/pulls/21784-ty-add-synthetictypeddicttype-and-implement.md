```yaml
number: 21784
title: "[ty] add `SyntheticTypedDictType` and implement `normalized` and `is_equivalent_to`"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: synthesized_typeddict
created_at: 2025-12-04T04:04:12Z
updated_at: 2025-12-10T20:51:46Z
url: https://github.com/astral-sh/ruff/pull/21784
synced_at: 2026-01-10T16:42:11Z
```

# [ty] add `SyntheticTypedDictType` and implement `normalized` and `is_equivalent_to`

---

_Pull request opened by @oconnor663 on 2025-12-04 04:04_

This PR cribs a lot of @ibraheemdev's work on https://github.com/astral-sh/ruff/pull/20732.

This depends on a couple of upstream changes, and the first commit is a temporary/dummy commit that pins git hashes for these:

- https://github.com/salsa-rs/salsa/pull/1033
- https://github.com/bircni/get-size2/pull/40

Those pins (at least the second one, which overwrites a published version number) aren't intended to land, and ~I'll need to fix up this PR once / assuming I can get the upstream changes shipped.~ Update: This is done.

---

_Review requested from @carljm by @oconnor663 on 2025-12-04 04:04_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-12-04 04:04_

---

_Review requested from @sharkdp by @oconnor663 on 2025-12-04 04:04_

---

_Review requested from @dcreager by @oconnor663 on 2025-12-04 04:04_

---

_Review requested from @MichaReiser by @oconnor663 on 2025-12-04 04:04_

---

_Renamed from "add `SyntheticTypedDictType` and implement `normalized` and `is_equivalent_to`" to "[ty] add `SyntheticTypedDictType` and implement `normalized` and `is_equivalent_to`" by @oconnor663 on 2025-12-04 04:04_

---

_Label `ty` added by @oconnor663 on 2025-12-04 04:04_

---

_@oconnor663 reviewed on 2025-12-04 04:06_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:743 on 2025-12-04 04:06_

Should a `SynthesizedTypedDictType` have a name? I've gone with no here since it's the output of `normalized`, but if this is going to be reused for functional `TypedDict`s at some point, clearly those will need to put their name somewhere.

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 04:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@oconnor663 reviewed on 2025-12-04 04:06_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/display.rs`:920 on 2025-12-04 04:06_

What's a good way to get test coverage of displaying a normalized value?

---

_@oconnor663 reviewed on 2025-12-04 04:07_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types.rs`:4294 on 2025-12-04 04:07_

I've copied this special case from https://github.com/astral-sh/ruff/pull/20732, but I'm not sure how to test it. What's a good way to access a member of a normalized type?

---

_@chatgpt-codex-connector[bot] reviewed on 2025-12-04 04:07_


### 💡 Codex Review

Here are some automated review suggestions for this pull request.
    

<details> <summary>ℹ️ About Codex in GitHub</summary>
<br/>

[Your team has set up Codex to review pull requests in this repo](http://chatgpt.com/codex/settings/general). Reviews are triggered when you
- Open a pull request for review
- Mark a draft as ready
- Comment "@codex review".

If Codex has suggestions, it will comment; otherwise it will react with 👍.




Codex can also answer questions or update the PR. Try commenting "@codex address that feedback".
            
</details>

---

_Review comment by @chatgpt-codex-connector[bot] on `crates/ty_python_semantic/src/types/typed_dict.rs`:307 on 2025-12-04 04:07_

**<sub><sub>![P0 Badge](https://img.shields.io/badge/P0-red?style=flat)</sub></sub>  Clone items before interning normalized TypedDict**

In `TypedDictType::normalized_impl` you call `SynthesizedTypedDictType::new(db, self.params(db), self.items(db))`, but `items` returns a borrowed `&FxOrderMap<Name, Field>` while the interned constructor expects an owned map. This does not compile (a shared reference cannot be moved into the interned value), so any attempt to normalize a TypedDict will fail at build time. You need to clone or otherwise materialize an owned map before interning.

Useful? React with 👍 / 👎.

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 04:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/structured_configs/_implementations.py:2982:60: error[invalid-argument-type] Argument to function `make_dataclass` is incorrect: Expected `dict[str, Any] | None`, found `bool`
+ src/hydra_zen/structured_configs/_implementations.py:2982:60: error[invalid-argument-type] Argument to function `make_dataclass` is incorrect: Expected `bool`, found `str | None`
+ src/hydra_zen/structured_configs/_implementations.py:2982:60: error[invalid-argument-type] Argument to function `make_dataclass` is incorrect: Expected `bool`, found `dict[str, Any] | None`
+ src/hydra_zen/structured_configs/_implementations.py:3342:52: error[invalid-argument-type] Argument to function `make_dataclass` is incorrect: Expected `dict[str, Any] | None`, found `bool`
+ src/hydra_zen/structured_configs/_implementations.py:3342:52: error[invalid-argument-type] Argument to function `make_dataclass` is incorrect: Expected `bool`, found `str | None`
+ src/hydra_zen/structured_configs/_implementations.py:3342:52: error[invalid-argument-type] Argument to function `make_dataclass` is incorrect: Expected `bool`, found `dict[str, Any] | None`
- Found 540 diagnostics
+ Found 546 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 41 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_schema_gather.py:126:28: error[invalid-key] Unknown key "steps" for TypedDict `DataclassSchema` - did you mean "type"?
+ pydantic/_internal/_schema_gather.py:126:28: error[invalid-key] Unknown key "steps" for TypedDict `DataclassSchema` - did you mean "slots"?
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



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`

prefect (https://github.com/PrefectHQ/prefect)
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`
+ WARN expected `heap_size` to be provided by Salsa query `class_based_items`


```

</details>




---

_@oconnor663 reviewed on 2025-12-04 04:10_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:105 on 2025-12-04 04:10_

Making `.items()` work here is the ultimately why this PR replaces `FxIndexMap` with `FxOrderMap` in several places, and why it needs the upstream changes. The items need to be `Hash` to be a valid member of `struct SynthesizedTypedDictType` (which is interned), but they also need to be `salsa::Update` to be a valid return type from `ClassLiteral::fields` (which is tracked).

---

_@oconnor663 reviewed on 2025-12-04 04:13_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/class.rs`:1435 on 2025-12-04 04:13_

Does discarding `first_declaration` cause any problems here? Normalization does rely on it, and one of the union test cases breaks if we retain `Some(_)` here.

---

_@oconnor663 reviewed on 2025-12-04 04:14_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:305 on 2025-12-04 04:14_

Would it be better to inline `SynthesizedTypedDictType::normalized_impl` here instead of creating two instances here and throwing the first away?

---

_@oconnor663 reviewed on 2025-12-04 04:16_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:307 on 2025-12-04 04:16_

Actually is _is_ surprising to me that this works. (But it does work.)

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:105 on 2025-12-04 04:18_

Relevant: https://github.com/astral-sh/ruff/pull/21512/files#r2538696880

---

_@oconnor663 reviewed on 2025-12-04 04:18_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 04:24_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @codspeed-hq[bot] on 2025-12-04 04:24_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/synthesized_typeddict?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21784 will **not alter performance**

<sub>Comparing <code>synthesized_typeddict</code> (c127766) with <code>main</code> (a2fb2ee)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/synthesized_typeddict?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@MichaReiser reviewed on 2025-12-04 08:08_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/typed_dict.rs`:307 on 2025-12-04 08:08_

Salsa allows interned value lookups by reference and only clones creates an owned instance (by cloning) if the value hasn't been interned yet

I'm not sure I understand what chatgpt is asking recommending or even trying to protect against.

---

_@AlexWaygood reviewed on 2025-12-04 11:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:105 on 2025-12-04 11:49_

...I think `TypedDict::items()` actually needs to return a `BTreeMap` rather than an `FxIndexMap` or an `FxOrderMap`. It's important for `ClassLiteral::fields()` to return a map that keeps track of insertion order, because for dataclasses and namedtuples, we need to be able to check whether any fields without default values come after fields with default values. But `TypedDict::items()` has different concerns. We need this assertion to pass, and it currently doesn't on your branch, because the fields of the `TypedDict` appear in different orders for the two `TypedDict`s:

```py
from ty_extensions import static_assert, is_equivalent_to
from typing import TypedDict

class Foo(TypedDict):
    x: int
    y: str

class Baz(TypedDict):
    y: str
    x: int

static_assert(is_equivalent_to(int | Foo, int | Baz))
```

that assertion will only pass if the `.items()` returned for `Foo` compares equal to the `.items()` returned by `Baz`. I.e., the maps must either have an equality semantics that doesn't care about order, or the maps must maintain _sorted_ order rather than _insertion_ order.

`ProtocolInterface` is a wrapper around a `BTreeMap` (rather than `FxHashMap`, `IndexMap` or `OrderMap`), for very similar reasons:

https://github.com/astral-sh/ruff/blob/b8ecc83a54fd5d7955bf1ab4fb82fe18dcb52283/crates/ty_python_semantic/src/types/protocol_class.rs#L174-L184

---

_Comment by @AlexWaygood on 2025-12-04 14:04_

> This depends on a couple of upstream changes, and the first commit is a temporary/dummy commit that pins git hashes for these:
> 
> - https://github.com/salsa-rs/salsa/pull/1033
> - https://github.com/bircni/get-size2/pull/40

while we wait for the upstream `getsize2` change to land, you can use the implementations of `heap_size` we have in this repo for `OrderMap` and `OrderSet`: https://github.com/astral-sh/ruff/blob/326025d45f87548caba9a56c5606d80f85abc5ff/crates/ruff_memory_usage/src/lib.rs#L46-L57

But see my comment at https://github.com/astral-sh/ruff/pull/21784#discussion_r2588745548 -- I think we may actually need to use a `BTreeMap` for the items of a synthesized typeddict, since we need two equivalent typeddicts to normalize to synthesized typeddicts that compare equal

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:743 on 2025-12-04 14:06_

They should not have a name -- two equivalent TypedDicts must normalize to synthesized typeddicts that compare equal, and that becomes harder if synthesized typeddicts carry a name around with them.

If we reuse this infrastructure for functional typeddicts, functional typeddicts will have to be a wrapper around a synthesized typeddict, since functional typeddicts definitely need to carry a name around with them. But it's this kind of question that makes me sceptical that functional typeddicts and synthesized typeddicts really have that much in common; I'm not sure we _will_ end up reusing this infrastructure much for functional typeddicts.

---

_@AlexWaygood reviewed on 2025-12-04 14:06_

---

_@AlexWaygood reviewed on 2025-12-04 14:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:920 on 2025-12-04 14:07_

For protocols, I just added some Rust unit tests at the bottom of this module:

https://github.com/astral-sh/ruff/blob/326025d45f87548caba9a56c5606d80f85abc5ff/crates/ty_python_semantic/src/types/display.rs#L2322-L2345

---

_@AlexWaygood reviewed on 2025-12-04 14:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1435 on 2025-12-04 14:10_

I think this is the right thing to do. We only use `first_declaration` as a way to easily figure out what the appropriate range is for subdiagnostics in various places. Synthesized typeddicts should only ever be created "on the fly" so shouldn't feature in diagnostics much.

---

_Comment by @bircni on 2025-12-04 15:54_

Just published the new get-size version with the implementation!

---

_Comment by @oconnor663 on 2025-12-04 16:04_

@bircni incredible thank you!

---

_@oconnor663 reviewed on 2025-12-04 16:35_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:105 on 2025-12-04 16:35_

Hmm, a type mismatch between `ClassLiteral::fields` and `SynthesizedTypedDictType::items` here will be awkward, because currently we want to return a reference to the map. We could refactor things to return an opaque iterator instead (and then we'd need a separate method for lookup-by-name?). Or I could just make sure that the `SynthesizedTypedDictType` map is always sorted during construction (or possibly during normalization, though is_equivalent_to doesn't normalize, so construction is probably better). Do you have a preference?

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:105 on 2025-12-04 19:25_

Ah I forgot that I did an order-independent comparison in `is_equivalent_to_impl`, so I only need to sort in `normalized_impl`. I think this works: c985fa9081d6d2a9adc10e32fadf3a0862e2b7ed

---

_@oconnor663 reviewed on 2025-12-04 19:25_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-12-04 19:31_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 19:40_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unsupported-base` | 2 | 0 | 0 |
| `redundant-cast` | 0 | 0 | 1 |
| **Total** | **2** | **0** | **1** |

**[Full report with detailed diff](https://synthesized-typeddict.ecosystem-663.pages.dev/diff)** ([timing results](https://synthesized-typeddict.ecosystem-663.pages.dev/timing))




---

_@oconnor663 reviewed on 2025-12-04 19:46_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/display.rs`:920 on 2025-12-04 19:46_

e1976473c0fd0dc3b9e36bdf02f67c3a225677c3

---

_Comment by @oconnor663 on 2025-12-04 19:55_

Hmm, the change I made to redundant cast warnings changed one unrelated union cast warning in the ecosystem analysis. Is this better or worse than before? Before:

```
warning[redundant-cast]: Value is already of type `Literal["function", "class", "method", "module"]`
  --> basic_checker.py:85:21
   |
83 |     lines = ["type", "number", "old number", "difference", "%documented", "%badname"]
84 |     for node_type in ("module", "class", "method", "function"):
85 |         node_type = cast(Literal["function", "class", "method", "module"], node_type)
   |                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
86 |         new = stats.get_node_count(node_type)
87 |         old = old_stats.get_node_count(node_type) if old_stats else None
   |
info: rule `redundant-cast` is enabled by default
```

After:
```
warning[redundant-cast]: Value is already of type `Literal["module", "class", "method", "function"]`, which is equivalent to `Literal["function", "class", "method", "module"]`
  --> basic_checker.py:85:21
   |
83 |     lines = ["type", "number", "old number", "difference", "%documented", "%badname"]
84 |     for node_type in ("module", "class", "method", "function"):
85 |         node_type = cast(Literal["function", "class", "method", "module"], node_type)
   |                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
86 |         new = stats.get_node_count(node_type)
87 |         old = old_stats.get_node_count(node_type) if old_stats else None
   |
info: rule `redundant-cast` is enabled by default
```

Is this unhelpful with unions? Like it's _interesting_ and possibly _surprising_ that `Foo` and `Bar` typeddicts can be equivalent, but it's _boring_ and _obvious_ that the same union in a different order is the same union? I could check specifically for `TypedDict` (and `Protocol`) when emitting this?

---

_Comment by @oconnor663 on 2025-12-04 20:46_

@AlexWaygood says the `beartype` diagnostic in the ecosystem report is flaky, so other than the "eye doctor: better or worse, better or worse" question above, the report is clean.

---

_Comment by @carljm on 2025-12-05 00:14_

I think the redundant-cast change is fine as-is, if anything a small improvement, even in the "obvious" case. I suppose we could even make it more explicit by naming both types in any case where they are equivalent-but-not-identical: "Value is already of type X which is equivalent to type Y". But I don't know that it's worth bothering to do that unless we get evidence that someone is confused by the shorter form that assumes you can read the `cast` type yourself.

EDIT: oops, I failed to scroll to the right and notice that the longer form I suggested is exactly what you already did! I think that's just fine, even in a more-obvious case.

---

_@oconnor663 reviewed on 2025-12-05 05:33_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types.rs`:4294 on 2025-12-05 05:33_

I've expanded the TODO comment here.

---

_Comment by @oconnor663 on 2025-12-05 15:09_

~~The last unaddressed issue that I know of on this PR is the Pydantic regression.~~ I'm looking at that now. It's presumably our old friend the gigantic recursive union?

Edit: _very_ premature :grimacing: 

---

_Comment by @AlexWaygood on 2025-12-05 15:12_

I would only _expect_ a pydantic regression on this PR if (1) we're using `Type::is_equivalent_to` in our codebase somewhere where we shouldn't be or (2) pydantic is making extensive use of `assert_type` and/or `cast`. It might be worth checking those things?

---

_@AlexWaygood reviewed on 2025-12-05 18:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:105 on 2025-12-05 18:35_

It's _possible_ to make it work with a `BTreeMap`:

<details>
<summary>Patch</summary>

```diff
diff --git a/crates/ty_python_semantic/src/types/diagnostic.rs b/crates/ty_python_semantic/src/types/diagnostic.rs
index bf32c6aa7f..d839b5d011 100644
--- a/crates/ty_python_semantic/src/types/diagnostic.rs
+++ b/crates/ty_python_semantic/src/types/diagnostic.rs
@@ -33,7 +33,7 @@ use crate::types::{
 };
 use crate::types::{KnownInstanceType, MemberLookupPolicy};
 use crate::{
-    Db, DisplaySettings, FxIndexMap, FxOrderMap, Module, ModuleName, Program, declare_lint,
+    Db, DisplaySettings, FxIndexMap, Module, ModuleName, Program, declare_lint,
 };
 use itertools::Itertools;
 use ruff_db::{
@@ -46,6 +46,7 @@ use ruff_python_ast::token::parentheses_iterator;
 use ruff_python_ast::{self as ast, AnyNodeRef, StringFlags};
 use ruff_text_size::{Ranged, TextRange};
 use rustc_hash::FxHashSet;
+use std::collections::BTreeMap;
 use std::fmt::{self, Formatter};
 
 /// Registers all known type check lints.
@@ -3473,7 +3474,7 @@ pub(crate) fn report_invalid_key_on_typed_dict<'db>(
     typed_dict_ty: Type<'db>,
     full_object_ty: Option<Type<'db>>,
     key_ty: Type<'db>,
-    items: &FxOrderMap<Name, Field<'db>>,
+    items: &BTreeMap<Name, Field<'db>>,
 ) {
     let db = context.db();
     if let Some(builder) = context.report_lint(&INVALID_KEY, key_node) {
diff --git a/crates/ty_python_semantic/src/types/display.rs b/crates/ty_python_semantic/src/types/display.rs
index f5e98ec86a..9abd93bdee 100644
--- a/crates/ty_python_semantic/src/types/display.rs
+++ b/crates/ty_python_semantic/src/types/display.rs
@@ -2307,9 +2307,12 @@ impl<'db> FmtDetailed<'db> for DisplayKnownInstanceRepr<'db> {
 
 #[cfg(test)]
 mod tests {
+    use std::collections::BTreeMap;
+
     use insta::assert_snapshot;
     use ruff_python_ast::name::Name;
 
+    use crate::Db;
     use crate::db::tests::setup_db;
     use crate::place::typing_extensions_symbol;
     use crate::types::class::{Field, FieldKind};
@@ -2318,7 +2321,6 @@ mod tests {
         KnownClass, Parameter, Parameters, Signature, StringLiteralType, Type, TypedDictParams,
         TypedDictType,
     };
-    use crate::{Db, FxOrderMap};
 
     #[test]
     fn string_literal_display() {
@@ -2367,7 +2369,7 @@ mod tests {
     fn synthesized_typeddict_display() {
         let db = setup_db();
 
-        let mut items = FxOrderMap::default();
+        let mut items = BTreeMap::default();
         items.insert(
             Name::new("foo"),
             Field {
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index 6e4d2d5726..3b8affad9c 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -1,3 +1,5 @@
+use std::collections::BTreeMap;
+
 use bitflags::bitflags;
 use ruff_db::diagnostic::{Annotation, Diagnostic, Span, SubDiagnostic, SubDiagnosticSeverity};
 use ruff_db::parsed::parsed_module;
@@ -12,6 +14,7 @@ use super::diagnostic::{
     report_missing_typed_dict_key,
 };
 use super::{ApplyTypeMappingVisitor, Type, TypeMapping, visitor};
+use crate::Db;
 use crate::semantic_index::definition::Definition;
 use crate::types::constraints::ConstraintSet;
 use crate::types::generics::InferableTypeVars;
@@ -20,7 +23,6 @@ use crate::types::{
     IsDisjointVisitor, IsEquivalentVisitor, KnownClass, NormalizedVisitor, Parameter, Parameters,
     Signature, StringLiteralType, TypeContext, TypeRelation, TypeVarVariance, UnionType,
 };
-use crate::{Db, FxOrderMap};
 
 use ordermap::OrderSet;
 
@@ -67,12 +69,22 @@ impl<'db> TypedDictType<'db> {
         }
     }
 
-    pub(crate) fn items(self, db: &'db dyn Db) -> &'db FxOrderMap<Name, Field<'db>> {
+    pub(crate) fn items(self, db: &'db dyn Db) -> &'db BTreeMap<Name, Field<'db>> {
+        #[salsa::tracked(returns(ref))]
+        fn class_based_items<'db>(
+            db: &'db dyn Db,
+            class: ClassType<'db>,
+        ) -> BTreeMap<Name, Field<'db>> {
+            let (class_literal, specialization) = class.class_literal(db);
+            class_literal
+                .fields(db, specialization, CodeGeneratorKind::TypedDict)
+                .into_iter()
+                .map(|(name, field)| (name.clone(), field.clone()))
+                .collect()
+        }
+
         match self {
-            Self::Class(defining_class) => {
-                let (class_literal, specialization) = defining_class.class_literal(db);
-                class_literal.fields(db, specialization, CodeGeneratorKind::TypedDict)
-            }
+            Self::Class(defining_class) => class_based_items(db, defining_class),
             Self::Synthesized(synthesized) => synthesized.items(db),
         }
     }
@@ -355,14 +367,14 @@ impl<'db> TypedDictType<'db> {
     pub(crate) fn synthesized_member(
         db: &'db dyn Db,
         instance_ty: Type<'db>,
-        fields: &FxOrderMap<Name, Field<'db>>,
+        fields: impl IntoIterator<Item = (&'db Name, &'db Field<'db>)>,
         name: &str,
     ) -> Option<Type<'db>> {
         match name {
             "__setitem__" => {
                 // Add (key type, value type) overloads for all TypedDict items ("fields") that are not read-only:
                 let mut writeable_fields = fields
-                    .iter()
+                    .into_iter()
                     .filter(|(_, field)| !field.is_read_only())
                     .peekable();
 
@@ -418,7 +430,7 @@ impl<'db> TypedDictType<'db> {
             }
             "__getitem__" => {
                 // Add (key -> value type) overloads for all TypedDict items ("fields"):
-                let overloads = fields.iter().map(|(name, field)| {
+                let overloads = fields.into_iter().map(|(name, field)| {
                     let key_type = Type::StringLiteral(StringLiteralType::new(db, name.as_str()));
 
                     Signature::new(
@@ -550,7 +562,7 @@ impl<'db> TypedDictType<'db> {
             }
             "pop" => {
                 let overloads = fields
-                    .iter()
+                    .into_iter()
                     .filter(|(_, field)| {
                         // Only synthesize `pop` for fields that are not required.
                         !field.is_required()
@@ -608,7 +620,7 @@ impl<'db> TypedDictType<'db> {
                 )))
             }
             "setdefault" => {
-                let overloads = fields.iter().map(|(name, field)| {
+                let overloads = fields.into_iter().map(|(name, field)| {
                     let key_type = Type::StringLiteral(StringLiteralType::new(db, name.as_str()));
 
                     // `setdefault` always returns the field type
@@ -1049,13 +1061,13 @@ pub(super) fn validate_typed_dict_dict_literal<'db>(
     }
 }
 
-#[salsa::interned(debug, heap_size=SynthesizedTypedDictType::heap_size)]
+#[salsa::interned(debug, heap_size=ruff_memory_usage::heap_size)]
 #[derive(PartialOrd, Ord)]
 pub struct SynthesizedTypedDictType<'db> {
     pub(crate) params: TypedDictParams,
 
     #[returns(ref)]
-    pub(crate) items: FxOrderMap<Name, Field<'db>>,
+    pub(crate) items: BTreeMap<Name, Field<'db>>,
 }
 
 // The Salsa heap is tracked separately.
@@ -1079,26 +1091,20 @@ impl<'db> SynthesizedTypedDictType<'db> {
 
                 (name.clone(), field)
             })
-            .collect::<FxOrderMap<_, _>>();
+            .collect::<BTreeMap<_, _>>();
 
         SynthesizedTypedDictType::new(db, self.params(db), items)
     }
 
     pub(crate) fn normalized_impl(self, db: &'db dyn Db, visitor: &NormalizedVisitor<'db>) -> Self {
-        let mut items = self
+        let items = self
             .items(db)
             .iter()
             .map(|(name, field)| {
                 let field = field.clone().normalized_impl(db, visitor);
                 (name.clone(), field)
             })
-            .collect::<FxOrderMap<_, _>>();
-        // `Hash`/`Eq` for `FxOrderMap` includes the key order, so we need to sort.
-        items.sort_unstable_keys();
+            .collect::<BTreeMap<_, _>>();
         Self::new(db, self.params(db), items)
     }
-
-    fn heap_size((params, items): &(TypedDictParams, FxOrderMap<Name, Field<'db>>)) -> usize {
-        ruff_memory_usage::heap_size(params) + ruff_memory_usage::order_map_heap_size(items)
-    }
 }
```

</details>

And in some ways I like ^that approach better, because the invariant that `TypedDict` items should always be in sorted order is then maintained by the type itself rather than us having to remember to sort the `FxOrderMap` every time we create a synthesized typed dict. The disadvantage though is that we have to add yet another Salsa query, which probably means that it uses more memory. And it's also more code. And it doesn't have any impact on semantics. So I think what you have already with the `FxOrderMap` approach is totally fine!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:927 on 2025-12-05 18:41_

to me these `# error: [type-assertion-failure]` assertions feel a little confusing -- 99% of the time when there's a `type-assertion-failure` diagnostic or `static-assert` diagnostic in an mdtest, it has a big `TODO` comment next to it saying that it _should_ pass, it just doesn't yet. But these `assert_type` calls are _deliberately_ failing, which is a bit unusual!

I think this is really just saying the same thing as the `static_assert(not is_equivalent_to(Foo, FewerFields))` call immediately above, so I'd be inclined to omit it (same for the other `# error: [type-assertion-failure]` calls in this file)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:906 on 2025-12-05 19:26_

looks like this TODO is now done :-)

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:286 on 2025-12-05 19:35_

I don't think `to_meta_class` is an accurate name for what this method is doing (and... I don't think it's doing what it's trying to do correctly either 😄). A synthesized typeddict doesn't have a class, so it doesn't have a metaclass either. A class-based TypedDict _does_ have a class, and therefore it also has a metaclass, but this method doesn't return the _metaclass_ of a class-based typeddict, it just returns that class-based typeddict's _defining_ class.

It looks like this method is actually being used to return a `ClassType` that can then be used to construct the meta-_type_ of a `TypedDict` instance-type. So on that basis, we should rename this method `to_meta_type`, and have it return a `Type` rather than a `ClassType`.

But I also don't think this method gives the correct answer for the meta-type of a synthesized typeddict. A typeddict's meta-type is used for looking up synthesized TypedDict methods such as `__getitem__` -- if you return `type[TypedDictFallback]` here rather than `type[<synthetic typeddict>]`, then when we start doing member lookups on synthetic `TypedDict`s, we'll return the wrong results for members like `__getitem__`. As you noted in https://github.com/astral-sh/ruff/pull/21784/files#r2587403973, we don't ever do any member lookups on synthesized typeddicts yet, but we _will_ if we start adding them to intersections in order to fix the `TypedDict` part of https://github.com/astral-sh/ty/issues/1479. Giving a good answer for the meta-type of a synthesized `TypedDict` may involve adding a new variant to the `SubclassOfInner` enum in `subclass_of.rs`.

For now, I would recommend applying something like this patch. Then we can revisit the question of an accurate meta-type for synthesized `TypedDict`s when we actually start using them in more places. It's very hard to write tests for right now, when we use them in so few places :-)

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index e7459ce7fb..2ab9c3beba 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -7524,7 +7524,13 @@ impl<'db> Type<'db> {
             Type::ProtocolInstance(protocol) => protocol.to_meta_type(db),
             // `TypedDict` instances are instances of `dict` at runtime, but its important that we
             // understand a more specific meta type in order to correctly handle `__getitem__`.
-            Type::TypedDict(typed_dict) => typed_dict.to_meta_class(db).into(),
+            Type::TypedDict(typed_dict) => match typed_dict {
+                TypedDictType::Class(class) => SubclassOfType::from(db, class),
+                TypedDictType::Synthesized(_) => SubclassOfType::from(
+                    db,
+                    todo_type!("TypedDict synthesized meta-type").expect_dynamic(),
+                ),
+            },
             Type::TypeAlias(alias) => alias.value_type(db).to_meta_type(db),
             Type::NewTypeInstance(newtype) => Type::from(newtype.base_class_type(db)),
         }
diff --git a/crates/ty_python_semantic/src/types/subclass_of.rs b/crates/ty_python_semantic/src/types/subclass_of.rs
index 1045817a53..c6bb9d0378 100644
--- a/crates/ty_python_semantic/src/types/subclass_of.rs
+++ b/crates/ty_python_semantic/src/types/subclass_of.rs
@@ -8,7 +8,7 @@ use crate::types::{
     ApplyTypeMappingVisitor, BoundTypeVarInstance, ClassType, DynamicType,
     FindLegacyTypeVarsVisitor, HasRelationToVisitor, IsDisjointVisitor, KnownClass,
     MaterializationKind, MemberLookupPolicy, NormalizedVisitor, SpecialFormType, Type, TypeContext,
-    TypeMapping, TypeRelation, TypeVarBoundOrConstraints, todo_type,
+    TypeMapping, TypeRelation, TypeVarBoundOrConstraints, TypedDictType, todo_type,
 };
 use crate::{Db, FxOrderSet};
 
@@ -381,7 +381,12 @@ impl<'db> SubclassOfInner<'db> {
     pub(crate) fn try_from_instance(db: &'db dyn Db, ty: Type<'db>) -> Option<Self> {
         Some(match ty {
             Type::NominalInstance(instance) => SubclassOfInner::Class(instance.class(db)),
-            Type::TypedDict(typed_dict) => SubclassOfInner::Class(typed_dict.to_meta_class(db)),
+            Type::TypedDict(typed_dict) => match typed_dict {
+                TypedDictType::Class(class) => SubclassOfInner::Class(class),
+                TypedDictType::Synthesized(_) => SubclassOfInner::Dynamic(
+                    todo_type!("type[T] for synthesized TypedDicts").expect_dynamic(),
+                ),
+            },
             Type::TypeVar(bound_typevar) => SubclassOfInner::TypeVar(bound_typevar),
             Type::Dynamic(DynamicType::Any) => SubclassOfInner::Dynamic(DynamicType::Any),
             Type::Dynamic(DynamicType::Unknown) => SubclassOfInner::Dynamic(DynamicType::Unknown),
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index 6e4d2d5726..a7e5360a06 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -282,24 +282,6 @@ impl<'db> TypedDictType<'db> {
         }
     }
 
-    /// Return the meta-type of this `TypedDict` type.
-    pub(super) fn to_meta_class(self, db: &'db dyn Db) -> ClassType<'db> {
-        // `TypedDict` instances are instances of `dict` at runtime, but its important that we
-        // understand a more specific meta type in order to correctly handle `__getitem__`.
-        match self {
-            TypedDictType::Class(defining_class) => defining_class,
-            TypedDictType::Synthesized(_) => KnownClass::TypedDictFallback
-                .try_to_class_literal(db)
-                .map(|class| class.default_specialization(db))
-                .unwrap_or_else(|| {
-                    KnownClass::Object
-                        .try_to_class_literal(db)
-                        .map(|class| class.default_specialization(db))
-                        .expect("object class must exist")
-                }),
-        }
-    }
-
     pub(crate) fn normalized_impl(self, db: &'db dyn Db, visitor: &NormalizedVisitor<'db>) -> Self {
         match self {
             TypedDictType::Class(_) => {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4294 on 2025-12-05 19:40_

Given that no tests fail if I remove this right now, I would be inclined to remove this special case for now and add it back when we actually start creating synthesized typeddicts in more places, as part of https://github.com/astral-sh/ty/issues/1479. It's very hard to write tests for this right now, as you say, so it's very hard to verify whether this is in fact correct.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1619 on 2025-12-05 19:45_

What about using an `info` subdiagnostic for the extra information? it feels a _bit_ verbose to me for `--output-format=concise`:

```suggestion
                        let mut diagnostic = builder.into_diagnostic(format_args!(
                            "Value is already of type `{}`",
                            casted_type.display(db),
                        ));
                        diagnostic.info(format_args!(
                            "This type is equivalent to `{}`",
                            source_type.display(db),
                        ));
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:325 on 2025-12-05 19:51_

I don't think this is correct. These assertions should pass, but currently doesn't on your branch:

```py
from ty_extensions import static_assert, is_equivalent_to
from typing_extensions import TypedDict, NotRequired

class Foo(TypedDict, total=False):
    x: int
    y: str

class Baz(TypedDict):
    y: NotRequired[str]
    x: NotRequired[int]

static_assert(is_equivalent_to(Foo, Baz))
static_assert(is_equivalent_to(Foo | int, int | Baz))
```

A `total=False` `TypedDict` can still be equivalent to a `total=True` `TypedDict` if all the fields in the `total=True` `TypedDict` are marked as `NotRequired`. These should also pass, for similar reasons:

```py
from ty_extensions import static_assert, is_equivalent_to
from typing_extensions import TypedDict, NotRequired, Required

class Foo(TypedDict, total=False):
    x: int
    y: Required[str]

class Baz(TypedDict):
    y: str
    x: NotRequired[int]

static_assert(is_equivalent_to(Foo, Baz))
static_assert(is_equivalent_to(Foo | int, int | Baz))
```

I think what this also implies is that `SyntheticTypedDictType` should not have a `params` field. Instead, normalizing a class-based `TypedDict` should apply the `total=False` parameter to the `is_required` flag on each `TypedDict` field

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:349 on 2025-12-05 19:52_

I think you're missing an opportunity for an early return here:

```suggestion
            constraints.intersect(
                db,
                field.declared_ty.is_equivalent_to_impl(
                    db,
                    other_field.declared_ty,
                    inferable,
                    visitor,
                ),
            );
            if constraints.is_never_satisfied(db) {
                return constraints;
            }
```

---

_@AlexWaygood reviewed on 2025-12-05 19:57_

Thanks! A lot of this looks good, but I think there's a few issues here to iron out

---

_@oconnor663 reviewed on 2025-12-05 22:57_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:105 on 2025-12-05 22:57_

> rather than us having to remember to sort the FxOrderMap every time we create a synthesized typed dict

Fwiw, I think we currently only need to remember this when we _normalize_ a typeddict, which feels in line with other things we need to remember to normalize stuff.

---

_@oconnor663 reviewed on 2025-12-05 23:02_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:927 on 2025-12-05 23:02_

241e53584d

---

_@oconnor663 reviewed on 2025-12-05 23:21_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:286 on 2025-12-05 23:21_

Oof, thanks for walking me through this.

> Giving a good answer for the meta-type of a synthesized TypedDict may involve adding a new variant to the SubclassOfInner enum in subclass_of.rs.

Yes now that you mention it, that's what https://github.com/astral-sh/ruff/pull/20732 did.

---

_@oconnor663 reviewed on 2025-12-05 23:34_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:349 on 2025-12-05 23:34_

Aside: Why does GitHub display this diff as though it modifies the 9 lines above, when it actually only adds 3 new lines below? It this just a hack to add in some context?

---

_@oconnor663 reviewed on 2025-12-05 23:45_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/function.rs`:1619 on 2025-12-05 23:45_

This `info:` diagnostic is obviously better than my super long line, but I think we still want to suppress it if the source and cast types are in fact (visually) identical. I.e. we don't want to say "Value is already of type `int`...info: This type is equivalent to `int`". I've put the check back in in d470fc0b82.

---

_@oconnor663 reviewed on 2025-12-06 00:40_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:325 on 2025-12-06 00:40_

Another oof. Thanks for catching these.

Thinking ahead to when we add support for `closed` and `extra_items`, there's no way to "distribute" those across the fields, so in those cases I think `SyntheticTypedDictType` will need to track them. But it's probably cleaner to track those two individually, rather than including a whole `TypedDictParams` and normalizing and/or ignoring the `TOTAL` bit in there? So yes, agreed on removing the `params` field.

---

_@oconnor663 reviewed on 2025-12-06 01:03_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:325 on 2025-12-06 01:03_

0de150a1ec3b036617c69b325b71d026c74cd7e5

---

_@AlexWaygood reviewed on 2025-12-07 15:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:325 on 2025-12-07 15:02_

> Thinking ahead to when we add support for `closed` and `extra_items`, there's no way to "distribute" those across the fields, so in those cases I think `SyntheticTypedDictType` will need to track them. But it's probably cleaner to track those two individually, rather than including a whole `TypedDictParams` and normalizing and/or ignoring the `TOTAL` bit in there? So yes, agreed on removing the `params` field.

Right. And that does make me wonder, once again, if it would give us more flexibility if `TypedDictType::items()` returned a different type to `ClassLiteral::fields()`. E.g. if we had something like this, it would be trivial to add support for `closed` and `extra_items` in the future:

<details>
<summary>Patch</summary>

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index f0fdfd38e8..ecd1518678 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -1417,7 +1417,7 @@ pub(crate) enum FieldKind<'db> {
 
 /// Metadata regarding a dataclass field/attribute or a `TypedDict` "item" / key-value pair.
 #[derive(Debug, Clone, PartialEq, Eq, Hash, salsa::Update, get_size2::GetSize)]
-pub struct Field<'db> {
+pub(super) struct Field<'db> {
     /// The declared type of the field
     pub(crate) declared_ty: Type<'db>,
     /// Kind-specific metadata for this field
@@ -1447,34 +1447,6 @@ impl<'db> Field<'db> {
         }
     }
 
-    pub(super) fn apply_type_mapping_impl<'a>(
-        self,
-        db: &'db dyn Db,
-        type_mapping: &TypeMapping<'a, 'db>,
-        tcx: TypeContext<'db>,
-        visitor: &ApplyTypeMappingVisitor<'db>,
-    ) -> Self {
-        Field {
-            kind: self.kind,
-            first_declaration: self.first_declaration,
-            declared_ty: self
-                .declared_ty
-                .apply_type_mapping_impl(db, type_mapping, tcx, visitor),
-        }
-    }
-
-    pub(super) fn normalized_impl(
-        &self,
-        db: &'db dyn Db,
-        visitor: &NormalizedVisitor<'db>,
-    ) -> Self {
-        Field {
-            kind: self.kind.clone(),
-            first_declaration: None,
-            declared_ty: self.declared_ty.normalized_impl(db, visitor),
-        }
-    }
-
     /// Returns true if this field is a `dataclasses.KW_ONLY` sentinel.
     /// <https://docs.python.org/3/library/dataclasses.html#dataclasses.KW_ONLY>
     pub(crate) fn is_kw_only_sentinel(&self, db: &'db dyn Db) -> bool {
diff --git a/crates/ty_python_semantic/src/types/diagnostic.rs b/crates/ty_python_semantic/src/types/diagnostic.rs
index bf32c6aa7f..23bb5b1a16 100644
--- a/crates/ty_python_semantic/src/types/diagnostic.rs
+++ b/crates/ty_python_semantic/src/types/diagnostic.rs
@@ -14,9 +14,7 @@ use crate::semantic_index::place::{PlaceTable, ScopedPlaceId};
 use crate::semantic_index::{global_scope, place_table, use_def_map};
 use crate::suppression::FileSuppressionId;
 use crate::types::call::CallError;
-use crate::types::class::{
-    CodeGeneratorKind, DisjointBase, DisjointBaseKind, Field, MethodDecorator,
-};
+use crate::types::class::{CodeGeneratorKind, DisjointBase, DisjointBaseKind, MethodDecorator};
 use crate::types::function::{FunctionDecorators, FunctionType, KnownFunction, OverloadLiteral};
 use crate::types::infer::UnsupportedComparisonError;
 use crate::types::overrides::MethodKind;
@@ -26,15 +24,14 @@ use crate::types::string_annotation::{
     RAW_STRING_TYPE_ANNOTATION,
 };
 use crate::types::tuple::TupleSpec;
+use crate::types::typed_dict::TypedDictSchema;
 use crate::types::{
     BoundTypeVarInstance, ClassType, DynamicType, LintDiagnosticGuard, Protocol,
     ProtocolInstanceType, SpecialFormType, SubclassOfInner, Type, TypeContext, binding_type,
     protocol_class::ProtocolClass,
 };
 use crate::types::{KnownInstanceType, MemberLookupPolicy};
-use crate::{
-    Db, DisplaySettings, FxIndexMap, FxOrderMap, Module, ModuleName, Program, declare_lint,
-};
+use crate::{Db, DisplaySettings, FxIndexMap, Module, ModuleName, Program, declare_lint};
 use itertools::Itertools;
 use ruff_db::{
     diagnostic::{Annotation, Diagnostic, Span, SubDiagnostic, SubDiagnosticSeverity},
@@ -3473,7 +3470,7 @@ pub(crate) fn report_invalid_key_on_typed_dict<'db>(
     typed_dict_ty: Type<'db>,
     full_object_ty: Option<Type<'db>>,
     key_ty: Type<'db>,
-    items: &FxOrderMap<Name, Field<'db>>,
+    items: &TypedDictSchema<'db>,
 ) {
     let db = context.db();
     if let Some(builder) = context.report_lint(&INVALID_KEY, key_node) {
diff --git a/crates/ty_python_semantic/src/types/display.rs b/crates/ty_python_semantic/src/types/display.rs
index db6a8b6e37..f7ce65834e 100644
--- a/crates/ty_python_semantic/src/types/display.rs
+++ b/crates/ty_python_semantic/src/types/display.rs
@@ -2382,14 +2382,15 @@ mod tests {
     use insta::assert_snapshot;
     use ruff_python_ast::name::Name;
 
+    use crate::Db;
     use crate::db::tests::setup_db;
     use crate::place::typing_extensions_symbol;
-    use crate::types::class::{Field, FieldKind};
-    use crate::types::typed_dict::SynthesizedTypedDictType;
+    use crate::types::typed_dict::{
+        SynthesizedTypedDictType, TypedDictFieldBuilder, TypedDictSchema,
+    };
     use crate::types::{
-        KnownClass, Parameter, Parameters, Signature, StringLiteralType, Type, TypedDictType,
+        KnownClass, Parameter, Parameters, Signature, Type, TypedDictType,
     };
-    use crate::{Db, FxOrderMap};
 
     #[test]
     fn string_literal_display() {
@@ -2438,28 +2439,18 @@ mod tests {
     fn synthesized_typeddict_display() {
         let db = setup_db();
 
-        let mut items = FxOrderMap::default();
+        let mut items = TypedDictSchema::default();
         items.insert(
             Name::new("foo"),
-            Field {
-                declared_ty: Type::IntLiteral(42),
-                kind: FieldKind::TypedDict {
-                    is_required: true,
-                    is_read_only: false,
-                },
-                first_declaration: None,
-            },
+            TypedDictFieldBuilder::new(Type::IntLiteral(42))
+                .required(true)
+                .build(),
         );
         items.insert(
             Name::new("bar"),
-            Field {
-                declared_ty: Type::StringLiteral(StringLiteralType::new(&db, "hello")),
-                kind: FieldKind::TypedDict {
-                    is_required: true,
-                    is_read_only: false,
-                },
-                first_declaration: None,
-            },
+            TypedDictFieldBuilder::new(Type::string_literal(&db, "hello"))
+                .required(true)
+                .build(),
         );
 
         let synthesized = SynthesizedTypedDictType::new(&db, items);
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index a3be4fb05a..d0ae2f0bde 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -1,3 +1,6 @@
+use std::collections::BTreeMap;
+use std::ops::{Deref, DerefMut};
+
 use bitflags::bitflags;
 use ruff_db::diagnostic::{Annotation, Diagnostic, Span, SubDiagnostic, SubDiagnosticSeverity};
 use ruff_db::parsed::parsed_module;
@@ -12,14 +15,15 @@ use super::diagnostic::{
     report_missing_typed_dict_key,
 };
 use super::{ApplyTypeMappingVisitor, Type, TypeMapping, visitor};
+use crate::Db;
 use crate::semantic_index::definition::Definition;
+use crate::types::class::FieldKind;
 use crate::types::constraints::ConstraintSet;
 use crate::types::generics::InferableTypeVars;
 use crate::types::{
     HasRelationToVisitor, IsDisjointVisitor, IsEquivalentVisitor, NormalizedVisitor, TypeContext,
     TypeRelation,
 };
-use crate::{Db, FxOrderMap};
 
 use ordermap::OrderSet;
 
@@ -66,12 +70,37 @@ impl<'db> TypedDictType<'db> {
         }
     }
 
-    pub(crate) fn items(self, db: &'db dyn Db) -> &'db FxOrderMap<Name, Field<'db>> {
+    pub(crate) fn items(self, db: &'db dyn Db) -> &'db TypedDictSchema<'db> {
+        #[salsa::tracked(returns(ref))]
+        fn class_based_items<'db>(db: &'db dyn Db, class: ClassType<'db>) -> TypedDictSchema<'db> {
+            let (class_literal, specialization) = class.class_literal(db);
+            class_literal
+                .fields(db, specialization, CodeGeneratorKind::TypedDict)
+                .into_iter()
+                .map(|(name, field)| {
+                    let field = match field {
+                        Field {
+                            first_declaration,
+                            declared_ty,
+                            kind:
+                                FieldKind::TypedDict {
+                                    is_required,
+                                    is_read_only,
+                                },
+                        } => TypedDictFieldBuilder::new(*declared_ty)
+                            .required(*is_required)
+                            .read_only(*is_read_only)
+                            .first_declaration(*first_declaration)
+                            .build(),
+                        _ => unreachable!("TypedDict field expected"),
+                    };
+                    (name.clone(), field)
+                })
+                .collect()
+        }
+
         match self {
-            Self::Class(defining_class) => {
-                let (class_literal, specialization) = defining_class.class_literal(db);
-                class_literal.fields(db, specialization, CodeGeneratorKind::TypedDict)
-            }
+            Self::Class(defining_class) => class_based_items(db, defining_class),
             Self::Synthesized(synthesized) => synthesized.items(db),
         }
     }
@@ -303,7 +332,7 @@ impl<'db> TypedDictType<'db> {
             let Some(other_field) = other_items.get(name) else {
                 return ConstraintSet::from(false);
             };
-            if field.kind != other_field.kind {
+            if field.flags != other_field.flags {
                 return ConstraintSet::from(false);
             }
             constraints.intersect(
@@ -716,11 +745,11 @@ pub(super) fn validate_typed_dict_dict_literal<'db>(
     }
 }
 
-#[salsa::interned(debug, heap_size=SynthesizedTypedDictType::heap_size)]
+#[salsa::interned(debug)]
 #[derive(PartialOrd, Ord)]
 pub struct SynthesizedTypedDictType<'db> {
     #[returns(ref)]
-    pub(crate) items: FxOrderMap<Name, Field<'db>>,
+    pub(crate) items: TypedDictSchema<'db>,
 }
 
 // The Salsa heap is tracked separately.
@@ -744,26 +773,145 @@ impl<'db> SynthesizedTypedDictType<'db> {
 
                 (name.clone(), field)
             })
-            .collect::<FxOrderMap<_, _>>();
+            .collect::<TypedDictSchema<'db>>();
 
         SynthesizedTypedDictType::new(db, items)
     }
 
     pub(crate) fn normalized_impl(self, db: &'db dyn Db, visitor: &NormalizedVisitor<'db>) -> Self {
-        let mut items = self
+        let items = self
             .items(db)
             .iter()
             .map(|(name, field)| {
                 let field = field.clone().normalized_impl(db, visitor);
                 (name.clone(), field)
             })
-            .collect::<FxOrderMap<_, _>>();
-        // `Hash`/`Eq` for `FxOrderMap` includes the key order, so we need to sort.
-        items.sort_unstable_keys();
+            .collect::<TypedDictSchema<'db>>();
         Self::new(db, items)
     }
+}
+
+#[derive(Debug, Clone, PartialEq, Eq, Hash, Default, get_size2::GetSize, salsa::Update)]
+pub struct TypedDictSchema<'db>(BTreeMap<Name, TypedDictField<'db>>);
+
+impl<'db> Deref for TypedDictSchema<'db> {
+    type Target = BTreeMap<Name, TypedDictField<'db>>;
+
+    fn deref(&self) -> &Self::Target {
+        &self.0
+    }
+}
+
+impl DerefMut for TypedDictSchema<'_> {
+    fn deref_mut(&mut self) -> &mut Self::Target {
+        &mut self.0
+    }
+}
+
+impl<'a> IntoIterator for &'a TypedDictSchema<'_> {
+    type Item = (&'a Name, &'a TypedDictField<'a>);
+    type IntoIter = std::collections::btree_map::Iter<'a, Name, TypedDictField<'a>>;
+
+    fn into_iter(self) -> Self::IntoIter {
+        self.0.iter()
+    }
+}
+
+impl<'db> FromIterator<(Name, TypedDictField<'db>)> for TypedDictSchema<'db> {
+    fn from_iter<T: IntoIterator<Item = (Name, TypedDictField<'db>)>>(iter: T) -> Self {
+        Self(iter.into_iter().collect())
+    }
+}
+
+#[derive(Debug, Clone, PartialEq, Eq, Hash, get_size2::GetSize, salsa::Update)]
+pub struct TypedDictField<'db> {
+    pub(super) declared_ty: Type<'db>,
+    flags: TypedDictFieldFlags,
+    first_declaration: Option<Definition<'db>>,
+}
+
+impl<'db> TypedDictField<'db> {
+    pub(crate) const fn is_required(&self) -> bool {
+        self.flags.contains(TypedDictFieldFlags::REQUIRED)
+    }
+
+    pub(crate) const fn is_read_only(&self) -> bool {
+        self.flags.contains(TypedDictFieldFlags::READ_ONLY)
+    }
+
+    pub(crate) fn apply_type_mapping_impl<'a>(
+        self,
+        db: &'db dyn Db,
+        type_mapping: &TypeMapping<'a, 'db>,
+        tcx: TypeContext<'db>,
+        visitor: &ApplyTypeMappingVisitor<'db>,
+    ) -> Self {
+        Self {
+            declared_ty: self
+                .declared_ty
+                .apply_type_mapping_impl(db, type_mapping, tcx, visitor),
+            flags: self.flags,
+            first_declaration: self.first_declaration,
+        }
+    }
+
+    pub(crate) fn normalized_impl(self, db: &'db dyn Db, visitor: &NormalizedVisitor<'db>) -> Self {
+        Self {
+            declared_ty: self.declared_ty.normalized_impl(db, visitor),
+            flags: self.flags,
+            // A normalized typed-dict field does not hold onto the original declaration,
+            // since a normalized typed-dict is an abstract type where equality does not depend
+            // on the source-code definition.
+            first_declaration: None,
+        }
+    }
+}
+
+pub(super) struct TypedDictFieldBuilder<'db> {
+    declared_ty: Type<'db>,
+    flags: TypedDictFieldFlags,
+    first_declaration: Option<Definition<'db>>,
+}
+
+impl<'db> TypedDictFieldBuilder<'db> {
+    pub(crate) fn new(declared_ty: Type<'db>) -> Self {
+        Self {
+            declared_ty,
+            flags: TypedDictFieldFlags::empty(),
+            first_declaration: None,
+        }
+    }
+
+    pub(crate) fn required(mut self, yes: bool) -> Self {
+        self.flags.set(TypedDictFieldFlags::REQUIRED, yes);
+        self
+    }
+
+    pub(crate) fn read_only(mut self, yes: bool) -> Self {
+        self.flags.set(TypedDictFieldFlags::READ_ONLY, yes);
+        self
+    }
 
-    fn heap_size((items,): &(FxOrderMap<Name, Field<'db>>,)) -> usize {
-        ruff_memory_usage::order_map_heap_size(items)
+    pub(crate) fn first_declaration(mut self, definition: Option<Definition<'db>>) -> Self {
+        self.first_declaration = definition;
+        self
+    }
+
+    pub(crate) fn build(self) -> TypedDictField<'db> {
+        TypedDictField {
+            declared_ty: self.declared_ty,
+            flags: self.flags,
+            first_declaration: self.first_declaration,
+        }
     }
 }
+
+bitflags! {
+    #[derive(Debug, Clone, Copy, PartialEq, Eq, Hash, salsa::Update)]
+    struct TypedDictFieldFlags: u8 {
+        const REQUIRED = 1 << 0;
+        const READ_ONLY = 1 << 1;
+    }
+}
+
+impl get_size2::GetSize for TypedDictFieldFlags {}
```

</details>

It also feels like it would be more strongly typed, since we _know_ that `field.kind` won't ever be a `FieldKind::Dataclass` or `FieldKind::NamedTuple` in the context of the map returned by `TypedDictType::items()`, but that isn't currently reflected by the type that the method returns.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1626 on 2025-12-07 15:06_

nit

```suggestion
                        let source_display = source_type.display(db).to_string();
                        let casted_display = casted_type.display(db).to_string();
                        let mut diagnostic = builder.into_diagnostic(format_args!(
                            "Value is already of type `{casted_display}`",
                        ));
                        if source_display != casted_display {
                            diagnostic.info(format_args!(
                                "`{casted_display}` is equivalent to `{source_display}`",
                            ));
                        }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:305 on 2025-12-07 15:07_

I think this is fine

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:322 on 2025-12-07 15:11_

nit: you could do this a bit more concisely if you imported the `IteratorConstraintsExtension` trait:

```diff
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index a3be4fb05a..9aa2c9a4ac 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -13,7 +13,7 @@ use super::diagnostic::{
 };
 use super::{ApplyTypeMappingVisitor, Type, TypeMapping, visitor};
 use crate::semantic_index::definition::Definition;
-use crate::types::constraints::ConstraintSet;
+use crate::types::constraints::{ConstraintSet, IteratorConstraintsExtension};
 use crate::types::generics::InferableTypeVars;
 use crate::types::{
     HasRelationToVisitor, IsDisjointVisitor, IsEquivalentVisitor, NormalizedVisitor, TypeContext,
@@ -298,28 +298,17 @@ impl<'db> TypedDictType<'db> {
             return ConstraintSet::from(false);
         }
         let other_items = other.items(db);
-        let mut constraints = ConstraintSet::from(true);
-        for (name, field) in self.items(db) {
+        self.items(db).iter().when_all(db, |(name, field)| {
             let Some(other_field) = other_items.get(name) else {
                 return ConstraintSet::from(false);
             };
             if field.kind != other_field.kind {
                 return ConstraintSet::from(false);
             }
-            constraints.intersect(
-                db,
-                field.declared_ty.is_equivalent_to_impl(
-                    db,
-                    other_field.declared_ty,
-                    inferable,
-                    visitor,
-                ),
-            );
-            if constraints.is_never_satisfied(db) {
-                return constraints;
-            }
-        }
-        constraints
+            field
+                .declared_ty
+                .is_equivalent_to_impl(db, other_field.declared_ty, inferable, visitor)
+        })
     }
 }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:1 on 2025-12-07 15:12_

Could you add a few tests that involve recursive TypedDicts?

---

_@AlexWaygood approved on 2025-12-07 15:12_

Thank you!! LGTM, though I'd still love it if we could figure out why the big pydantic regression is occurring

---

_Review request for @sharkdp removed by @sharkdp on 2025-12-08 09:55_

---

_@oconnor663 reviewed on 2025-12-08 19:51_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:325 on 2025-12-08 19:51_

Ah this is definitely a lot cleaner. And again #20732 did something similar: https://github.com/astral-sh/ruff/pull/20732/files#diff-2f6787010a635a3508de77c9d28673b9d281045bd33cb5bda175d35b0a74120cR387

Out of curiosity, why provide `TypedDictFieldBuilder`? It seems pretty straightforward to construct a `TypedDictField` in place. (Edit: Oh now I see it, it makes things things shorter in `display.rs` tests.)

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:325 on 2025-12-08 20:04_

a7dd59ab88

---

_@oconnor663 reviewed on 2025-12-08 20:04_

---

_@oconnor663 reviewed on 2025-12-08 20:08_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:322 on 2025-12-08 20:08_

neato

---

_@oconnor663 reviewed on 2025-12-08 20:26_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:1 on 2025-12-08 20:26_

Interesting, direct recursion works fine, but recursion via `list[]` crashes...

---

_@oconnor663 reviewed on 2025-12-08 20:41_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:1 on 2025-12-08 20:41_

Looks like it was just a missing `visit` call: 1c05f28472

---

_Comment by @AlexWaygood on 2025-12-08 21:49_

I think the updates to the Cargo.toml/Cargo.lock files are probably not necessary with the latest version of this PR? They're obviously harmless, but they could probably be a standalone change at this point

---

_@AlexWaygood reviewed on 2025-12-08 21:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:1 on 2025-12-08 21:49_

Nice, that looks correct!

---

_Comment by @oconnor663 on 2025-12-08 22:47_

> I think the updates to the Cargo.toml/Cargo.lock files are probably not necessary with the latest version of this PR? They're obviously harmless, but they could probably be a standalone change at this point

There are still a few places in `class.rs` where this PR changes `IndexMap` -> `OrderMap`, which depends on the `Cargo.toml` bumps. This PR no longer requires those changes after your patch, but we've also said (or at least Ibraheem said?) that would generally prefer to switch to `OrderMap`, so I've split all that out into a separate PR: https://github.com/astral-sh/ruff/pull/21854

---

_Comment by @oconnor663 on 2025-12-09 01:59_

Some notes from digging into the performance regression today:

It's definitely our old friend `CoreSchema`, the gigantic `TypedDict` union. Not very surprising. One of the files affected in this particular PR is [`functional_serializers.py`](https://github.com/pydantic/pydantic/blob/main/pydantic/functional_serializers.py), which typechecks in ~0.05s on `main` and ~1s on this branch. I can shrink that entire file down to this while still preserving most of that gap:

```py
from pydantic_core.core_schema import CoreSchema

def foo(core_schema: CoreSchema):
    core_schema.copy()
```

I'm not sure why `.copy()` in particular would be affected by these changes?

---

_Comment by @carljm on 2025-12-09 02:46_

Can you profile main vs this branch checking that minimized example (e.g. using `samply record`)? With that big a difference in runtime, I would think the more-precise location of the extra time spent should jump out (and the stacks should help clarify from where we are calling the newly-expensive function).

---

_Comment by @MichaReiser on 2025-12-09 19:01_

I already shared this with @oconnor663 but for broader visibility:

* Here's my profile https://share.firefox.dev/4pzdJ0u
* What stands out is that we spend a significant amount within `Eq` and `Hash` that we didn't before. We call `Eq` a lot within `UnionBuilder` and `Hash` in `normalize_impl` (because we intern the value). 

I'm not aware of any tricks to make either of those magically go away, but some of our typing wizards may do.

---

_@MichaReiser reviewed on 2025-12-09 19:17_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/typed_dict.rs`:305 on 2025-12-09 19:17_

An alternative that might be worth here is to cache `synthesized instead of `self.items()` like this


```diff
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index 89ef0016f1..0a3f2f9cdd 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -71,36 +71,10 @@ impl<'db> TypedDictType<'db> {
     }
 
     pub(crate) fn items(self, db: &'db dyn Db) -> &'db TypedDictSchema<'db> {
-        #[salsa::tracked(returns(ref))]
-        fn class_based_items<'db>(db: &'db dyn Db, class: ClassType<'db>) -> TypedDictSchema<'db> {
-            let (class_literal, specialization) = class.class_literal(db);
-            class_literal
-                .fields(db, specialization, CodeGeneratorKind::TypedDict)
-                .into_iter()
-                .map(|(name, field)| {
-                    let field = match field {
-                        Field {
-                            first_declaration,
-                            declared_ty,
-                            kind:
-                                FieldKind::TypedDict {
-                                    is_required,
-                                    is_read_only,
-                                },
-                        } => TypedDictFieldBuilder::new(*declared_ty)
-                            .required(*is_required)
-                            .read_only(*is_read_only)
-                            .first_declaration(*first_declaration)
-                            .build(),
-                        _ => unreachable!("TypedDict field expected"),
-                    };
-                    (name.clone(), field)
-                })
-                .collect()
-        }
-
         match self {
-            Self::Class(defining_class) => class_based_items(db, defining_class),
+            Self::Class(defining_class) => {
+                SynthesizedTypedDictType::for_class(db, defining_class).items(db)
+            }
             Self::Synthesized(synthesized) => synthesized.items(db),
         }
     }
@@ -300,8 +274,8 @@ impl<'db> TypedDictType<'db> {
 
     pub(crate) fn normalized_impl(self, db: &'db dyn Db, visitor: &NormalizedVisitor<'db>) -> Self {
         match self {
-            TypedDictType::Class(_) => {
-                let synthesized = SynthesizedTypedDictType::new(db, self.items(db));
+            TypedDictType::Class(class) => {
+                let synthesized = SynthesizedTypedDictType::for_class(db, class);
                 TypedDictType::Synthesized(synthesized.normalized_impl(db, visitor))
             }
             TypedDictType::Synthesized(synthesized) => {
@@ -323,11 +297,13 @@ impl<'db> TypedDictType<'db> {
         // Compare the fields without requiring them to be in sorted order. Class-based `TypedDict`
         // fields are not sorted. We do sort synthetic fields in `normalized_impl`, but there will
         // soon be other sources of `SynthesizedTypedDictType` besides normalization.
-        if self.items(db).len() != other.items(db).len() {
+        let self_items = self.items(db);
+        let other_items = other.items(db);
+        if self_items.len() != other_items.len() {
             return ConstraintSet::from(false);
         }
-        let other_items = other.items(db);
-        self.items(db).iter().when_all(db, |(name, field)| {
+
+        self_items.iter().when_all(db, |(name, field)| {
             let Some(other_field) = other_items.get(name) else {
                 return ConstraintSet::from(false);
             };
@@ -744,7 +720,37 @@ pub struct SynthesizedTypedDictType<'db> {
 // The Salsa heap is tracked separately.
 impl get_size2::GetSize for SynthesizedTypedDictType<'_> {}
 
+#[salsa::tracked]
 impl<'db> SynthesizedTypedDictType<'db> {
+    #[salsa::tracked]
+    fn for_class(db: &'db dyn Db, class: ClassType<'db>) -> SynthesizedTypedDictType<'db> {
+        let (class_literal, specialization) = class.class_literal(db);
+        let items: TypedDictSchema<'db> = class_literal
+            .fields(db, specialization, CodeGeneratorKind::TypedDict)
+            .into_iter()
+            .map(|(name, field)| {
+                let field = match field {
+                    Field {
+                        first_declaration,
+                        declared_ty,
+                        kind:
+                            FieldKind::TypedDict {
+                                is_required,
+                                is_read_only,
+                            },
+                    } => TypedDictFieldBuilder::new(*declared_ty)
+                        .required(*is_required)
+                        .read_only(*is_read_only)
+                        .first_declaration(*first_declaration)
+                        .build(),
+                    _ => unreachable!("TypedDict field expected"),
+                };
+                (name.clone(), field)
+            })
+            .collect();
+        SynthesizedTypedDictType::new(db, items)
+    }
+
     pub(super) fn apply_type_mapping_impl<'a>(
         self,
         db: &'db dyn Db,
@@ -768,15 +774,20 @@ impl<'db> SynthesizedTypedDictType<'db> {
     }
 
     pub(crate) fn normalized_impl(self, db: &'db dyn Db, visitor: &NormalizedVisitor<'db>) -> Self {
+        let mut changed = false;
         let items = self
             .items(db)
             .iter()
             .map(|(name, field)| {
-                let field = field.clone().normalized_impl(db, visitor);
-                (name.clone(), field)
+                let new_field = field.clone().normalized_impl(db, visitor);
+                if !changed && &new_field != field {
+                    changed = true;
+                }
+                (name.clone(), new_field)
             })
             .collect::<TypedDictSchema<'db>>();
-        Self::new(db, items)
+
+        if changed { Self::new(db, items) } else { self }
     }
 }
 
```

---

_Comment by @oconnor663 on 2025-12-09 19:35_

@MichaReiser + @AlexWaygood, thanks for chatting up a storm with me this morning. Here's something more I've noticed staring at all our profiles. The reduced `copy()` example above spends ~all of its time inside of `BoundMethodType::has_relation_to_impl`. Two things jump out at me about that:
- That methods recurses on both the function/method type being called, and on the receiver object. Given that `CoreSchema` shows up on both sides of that, that could be part of why we're blowing up here?
- The function/method side of that immediately calls `normalize` in the `Redundancy` case.

---

_@MichaReiser reviewed on 2025-12-09 19:40_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/typed_dict.rs`:340 on 2025-12-09 19:40_

I don't think this will change performance much but you could make use of the fact that you have two `BTreeMap` where both should return the keys in the same order if they have the same fields:

```suggestion
        let mut other_items_iter = other_items.iter();

        self_items.iter().when_all(db, |(name, field)| {
            let Some((other_name, other_field)) = other_items_iter.next() else {
                return ConstraintSet::from(false);
            };

            if name != other_name || field.flags != other_field.flags {
                return ConstraintSet::from(false);
            }
            field
                .declared_ty
                .is_equivalent_to_impl(db, other_field.declared_ty, inferable, visitor)
        })
```

Doing the same in `has_relation_to` seems a bit trickier and would require `peek_if`

---

_@MichaReiser reviewed on 2025-12-09 19:59_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/typed_dict.rs`:340 on 2025-12-09 19:59_

This is how you could do the same in `has_relation_to` but it's a bit trickier:

```rust
let mut self_items_iter = self_items.iter().peekable();

        for (target_item_name, target_item_field) in target_items {
            // Skip over preceeding fields.
            let _ = {
                self_items_iter
                    .peeking_take_while(|(name, _)| *name < target_item_name)
                    .last()
            };
            let self_item_field = self_items_iter
                .peeking_next(|(name, _)| *name == target_item_name)
                .map(|(_, field)| field);
```

or use `Itertools::merge_by`

```rust
for pair in a.iter().merge_join_by(b.iter(), |(k1, _), (k2, _)| k1.cmp(k2)) {
    match pair {
        EitherOrBoth::Both((k, v1), (_, v2)) => {
            // Key exists in both maps
        }
        EitherOrBoth::Left((k, v1)) => {
            // Only in `a`
        }
        EitherOrBoth::Right((k, v2)) => {
            // Only in `b`
        }
    }
}
```

---

_@oconnor663 reviewed on 2025-12-09 20:20_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:340 on 2025-12-09 20:20_

Yes (to the first diff above), and the comment here about not being in sorted order is entirely wrong now :) I think since we're doing a length check up front, we can go ahead and `zip` the iterators, which is a little shorter: 3e5b534c9a64e1a5a09b0ae57b7ca2695849af72

---

_Comment by @oconnor663 on 2025-12-09 20:58_

- Moving the `TypedDictType` special case as early as possible in `UnionBuilder::push_type` doesn't make a difference here.
- Caching the conversion from class-based to synthetic typeddict saves 10% here in this most pathological case, but it doesn't close the bulk of the gap. As far as I know, I can't cache normalized_impl directly (which is what I'd really prefer to do), because of the visitor it takes.

---

_Comment by @oconnor663 on 2025-12-10 18:11_

I've pushed an `EXPERIMENTAL COMMIT` which I don't intend to actually land, which deletes the call to `normalized` that I think is at the heart of this PR's regression:

```diff
diff --git a/crates/ty_python_semantic/src/types/function.rs b/crates/ty_python_semantic/src/types/function.rs
index 421504e09b..9bf73f116d 100644
--- a/crates/ty_python_semantic/src/types/function.rs
+++ b/crates/ty_python_semantic/src/types/function.rs
@@ -1022,30 +1022,18 @@ impl<'db> FunctionType<'db> {
     pub(crate) fn has_relation_to_impl(
         self,
         db: &'db dyn Db,
         other: Self,
         inferable: InferableTypeVars<'_, 'db>,
         relation: TypeRelation<'db>,
         relation_visitor: &HasRelationToVisitor<'db>,
         disjointness_visitor: &IsDisjointVisitor<'db>,
     ) -> ConstraintSet<'db> {
-        // A function type is the subtype of itself, and not of any other function type. However,
-        // our representation of a function type includes any specialization that should be applied
-        // to the signature. Different specializations of the same function type are only subtypes
-        // of each other if they result in subtype signatures.
-        if matches!(
-            relation,
-            TypeRelation::Subtyping | TypeRelation::Redundancy | TypeRelation::SubtypingAssuming(_)
-        ) && self.normalized(db) == other.normalized(db)
-        {
-            return ConstraintSet::from(true);
-        }
-
         if self.literal(db) != other.literal(db) {
             return ConstraintSet::from(false);
         }
```

This does seem to fix the perf regression (and not break tests) locally. I'm curious to see what CodSpeed says. If CodSpeed is green, I will probably land this PR as-is (reverting this experimental commit of course) and fork off an issue to track this regression.

(btw I'm sure there's a more direct way to run CodSpeed on a random branch, either locally or in the cloud, so if anyone knows the non-dumb workflow for this please do let me know)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:740 on 2025-12-10 18:37_

Nit: for most other salsa-interned structs where we derive Ord, we add a note like this one here: https://github.com/astral-sh/ruff/blob/f7528bd325e20205facad414cf8462e922bbffa8/crates/ty_python_semantic/src/types.rs#L8652

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1 on 2025-12-10 18:38_

Nit: are the new `Hash` derives in this file still necessary on the latest version of this PR?

---

_@AlexWaygood approved on 2025-12-10 18:39_

The performance experiment looks like it was pretty successful to me!! It totally solved the regression and had zero impact on both our test suite and the primer report for this PR. Great job 😃

This looks ready to go now 

---

_@oconnor663 reviewed on 2025-12-10 19:47_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:305 on 2025-12-10 19:47_

My intuition is that normal code (read: not Pydantic) is going to spend more time looking up the `.items()` of regular class-based `TypedDict`s than it spends normalizing, so caching `.items()` makes sense to me.

---

_@oconnor663 reviewed on 2025-12-10 19:55_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:740 on 2025-12-10 19:55_

These derives were no longer necessary.

---

_@oconnor663 reviewed on 2025-12-10 19:55_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/class.rs`:1 on 2025-12-10 19:55_

I ended up reverting _all_ the changes in `class.rs` :)

---

_Comment by @carljm on 2025-12-10 19:57_

If the "experimental" commit fixed the pydantic regression and didn't add regressions anywhere else (which is what it looks like to me in CodSpeed), then I don't see any reason to revert that change here or wait for later to re-evaluate in https://github.com/astral-sh/ty/issues/1845 -- I think we should just include that change in this PR. What's the downside?

---

_Comment by @carljm on 2025-12-10 20:03_

> (btw I'm sure there's a more direct way to run CodSpeed on a random branch, either locally or in the cloud, so if anyone knows the non-dumb workflow for this please do let me know)

AFAIK you have to make a PR, but it could be a separate draft PR (possibly based on an existing PR) if you want to "hide" it more.

---

_Comment by @AlexWaygood on 2025-12-10 20:06_

> If the "experimental" commit fixed the pydantic regression and didn't add regressions anywhere else (which is what it looks like to me in CodSpeed), then I don't see any reason to revert that change here or wait for later to re-evaluate in [astral-sh/ty#1845](https://github.com/astral-sh/ty/issues/1845) -- I think we should just include that change in this PR. What's the downside?

Wait, yeah, same comment! Why revert a successful experiment? If we don't introduce a performance regression in the first place, there's no need to create a followup issue. It seemed like that commit had zero downsides!

---

_Merged by @oconnor663 on 2025-12-10 20:36_

---

_Closed by @oconnor663 on 2025-12-10 20:36_

---

_Branch deleted on 2025-12-10 20:36_

---
