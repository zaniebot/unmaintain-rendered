```yaml
number: 22718
title: "[ty] Support recursive and stringified annotations in functional `typing.NamedTuple`s"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: alex/defer-namedtuple
created_at: 2026-01-19T11:42:59Z
updated_at: 2026-01-19T11:52:15Z
url: https://github.com/astral-sh/ruff/pull/22718
synced_at: 2026-01-19T12:32:36Z
```

# [ty] Support recursive and stringified annotations in functional `typing.NamedTuple`s

---

_@AlexWaygood_

_No description provided._

---

_Label `ty` added by @AlexWaygood on 2026-01-19 11:43_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-19 11:43_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 11:45_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-19 11:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/tests/utils/mock.py:74:39: error[invalid-argument-type] Invalid `NamedTuple()` field definition: Expected a `(name, type)` tuple, found `Literal["version"]`
+ rotkehlchen/tests/utils/mock.py:74:38: error[invalid-named-tuple] Invalid argument to parameter `fields` of `NamedTuple()`: `fields` must be a sequence of literal lists or tuples

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14497 diagnostics
+ Found 14498 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
+ WARN expected `heap_size` to be provided by Salsa query `deferred_spec`
+ WARN expected `heap_size` to be provided by Salsa query `deferred_spec`
+ WARN expected `heap_size` to be provided by Salsa query `deferred_spec`
+ WARN expected `heap_size` to be provided by Salsa query `deferred_spec`
+ WARN expected `heap_size` to be provided by Salsa query `deferred_spec`
+ WARN expected `heap_size` to be provided by Salsa query `deferred_spec`
+ WARN expected `heap_size` to be provided by Salsa query `deferred_spec`
+ WARN expected `heap_size` to be provided by Salsa query `deferred_spec`
+ WARN expected `heap_size` to be provided by Salsa query `deferred_spec`


```

</details>




---

_Comment by @astral-sh-bot[bot] on 2026-01-19 11:49_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-argument-type` | 2 | 3 | 1 |
| `invalid-return-type` | 4 | 0 | 2 |
| `possibly-missing-attribute` | 3 | 0 | 1 |
| `unused-ignore-comment` | 2 | 1 | 0 |
| `invalid-await` | 2 | 0 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `invalid-named-tuple` | 1 | 0 | 0 |
| **Total** | **14** | **4** | **13** |


**[Full report with detailed diff](https://c9a81d99.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://c9a81d99.ty-ecosystem-ext.pages.dev/timing))



---
