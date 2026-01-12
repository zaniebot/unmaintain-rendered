```yaml
number: 22088
title: "[ty] Avoid including `property` in subclasses properties"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: charlie/property-fix
created_at: 2025-12-19T16:24:28Z
updated_at: 2025-12-30T03:28:05Z
url: https://github.com/astral-sh/ruff/pull/22088
synced_at: 2026-01-12T15:57:41Z
```

# [ty] Avoid including `property` in subclasses properties

---

_@charliermarsh_

## Summary

As-is, the following rejects `return self.value` in `def other` in the subclass ([link](https://play.ty.dev/f55b47b2-313e-45d1-ba45-fde410bed32e)) because `self.value` is resolving to `Unknown | int | float | property`:

```python
class Base:
    _value: float = 0.0

    @property
    def value(self) -> float:
        return self._value

    @value.setter
    def value(self, v: float) -> None:
        self._value = v

    @property
    def other(self) -> float:
        return self.value

    @other.setter
    def other(self, v: float) -> None:
        self.value = v

class Derived(Base):
    @property
    def other(self) -> float:
        return self.value

    @other.setter
    def other(self, v: float) -> None:
        reveal_type(self.value)  # revealed: int | float
        self.value = v
```

I believe the root cause is that we're not excluding properties when searching for class methods, so we're treating the `other` setter as a classmethod. I don't fully understand how that ends up materializing as `| property` on the union though.


---

_Comment by @astral-sh-bot[bot] on 2025-12-19 16:26_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 16:27_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
+ bidict/_base.py:200:9: error[invalid-assignment] Object of type `ReferenceType[BidictBase[VT@BidictBase, KT@BidictBase] | Self@inverse]` is not assignable to attribute `_invweak` of type `ReferenceType[BidictBase[VT@BidictBase, KT@BidictBase]] | None`
- Found 14 diagnostics
+ Found 15 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/spec.py:1587:48: error[invalid-argument-type] Argument to function `path_to_os_path` is incorrect: Expected `str`, found `Unknown | None`
- Found 4292 diagnostics
+ Found 4293 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/auth.py:201:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, str | None]`, found `Unknown | CallbackDict[str, str] | dict[str, str | None]`
- Found 386 diagnostics
+ Found 385 diagnostics

python-htmlgen (https://github.com/srittau/python-htmlgen)
+ htmlgen/form.py:206:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `value` on type `Self@date` with custom `__set__` method
+ htmlgen/form.py:356:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `value` on type `Self@__init__` with custom `__set__` method
+ htmlgen/form.py:364:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `value` on type `Self@label` with custom `__set__` method
- Found 22 diagnostics
+ Found 25 diagnostics

rich (https://github.com/Textualize/rich)
- tests/test_text.py:247:9: warning[possibly-missing-attribute] Attribute `link` may be missing on object of type `Unknown | str | Style`
+ tests/test_text.py:247:9: warning[possibly-missing-attribute] Attribute `link` may be missing on object of type `str | Style`
- tests/test_text.py:261:9: warning[possibly-missing-attribute] Attribute `link` may be missing on object of type `Unknown | str | Style`
+ tests/test_text.py:261:9: warning[possibly-missing-attribute] Attribute `link` may be missing on object of type `str | Style`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/cursor.py:176:54: error[invalid-argument-type] Argument to bound method `load_row` is incorrect: Expected `RowMaker[tuple[Any, ...]]`, found `Unknown | RowMaker[Row@Cursor]`
- psycopg/psycopg/cursor.py:250:51: error[invalid-argument-type] Argument to bound method `load_row` is incorrect: Expected `RowMaker[tuple[Any, ...]]`, found `Unknown | RowMaker[Row@Cursor]`
- psycopg/psycopg/cursor.py:252:20: error[invalid-return-type] Return type does not match returned value: expected `Row@Cursor | None`, found `tuple[Any, ...]`
- psycopg/psycopg/cursor.py:269:60: error[invalid-argument-type] Argument to bound method `load_rows` is incorrect: Expected `RowMaker[tuple[Any, ...]]`, found `Unknown | RowMaker[Row@Cursor]`
- psycopg/psycopg/cursor.py:272:16: error[invalid-return-type] Return type does not match returned value: expected `list[Row@Cursor]`, found `list[tuple[Any, ...]]`
- psycopg/psycopg/cursor.py:282:62: error[invalid-argument-type] Argument to bound method `load_rows` is incorrect: Expected `RowMaker[tuple[Any, ...]]`, found `Unknown | RowMaker[Row@Cursor]`
- psycopg/psycopg/cursor.py:284:16: error[invalid-return-type] Return type does not match returned value: expected `list[Row@Cursor]`, found `list[tuple[Any, ...]]`
- psycopg/psycopg/cursor.py:293:51: error[invalid-argument-type] Argument to bound method `load_row` is incorrect: Expected `RowMaker[tuple[Any, ...]]`, found `Unknown | RowMaker[Row@Cursor]`
- psycopg/psycopg/cursor.py:295:20: error[invalid-return-type] Return type does not match returned value: expected `Row@Cursor`, found `tuple[Any, ...]`
- psycopg/psycopg/cursor_async.py:176:54: error[invalid-argument-type] Argument to bound method `load_row` is incorrect: Expected `RowMaker[tuple[Any, ...]]`, found `Unknown | RowMaker[Row@AsyncCursor]`
- psycopg/psycopg/cursor_async.py:252:51: error[invalid-argument-type] Argument to bound method `load_row` is incorrect: Expected `RowMaker[tuple[Any, ...]]`, found `Unknown | RowMaker[Row@AsyncCursor]`
- psycopg/psycopg/cursor_async.py:254:20: error[invalid-return-type] Return type does not match returned value: expected `Row@AsyncCursor | None`, found `tuple[Any, ...]`
- psycopg/psycopg/cursor_async.py:271:60: error[invalid-argument-type] Argument to bound method `load_rows` is incorrect: Expected `RowMaker[tuple[Any, ...]]`, found `Unknown | RowMaker[Row@AsyncCursor]`
- psycopg/psycopg/cursor_async.py:274:16: error[invalid-return-type] Return type does not match returned value: expected `list[Row@AsyncCursor]`, found `list[tuple[Any, ...]]`
- psycopg/psycopg/cursor_async.py:284:62: error[invalid-argument-type] Argument to bound method `load_rows` is incorrect: Expected `RowMaker[tuple[Any, ...]]`, found `Unknown | RowMaker[Row@AsyncCursor]`
- psycopg/psycopg/cursor_async.py:286:16: error[invalid-return-type] Return type does not match returned value: expected `list[Row@AsyncCursor]`, found `list[tuple[Any, ...]]`
- psycopg/psycopg/cursor_async.py:295:51: error[invalid-argument-type] Argument to bound method `load_row` is incorrect: Expected `RowMaker[tuple[Any, ...]]`, found `Unknown | RowMaker[Row@AsyncCursor]`
- psycopg/psycopg/cursor_async.py:297:20: error[invalid-return-type] Return type does not match returned value: expected `Row@AsyncCursor`, found `tuple[Any, ...]`
- Found 682 diagnostics
+ Found 664 diagnostics

apprise (https://github.com/caronc/apprise)
- apprise/attachment/base.py:248:21: error[invalid-argument-type] Argument to function `guess_type` is incorrect: Expected `str | PathLike[str]`, found `Unknown | str | None`
+ apprise/attachment/base.py:248:21: error[invalid-argument-type] Argument to function `guess_type` is incorrect: Expected `str | PathLike[str]`, found `Unknown | None | str`

archinstall (https://github.com/archlinux/archinstall)
- archinstall/lib/profile/profiles_handler.py:144:10: error[invalid-return-type] Return type does not match returned value: expected `list[Profile]`, found `Unknown | Divergent | list[Profile] | None`
- Found 43 diagnostics
+ Found 42 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/backports/tarfile/__init__.py:999:44: warning[possibly-missing-attribute] Attribute `endswith` may be missing on object of type `Unknown | None | int | bytes | str`
+ setuptools/_vendor/backports/tarfile/__init__.py:999:44: warning[possibly-missing-attribute] Attribute `endswith` may be missing on object of type `Unknown | str | None | int | bytes`

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-dask/tests/test_task_runners.py:352:24: warning[possibly-missing-attribute] Attribute `_adapt_called` may be missing on object of type `Unknown | None`
- Found 5535 diagnostics
+ Found 5536 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/heap/ptmalloc.py:1894:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | int | None`
+ pwndbg/aglib/heap/ptmalloc.py:1894:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `int | None | Unknown`
- pwndbg/aglib/kernel/paging.py:629:27: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | int | None` and `Literal[33554432]`
+ pwndbg/aglib/kernel/paging.py:629:27: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None | int` and `Literal[33554432]`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/tests/test_coordinate_transform.py:114:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_coordinate_transform.py:120:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_variable.py:1416:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_variable.py:1420:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_variable.py:2943:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1773 diagnostics
+ Found 1768 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
+ aiohttp/web_response.py:647:34: error[invalid-argument-type] Argument to bound method `encode` is incorrect: Expected `str`, found `str | None`
- Found 181 diagnostics
+ Found 182 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- tests/ci_visibility/api/fake_runner_efd_all_pass.py:26:5: error[invalid-assignment] Object of type `Unknown | int` is not assignable to attribute `start_ns` on type `Span | None`
+ tests/ci_visibility/api/fake_runner_efd_all_pass.py:26:5: error[invalid-assignment] Object of type `int` is not assignable to attribute `start_ns` on type `Span | None`
- tests/ci_visibility/api/fake_runner_efd_mix_fail.py:26:5: error[invalid-assignment] Object of type `Unknown | int` is not assignable to attribute `start_ns` on type `Span | None`
+ tests/ci_visibility/api/fake_runner_efd_mix_fail.py:26:5: error[invalid-assignment] Object of type `int` is not assignable to attribute `start_ns` on type `Span | None`
- tests/ci_visibility/api/fake_runner_efd_mix_pass.py:26:5: error[invalid-assignment] Object of type `Unknown | int` is not assignable to attribute `start_ns` on type `Span | None`
+ tests/ci_visibility/api/fake_runner_efd_mix_pass.py:26:5: error[invalid-assignment] Object of type `int` is not assignable to attribute `start_ns` on type `Span | None`

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/auxs/calibtools.py:982:25: error[invalid-argument-type] Argument to function `log` is incorrect: Expected `SupportsFloat | SupportsIndex`, found `Unknown | int | float | property`
- Found 685 diagnostics
+ Found 684 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Self@iloc | Series[Any, Any], generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Self@iloc | SeriesHE[Any, Any], generic[object]]`
- static_frame/test/unit/test_frame.py:14674:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1844 diagnostics
+ Found 1843 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/physics/continuum_mechanics/arch.py:685:40: error[unsupported-operator] Operator `*` is not supported between objects of type `Unknown | None` and `float`
+ sympy/physics/continuum_mechanics/arch.py:747:51: error[unsupported-operator] Operator `*` is not supported between objects of type `Unknown | None` and `float`
+ sympy/physics/continuum_mechanics/arch.py:748:51: error[unsupported-operator] Operator `*` is not supported between objects of type `Unknown | None` and `float`
+ sympy/physics/continuum_mechanics/arch.py:763:39: error[unsupported-operator] Operator `*` is not supported between objects of type `float` and `Unknown | None`
+ sympy/physics/continuum_mechanics/arch.py:766:24: error[unsupported-operator] Operator `*` is not supported between objects of type `float` and `Unknown | None`
+ sympy/physics/continuum_mechanics/arch.py:858:39: error[unsupported-operator] Operator `*` is not supported between objects of type `float` and `Unknown | None`
+ sympy/physics/continuum_mechanics/arch.py:861:24: error[unsupported-operator] Operator `*` is not supported between objects of type `float` and `Unknown | None`
+ sympy/physics/continuum_mechanics/arch.py:923:39: error[unsupported-operator] Operator `*` is not supported between objects of type `float` and `Unknown | None`
+ sympy/physics/continuum_mechanics/arch.py:926:24: error[unsupported-operator] Operator `*` is not supported between objects of type `float` and `Unknown | None`
+ sympy/physics/continuum_mechanics/arch.py:1008:39: error[unsupported-operator] Operator `*` is not supported between objects of type `float` and `Unknown | None`
+ sympy/physics/continuum_mechanics/arch.py:1011:24: error[unsupported-operator] Operator `*` is not supported between objects of type `float` and `Unknown | None`
+ sympy/physics/mechanics/system.py:1480:19: warning[possibly-missing-attribute] Attribute `LUsolve` may be missing on object of type `Unknown | None`
- Found 15407 diagnostics
+ Found 15419 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1228:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5151 diagnostics
+ Found 5150 diagnostics

colour (https://github.com/colour-science/colour)
- colour/colorimetry/spectrum.py:815:16: error[invalid-return-type] Return type does not match returned value: expected `ndarray[tuple[Any, ...], dtype[floating[_16Bit] | floating[_32Bit] | float64]]`, found `Unknown | ndarray[tuple[Any, ...], dtype[floating[_16Bit] | floating[_32Bit] | float64]] | property`
- colour/colorimetry/spectrum.py:839:16: error[invalid-return-type] Return type does not match returned value: expected `ndarray[tuple[Any, ...], dtype[floating[_16Bit] | floating[_32Bit] | float64]]`, found `Unknown | ndarray[tuple[Any, ...], dtype[floating[_16Bit] | floating[_32Bit] | float64]] | property`
- colour/colorimetry/spectrum.py:1603:43: error[unsupported-operator] Operator `>=` is not supported between objects of type `Unknown | ndarray[tuple[Any, ...], dtype[floating[_16Bit] | floating[_32Bit] | float64]] | property` and `Unknown | int | float`
- colour/colorimetry/spectrum.py:1603:65: error[unsupported-operator] Operator `<=` is not supported between objects of type `Unknown | ndarray[tuple[Any, ...], dtype[floating[_16Bit] | floating[_32Bit] | float64]] | property` and `Unknown | int | float`
- colour/colorimetry/tests/test_spectrum.py:1681:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_Buffer | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | ... omitted 8 union elements`, found `Unknown | ndarray[tuple[Any, ...], dtype[floating[_16Bit] | floating[_32Bit] | float64]] | property`
- colour/colorimetry/tests/test_spectrum.py:1691:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_Buffer | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | ... omitted 8 union elements`, found `Unknown | ndarray[tuple[Any, ...], dtype[floating[_16Bit] | floating[_32Bit] | float64]] | property`
- colour/colorimetry/tristimulus_values.py:1095:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | ndarray[tuple[Any, ...], dtype[floating[_16Bit] | floating[_32Bit] | float64]] | property`
- colour/colorimetry/tristimulus_values.py:1102:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | ndarray[tuple[Any, ...], dtype[floating[_16Bit] | floating[_32Bit] | float64]] | property`
- colour/io/tm2714.py:1638:36: error[invalid-argument-type] Argument to function `tstack` is incorrect: Expected `_Buffer | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | ... omitted 5 union elements`, found `list[Unknown | ndarray[tuple[Any, ...], dtype[floating[_16Bit] | floating[_32Bit] | float64]] | property]`
- colour/recovery/gaussian.py:157:27: warning[possibly-missing-attribute] Attribute `max` may be missing on object of type `Unknown | ndarray[tuple[Any, ...], dtype[floating[_16Bit] | floating[_32Bit] | float64]] | property`
- colour/utilities/network.py:232:16: error[invalid-return-type] Return type does not match returned value: expected `Self@parent | None`, found `Unknown | Self@parent | None | Self@__init__`
+ colour/utilities/network.py:232:16: error[invalid-return-type] Return type does not match returned value: expected `Self@parent | None`, found `Self@__init__ | None`
+ colour/utilities/network.py:249:9: error[invalid-assignment] Object of type `Self@parent | None` is not assignable to attribute `_parent` of type `Self@__init__ | None`
- colour/utilities/network.py:267:16: error[invalid-return-type] Return type does not match returned value: expected `list[Self@children]`, found `Unknown | list[Self@children] | list[Self@__init__]`
+ colour/utilities/network.py:267:16: error[invalid-return-type] Return type does not match returned value: expected `list[Self@children]`, found `list[Self@__init__]`
- colour/utilities/network.py:1718:31: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Self@children`, found `PortNode`
+ colour/utilities/network.py:290:9: error[invalid-assignment] Object of type `list[Self@children]` is not assignable to attribute `_children` of type `list[Self@__init__]`
- colour/utilities/network.py:1772:31: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `Self@children`, found `PortNode`
+ colour/utilities/network.py:1719:9: error[invalid-assignment] Object of type `Self@add_node` is not assignable to attribute `_parent` of type `Self@__init__ | None`
- colour/utilities/network.py:1814:41: warning[possibly-missing-attribute] Attribute `edges` may be missing on object of type `Unknown | Self@children | Self@__init__`
+ colour/utilities/network.py:1814:41: error[unresolved-attribute] Object of type `Self@__init__` has no attribute `edges`
+ colour/utilities/tests/test_network.py:1080:9: error[invalid-assignment] Object of type `PortGraph` is not assignable to attribute `_parent` of type `Self@__init__ | None`
- Found 429 diagnostics
+ Found 421 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/manual_mqtt/alarm_control_panel.py:321:24: error[invalid-return-type] Return type does not match returned value: expected `AlarmControlPanelState`, found `Unknown | str`
+ homeassistant/components/manual_mqtt/alarm_control_panel.py:328:16: error[invalid-return-type] Return type does not match returned value: expected `AlarmControlPanelState`, found `Unknown | str`
- Found 14446 diagnostics
+ Found 14448 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Renamed from "Avoid including `property` in subclasses properties" to "[ty] Avoid including `property` in subclasses properties" by @charliermarsh on 2025-12-19 16:29_

