```yaml
number: 22349
title: "[ty] narrow `TypedDict` unions with `not in`"
type: pull_request
state: merged
author: felixscherz
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: feat/narrow-typeddict-unions-with-not-in
created_at: 2026-01-02T21:59:42Z
updated_at: 2026-01-03T13:12:58Z
url: https://github.com/astral-sh/ruff/pull/22349
synced_at: 2026-01-10T16:36:19Z
```

# [ty] narrow `TypedDict` unions with `not in`

---

_Pull request opened by @felixscherz on 2026-01-02 21:59_

Hi, this fixes https://github.com/astral-sh/ty/issues/2191.

## Summary

Added narrowing for unions of `TypedDict` where union members can be excluded based on the absence of a key.

```python
class Foo(TypedDict):
    foo: int

class Bar(TypedDict):
    bar: int

def _(u: Foo | Bar):
    if "foo" not in u:
        reveal_type(u)  # was `Foo | Bar`, is now `Bar`
```


## Test Plan

Added markdown tests.


---

_Review requested from @carljm by @felixscherz on 2026-01-02 21:59_

---

_Review requested from @AlexWaygood by @felixscherz on 2026-01-02 21:59_

---

_Review requested from @sharkdp by @felixscherz on 2026-01-02 21:59_

---

_Review requested from @dcreager by @felixscherz on 2026-01-02 21:59_

---

_Label `ty` added by @AlexWaygood on 2026-01-02 22:00_

---

_Comment by @AlexWaygood on 2026-01-02 22:08_

Thanks! It looks like your branch is a bit behind `main`, though, which is causing compilation failures (https://github.com/astral-sh/ruff/pull/20974 reworked our narrowing-constraints machinery a bit). Could you rebase and fix the compilation failures?

---

_Comment by @astral-sh-bot[bot] on 2026-01-02 22:31_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-02 22:32_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
- backend/services/utils.py:71:16: error[invalid-return-type] Return type does not match returned value: expected `dict[Literal["data"], Any] | None`, found `SrcErrorResultDto`
- Found 29 diagnostics
+ Found 28 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/modules/simd.py:103:54: error[invalid-argument-type] Argument to function `extract_as_list` is incorrect: Expected `dict[str, Unknown]`, found `StaticLibrary`
+ mesonbuild/modules/simd.py:103:54: error[invalid-argument-type] Argument to function `extract_as_list` is incorrect: Expected `dict[Never, Unknown]`, found `StaticLibrary`
- mesonbuild/modules/simd.py:105:24: error[invalid-key] TypedDict `StaticLibrary` can only be subscripted with a string literal key, got key of type `str`.
+ mesonbuild/modules/simd.py:105:24: error[invalid-assignment] Cannot assign value of type `Never` to key of type `Never` on TypedDict `StaticLibrary`

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/plugins/openlibrary/lists.py:89:19: error[invalid-key] Unknown key "key" for TypedDict `AnnotatedSeedDict`: Unknown key "key"
+ openlibrary/plugins/openlibrary/lists.py:90:25: warning[redundant-cast] Value is already of type `ThingReferenceDict`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/_trace/utils_botocore/span_pointers/dynamodb.py:483:25: warning[redundant-cast] Value is already of type `_DynamoDBDeleteRequestWriteRequest`
+ ddtrace/_trace/utils_botocore/span_pointers/dynamodb.py:554:34: warning[redundant-cast] Value is already of type `_DynamoDBTransactUpdateItem`
- Found 8425 diagnostics
+ Found 8427 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2801 diagnostics
+ Found 2798 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5160 diagnostics
+ Found 5159 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/tts/__init__.py:292:53: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `MediaSourceOptions | str`
- homeassistant/components/tts/__init__.py:292:53: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `MediaSourceOptions | str`
- homeassistant/components/tts/__init__.py:292:62: error[invalid-key] Unknown key "options" for TypedDict `ParsedMediaSourceStreamId`: Unknown key "options"
- homeassistant/components/tts/__init__.py:293:41: error[invalid-key] Unknown key "message" for TypedDict `ParsedMediaSourceStreamId`: Unknown key "message"
- homeassistant/components/tts/media_source.py:145:61: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `MediaSourceOptions | str`
- homeassistant/components/tts/media_source.py:145:61: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `MediaSourceOptions | str`
- homeassistant/components/tts/media_source.py:145:70: error[invalid-key] Unknown key "options" for TypedDict `ParsedMediaSourceStreamId`: Unknown key "options"
- homeassistant/components/tts/media_source.py:146:49: error[invalid-key] Unknown key "message" for TypedDict `ParsedMediaSourceStreamId`: Unknown key "message"
- Found 14443 diagnostics
+ Found 14435 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-02 22:34_

---

_Comment by @felixscherz on 2026-01-02 22:34_

Sorry about that, I rebased onto the latest changes and fixed the compilation error.
I'm also going to fix the clippy error that failed the CI run.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:1155 on 2026-01-02 22:37_

```suggestion
                        union.filter(self.db, |ty| match ty {
                            Type::TypedDict(td) => !requires_key(td),
                            Type::Intersection(intersection) => {
                                !intersection.positive(self.db).iter().any(|ty| match ty {
                                    Type::TypedDict(td) => requires_key(td),
                                    _ => false,
                                })
                            }
                            _ => true,
                        })
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:1161 on 2026-01-02 22:39_

similar to https://github.com/astral-sh/ruff/pull/22348, I think this would be slightly more efficient if we used a "typeguard constraint" here rather than a "regular constraint". A regular constraint creates an intersection with the pre-existing type, but that's unnecessary here because we know what the intersection of the unfiltered union with the filtered union will simplify to: it will simplify to the filtered union.

We can also avoid adding the constraint if the narrowed type is exactly the same as the original type.

```suggestion
                if narrowed != rhs_type {
	                let place = self.expect_place(&rhs_place_expr);
	                constraints.insert(place, NarrowingConstraint::typeguard(narrowed));
                }
```

---

_@AlexWaygood reviewed on 2026-01-02 22:39_

Looks great!

---

_Renamed from "feat: narrow `TypedDict` unions with `not in`" to "[ty] narrow `TypedDict` unions with `not in`" by @AlexWaygood on 2026-01-02 22:41_

---

_Comment by @astral-sh-bot[bot] on 2026-01-02 22:41_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 4 | 2 |
| `invalid-key` | 0 | 6 | 0 |
| `invalid-return-type` | 0 | 1 | 4 |
| `redundant-cast` | 3 | 0 | 0 |
| `invalid-assignment` | 1 | 0 | 0 |
| **Total** | **4** | **11** | **6** |


**[Full report with detailed diff](https://df0f81c4.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://df0f81c4.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @codspeed-hq[bot] on 2026-01-02 22:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/felixscherz%3Afeat%2Fnarrow-typeddict-unions-with-not-in?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22349 will **not alter performance**

<sub>Comparing <code>felixscherz:feat/narrow-typeddict-unions-with-not-in</code> (840c4ad) with <code>main</code> (d0f841b)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  




[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/felixscherz%3Afeat%2Fnarrow-typeddict-unions-with-not-in?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@AlexWaygood approved on 2026-01-03 13:02_

Thank you!

---

_Merged by @AlexWaygood on 2026-01-03 13:12_

---

_Closed by @AlexWaygood on 2026-01-03 13:12_

---
