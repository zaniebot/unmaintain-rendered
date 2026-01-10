```yaml
number: 20692
title: "[ty] Don't consider bound `self` parameter when comparing bound methods"
type: pull_request
state: closed
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: dcreager/bound-method-assignability
created_at: 2025-10-03T14:19:58Z
updated_at: 2025-10-03T15:15:36Z
url: https://github.com/astral-sh/ruff/pull/20692
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Don't consider bound `self` parameter when comparing bound methods

---

_Pull request opened by @dcreager on 2025-10-03 14:19_

In a bound method, the bound `self` parameter is no longer available for callers to provide an argument for. (That's the whole point of binding it!) That means we should not consider it when checking subtyping/assignability of bound methods.

We were already doing this correctly when comparing a bound method to other callable types, since we would coerce the bound method into its equivalent `Callable` type before performing the check, and our conversion into a `Callable` type correctly removes the bound `self` parameter from the signature.

But we weren't doing the same when comparing a bound method to another bound method — instead we were comparing the _unbound_ function signatures, and also comparing the bound `self` instance types. We can get the correct behavior (and eliminate having to duplicate logic) by coercing both bound methods into `Callable` types for these checks, too.

Closes https://github.com/astral-sh/ty/issues/1301

---

_Review requested from @carljm by @dcreager on 2025-10-03 14:19_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-03 14:19_

---

_Review requested from @sharkdp by @dcreager on 2025-10-03 14:19_

---

_Comment by @github-actions[bot] on 2025-10-03 14:22_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `ty` added by @AlexWaygood on 2025-10-03 14:23_

---

_Comment by @github-actions[bot] on 2025-10-03 14:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/skipping.py:292:25: error[invalid-argument-type] Argument to bound method `matches` is incorrect: Argument type `object` does not satisfy upper bound `BaseException` of type variable `BaseExcT_1`
- Found 489 diagnostics
+ Found 490 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/porcelain.py:3177:41: error[invalid-argument-type] Argument to function `_make_branch_ref` is incorrect: Expected `str | bytes`, found `Unknown | (Sequence[str | bytes] & ~Top[list[Unknown]] & ~tuple[object, ...]) | bytes`
- dulwich/worktree.py:923:17: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method DiskRefsContainer.__setitem__(name: bytes, ref: bytes) -> None) | (bound method ReftableRefsContainer.__setitem__(name: bytes, ref: bytes) -> None)` cannot be called with a key of type `str | bytes` and a value of type `bytes` on object of type `Unknown | DiskRefsContainer | ReftableRefsContainer`
+ dulwich/worktree.py:923:17: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method DiskRefsContainer.__setitem__(name: bytes, ref: bytes) -> None)` cannot be called with a key of type `str | bytes` and a value of type `bytes` on object of type `Unknown | DiskRefsContainer | ReftableRefsContainer`
- dulwich/worktree.py:948:48: error[invalid-argument-type] Argument to bound method `set_symbolic_ref` is incorrect: Expected `bytes`, found `str | bytes`
- Found 195 diagnostics
+ Found 193 diagnostics

artigraph (https://github.com/artigraph/artigraph)
+ src/arti/internal/type_hints.py:172:70: error[not-iterable] Object of type `T@lenient_issubclass & tuple[object, ...]` is not iterable
- Found 141 diagnostics
+ Found 142 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ psycopg_pool/psycopg_pool/_acompat.py:185:12: error[invalid-return-type] Return type does not match returned value: expected `T@ensure_async`, found `object`
- Found 685 diagnostics
+ Found 686 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ discord/ui/section.py:189:9: error[invalid-argument-type] Argument to bound method `_update_view` is incorrect: Argument type `Item[object]` does not satisfy upper bound `Item[V@Item]` of type variable `Self`
- Found 514 diagnostics
+ Found 515 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataset.py:4616:17: error[invalid-assignment] Method `__setitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Unknown` and a value of type `int` on object of type `Mapping[Hashable, Any] & Top[MutableMapping[Unknown, Unknown]]`
+ xarray/core/dataset.py:4616:17: error[invalid-assignment] Method `__setitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Hashable` and a value of type `int` on object of type `Mapping[Hashable, Any] & Top[MutableMapping[Unknown, Unknown]]`

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/generic.py:236:21: error[not-iterable] Object of type `ListOrTupleOrSetAny@UniqueItems` may not be iterable
+ koda_validate/generic.py:236:21: error[not-iterable] Object of type `ListOrTupleOrSetAny@UniqueItems` is not iterable

trio (https://github.com/python-trio/trio)
- src/trio/_core/_tests/test_ki.py:690:25: error[invalid-argument-type] Argument to bound method `send` is incorrect: Expected `Never`, found `None`
- Found 656 diagnostics
+ Found 655 diagnostics

apprise (https://github.com/caronc/apprise)
- apprise/plugins/email/base.py:1129:17: error[invalid-assignment] Method `__setitem__` of type `(bound method MIMEMultipart.__setitem__(name: str, val: str) -> None) | (bound method MIMEText.__setitem__(name: str, val: str) -> None)` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEMultipart | MIMEText`
+ apprise/plugins/email/base.py:1129:17: error[invalid-assignment] Method `__setitem__` of type `bound method MIMEMultipart.__setitem__(name: str, val: str) -> None` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEMultipart | MIMEText`
- apprise/plugins/email/base.py:1165:17: error[invalid-assignment] Method `__setitem__` of type `(bound method MIMEMultipart.__setitem__(name: str, val: str) -> None) | (bound method MIMEText.__setitem__(name: str, val: str) -> None)` cannot be called with a key of type `Unknown` and a value of type `Header` on object of type `MIMEMultipart | MIMEText`
- apprise/plugins/email/base.py:1167:13: error[invalid-assignment] Method `__setitem__` of type `(bound method MIMEMultipart.__setitem__(name: str, val: str) -> None) | (bound method MIMEText.__setitem__(name: str, val: str) -> None)` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEMultipart | MIMEText`
+ apprise/plugins/email/base.py:1165:17: error[invalid-assignment] Method `__setitem__` of type `bound method MIMEMultipart.__setitem__(name: str, val: str) -> None` cannot be called with a key of type `Unknown` and a value of type `Header` on object of type `MIMEMultipart | MIMEText`
+ apprise/plugins/email/base.py:1167:13: error[invalid-assignment] Method `__setitem__` of type `bound method MIMEMultipart.__setitem__(name: str, val: str) -> None` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEMultipart | MIMEText`
- apprise/plugins/ses.py:470:13: error[invalid-assignment] Method `__setitem__` of type `(bound method MIMEMultipart.__setitem__(name: str, val: str) -> None) | (bound method MIMEText.__setitem__(name: str, val: str) -> None)` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEMultipart | MIMEText`
+ apprise/plugins/ses.py:470:13: error[invalid-assignment] Method `__setitem__` of type `bound method MIMEMultipart.__setitem__(name: str, val: str) -> None` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEMultipart | MIMEText`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/structured_configs/_utils.py:257:22: error[invalid-key] Cannot access `AllConvert` with a key of type `str`. Only string literals are allowed as keys on TypedDicts.
- Found 557 diagnostics
+ Found 558 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ cwltool/cwlprov/ro.py:664:25: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None) | (@Todo & (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None)) | (Overload[(index: int, value: MutableMapping[str, @Todo | None]) -> None, (index: slice[Any, Any, Any], value: Iterable[MutableMapping[str, @Todo | None]]) -> None])` cannot be called with a key of type `Literal["checksum"]` and a value of type `str` on object of type `MutableMapping[str, @Todo | None] | (@Todo & Top[MutableMapping[Unknown, Unknown]]) | (MutableSequence[MutableMapping[str, @Todo | None]] & Top[MutableMapping[Unknown, Unknown]])`
+ cwltool/cwlprov/ro.py:666:17: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None) | (@Todo & (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None)) | (Overload[(index: int, value: MutableMapping[str, @Todo | None]) -> None, (index: slice[Any, Any, Any], value: Iterable[MutableMapping[str, @Todo | None]]) -> None])` cannot be called with a key of type `Literal["location"]` and a value of type `str` on object of type `MutableMapping[str, @Todo | None] | (@Todo & Top[MutableMapping[Unknown, Unknown]]) | (MutableSequence[MutableMapping[str, @Todo | None]] & Top[MutableMapping[Unknown, Unknown]])`
- cwltool/load_tool.py:176:33: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None) | (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None)` cannot be called with a key of type `Unknown | str` and a value of type `str` on object of type `MutableMapping[str, @Todo | None] | (MutableSequence[MutableMapping[str, @Todo | None] | str | int] & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]])`
+ cwltool/load_tool.py:176:33: error[invalid-assignment] Method `__setitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Unknown | str` and a value of type `str` on object of type `MutableMapping[str, @Todo | None] | (MutableSequence[MutableMapping[str, @Todo | None] | str | int] & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]])`
- cwltool/load_tool.py:190:25: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None) | (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None)` cannot be called with a key of type `Literal["stdin"]` and a value of type `str` on object of type `MutableMapping[str, @Todo | None] | (MutableSequence[MutableMapping[str, @Todo | None] | str | int] & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]])`
+ cwltool/load_tool.py:190:25: error[invalid-assignment] Method `__setitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Literal["stdin"]` and a value of type `str` on object of type `MutableMapping[str, @Todo | None] | (MutableSequence[MutableMapping[str, @Todo | None] | str | int] & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]])`
- cwltool/main.py:282:21: error[invalid-assignment] Method `__setitem__` of type `(bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None) | (bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None)` cannot be called with a key of type `Literal["type"]` and a value of type `@Todo` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
- cwltool/main.py:293:17: error[invalid-assignment] Method `__setitem__` of type `(bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None) | (bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None)` cannot be called with a key of type `Literal["type"]` and a value of type `@Todo` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
- cwltool/main.py:301:17: error[invalid-assignment] Method `__setitem__` of type `(bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None) | (bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None)` cannot be called with a key of type `Literal["type"]` and a value of type `@Todo` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
- cwltool/main.py:307:17: error[invalid-assignment] Method `__setitem__` of type `(bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None) | (bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None)` cannot be called with a key of type `Literal["items"]` and a value of type `@Todo` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
- cwltool/main.py:315:17: error[invalid-assignment] Method `__setitem__` of type `(bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None) | (bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None)` cannot be called with a key of type `Literal["fields"]` and a value of type `@Todo` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
+ cwltool/main.py:282:21: error[invalid-assignment] Method `__setitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Literal["type"]` and a value of type `@Todo` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
+ cwltool/main.py:293:17: error[invalid-assignment] Method `__setitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Literal["type"]` and a value of type `@Todo` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
+ cwltool/main.py:301:17: error[invalid-assignment] Method `__setitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Literal["type"]` and a value of type `@Todo` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
+ cwltool/main.py:307:17: error[invalid-assignment] Method `__setitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Literal["items"]` and a value of type `@Todo` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
+ cwltool/main.py:315:17: error[invalid-assignment] Method `__setitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Literal["fields"]` and a value of type `@Todo` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
+ cwltool/process.py:318:47: error[invalid-argument-type] Argument to function `_collectDirEntries` is incorrect: Expected `MutableMapping[str, @Todo | None] | MutableSequence[MutableMapping[str, @Todo | None]] | None`, found `str`
+ cwltool/utils.py:177:13: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, Any].__setitem__(key: str, value: Any, /) -> None) | (Overload[(index: int, value: Any) -> None, (index: slice[Any, Any, Any], value: Iterable[Any]) -> None]) | (Any & (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None))` cannot be called with a key of type `str` and a value of type `str | MutableSequence[Any] | MutableMapping[str, Any]` on object of type `MutableMapping[str, Any] | (MutableSequence[Any] & Top[MutableMapping[Unknown, Unknown]]) | (Any & Top[MutableMapping[Unknown, Unknown]])`
- Found 162 diagnostics
+ Found 166 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/metadata/__init__.py:79:78: error[not-iterable] Object of type `T@_process_dynamic_metadata & Top[list[Unknown]]` is not iterable
+ src/scikit_build_core/metadata/__init__.py:85:41: error[invalid-argument-type] Argument to bound method `values` is incorrect: Argument type `T@_process_dynamic_metadata` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ src/scikit_build_core/metadata/__init__.py:85:41: error[invalid-argument-type] Argument to bound method `values` is incorrect: Argument type `T@_process_dynamic_metadata` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ src/scikit_build_core/metadata/__init__.py:85:41: error[invalid-argument-type] Argument to bound method `values` is incorrect: Argument type `T@_process_dynamic_metadata` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ src/scikit_build_core/metadata/__init__.py:93:22: error[not-iterable] Object of type `T@_process_dynamic_metadata & Top[list[Unknown]]` is not iterable
+ src/scikit_build_core/metadata/__init__.py:103:22: error[invalid-argument-type] Argument to bound method `values` is incorrect: Argument type `T@_process_dynamic_metadata` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ src/scikit_build_core/metadata/__init__.py:103:22: error[invalid-argument-type] Argument to bound method `values` is incorrect: Argument type `T@_process_dynamic_metadata` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ src/scikit_build_core/metadata/__init__.py:103:22: error[invalid-argument-type] Argument to bound method `values` is incorrect: Argument type `T@_process_dynamic_metadata` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ src/scikit_build_core/metadata/__init__.py:108:51: error[unresolved-attribute] Type `str` has no attribute `items`
+ src/scikit_build_core/metadata/__init__.py:109:27: error[invalid-argument-type] Argument to bound method `items` is incorrect: Argument type `T@_process_dynamic_metadata` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ src/scikit_build_core/metadata/__init__.py:109:27: error[invalid-argument-type] Argument to bound method `items` is incorrect: Argument type `T@_process_dynamic_metadata` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ src/scikit_build_core/metadata/__init__.py:109:27: error[invalid-argument-type] Argument to bound method `items` is incorrect: Argument type `T@_process_dynamic_metadata` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ src/scikit_build_core/metadata/__init__.py:113:42: error[invalid-argument-type] Argument to bound method `values` is incorrect: Argument type `T@_process_dynamic_metadata` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ src/scikit_build_core/metadata/__init__.py:113:42: error[invalid-argument-type] Argument to bound method `values` is incorrect: Argument type `T@_process_dynamic_metadata` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ src/scikit_build_core/metadata/__init__.py:113:42: error[invalid-argument-type] Argument to bound method `values` is incorrect: Argument type `T@_process_dynamic_metadata` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
- Found 53 diagnostics
+ Found 68 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/numpy/tensor_contractions.py:548:46: error[invalid-argument-type] Argument to function `canonicalize_axis` is incorrect: Expected `SupportsIndex`, found `int | Sequence[int]`
+ jax/_src/numpy/tensor_contractions.py:549:46: error[invalid-argument-type] Argument to function `canonicalize_axis` is incorrect: Expected `SupportsIndex`, found `int | Sequence[int]`
- Found 2266 diagnostics
+ Found 2268 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `(bound method type[Index[Any]].from_labels(labels: Iterable[@Todo], *, /, name: @Todo = None) -> Index[Any]) | (bound method <class 'Index'>.from_labels(labels: Iterable[@Todo], *, /, name: @Todo = None) -> Index[Any])`
+ static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `bound method type[Index[Any]].from_labels(labels: Iterable[@Todo], *, /, name: @Todo = None) -> Index[Any]`

