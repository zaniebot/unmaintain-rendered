```yaml
number: 21862
title: " [ty] Infer type variables within generic unions "
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-1772
created_at: 2025-12-09T10:51:39Z
updated_at: 2025-12-09T15:23:02Z
url: https://github.com/astral-sh/ruff/pull/21862
synced_at: 2026-01-10T16:42:11Z
```

#  [ty] Infer type variables within generic unions 

---

_Pull request opened by @sharkdp on 2025-12-09 10:51_

## Summary

This PR allows our generics solver to find a solution for `T` in cases like the following:
```py
def extract_t[T](x: P[T] | Q[T]) -> T:
    raise NotImplementedError

reveal_type(extract_t(P[int]()))  # revealed: int
reveal_type(extract_t(Q[str]()))  # revealed: str
```

closes https://github.com/astral-sh/ty/issues/1772
closes https://github.com/astral-sh/ty/issues/1314

## Ecosystem

The impact here looks very good!

It took me a long time to figure this out, but the new diagnostics on bokeh are actually true positives. I should have tested with another type-checker immediately, I guess. All other type checkers also emit errors on these `__init__` calls. MRE [here](https://play.ty.dev/5c19d260-65e2-4f70-a75e-1a25780843a2) (no error on main, diagnostic on this branch)

A lot of false positives on home-assistant go away for calls to functions like [`async_listen`](https://github.com/home-assistant/core/blob/180053fe9859f2a201ed2c33375db5316b50b7b5/homeassistant/core.py#L1581-L1587) which take a `event_type: EventType[_DataT] | str` parameter. We can now solve for `_DataT` here, which was previously falling back to its default value, and then caused problems because it was used as an argument to an invariant generic class.

## Test Plan

New Markdown tests

---

_Label `ty` added by @sharkdp on 2025-12-09 10:51_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-09 10:51_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 10:53_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-09 10:55_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/datastructures/auth.py:201:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, str | None]`, found `Unknown | CallbackDict[str, str] | dict[str, str | None]`
- src/werkzeug/datastructures/auth.py:205:9: error[unresolved-attribute] Cannot assign object of type `CallbackDict[Unknown, Unknown]` to attribute `_parameters` on type `Self@parameters` with custom `__setattr__` method.
+ src/werkzeug/datastructures/auth.py:205:9: error[unresolved-attribute] Cannot assign object of type `CallbackDict[str, str]` to attribute `_parameters` on type `Self@parameters` with custom `__setattr__` method.
- Found 366 diagnostics
+ Found 367 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- testing/test_runner.py:101:22: error[unresolved-attribute] Object of type `BaseException` has no attribute `exceptions`
- testing/test_runner.py:138:21: error[unresolved-attribute] Object of type `BaseException` has no attribute `exceptions`
- Found 446 diagnostics
+ Found 444 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/instance/kubernetes.py:858:12: error[invalid-return-type] Return type does not match returned value: expected `KubernetesVersionDict`, found `dict[Unknown | str, Unknown | str | int | list[Unknown] | None]`
+ paasta_tools/instance/kubernetes.py:858:12: error[invalid-return-type] Return type does not match returned value: expected `KubernetesVersionDict`, found `dict[Unknown | str, Unknown | str | int | ... omitted 3 union elements]`

psycopg (https://github.com/psycopg/psycopg)
- tests/utils.py:166:14: error[invalid-context-manager] Object of type `ExceptionInfo[BaseException]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ tests/utils.py:166:14: error[invalid-context-manager] Object of type `ExceptionInfo[Unknown]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`

optuna (https://github.com/optuna/optuna)
- optuna/storages/_rdb/alembic/versions/v3.0.0.a.py:159:17: error[invalid-argument-type] Argument to function `migrate_new_distribution` is incorrect: Expected `str`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/alembic/versions/v3.0.0.a.py:159:17: error[invalid-argument-type] Argument to function `migrate_new_distribution` is incorrect: Expected `str`, found `Unknown | Column[str]`
- optuna/storages/_rdb/alembic/versions/v3.0.0.a.py:188:17: error[invalid-argument-type] Argument to function `restore_old_distribution` is incorrect: Expected `str`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/alembic/versions/v3.0.0.a.py:188:17: error[invalid-argument-type] Argument to function `restore_old_distribution` is incorrect: Expected `str`, found `Unknown | Column[str]`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/interpreter/type_checking.py:476:5: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[list[str | File | CustomTarget | ... omitted 5 union elements]] | tuple[type[list[str | File | CustomTarget | ... omitted 5 union elements]] | ContainerTypeInfo, ...] | ContainerTypeInfo`, found `tuple[<class 'NoneType'>, ContainerTypeInfo]`
+ mesonbuild/interpreter/type_checking.py:474:48: error[invalid-assignment] Object of type `KwargInfo[list[str | File | CustomTarget | ... omitted 5 union elements] | None]` is not assignable to `KwargInfo[list[str | File | CustomTarget | ... omitted 5 union elements]]`
+ mesonbuild/interpreter/type_checking.py:481:45: error[invalid-assignment] Object of type `KwargInfo[dict[str, str] | str]` is not assignable to `KwargInfo[dict[str, str]]`
+ mesonbuild/interpreter/type_checking.py:485:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1933 diagnostics
+ Found 1935 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/flow_runs.py:270:26: error[invalid-assignment] Object of type `type[AutomaticRunInput[Unknown]] | type[Unknown]` is not assignable to `type[T@_in_process_pause] | None`
+ src/prefect/flow_runs.py:270:26: error[invalid-assignment] Object of type `type[AutomaticRunInput[@Todo]] | type[Unknown]` is not assignable to `type[T@_in_process_pause] | None`
- src/prefect/flow_runs.py:425:26: error[invalid-assignment] Object of type `type[AutomaticRunInput[Unknown]] | type[Unknown]` is not assignable to `type[T@suspend_flow_run] | None`
+ src/prefect/flow_runs.py:425:26: error[invalid-assignment] Object of type `type[AutomaticRunInput[@Todo]] | type[Unknown]` is not assignable to `type[T@suspend_flow_run] | None`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 38 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
+ tests/optimize/test_constraints.pyi:26:1: error[type-assertion-failure] Type `Bounds[tuple[int], float64]` does not match asserted type `Bounds[tuple[int], float64 | signedinteger[_64Bit]]`
- tests/sparse/test_lil.pyi:29:1: error[type-assertion-failure] Type `lil_matrix[floating[_32Bit]]` does not match asserted type `lil_matrix[Any]`
- tests/sparse/test_lil.pyi:30:1: error[type-assertion-failure] Type `lil_matrix[floating[_32Bit]]` does not match asserted type `lil_matrix[Any]`
- tests/sparse/test_lil.pyi:52:1: error[type-assertion-failure] Type `lil_array[floating[_32Bit]]` does not match asserted type `lil_array[Any]`
- tests/sparse/test_lil.pyi:53:1: error[type-assertion-failure] Type `lil_array[floating[_32Bit]]` does not match asserted type `lil_array[Any]`
- Found 1214 diagnostics
+ Found 1211 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/document/config.py:69:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Notifications | InstanceDefault[Notifications]]] | Property[Notifications | InstanceDefault[Notifications]]`, found `Instance[Notifications]`
- src/bokeh/models/annotations/annotation.py:67:14: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[ColumnDataSource]]`
+ src/bokeh/models/annotations/annotation.py:67:14: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[DataSource | InstanceDefault[ColumnDataSource]]`
- src/bokeh/models/annotations/annotation.py:67:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[ColumnDataSource]] | (() -> type[InstanceDefault[ColumnDataSource]]) | str`, found `<class 'DataSource'>`
+ src/bokeh/models/annotations/arrows.py:187:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[ArrowHead | InstanceDefault[OpenHead]]] | Property[ArrowHead | InstanceDefault[OpenHead]]`, found `Instance[ArrowHead]`
- src/bokeh/models/annotations/dimensional.py:104:16: error[unsupported-operator] Operator `in` is not supported between objects of type `str` and `Unknown | Required[Unknown]`
+ src/bokeh/models/annotations/dimensional.py:104:16: error[unsupported-operator] Operator `in` is not supported between objects of type `str` and `Unknown | Required[Any]`
- src/bokeh/models/annotations/geometry.py:298:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[() -> Unknown]`
+ src/bokeh/models/annotations/geometry.py:298:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[BoxInteractionHandles | (() -> Unknown)]`
+ src/bokeh/models/annotations/geometry.py:560:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[ArrowHead | InstanceDefault[TeeHead]]] | Property[ArrowHead | InstanceDefault[TeeHead]]`, found `Instance[ArrowHead]`
+ src/bokeh/models/annotations/geometry.py:568:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[ArrowHead | InstanceDefault[TeeHead]]] | Property[ArrowHead | InstanceDefault[TeeHead]]`, found `Instance[ArrowHead]`
- src/bokeh/models/annotations/legends.py:170:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[NoOverlap]]`
+ src/bokeh/models/annotations/legends.py:170:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[LabelingPolicy | InstanceDefault[NoOverlap]]`
- src/bokeh/models/annotations/legends.py:170:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[NoOverlap]] | (() -> type[InstanceDefault[NoOverlap]]) | str`, found `<class 'LabelingPolicy'>`
- src/bokeh/models/annotations/legends.py:352:44: error[not-iterable] Object of type `Unknown | List[Unknown]` may not be iterable
+ src/bokeh/models/annotations/legends.py:352:44: error[not-iterable] Object of type `Unknown | List[GlyphRenderer[Unknown]]` may not be iterable
- src/bokeh/models/annotations/legends.py:358:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | List[Unknown]`
+ src/bokeh/models/annotations/legends.py:358:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | List[GlyphRenderer[Unknown]]`
- src/bokeh/models/annotations/legends.py:360:22: error[non-subscriptable] Cannot subscript object of type `List[Unknown]` with no `__getitem__` method
+ src/bokeh/models/annotations/legends.py:360:22: error[non-subscriptable] Cannot subscript object of type `List[GlyphRenderer[Unknown]]` with no `__getitem__` method
- src/bokeh/models/annotations/legends.py:592:19: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[MetricLength]]`
+ src/bokeh/models/annotations/legends.py:592:19: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Dimensional | InstanceDefault[MetricLength]]`
- src/bokeh/models/annotations/legends.py:592:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[MetricLength]] | (() -> type[InstanceDefault[MetricLength]]) | str`, found `<class 'Dimensional'>`
- src/bokeh/models/annotations/legends.py:722:14: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[FixedTicker]]`
+ src/bokeh/models/annotations/legends.py:722:14: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Ticker | InstanceDefault[FixedTicker]]`
- src/bokeh/models/annotations/legends.py:722:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[FixedTicker]] | (() -> type[InstanceDefault[FixedTicker]]) | str`, found `<class 'Ticker'>`
- src/bokeh/models/annotations/legends.py:823:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[NoOverlap]]`
+ src/bokeh/models/annotations/legends.py:823:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[LabelingPolicy | InstanceDefault[NoOverlap]]`
- src/bokeh/models/annotations/legends.py:823:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[NoOverlap]] | (() -> type[InstanceDefault[NoOverlap]]) | str`, found `<class 'LabelingPolicy'>`
- src/bokeh/models/axes.py:201:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[AllLabels]]`
+ src/bokeh/models/axes.py:201:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[LabelingPolicy | InstanceDefault[AllLabels]]`
- src/bokeh/models/axes.py:201:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[AllLabels]] | (() -> type[InstanceDefault[AllLabels]]) | str`, found `<class 'LabelingPolicy'>`
- src/bokeh/models/callbacks.py:212:21: error[invalid-assignment] Object of type `Required[Unknown]` is not assignable to `HasProps`
+ src/bokeh/models/callbacks.py:212:21: error[invalid-assignment] Object of type `Required[HasProps]` is not assignable to `HasProps`
- src/bokeh/models/coordinates.py:48:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[DataRange1d]]`
+ src/bokeh/models/coordinates.py:48:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Range | InstanceDefault[DataRange1d]]`
- src/bokeh/models/coordinates.py:48:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[DataRange1d]] | (() -> type[InstanceDefault[DataRange1d]]) | str`, found `<class 'Range'>`
- src/bokeh/models/coordinates.py:52:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[DataRange1d]]`
+ src/bokeh/models/coordinates.py:52:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Range | InstanceDefault[DataRange1d]]`
- src/bokeh/models/coordinates.py:52:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[DataRange1d]] | (() -> type[InstanceDefault[DataRange1d]]) | str`, found `<class 'Range'>`
- src/bokeh/models/coordinates.py:56:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[LinearScale]]`
+ src/bokeh/models/coordinates.py:56:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Scale | InstanceDefault[LinearScale]]`
- src/bokeh/models/coordinates.py:56:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[LinearScale]] | (() -> type[InstanceDefault[LinearScale]]) | str`, found `<class 'Scale'>`
- src/bokeh/models/coordinates.py:61:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[LinearScale]]`
+ src/bokeh/models/coordinates.py:61:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Scale | InstanceDefault[LinearScale]]`
- src/bokeh/models/coordinates.py:61:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[LinearScale]] | (() -> type[InstanceDefault[LinearScale]]) | str`, found `<class 'Scale'>`
- src/bokeh/models/dom.py:182:12: warning[possibly-missing-attribute] Attribute `lookup` may be missing on object of type `Unknown | Required[Unknown]`
+ src/bokeh/models/dom.py:182:12: warning[possibly-missing-attribute] Attribute `lookup` may be missing on object of type `Unknown | Required[HasProps]`
- src/bokeh/models/glyphs.py:842:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[LinearColorMapper]]`
+ src/bokeh/models/glyphs.py:842:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[ColorMapper | InstanceDefault[LinearColorMapper]]`
- src/bokeh/models/glyphs.py:842:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[LinearColorMapper]] | (() -> type[InstanceDefault[LinearColorMapper]]) | str`, found `<class 'ColorMapper'>`
- src/bokeh/models/layouts.py:409:44: error[not-iterable] Object of type `Unknown | List[Unknown]` may not be iterable
+ src/bokeh/models/layouts.py:409:44: error[not-iterable] Object of type `Unknown | List[Any]` may not be iterable
- src/bokeh/models/layouts.py:481:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | List[Unknown]`
+ src/bokeh/models/layouts.py:481:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | List[UIElement]`
- src/bokeh/models/layouts.py:487:18: error[not-iterable] Object of type `Unknown | List[Unknown]` may not be iterable
+ src/bokeh/models/layouts.py:487:18: error[not-iterable] Object of type `Unknown | List[UIElement]` may not be iterable
- src/bokeh/models/layouts.py:497:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | List[Unknown]`
+ src/bokeh/models/layouts.py:497:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | List[UIElement]`
- src/bokeh/models/layouts.py:497:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | List[Unknown]`
+ src/bokeh/models/layouts.py:497:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | List[UIElement]`
- src/bokeh/models/layouts.py:521:57: error[not-iterable] Object of type `Unknown | List[Unknown]` may not be iterable
+ src/bokeh/models/layouts.py:521:57: error[not-iterable] Object of type `Unknown | List[UIElement]` may not be iterable
- src/bokeh/models/layouts.py:523:53: error[not-iterable] Object of type `Unknown | List[Unknown]` may not be iterable
+ src/bokeh/models/layouts.py:523:53: error[not-iterable] Object of type `Unknown | List[UIElement]` may not be iterable
- src/bokeh/models/layouts.py:537:57: error[not-iterable] Object of type `Unknown | List[Unknown]` may not be iterable
+ src/bokeh/models/layouts.py:537:57: error[not-iterable] Object of type `Unknown | List[UIElement]` may not be iterable
- src/bokeh/models/layouts.py:539:53: error[not-iterable] Object of type `Unknown | List[Unknown]` may not be iterable
+ src/bokeh/models/layouts.py:539:53: error[not-iterable] Object of type `Unknown | List[UIElement]` may not be iterable
- src/bokeh/models/plots.py:238:18: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | List[Unknown]` and `Unknown | List[Unknown]`
+ src/bokeh/models/plots.py:238:18: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | List[Any]` and `Unknown | List[Any]`
- src/bokeh/models/plots.py:251:32: error[not-iterable] Object of type `Unknown | List[Unknown]` may not be iterable
+ src/bokeh/models/plots.py:251:32: error[not-iterable] Object of type `Unknown | List[Any]` may not be iterable
- src/bokeh/models/plots.py:389:9: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | List[Unknown]`
+ src/bokeh/models/plots.py:389:9: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | List[Renderer]`
- src/bokeh/models/plots.py:442:9: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | List[Unknown]`
+ src/bokeh/models/plots.py:442:9: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | List[Renderer]`
- src/bokeh/models/plots.py:501:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | List[Unknown]`
+ src/bokeh/models/plots.py:501:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | List[Renderer]`
- src/bokeh/models/plots.py:501:57: error[not-iterable] Object of type `Unknown | List[Unknown]` may not be iterable
+ src/bokeh/models/plots.py:501:57: error[not-iterable] Object of type `Unknown | List[Any]` may not be iterable
- src/bokeh/models/plots.py:523:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[DataRange1d]]`
+ src/bokeh/models/plots.py:523:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Range | InstanceDefault[DataRange1d]]`
- src/bokeh/models/plots.py:523:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[DataRange1d]] | (() -> type[InstanceDefault[DataRange1d]]) | str`, found `<class 'Range'>`
- src/bokeh/models/plots.py:527:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[DataRange1d]]`
+ src/bokeh/models/plots.py:527:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Range | InstanceDefault[DataRange1d]]`
- src/bokeh/models/plots.py:527:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[DataRange1d]] | (() -> type[InstanceDefault[DataRange1d]]) | str`, found `<class 'Range'>`
- src/bokeh/models/plots.py:531:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[LinearScale]]`
+ src/bokeh/models/plots.py:531:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Scale | InstanceDefault[LinearScale]]`
- src/bokeh/models/plots.py:531:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[LinearScale]] | (() -> type[InstanceDefault[LinearScale]]) | str`, found `<class 'Scale'>`
- src/bokeh/models/plots.py:536:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[LinearScale]]`
+ src/bokeh/models/plots.py:536:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Scale | InstanceDefault[LinearScale]]`
- src/bokeh/models/plots.py:536:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[LinearScale]] | (() -> type[InstanceDefault[LinearScale]]) | str`, found `<class 'Scale'>`
- src/bokeh/models/plots.py:606:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[Toolbar]]`
+ src/bokeh/models/plots.py:606:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Toolbar | InstanceDefault[Toolbar]]`
- src/bokeh/models/plots.py:606:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[Toolbar]] | (() -> type[InstanceDefault[Toolbar]]) | str`, found `<class 'Toolbar'>`
- src/bokeh/models/plots.py:929:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[Toolbar]]`
+ src/bokeh/models/plots.py:929:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Toolbar | InstanceDefault[Toolbar]]`
- src/bokeh/models/plots.py:929:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[Toolbar]] | (() -> type[InstanceDefault[Toolbar]]) | str`, found `<class 'Toolbar'>`
- src/bokeh/models/plots.py:948:44: error[not-iterable] Object of type `Unknown | List[Unknown]` may not be iterable
+ src/bokeh/models/plots.py:948:44: error[not-iterable] Object of type `Unknown | List[Any]` may not be iterable
- src/bokeh/models/renderers/contour_renderer.py:82:29: warning[possibly-missing-attribute] Attribute `data_source` may be missing on object of type `Unknown | Instance[Unknown]`
+ src/bokeh/models/renderers/contour_renderer.py:82:29: warning[possibly-missing-attribute] Attribute `data_source` may be missing on object of type `Unknown | Instance[GlyphRenderer[Unknown]]`
- src/bokeh/models/renderers/contour_renderer.py:86:13: warning[possibly-missing-attribute] Attribute `data_source` may be missing on object of type `Unknown | Instance[Unknown]`
+ src/bokeh/models/renderers/contour_renderer.py:86:13: warning[possibly-missing-attribute] Attribute `data_source` may be missing on object of type `Unknown | Instance[GlyphRenderer[Unknown]]`
- src/bokeh/models/renderers/contour_renderer.py:88:13: warning[possibly-missing-attribute] Attribute `data_source` may be missing on object of type `Unknown | Instance[Unknown]`
+ src/bokeh/models/renderers/contour_renderer.py:88:13: warning[possibly-missing-attribute] Attribute `data_source` may be missing on object of type `Unknown | Instance[GlyphRenderer[Unknown]]`
- src/bokeh/models/renderers/contour_renderer.py:94:29: warning[possibly-missing-attribute] Attribute `data_source` may be missing on object of type `Unknown | Instance[Unknown]`
+ src/bokeh/models/renderers/contour_renderer.py:94:29: warning[possibly-missing-attribute] Attribute `data_source` may be missing on object of type `Unknown | Instance[GlyphRenderer[Unknown]]`
- src/bokeh/models/renderers/contour_renderer.py:98:13: warning[possibly-missing-attribute] Attribute `data_source` may be missing on object of type `Unknown | Instance[Unknown]`
+ src/bokeh/models/renderers/contour_renderer.py:98:13: warning[possibly-missing-attribute] Attribute `data_source` may be missing on object of type `Unknown | Instance[GlyphRenderer[Unknown]]`
- src/bokeh/models/renderers/contour_renderer.py:100:13: warning[possibly-missing-attribute] Attribute `data_source` may be missing on object of type `Unknown | Instance[Unknown]`
+ src/bokeh/models/renderers/contour_renderer.py:100:13: warning[possibly-missing-attribute] Attribute `data_source` may be missing on object of type `Unknown | Instance[GlyphRenderer[Unknown]]`
- src/bokeh/models/renderers/contour_renderer.py:112:13: error[invalid-argument-type] Argument is incorrect: Expected `GlyphRenderer[Glyph]`, found `Unknown | Instance[Unknown]`
+ src/bokeh/models/renderers/contour_renderer.py:112:13: error[invalid-argument-type] Argument is incorrect: Expected `GlyphRenderer[Glyph]`, found `Unknown | Instance[GlyphRenderer[Unknown]]`
- src/bokeh/models/renderers/contour_renderer.py:113:13: error[invalid-argument-type] Argument is incorrect: Expected `GlyphRenderer[Glyph]`, found `Unknown | Instance[Unknown]`
+ src/bokeh/models/renderers/contour_renderer.py:113:13: error[invalid-argument-type] Argument is incorrect: Expected `GlyphRenderer[Glyph]`, found `Unknown | Instance[GlyphRenderer[Unknown]]`
- src/bokeh/models/renderers/glyph_renderer.py:88:17: warning[possibly-missing-attribute] Attribute `properties_with_values` may be missing on object of type `Unknown | Required[Unknown]`
+ src/bokeh/models/renderers/glyph_renderer.py:88:17: warning[possibly-missing-attribute] Attribute `properties_with_values` may be missing on object of type `Unknown | Required[Glyph]`
- src/bokeh/models/renderers/glyph_renderer.py:89:17: warning[possibly-missing-attribute] Attribute `dataspecs` may be missing on object of type `Unknown | Required[Unknown]`
+ src/bokeh/models/renderers/glyph_renderer.py:89:17: warning[possibly-missing-attribute] Attribute `dataspecs` may be missing on object of type `Unknown | Required[Glyph]`
- src/bokeh/models/renderers/glyph_renderer.py:107:12: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[CDSView]]`
+ src/bokeh/models/renderers/glyph_renderer.py:107:12: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[CDSView | InstanceDefault[CDSView]]`
- src/bokeh/models/renderers/glyph_renderer.py:107:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[CDSView]] | (() -> type[InstanceDefault[CDSView]]) | str`, found `<class 'CDSView'>`
- src/bokeh/models/renderers/glyph_renderer.py:195:26: warning[possibly-missing-attribute] Attribute `fill_color` may be missing on object of type `(Unknown & FillGlyph) | (Required[Unknown] & FillGlyph)`
+ src/bokeh/models/renderers/glyph_renderer.py:195:26: warning[possibly-missing-attribute] Attribute `fill_color` may be missing on object of type `(Unknown & FillGlyph) | (Required[Glyph] & FillGlyph)`
- src/bokeh/models/renderers/glyph_renderer.py:201:26: warning[possibly-missing-attribute] Attribute `line_color` may be missing on object of type `(Unknown & LineGlyph & ~FillGlyph) | (Required[Unknown] & LineGlyph & ~FillGlyph)`
+ src/bokeh/models/renderers/glyph_renderer.py:201:26: warning[possibly-missing-attribute] Attribute `line_color` may be missing on object of type `(Unknown & LineGlyph & ~FillGlyph) | (Required[Glyph] & LineGlyph & ~FillGlyph)`
- src/bokeh/models/renderers/glyph_renderer.py:207:26: warning[possibly-missing-attribute] Attribute `text_color` may be missing on object of type `(Unknown & TextGlyph & ~FillGlyph & ~LineGlyph) | (Required[Unknown] & TextGlyph & ~FillGlyph & ~LineGlyph)`
+ src/bokeh/models/renderers/glyph_renderer.py:207:26: warning[possibly-missing-attribute] Attribute `text_color` may be missing on object of type `(Unknown & TextGlyph & ~FillGlyph & ~LineGlyph) | (Required[Glyph] & TextGlyph & ~FillGlyph & ~LineGlyph)`
- src/bokeh/models/renderers/graph_renderer.py:84:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[() -> Unknown]`
+ src/bokeh/models/renderers/graph_renderer.py:84:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[GlyphRenderer[Unknown] | (() -> Unknown)]`
- src/bokeh/models/renderers/graph_renderer.py:89:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[() -> Unknown]`
+ src/bokeh/models/renderers/graph_renderer.py:89:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[GlyphRenderer[Unknown] | (() -> Unknown)]`
- src/bokeh/models/renderers/graph_renderer.py:94:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[NodesOnly]]`
+ src/bokeh/models/renderers/graph_renderer.py:94:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[GraphHitTestPolicy | InstanceDefault[NodesOnly]]`
- src/bokeh/models/renderers/graph_renderer.py:94:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[NodesOnly]] | (() -> type[InstanceDefault[NodesOnly]]) | str`, found `<class 'GraphHitTestPolicy'>`
- src/bokeh/models/renderers/graph_renderer.py:99:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[NodesOnly]]`
+ src/bokeh/models/renderers/graph_renderer.py:99:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[GraphHitTestPolicy | InstanceDefault[NodesOnly]]`
- src/bokeh/models/renderers/graph_renderer.py:99:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[NodesOnly]] | (() -> type[InstanceDefault[NodesOnly]]) | str`, found `<class 'GraphHitTestPolicy'>`
- src/bokeh/models/renderers/tile_renderer.py:53:19: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[WMTSTileSource]]`
+ src/bokeh/models/renderers/tile_renderer.py:53:19: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[TileSource | InstanceDefault[WMTSTileSource]]`
- src/bokeh/models/renderers/tile_renderer.py:53:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[WMTSTileSource]] | (() -> type[InstanceDefault[WMTSTileSource]]) | str`, found `<class 'TileSource'>`
+ src/bokeh/models/sources.py:97:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Selection | InstanceDefault[Selection]]] | Property[Selection | InstanceDefault[Selection]]`, found `Instance[Selection]`
- src/bokeh/models/sources.py:123:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[UnionRenderers]]`
+ src/bokeh/models/sources.py:123:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[SelectionPolicy | InstanceDefault[UnionRenderers]]`
- src/bokeh/models/sources.py:123:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[UnionRenderers]] | (() -> type[InstanceDefault[UnionRenderers]]) | str`, found `<class 'SelectionPolicy'>`
- src/bokeh/models/sources.py:788:14: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[AllIndices]]`
+ src/bokeh/models/sources.py:788:14: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Filter | InstanceDefault[AllIndices]]`
- src/bokeh/models/sources.py:788:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[AllIndices]] | (() -> type[InstanceDefault[AllIndices]]) | str`, found `<class 'Filter'>`
- src/bokeh/models/tools.py:573:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[() -> Unknown]`
+ src/bokeh/models/tools.py:573:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[BoxAnnotation | (() -> Unknown)]`
- src/bokeh/models/tools.py:1138:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[BoxAnnotation]]`
+ src/bokeh/models/tools.py:1138:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[BoxAnnotation | InstanceDefault[BoxAnnotation]]`
- src/bokeh/models/tools.py:1138:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[BoxAnnotation]] | (() -> type[InstanceDefault[BoxAnnotation]]) | str`, found `<class 'BoxAnnotation'>`
- src/bokeh/models/tools.py:1257:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[BoxAnnotation]]`
+ src/bokeh/models/tools.py:1257:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[BoxAnnotation | InstanceDefault[BoxAnnotation]]`
- src/bokeh/models/tools.py:1257:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[BoxAnnotation]] | (() -> type[InstanceDefault[BoxAnnotation]]) | str`, found `<class 'BoxAnnotation'>`
- src/bokeh/models/tools.py:1308:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[PolyAnnotation]]`
+ src/bokeh/models/tools.py:1308:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[PolyAnnotation | InstanceDefault[PolyAnnotation]]`
- src/bokeh/models/tools.py:1308:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[PolyAnnotation]] | (() -> type[InstanceDefault[PolyAnnotation]]) | str`, found `<class 'PolyAnnotation'>`
- src/bokeh/models/tools.py:1342:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[PolyAnnotation]]`
+ src/bokeh/models/tools.py:1342:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[PolyAnnotation | InstanceDefault[PolyAnnotation]]`
- src/bokeh/models/tools.py:1342:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[PolyAnnotation]] | (() -> type[InstanceDefault[PolyAnnotation]]) | str`, found `<class 'PolyAnnotation'>`
- src/bokeh/models/widgets/tables.py:713:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[StringFormatter]]`
+ src/bokeh/models/widgets/tables.py:713:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[CellFormatter | InstanceDefault[StringFormatter]]`
- src/bokeh/models/widgets/tables.py:713:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[StringFormatter]] | (() -> type[InstanceDefault[StringFormatter]]) | str`, found `<class 'CellFormatter'>`
- src/bokeh/models/widgets/tables.py:718:14: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[StringEditor]]`
+ src/bokeh/models/widgets/tables.py:718:14: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[CellEditor | InstanceDefault[StringEditor]]`
- src/bokeh/models/widgets/tables.py:718:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[StringEditor]] | (() -> type[InstanceDefault[StringEditor]]) | str`, found `<class 'CellEditor'>`
- src/bokeh/models/widgets/tables.py:749:14: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[ColumnDataSource]]`
+ src/bokeh/models/widgets/tables.py:749:14: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[DataSource | InstanceDefault[ColumnDataSource]]`
- src/bokeh/models/widgets/tables.py:749:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[ColumnDataSource]] | (() -> type[InstanceDefault[ColumnDataSource]]) | str`, found `<class 'DataSource'>`
- src/bokeh/models/widgets/tables.py:753:12: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[InstanceDefault[CDSView]]`
+ src/bokeh/models/widgets/tables.py:753:12: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[CDSView | InstanceDefault[CDSView]]`
- src/bokeh/models/widgets/tables.py:753:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[InstanceDefault[CDSView]] | (() -> type[InstanceDefault[CDSView]]) | str`, found `<class 'CDSView'>`
- src/bokeh/plotting/_figure.py:198:48: error[invalid-argument-type] Argument to function `get_scale` is incorrect: Expected `Literal["linear", "log", "datetime", "timedelta", "mercator", "auto"] | None`, found `Unknown | ParameterizedProperty[Unknown | None]`
+ src/bokeh/plotting/_figure.py:198:48: error[invalid-argument-type] Argument to function `get_scale` is incorrect: Expected `Literal["linear", "log", "datetime", "timedelta", "mercator", "auto"] | None`, found `Unknown | ParameterizedProperty[Any | None]`
- src/bokeh/plotting/_figure.py:199:48: error[invalid-argument-type] Argument to function `get_scale` is incorrect: Expected `Literal["linear", "log", "datetime", "timedelta", "mercator", "auto"] | None`, found `Unknown | ParameterizedProperty[Unknown | None]`
+ src/bokeh/plotting/_figure.py:199:48: error[invalid-argument-type] Argument to function `get_scale` is incorrect: Expected `Literal["linear", "log", "datetime", "timedelta", "mercator", "auto"] | None`, found `Unknown | ParameterizedProperty[Any | None]`
- src/bokeh/plotting/_figure.py:201:37: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `Literal["linear", "log", "datetime", "timedelta", "mercator", "auto"] | None`, found `Unknown | ParameterizedProperty[Unknown | None]`
+ src/bokeh/plotting/_figure.py:201:37: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `Literal["linear", "log", "datetime", "timedelta", "mercator", "auto"] | None`, found `Unknown | ParameterizedProperty[Any | None]`
- src/bokeh/plotting/_figure.py:201:55: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `Literal["above", "below", "left", "right"] | None`, found `Unknown | Nullable[str]`
+ src/bokeh/plotting/_figure.py:201:55: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `Literal["above", "below", "left", "right"] | None`, found `Unknown | Nullable[Any | Literal["below"]]`
- src/bokeh/plotting/_figure.py:202:37: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `Literal["linear", "log", "datetime", "timedelta", "mercator", "auto"] | None`, found `Unknown | ParameterizedProperty[Unknown | None]`
+ src/bokeh/plotting/_figure.py:202:37: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `Literal["linear", "log", "datetime", "timedelta", "mercator", "auto"] | None`, found `Unknown | ParameterizedProperty[Any | None]`
- src/bokeh/plotting/_figure.py:202:55: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `Literal["above", "below", "left", "right"] | None`, found `Unknown | Nullable[str]`
+ src/bokeh/plotting/_figure.py:202:55: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `Literal["above", "below", "left", "right"] | None`, found `Unknown | Nullable[Any | Literal["left"]]`
- src/bokeh/plotting/_figure.py:204:67: error[invalid-argument-type] Argument to function `process_tools_arg` is incorrect: Expected `str | list[tuple[str, str]] | None`, found `Unknown | Nullable[Unknown]`
+ src/bokeh/plotting/_figure.py:204:67: error[invalid-argument-type] Argument to function `process_tools_arg` is incorrect: Expected `str | list[tuple[str, str]] | None`, found `Unknown | Nullable[Any]`
- src/bokeh/plotting/_figure.py:209:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `Drag | str | None`, found `Unknown | Nullable[str]`
+ src/bokeh/plotting/_figure.py:209:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `Drag | str | None`, found `Unknown | Nullable[Any | Literal["auto"]]`
- src/bokeh/plotting/_figure.py:210:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `list[InspectTool] | InspectTool | str | None`, found `Unknown | Nullable[str]`
+ src/bokeh/plotting/_figure.py:210:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `list[InspectTool] | InspectTool | str | None`, found `Unknown | Nullable[Any | Literal["auto"]]`
- src/bokeh/plotting/_figure.py:211:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `Scroll | str | None`, found `Unknown | Nullable[str]`
+ src/bokeh/plotting/_figure.py:211:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `Scroll | str | None`, found `Unknown | Nullable[Any | Literal["auto"]]`
- src/bokeh/plotting/_figure.py:212:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `Tap | str | None`, found `Unknown | Nullable[str]`
+ src/bokeh/plotting/_figure.py:212:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `Tap | str | None`, found `Unknown | Nullable[Any | Literal["auto"]]`
- src/bokeh/plotting/_figure.py:213:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `GestureTool | str | None`, found `Unknown | Nullable[str]`
+ src/bokeh/plotting/_figure.py:213:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `GestureTool | str | None`, found `Unknown | Nullable[Any | Literal["auto"]]`
- Found 860 diagnostics
+ Found 834 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/series/test_series.py:3699:9: error[type-assertion-failure] Type `Series[str]` does not match asserted type `Series[Unknown]`
+ tests/series/test_series.py:3699:9: error[type-assertion-failure] Type `Series[str]` does not match asserted type `Series[Unknown | str]`
- tests/series/test_series.py:3715:9: error[type-assertion-failure] Type `Series[str]` does not match asserted type `Series[Unknown]`
+ tests/series/test_series.py:3715:9: error[type-assertion-failure] Type `Series[str]` does not match asserted type `Series[Any]`
- tests/series/test_series.py:3729:11: error[type-assertion-failure] Type `Series[str]` does not match asserted type `Series[Unknown]`
+ tests/series/test_series.py:3729:11: error[type-assertion-failure] Type `Series[str]` does not match asserted type `Unknown`
- tests/series/test_series.py:3741:11: error[type-assertion-failure] Type `Series[str]` does not match asserted type `Series[Unknown]`
+ tests/series/test_series.py:3741:11: error[type-assertion-failure] Type `Series[str]` does not match asserted type `Series[Any]`
- Found 5544 diagnostics
+ Found 5545 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/alexa/state_report.py:341:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventStateChangedData]`
- homeassistant/components/alexa/state_report.py:342:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `def _async_entity_state_listener(event_: Event[EventStateChangedData]) -> CoroutineType[Any, Any, None]`
- homeassistant/components/alexa/state_report.py:343:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `((Mapping[str, Any], /) -> bool) | None`, found `def _async_entity_state_filter(data: EventStateChangedData) -> bool`
- homeassistant/components/apache_kafka/__init__.py:133:37: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventStateChangedData]`
- homeassistant/components/apache_kafka/__init__.py:133:58: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `bound method Self@start.write(event: Event[EventStateChangedData]) -> CoroutineType[Any, Any, None]`
- homeassistant/components/cloud/alexa_config.py:272:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[_EventEntityRegistryUpdatedData_CreateRemove | _EventEntityRegistryUpdatedData_Update]`
- homeassistant/components/cloud/alexa_config.py:273:17: error[invalid-argu

... (truncated 94 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~49MB
+     struct metadata = ~52MB
-     memo metadata = ~167MB
+     memo metadata = ~176MB


```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-09 11:01_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 7 | 128 | 57 |
| `unresolved-attribute` | 0 | 21 | 1 |
| `possibly-missing-attribute` | 0 | 0 | 14 |
| `not-iterable` | 0 | 0 | 10 |
| `type-assertion-failure` | 1 | 4 | 4 |
| `invalid-assignment` | 2 | 0 | 3 |
| `invalid-return-type` | 1 | 0 | 1 |
| `unsupported-operator` | 0 | 0 | 2 |
| `invalid-context-manager` | 0 | 0 | 1 |
| `non-subscriptable` | 0 | 0 | 1 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **12** | **153** | **94** |

**[Full report with detailed diff](https://david-fix-1772.ecosystem-663.pages.dev/diff)** ([timing results](https://david-fix-1772.ecosystem-663.pages.dev/timing))




---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:318 on 2025-12-09 14:11_

nit: I can't remember who I saw do this first, but I like adding a comment to indicate that this field is present to force the typevar to be invariant:


```suggestion
    x: T  # invariant
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:321 on 2025-12-09 14:11_

ditto here 

```suggestion
    x: T  # invariant
```

---

_@dcreager reviewed on 2025-12-09 14:34_

This approach looks good to me. That match arm is a big pile of heuristics already, so if there are corner cases that this doesn't handle, I think it's fine to be generous about marking things as TODO. This seems like a clear win as-is in how it handles the `P[T] | Q[T]` case.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-12-09 14:55_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-09 14:55_

---

_Renamed from " [ty] Infer type parameters from generic unions " to " [ty] Infer type variables within generic unions " by @sharkdp on 2025-12-09 14:56_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-12-09 15:01_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-09 15:01_

---

_Marked ready for review by @sharkdp on 2025-12-09 15:04_

---

_Review requested from @carljm by @sharkdp on 2025-12-09 15:04_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-09 15:04_

---

_@sharkdp reviewed on 2025-12-09 15:08_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:462 on 2025-12-09 15:08_

Oh, haha. I guess even the most basic tasks (translating my pep695 tests to legacy tests) shouldn't be delegated to an LLM without a careful review

---

_@dcreager approved on 2025-12-09 15:10_

Brilliant!

---

_Merged by @sharkdp on 2025-12-09 15:23_

---

_Closed by @sharkdp on 2025-12-09 15:23_

---

_Branch deleted on 2025-12-09 15:23_

---