---

_Label `ty` added by @charliermarsh on 2025-12-19 16:29_

---

_Marked ready for review by @charliermarsh on 2025-12-19 16:34_

---

_Review requested from @carljm by @charliermarsh on 2025-12-19 16:35_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-19 16:35_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-19 16:35_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-19 16:35_

---

_Comment by @charliermarsh on 2025-12-19 17:01_

The SymPy diagnostics at least seem right on first glance, though not clear why they weren't firing before.

---

_Comment by @AlexWaygood on 2025-12-19 17:22_

can you update the assertion in the benchmark so that the benchmarks can run to completion?

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 17:44_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `ecosystem-analyzer` added by @charliermarsh on 2025-12-19 17:45_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 17:51_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 2 | 8 | 16 |
| `invalid-assignment` | 8 | 0 | 8 |
| `invalid-return-type` | 2 | 5 | 7 |
| `unsupported-operator` | 11 | 2 | 1 |
| `unused-ignore-comment` | 1 | 6 | 0 |
| `invalid-await` | 0 | 0 | 6 |
| `possibly-missing-attribute` | 2 | 1 | 3 |
| `unresolved-attribute` | 1 | 0 | 0 |
| **Total** | **27** | **22** | **41** |

**[Full report with detailed diff](https://charlie-property-fix.ecosystem-663.pages.dev/diff)** ([timing results](https://charlie-property-fix.ecosystem-663.pages.dev/timing))




---

_@carljm approved on 2025-12-30 02:15_

LGTM!

---

_Merged by @charliermarsh on 2025-12-30 03:28_

---

_Closed by @charliermarsh on 2025-12-30 03:28_

---

_Branch deleted on 2025-12-30 03:28_

---
