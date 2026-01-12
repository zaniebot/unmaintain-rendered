```yaml
number: 21143
title: "[ty] Use declared attribute types as type context"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/declared-attribute-type-context
created_at: 2025-10-30T15:19:15Z
updated_at: 2025-10-31T15:48:30Z
url: https://github.com/astral-sh/ruff/pull/21143
synced_at: 2026-01-12T15:57:17Z
```

# [ty] Use declared attribute types as type context

---

_@ibraheemdev_

## Summary

For example:
```py
class X:
    x: list[Literal[1]]

def _(x: X):
    x.x = [1]
```

Resolves https://github.com/astral-sh/ty/issues/1375.


---

_Review requested from @carljm by @ibraheemdev on 2025-10-30 15:19_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-30 15:19_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-30 15:19_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-30 15:19_

---

_Label `ty` added by @ibraheemdev on 2025-10-30 15:19_

---

_Comment by @github-actions[bot] on 2025-10-30 15:21_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-30 15:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
ignite (https://github.com/pytorch/ignite)
- tests/ignite/handlers/test_base_logger.py:265:5: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to attribute `_events` of type `list[CallableEventWithFilter]`
+ tests/ignite/handlers/test_base_logger.py:265:5: error[invalid-assignment] Object of type `list[CallableEventWithFilter | Literal["unknown"]]` is not assignable to attribute `_events` of type `list[CallableEventWithFilter]`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/addons/test_tlsconfig.py:476:13: error[invalid-assignment] Object of type `list[Unknown | HttpProxy | int]` is not assignable to attribute `layers` of type `list[Layer]`
+ test/mitmproxy/addons/test_tlsconfig.py:476:13: error[invalid-assignment] Object of type `list[Layer | Literal[123]]` is not assignable to attribute `layers` of type `list[Layer]`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/dependencies/python.py:531:13: error[invalid-assignment] Object of type `list[Unknown | str | None]` is not assignable to attribute `link_args` of type `list[str]`
+ mesonbuild/dependencies/python.py:531:13: error[invalid-assignment] Object of type `list[str | Unknown | None]` is not assignable to attribute `link_args` of type `list[str]`
- unittests/allplatformstests.py:223:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[list[Unknown | str], str]]` is not assignable to attribute `values` of type `dict[str, tuple[str | int, str | None]]`
+ unittests/allplatformstests.py:223:9: error[invalid-assignment] Object of type `dict[str, tuple[str | int, str | None] | tuple[list[Unknown | str], str]]` is not assignable to attribute `values` of type `dict[str, tuple[str | int, str | None]]`

xarray (https://github.com/pydata/xarray)
- xarray/computation/rolling.py:112:9: error[invalid-assignment] Object of type `list[Literal[False]]` is not assignable to attribute `center` of type `list[bool]`
- Found 1734 diagnostics
+ Found 1733 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/weather/__init__.py:322:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | list[Unknown]]` is not assignable to attribute `_forecast_listeners` of type `dict[Literal["daily", "hourly", "twice_daily"], list[(list[JsonValueType] | None, /) -> None]]`
- homeassistant/util/async_iterator.py:123:9: error[invalid-assignment] Object of type `Future[None]` is not assignable to attribute `_write_future` of type `Future[bytes | None] | None`
- Found 14272 diagnostics
+ Found 14270 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@ibraheemdev reviewed on 2025-10-30 15:24_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:235 on 2025-10-30 15:24_

Hover tests would be nice here...

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-30 15:28_

---

_Comment by @github-actions[bot] on 2025-10-30 15:35_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 0 | 3 | 4 |
| **Total** | **0** | **3** | **4** |

**[Full report with detailed diff](https://ibraheem-declared-attribute.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-declared-attribute.ecosystem-663.pages.dev/timing))


---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-30 16:21_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-30 16:22_

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-30 20:03_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-30 20:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3654 on 2025-10-30 21:06_

should we open an issue for this, with an example of code that this would help us infer better?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4210 on 2025-10-30 21:07_

```suggestion
    #[expect(clippy::type_complexity)]
```

or we could also just give in to Clippy here and add a type alias ;)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:10302 on 2025-10-30 21:09_

```suggestion
    const fn is_panic(self) -> bool {
```

---

_@AlexWaygood approved on 2025-10-30 21:10_

very cool!

---

_Merged by @ibraheemdev on 2025-10-31 15:48_

---

_Closed by @ibraheemdev on 2025-10-31 15:48_

---

_Branch deleted on 2025-10-31 15:48_

---