sympy (https://github.com/sympy/sympy)
- sympy/polys/polyclasses.py:1289:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 14007 diagnostics
+ Found 14006 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/auth/permissions/merge.py:63:13: error[invalid-assignment] Method `__setitem__` of type `bound method Top[dict[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Never` and a value of type `Mapping[str, Mapping[str, Mapping[str, bool] | bool | None] | bool | None] | bool | None` on object of type `Mapping[str, SubCategoryType] & Top[dict[Unknown, Unknown]]`
+ homeassistant/auth/permissions/merge.py:63:13: error[invalid-assignment] Method `__setitem__` of type `bound method Top[dict[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `str` and a value of type `Mapping[str, Mapping[str, Mapping[str, bool] | bool | None] | bool | None] | bool | None` on object of type `Mapping[str, SubCategoryType] & Top[dict[Unknown, Unknown]]`
- homeassistant/components/telegram_bot/bot.py:411:50: warning[possibly-missing-attribute] Attribute `split` on type `@Todo | (Any & ~Top[list[Unknown]]) | None` may be missing
+ homeassistant/components/telegram_bot/bot.py:411:50: warning[possibly-missing-attribute] Attribute `split` on type `Unknown | (Any & ~Top[list[Unknown]]) | None` may be missing
- homeassistant/components/usage_prediction/__init__.py:64:16: error[invalid-return-type] Return type does not match returned value: expected `EntityUsagePredictions`, found `object`
+ homeassistant/components/xbox/media_source.py:54:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-03 14:27_

---

_Comment by @codspeed-hq[bot] on 2025-10-03 14:32_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fbound-method-assignability?runnerMode=WallTime)

### Merging #20692 will **improve performances by 10.87%**

<sub>Comparing <code>dcreager/bound-method-assignability</code> (4060082) with <code>main</code> (f73ead1)</sub>



### Summary

`⚡ 1` improvement  
`✅ 7` untouched  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` medium[colour-science] `` | 10.7 s | 9.6 s | +10.87% |


---

_Comment by @github-actions[bot] on 2025-10-03 14:32_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 17 | 1 | 1 |
| `invalid-assignment` | 3 | 0 | 14 |
| `not-iterable` | 3 | 0 | 1 |
| `invalid-return-type` | 1 | 1 | 0 |
| `unused-ignore-comment` | 1 | 1 | 0 |
| `invalid-key` | 1 | 0 | 0 |
| `no-matching-overload` | 0 | 1 | 0 |
| `possibly-missing-attribute` | 0 | 0 | 1 |
| `unresolved-attribute` | 1 | 0 | 0 |
| **Total** | **27** | **4** | **17** |

**[Full report with detailed diff](https://dcreager-bound-method-assign.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-bound-method-assign.ecosystem-663.pages.dev/timing))


---

_Comment by @codspeed-hq[bot] on 2025-10-03 14:33_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fbound-method-assignability?runnerMode=Instrumentation)

### Merging #20692 will **improve performances by 8.68%**

<sub>Comparing <code>dcreager/bound-method-assignability</code> (4060082) with <code>main</code> (f73ead1)</sub>



### Summary

`⚡ 2` improvements  
`✅ 11` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` attrs `` | 431.9 ms | 399.8 ms | +8.03% |
| ⚡ | `` hydra-zen `` | 851.9 ms | 783.9 ms | +8.68% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fbound-method-assignability?runnerMode=Instrumentation&sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Comment by @dcreager on 2025-10-03 15:15_

@AlexWaygood convinced me synchronously that this is wrong, and so is https://github.com/astral-sh/ty/issues/1301! The bound methods of two different classes really are different sets of objects, and are correctly not subtypes of each other. It was a different issue that I was encountering on #20677.

---

_Closed by @dcreager on 2025-10-03 15:15_

---
