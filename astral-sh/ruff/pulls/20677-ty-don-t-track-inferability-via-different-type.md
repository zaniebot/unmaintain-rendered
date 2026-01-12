```yaml
number: 20677
title: "[ty] Don't track inferability via different `Type` variants"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/non-non-inferable
created_at: 2025-10-02T00:28:54Z
updated_at: 2025-10-16T19:59:48Z
url: https://github.com/astral-sh/ruff/pull/20677
synced_at: 2026-01-12T15:57:07Z
```

# [ty] Don't track inferability via different `Type` variants

---

_@dcreager_

We have to track whether a typevar appears in a position where it's inferable or not. In a non-inferable position (in the body of the generic class or function that binds it), assignability must hold for every possible specialization of the typevar. In an inferable position, it only needs to hold for _some_ specialization. https://github.com/astral-sh/ruff/pull/20093 is working on using constraint sets to model assignability of typevars, and the constraint sets that we produce will be the same for inferable vs non-inferable typevars; what changes is what we _compare_ that constraint set to. (For a non-inferable typevar, the constraint set must equal the set of valid specializations; for an inferable typevar, it must not be `never`.)

When I first added support for tracking inferable vs non-inferable typevars, it seemed like it would be easiest to have separate `Type` variants for each. The alternative (which lines up with the Δ set in [POPL15](https://doi.org/10.1145/2676726.2676991)) would be to explicitly plumb through a list of inferable typevars through our type property methods. That seemed cumbersome.

In retrospect, that was the wrong decision. We've had to jump through hoops to translate types between the inferable and non-inferable variants, which has been quite brittle. Combined with the original point above, that much of the assignability logic will become more identical between inferable and non-inferable, there is less justification for the two `Type` variants. And plumbing an extra `inferable` parameter through all of these methods turns out to not be as bad as I anticipated.

---

_Label `internal` added by @dcreager on 2025-10-02 00:28_

---

_Label `ty` added by @dcreager on 2025-10-02 00:28_

---

_Comment by @github-actions[bot] on 2025-10-02 00:30_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-02 00:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/local.py:511:24: error[invalid-return-type] Return type does not match returned value: expected `T@LocalProxy`, found `~None | T@LocalProxy | Unknown`
- Found 383 diagnostics
+ Found 382 diagnostics

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
+ src/typeshed_stats/gather.py:1000:13: error[invalid-argument-type] Argument to bound method `from_lines` is incorrect: Expected `str | ((str, /) -> Pattern)`, found `<class 'GitWildMatchPattern'>`
- Found 24 diagnostics
+ Found 25 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/config/_projects.py:534:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_parent` on type `T@from_hierarchy | Unknown`
- Found 321 diagnostics
+ Found 320 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/contentviews/test__utils.py:23:38: error[invalid-argument-type] Argument to function `make_metadata` is incorrect: Expected `Message | TCPMessage | UDPMessage | WebSocketMessage | DNSMessage`, found `Response | None`
+ test/mitmproxy/contentviews/test__utils.py:23:38: error[invalid-argument-type] Argument to function `make_metadata` is incorrect: Expected `ContentviewMessage`, found `Response | None`

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/sourceline.py:17:33: error[invalid-argument-type] Argument to function `_add_lc_filename` is incorrect: Argument type `AnyStr@_add_lc_filename` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- schema_salad/sourceline.py:20:33: error[invalid-argument-type] Argument to function `_add_lc_filename` is incorrect: Argument type `AnyStr@_add_lc_filename` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- Found 166 diagnostics
+ Found 164 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/backends/api.py:1362:42: error[invalid-argument-type] Argument to function `_remove_path` is incorrect: Expected `NestedSequence[_FLike@_remove_path]`, found `(_FLike@_remove_path & Top[list[Unknown]]) | (NestedSequence[_FLike@_remove_path] & Top[list[Unknown]])`
+ xarray/tests/test_namedarray.py:449:19: error[no-matching-overload] No overload of bound method `_new` matches arguments

meson (https://github.com/mesonbuild/meson)
- mesonbuild/modules/hotdoc.py:52:12: error[invalid-return-type] Return type does not match returned value: expected `list[_T@ensure_list]`, found `(_T@ensure_list & Top[list[Unknown]]) | list[_T@ensure_list]`
- Found 887 diagnostics
+ Found 886 diagnostics

vision (https://github.com/pytorch/vision)
- references/detection/coco_utils.py:165:60: warning[possibly-unresolved-reference] Name `keypoints` used when possibly not defined
- Found 1481 diagnostics
+ Found 1480 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/config/config_options.py:219:20: error[invalid-return-type] Return type does not match returned value: expected `list[T@ListOfItems]`, found `Top[list[Unknown]] & ~AlwaysTruthy`
- Found 206 diagnostics
+ Found 205 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/_internal/concurrency/api.py:39:16: warning[redundant-cast] Value is already of type `Call[T@cast_to_call]`
- src/prefect/states.py:365:16: error[invalid-return-type] Return type does not match returned value: expected `State[R@return_value_to_state]`, found `R@return_value_to_state & Top[State[Any]]`

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/book_providers.py:747:9: error[invalid-argument-type] Argument to function `multisort_best` is incorrect: Expected `list[tuple[Literal["min", "max"], (@Todo, /) -> int | float]]`, found `list[Unknown | tuple[str, (rec) -> Unknown]]`
- Found 862 diagnostics
+ Found 863 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/pymongo/client.py:340:68: warning[possibly-unresolved-reference] Name `cursor` used when possibly not defined
- Found 7593 diagnostics
+ Found 7592 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/util.py:107:16: error[invalid-return-type] Return type does not match returned value: expected `list[V@promote_list]`, found `(V@promote_list & Top[list[Unknown]]) | (Iterable[V@promote_list] & Top[list[Unknown]])`
- Found 3293 diagnostics
+ Found 3292 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/core/property/container.py:136:24: error[invalid-return-type] Return type does not match returned value: expected `PropertyValueList[T@List]`, found `list[T@List] & Top[PropertyValueList[Unknown]]`
- src/bokeh/core/property/container.py:161:24: error[invalid-return-type] Return type does not match returned value: expected `PropertyValueSet[T@Set]`, found `set[T@Set] & Top[PropertyValueSet[Unknown]]`
- src/bokeh/layouts.py:668:24: error[invalid-return-type] Return type does not match returned value: expected `list[L@_parse_children_arg]`, found `(L@_parse_children_arg & Top[list[Unknown]]) | list[L@_parse_children_arg]`
- Found 450 diagnostics
+ Found 447 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/frame.py:2170:16: error[invalid-return-type] Return type does not match returned value: expected `MutableMappingT@to_dict | list[MutableMappingT@to_dict]`, found `@Todo | MutableMappingT@to_dict | <class 'dict'> | list[@Todo | MutableMappingT@to_dict | <class 'dict'>]`
- Found 3391 diagnostics
+ Found 3390 diagnostics

jax (https://github.com/google/jax)
- jax/_src/pallas/fuser/block_spec.py:1517:39: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 2397 diagnostics
+ Found 2396 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/functions/combinatorial/numbers.py:3193:61: warning[possibly-unresolved-reference] Name `s` used when possibly not defined
- sympy/ntheory/factor_.py:148:43: warning[possibly-unresolved-reference] Name `facs` used when possibly not defined
- sympy/ntheory/factor_.py:148:43: warning[possibly-unresolved-reference] Name `facs` used when possibly not defined
- sympy/polys/rings.py:871:22: error[unresolved-attribute] Type `object` has no attribute `domain`
+ sympy/polys/rings.py:872:55: error[unresolved-attribute] Type `object` has no attribute `is_element`
- sympy/polys/rings.py:880:20: error[invalid-return-type] Return type does not match returned value: expected `PolyElement[Er@PolyElement] | PolyElement[PolyElement[Er@PolyElement]]`, found `Top[PolyElement[Unknown]]`
- sympy/polys/rings.py:931:22: error[unresolved-attribute] Type `object` has no attribute `domain`
+ sympy/polys/rings.py:932:55: error[unresolved-attribute] Type `object` has no attribute `is_element`
- sympy/polys/rings.py:940:20: error[invalid-return-type] Return type does not match returned value: expected `PolyElement[Er@PolyElement] | PolyElement[PolyElement[Er@PolyElement]]`, found `Top[PolyElement[Unknown]]`
- sympy/polys/rings.py:1008:22: error[unresolved-attribute] Type `object` has no attribute `domain`
+ sympy/polys/rings.py:1009:55: error[unresolved-attribute] Type `object` has no attribute `is_element`
+ sympy/polys/rings.py:1098:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/rings.py:1133:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/rings.py:1169:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/rings.py:1206:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sympy/polys/rings.py:1017:20: error[invalid-return-type] Return type does not match returned value: expected `PolyElement[Er@PolyElement] | PolyElement[PolyElement[Er@PolyElement]]`, found `Top[PolyElement[Unknown]]`
- sympy/polys/rings.py:1095:28: error[unresolved-attribute] Type `object` has no attribute `domain`
- sympy/polys/rings.py:1096:21: error[unresolved-attribute] Type `object` has no attribute `domain`
- sympy/polys/rings.py:1130:28: error[unresolved-attribute] Type `object` has no attribute `domain`
- sympy/polys/rings.py:1131:21: error[unresolved-attribute] Type `object` has no attribute `domain`
- sympy/polys/rings.py:1166:28: error[unresolved-attribute] Type `object` has no attribute `domain`
- sympy/polys/rings.py:1167:21: error[unresolved-attribute] Type `object` has no attribute `domain`
- sympy/polys/rings.py:1203:28: error[unresolved-attribute] Type `object` has no attribute `domain`
- sympy/polys/rings.py:1204:21: error[unresolved-attribute] Type `object` has no attribute `domain`
- sympy/polys/rings.py:3456:17: error[unsupported-operator] Operator `*` is unsupported between objects of type `PolyElement[Er@PolyElement] | Unknown` and `Unknown | PolyElement[Er@PolyElement] | int | float`
- Found 11057 diagnostics
+ Found 11046 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/auth/permissions/merge.py:63:13: error[invalid-assignment] Method `__setitem__` of type `bound method Top[dict[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Never` and a value of type `Mapping[str, Mapping[str, Mapping[str, bool] | bool | None] | bool | None] | bool | None` on object of type `Mapping[str, SubCategoryType] & Top[dict[Unknown, Unknown]]`
+ homeassistant/auth/permissions/merge.py:63:13: error[invalid-assignment] Method `__setitem__` of type `bound method Top[dict[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Never` and a value of type `CategoryType` on object of type `Mapping[str, SubCategoryType] & Top[dict[Unknown, Unknown]]`
- homeassistant/components/google_assistant/report_state.py:164:54: error[invalid-argument-type] Argument to function `create_checker` is incorrect: Expected `((HomeAssistant, str, dict[Unknown, Unknown] | MappingProxyType[Unknown, Unknown], Any, str, dict[Unknown, Unknown] | MappingProxyType[Unknown, Unknown], Any, /) -> bool | None) | None`, found `def extra_significant_check(hass: HomeAssistant, old_state: str, old_attrs: dict[Unknown, Unknown], old_extra_arg: dict[Unknown, Unknown], new_state: str, new_attrs: dict[Unknown, Unknown], new_extra_arg: dict[Unknown, Unknown]) -> Unknown`
+ homeassistant/components/google_assistant/report_state.py:164:54: error[invalid-argument-type] Argument to function `create_checker` is incorrect: Expected `ExtraCheckTypeFunc | None`, found `def extra_significant_check(hass: HomeAssistant, old_state: str, old_attrs: dict[Unknown, Unknown], old_extra_arg: dict[Unknown, Unknown], new_state: str, new_attrs: dict[Unknown, Unknown], new_extra_arg: dict[Unknown, Unknown]) -> Unknown`
- homeassistant/components/otbr/__init__.py:40:57: error[invalid-argument-type] Argument to function `async_register_firmware_info_provider` is incorrect: Expected `SyncHardwareFirmwareInfoModule | AsyncHardwareFirmwareInfoModule`, found `<module 'homeassistant.components.otbr.homeassistant_hardware'>`
+ homeassistant/components/otbr/__init__.py:40:57: error[invalid-argument-type] Argument to function `async_register_firmware_info_provider` is incorrect: Expected `HardwareFirmwareInfoModule`, found `<module 'homeassistant.components.otbr.homeassistant_hardware'>`
- homeassistant/components/owntracks/__init__.py:148:9: error[invalid-assignment] Method `__setitem__` of type `(bound method dict[str, Any].__setitem__(key: str, value: Any, /) -> None) | (Overload[(key: SupportsIndex, value: Any, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Any], /) -> None])` cannot be called with a key of type `Literal["topic"]` and a value of type `Unknown` on object of type `dict[str, Any] | list[Any] | str | ... omitted 3 union elements`
+ homeassistant/components/owntracks/__init__.py:148:9: error[invalid-assignment] Method `__setitem__` of type `(bound method JsonValueType.__setitem__(key: str, value: dict[str, Any] | list[Any] | str | ... omitted 3 union elements, /) -> None) | (Overload[(key: SupportsIndex, value: dict[str, Any] | list[Any] | str | ... omitted 3 union elements, /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[str, Any] | list[Any] | str | ... omitted 3 union elements], /) -> None])` cannot be called with a key of type `Literal["topic"]` and a value of type `Unknown` on object of type `JsonValueType`
- homeassistant/components/random/config_flow.py:113:82: warning[possibly-unresolved-reference] Name `units` used when possibly not defined
- homeassistant/components/random/config_flow.py:113:82: warning[possibly-unresolved-reference] Name `units` used when possibly not defined
- homeassistant/components/template/config_flow.py:433:82: warning[possibly-unresolved-reference] Name `units` used when possibly not defined
- homeassistant/components/template/config_flow.py:433:82: warning[possibly-unresolved-reference] Name `units` used when possibly not defined
- homeassistant/components/template/config_flow.py:457:54: warning[possibly-unresolved-reference] Name `state_classes` used when possibly not defined
- homeassistant/components/template/config_flow.py:457:54: warning[possibly-unresolved-reference] Name `state_classes` used when possibly not defined
- homeassistant/components/zha/__init__.py:118:57: error[invalid-argument-type] Argument to function `async_register_firmware_info_provider` is incorrect: Expected `SyncHardwareFirmwareInfoModule | AsyncHardwareFirmwareInfoModule`, found `<module 'homeassistant.components.zha.homeassistant_hardware'>`
+ homeassistant/components/zha/__init__.py:118:57: error[invalid-argument-type] Argument to function `async_register_firmware_info_provider` is incorrect: Expected `HardwareFirmwareInfoModule`, found `<module 'homeassistant.components.zha.homeassistant_hardware'>`
+ homeassistant/core.py:1527:48: error[invalid-argument-type] Argument to function `_event_repr` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[_DataT@async_fire_internal] | str`
+ homeassistant/core.py:1547:17: error[invalid-assignment] Object of type `Event[Mapping[str, Any]]` is not assignable to `Event[_DataT@async_fire_internal] | None`
+ homeassistant/core.py:1548:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[_DataT@async_fire_internal] | str`
- homeassistant/helpers/singleton.py:83:12: error[invalid-return-type] Return type does not match returned value: expected `(_FuncType, /) -> _FuncType`, found `Overload[(func: (HomeAssistant, /) -> Coroutine[Any, Any, _T@singleton]) -> (HomeAssistant, /) -> Coroutine[Any, Any, _T@singleton], (func: (HomeAssistant, /) -> _U@singleton) -> (HomeAssistant, /) -> _U@singleton]`
+ homeassistant/helpers/singleton.py:83:12: error[invalid-return-type] Return type does not match returned value: expected `(_FuncType, /) -> _FuncType`, found `Overload[(func: _FuncType) -> _FuncType, (func: _FuncType) -> _FuncType]`
+ homeassistant/util/hass_dict.pyi:146:5: error[type-assertion-failure] Argument does not have asserted type `dict[str, int]`
+ homeassistant/util/hass_dict.pyi:147:5: error[type-assertion-failure] Argument does not have asserted type `int`
+ homeassistant/util/hass_dict.pyi:155:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ homeassistant/util/hass_dict.pyi:170:5: error[type-assertion-failure] Argument does not have asserted type `dict[str, int]`
- Found 13760 diagnostics
+ Found 13761 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct metadata = ~9MB
+     struct metadata = ~8MB

```
</details>


---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-02 01:24_

---

_Comment by @github-actions[bot] on 2025-10-02 01:29_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 5 | 5 | 4 |
| `unresolved-attribute` | 3 | 11 | 0 |
| `invalid-return-type` | 0 | 12 | 1 |
| `no-matching-overload` | 1 | 11 | 0 |
| `unused-ignore-comment` | 5 | 0 | 0 |
| `invalid-assignment` | 1 | 1 | 2 |
| `type-assertion-failure` | 3 | 0 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| `unsupported-operator` | 0 | 1 | 0 |
| **Total** | **19** | **41** | **7** |

**[Full report with detailed diff](https://dcreager-non-non-inferable.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-non-non-inferable.ecosystem-663.pages.dev/timing))


---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-10-02 18:11_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-02 18:11_

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-10-03 19:28_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-03 19:28_

---

_Comment by @dcreager on 2025-10-03 19:52_

ecosystem analysis:

Several projects have removed diagnostics, which look like correct better understanding of the types involved:

- `hauntsaninja/boostedblob`
- `homeassistant/core`
- `cwltool`
- `jax`
- `meson`
- `pytest`
- `schema_salad`
- `scikit-build-core`
- `strawberry`
- `werkzeug`
- `xarray`

Still looking at some of the new diagnostics. It also looks like we aren't expanding type aliases in as many places as before.

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-10-07 13:36_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-07 13:36_

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-10-07 17:35_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-07 17:40_

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-10-08 13:55_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-08 13:55_

---

_Comment by @codspeed-hq[bot] on 2025-10-08 14:05_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fnon-non-inferable)

### Merging #20677 will **improve performances by 4.49%**

<sub>Comparing <code>dcreager/non-non-inferable</code> (0997627) with <code>main</code> (1ade4f2)</sub>



### Summary

`⚡ 1` improvement  
`✅ 20` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Instrumentation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fnon-non-inferable?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation) | 784.1 ms | 750.3 ms | +4.49% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fnon-non-inferable?sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-10-10 19:16_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-10 19:16_

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-10-11 21:23_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-11 21:23_

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-10-14 00:17_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-14 00:35_

---

_Marked ready for review by @dcreager on 2025-10-14 19:28_

---

_Review requested from @carljm by @dcreager on 2025-10-14 19:28_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-14 19:28_

---

_Review requested from @sharkdp by @dcreager on 2025-10-14 19:28_

---

_Review requested from @MichaReiser by @dcreager on 2025-10-14 19:28_

---

_Comment by @dcreager on 2025-10-14 19:30_

The remaining ecosystem additions seem related to bidi checking of arguments and overload resolution. This overlaps with some of the work @ibraheemdev is doing e.g. in https://github.com/astral-sh/ruff/pull/20796, so for now, I'm opening this up for review, and would like to address the remaining ecosystem results with him as part of that work.

---

_Comment by @dcreager on 2025-10-14 19:31_

(Note that I pulled the pure refactoring part of this PR out into a separate https://github.com/astral-sh/ruff/pull/20865, so that it's easier to review the logic changes separate from the boilerplate API changes. Please review 20865 first)

---

_Review requested from @ibraheemdev by @MichaReiser on 2025-10-15 09:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1723 on 2025-10-16 17:37_

nit: this kind of logic might read more naturally if the `is_inferable` method was a method on `BoundTypeVarInstance` rather than `Inferable`? then this could be

```suggestion
            (Type::TypeVar(bound_typevar), Type::Union(union))
                if !bound_typevar.is_inferable(db, inferable)
                    && union.elements(db).contains(&self) =>
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2363 on 2025-10-16 17:40_

what happens if we fail to uphold this invariant... would that lead to very strange diagnostic messages later down the line? If so, might it be worth asserting this in release builds too? Or would that be prohibitively expensive?

```suggestion
                // All inferable cases should have been handled above
                assert!(!inferable.is_inferable(db, bound_typevar));
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3311 on 2025-10-16 17:42_

It looks like we previously always returned `false` here if it was an inferable `TypeVar` -- is the change in logic deliberate here?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3420 on 2025-10-16 17:44_

same here, this looks like a behaviour change -- is that deliberate? If this fixes a bug, would it be possible to add a test that makes sure it doesn't regress?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:331 on 2025-10-16 17:50_

lol. Is it worth nesting these methods inside the `inferable_typevars` method, so that it's obvious to readers that they're implementation details of that method?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:371 on 2025-10-16 17:53_

hmm, same comment as https://github.com/astral-sh/ruff/pull/20822#discussion_r2424885000 ;)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:611 on 2025-10-16 17:56_

(obviously not blocking, just curious) -- are the parentheses around `self.generic_context` to try to get rustfmt to format this the way you'd like it to be formatted?

---

_@AlexWaygood approved on 2025-10-16 17:59_

Looks great, thank you! Very relieved that I no longer have to worry about the differences between these two variants!

There's a couple of places where it looks like our behaviour is changing subtly, where I wonder if we could add regression tests if the new behaviour is desirable? Especially if this is the cause of the changes in the ecosystem. But if that's hard to do, then no need to spend too much time on it

---

_@ibraheemdev approved on 2025-10-16 18:42_

Very nice!

---

_@ibraheemdev reviewed on 2025-10-16 18:45_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/generics.rs`:331 on 2025-10-16 18:45_

Do you remember what the issue was here? Might be worth opening a Salsa issue, the macros should be relatively transparent.

---

_Comment by @dcreager on 2025-10-16 18:48_

The performance analysis keeps bouncing back and forth between being a regression and an improvement as I rebase to pick up more changes from `main`. :angry: 

---

_@dcreager reviewed on 2025-10-16 18:49_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:2363 on 2025-10-16 18:49_

Should not be too expensive — this was previously a case that the compiler rejected as not representable, so it shouldn't occur in practice. I'll change to a full assert as you suggest

---

_@dcreager reviewed on 2025-10-16 18:50_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1723 on 2025-10-16 18:50_

Ooh I like that

---

_Comment by @AlexWaygood on 2025-10-16 18:52_

> The performance analysis keeps bouncing back and forth between being a regression and an improvement as I rebase to pick up more changes from `main`. 😠

the `DateType` benchmark does move quite a lot in general because it's a small codebase that has some _very_ expensive subtype checks in it. (We originally added it because an early version of one of my protocol PRs would have meant that we never finished type-checking `DateType` 🙃)

So `DateType` is just very sensitive to small changes in efficiency of subtype checks between the specific types that are being used in that codebase. It looks like this PR is still a performance improvement on hydra-zen, which is a much bigger codebase!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1723 on 2025-10-16 18:56_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:3311 on 2025-10-16 19:00_

This reverts back to [what we had](https://github.com/astral-sh/ruff/pull/19786/files#diff-8049ab5af787dba29daa389bbe2b691560c15461ef536f122b1beab112a4b48aL2386) before #19786, which introduced the `NonInferableTypeVar` variant. I can't think of any way that an inferable typevar would affect whether we consider something a singleton — they should only appear in function/callable signatures, and we don't have to look at the signature to determine whether a function or callable is a singleton. Realistically, the `false` branch from before should have included a `debug_assert`.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:3420 on 2025-10-16 19:01_

Ditto above

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:371 on 2025-10-16 19:04_

whopps, done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:331 on 2025-10-16 19:08_

Putting `innerer` inside of `inner` didn't work, but I don't remember the specific error. Put all of it inside of the outer function works, though, since the type and `impl` block don't need to be inside the salsa-tracked function that way.

---

_@dcreager reviewed on 2025-10-16 19:10_

---

_@dcreager reviewed on 2025-10-16 19:11_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:611 on 2025-10-16 19:11_

Yep

---

_@AlexWaygood reviewed on 2025-10-16 19:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3311 on 2025-10-16 19:12_

Thanks!

---

_Merged by @dcreager on 2025-10-16 19:59_

---

_Closed by @dcreager on 2025-10-16 19:59_

---

_Branch deleted on 2025-10-16 19:59_

---
