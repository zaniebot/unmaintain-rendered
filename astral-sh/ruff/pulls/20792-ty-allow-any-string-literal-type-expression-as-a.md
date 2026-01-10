```yaml
number: 20792
title: "[ty] allow any string `Literal` type expression as a key when constructing a `TypedDict`"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: typed-dict-key-expr
created_at: 2025-10-09T17:52:20Z
updated_at: 2025-10-09T21:30:31Z
url: https://github.com/astral-sh/ruff/pull/20792
synced_at: 2026-01-10T17:34:34Z
```

# [ty] allow any string `Literal` type expression as a key when constructing a `TypedDict`

---

_Pull request opened by @mtshiba on 2025-10-09 17:52_

## Summary

This PR allows any string `Literal` type expression to be used as a key when constructing a `TypedDict`.

The pattern of reusing constant key strings when constructing a `TypedDict` seems to be common in several libraries (e.g., [home-assistant/core](https://github.com/home-assistant/core/blob/f6fb4c8d5a26d496ca0ebe790776bc208386e83d/homeassistant/components/environment_canada/weather.py#L227C1-L241C14)).
Currently, we only accept string literal keys when constructing a `TypedDict`, but any expression that can be inferred to be of type string `Literal` is also acceptable. In fact, [the spec](https://typing.python.org/en/latest/spec/typeddict.html#use-of-final-values-and-literal-types) explicitly states that `Literal` type expressions and `Final` names ​​should be allowed.

## Test Plan

New tests in `typed_dict.md`


---

_Comment by @github-actions[bot] on 2025-10-09 17:54_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-09 17:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
core (https://github.com/home-assistant/core)
- homeassistant/components/dhcp/__init__.py:228:33: error[missing-typed-dict-key] Missing required key 'hostname' in TypedDict `DHCPAddressData` constructor
- homeassistant/components/dhcp/__init__.py:228:33: error[missing-typed-dict-key] Missing required key 'ip' in TypedDict `DHCPAddressData` constructor
- homeassistant/components/meteo_france/weather.py:194:21: error[missing-typed-dict-key] Missing required key 'datetime' in TypedDict `Forecast` constructor
- homeassistant/components/meteo_france/weather.py:216:21: error[missing-typed-dict-key] Missing required key 'datetime' in TypedDict `Forecast` constructor
- homeassistant/components/nws/weather.py:250:30: error[missing-typed-dict-key] Missing required key 'datetime' in TypedDict `Forecast` constructor
- homeassistant/components/nws/weather.py:306:35: error[missing-typed-dict-key] Missing required key 'datetime' in TypedDict `ExtraForecast` constructor
- homeassistant/components/p1_monitor/coordinator.py:73:31: error[missing-typed-dict-key] Missing required key 'smartmeter' in TypedDict `P1MonitorData` constructor
- homeassistant/components/p1_monitor/coordinator.py:73:31: error[missing-typed-dict-key] Missing required key 'phases' in TypedDict `P1MonitorData` constructor
- homeassistant/components/p1_monitor/coordinator.py:73:31: error[missing-typed-dict-key] Missing required key 'settings' in TypedDict `P1MonitorData` constructor
- homeassistant/components/p1_monitor/coordinator.py:73:31: error[missing-typed-dict-key] Missing required key 'watermeter' in TypedDict `P1MonitorData` constructor
- homeassistant/components/tcp/entity.py:40:41: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `TcpSensorConfig` constructor
- homeassistant/components/tcp/entity.py:40:41: error[missing-typed-dict-key] Missing required key 'host' in TypedDict `TcpSensorConfig` constructor
- homeassistant/components/tcp/entity.py:40:41: error[missing-typed-dict-key] Missing required key 'port' in TypedDict `TcpSensorConfig` constructor
- homeassistant/components/tcp/entity.py:40:41: error[missing-typed-dict-key] Missing required key 'timeout' in TypedDict `TcpSensorConfig` constructor
- homeassistant/components/tcp/entity.py:40:41: error[missing-typed-dict-key] Missing required key 'payload' in TypedDict `TcpSensorConfig` constructor
- homeassistant/components/tcp/entity.py:40:41: error[missing-typed-dict-key] Missing required key 'unit_of_measurement' in TypedDict `TcpSensorConfig` constructor
- homeassistant/components/tcp/entity.py:40:41: error[missing-typed-dict-key] Missing required key 'value_template' in TypedDict `TcpSensorConfig` constructor
- homeassistant/components/tcp/entity.py:40:41: error[missing-typed-dict-key] Missing required key 'value_on' in TypedDict `TcpSensorConfig` constructor
- homeassistant/components/tcp/entity.py:40:41: error[missing-typed-dict-key] Missing required key 'buffer_size' in TypedDict `TcpSensorConfig` constructor
- homeassistant/components/tcp/entity.py:40:41: error[missing-typed-dict-key] Missing required key 'ssl' in TypedDict `TcpSensorConfig` constructor
- homeassistant/components/tcp/entity.py:40:41: error[missing-typed-dict-key] Missing required key 'verify_ssl' in TypedDict `TcpSensorConfig` constructor
- homeassistant/components/todoist/calendar.py:130:37: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `ProjectData` constructor
- homeassistant/components/todoist/calendar.py:130:37: error[missing-typed-dict-key] Missing required key 'id' in TypedDict `ProjectData` constructor
- homeassistant/components/todoist/calendar.py:170:37: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `ProjectData` constructor
- homeassistant/components/todoist/calendar.py:170:37: error[missing-typed-dict-key] Missing required key 'id' in TypedDict `ProjectData` constructor
- homeassistant/components/todoist/calendar.py:528:30: error[missing-typed-dict-key] Missing required key 'all_day' in TypedDict `TodoistEvent` constructor
- homeassistant/components/todoist/calendar.py:528:30: error[missing-typed-dict-key] Missing required key 'completed' in TypedDict `TodoistEvent` constructor
- homeassistant/components/todoist/calendar.py:528:30: error[missing-typed-dict-key] Missing required key 'description' in TypedDict `TodoistEvent` constructor
- homeassistant/components/todoist/calendar.py:528:30: error[missing-typed-dict-key] Missing required key 'due_today' in TypedDict `TodoistEvent` constructor
- homeassistant/components/todoist/calendar.py:528:30: error[missing-typed-dict-key] Missing required key 'end' in TypedDict `TodoistEvent` constructor
- homeassistant/components/todoist/calendar.py:528:30: error[missing-typed-dict-key] Missing required key 'labels' in TypedDict `TodoistEvent` constructor
- homeassistant/components/todoist/calendar.py:528:30: error[missing-typed-dict-key] Missing required key 'overdue' in TypedDict `TodoistEvent` constructor
- homeassistant/components/todoist/calendar.py:528:30: error[missing-typed-dict-key] Missing required key 'priority' in TypedDict `TodoistEvent` constructor
- homeassistant/components/todoist/calendar.py:528:30: error[missing-typed-dict-key] Missing required key 'start' in TypedDict `TodoistEvent` constructor
- homeassistant/components/todoist/calendar.py:528:30: error[missing-typed-dict-key] Missing required key 'summary' in TypedDict `TodoistEvent` constructor
- homeassistant/components/todoist/calendar.py:552:21: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `bool`, in comparing `Unknown` with `Unknown | bool | str | ... omitted 3 union elements`
- homeassistant/components/todoist/calendar.py:570:21: error[unsupported-operator] Operator `>` is not supported for types `bool` and `datetime`, in comparing `Unknown | bool | str | ... omitted 3 union elements` with `datetime`
+ homeassistant/components/todoist/calendar.py:570:21: error[unsupported-operator] Operator `>` is not supported for types `None` and `datetime`, in comparing `datetime | None` with `datetime`
- homeassistant/components/todoist/calendar.py:576:35: warning[possibly-missing-attribute] Attribute `date` on type `Unknown | bool | str | ... omitted 3 union elements` may be missing
+ homeassistant/components/todoist/calendar.py:576:35: warning[possibly-missing-attribute] Attribute `date` on type `datetime | None` may be missing
+ homeassistant/components/todoist/calendar.py:579:20: error[unsupported-operator] Operator `<=` is not supported for types `None` and `datetime`, in comparing `datetime | None` with `datetime`
- homeassistant/components/todoist/calendar.py:579:20: error[unsupported-operator] Operator `<=` is not supported for types `bool` and `str`, in comparing `Unknown | bool | str | ... omitted 3 union elements` with `Unknown | bool | str | ... omitted 3 union elements`
- homeassistant/components/todoist/calendar.py:584:33: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | bool | str | ... omitted 3 union elements` and `timedelta`
- homeassistant/core.py:1965:45: error[missing-typed-dict-key] Missing required key 's' in TypedDict `CompressedState` constructor
- homeassistant/core.py:1965:45: error[missing-typed-dict-key] Missing required key 'a' in TypedDict `CompressedState` constructor
- homeassistant/core.py:1965:45: error[missing-typed-dict-key] Missing required key 'c' in TypedDict `CompressedState` constructor
- homeassistant/core.py:1965:45: error[missing-typed-dict-key] Missing required key 'lc' in TypedDict `CompressedState` constructor
- Found 13736 diagnostics
+ Found 13695 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-10-09 17:56_

---

_Marked ready for review by @mtshiba on 2025-10-09 18:05_

---

_Review requested from @carljm by @mtshiba on 2025-10-09 18:05_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-10-09 18:05_

---

_Review requested from @sharkdp by @mtshiba on 2025-10-09 18:05_

---

_Review requested from @dcreager by @mtshiba on 2025-10-09 18:05_

---

_@AlexWaygood approved on 2025-10-09 18:19_

Great -- thank you!

---

_Merged by @AlexWaygood on 2025-10-09 18:24_

---

_Closed by @AlexWaygood on 2025-10-09 18:24_

---

_Branch deleted on 2025-10-09 18:28_

---

_Comment by @ibraheemdev on 2025-10-09 21:30_

> In fact, [the spec](https://typing.python.org/en/latest/spec/typeddict.html#use-of-final-values-and-literal-types) explicitly states that Literal type expressions and Final names ​​should be allowed.

FWIW I think the spec only states this for `TypedDict` *operations*, not definitions, but I agree that we should support this anyways as it's common in the ecosystem.

I think we can also update our bidirectional inference code, [which makes the same assumption](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/infer/builder.rs#L5860).

---
