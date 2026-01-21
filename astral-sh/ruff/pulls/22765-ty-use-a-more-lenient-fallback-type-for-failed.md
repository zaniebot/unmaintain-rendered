```yaml
number: 22765
title: "[ty] Use a more lenient fallback type for failed `namedtuple()` and `NamedTuple` calls"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/lenient-namedtuple-parsing-2
created_at: 2026-01-20T14:44:06Z
updated_at: 2026-01-21T16:13:31Z
url: https://github.com/astral-sh/ruff/pull/22765
synced_at: 2026-01-21T17:03:51Z
```

# [ty] Use a more lenient fallback type for failed `namedtuple()` and `NamedTuple` calls

---

_@AlexWaygood_

## Summary

This is stacked on top of #22718; review that first.

Currently if a user makes a bad `NamedTuple()` call like this:

```py
from typing import NamedTuple, reveal_type

Bad1 = NamedTuple()
Bad1()
```

then we emit errors on _both_ calls: one correct error for the bad `NamedTuple()` call (it's missing required arguments), and then another spurious error on the `Bad1()` call:

```
No overload of bound method `__init__` matches arguments
```

The reason for this second, spurious error is that we inferred `type[NamedTupleFallback]` for the result of the `Bad1()` call, and `NamedTupleFallback` has this `__init__` signature, which isn't appropriate in this context -- it actually reflects the arguments you need to pass to the `NamedTuple` function itself in order to _create_ a `NamedTuple` class, not the signature of the constructors that all `NamedTuple` classes have:

https://github.com/astral-sh/ruff/blob/7e1f07e5c606990e3a67c2c7d0c7367740636904/crates/ty_vendored/vendor/typeshed/stdlib/_typeshed/_type_checker_internals.pyi#L63-L69

This PR updates the fallback type we use when `NamedTuple` parsing fails, so that it's more lenient and the spurious error is no longer emitted.

## Test Plan

mdtests updated


---

_Review requested from @charliermarsh by @AlexWaygood on 2026-01-20 14:44_

---

_Label `ty` added by @AlexWaygood on 2026-01-20 14:44_

---

_@AlexWaygood reviewed on 2026-01-20 14:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6705 on 2026-01-20 14:44_

drive-by simplification; no functional change to this bit

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 14:45_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-20 14:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@charliermarsh approved on 2026-01-20 14:53_

Seems reasonable (assuming tests pass and ecosystem changes are clean).

---

_Marked ready for review by @AlexWaygood on 2026-01-20 14:54_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-20 14:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-20 14:54_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-20 14:54_

---

_Merged by @AlexWaygood on 2026-01-21 16:13_

---

_Closed by @AlexWaygood on 2026-01-21 16:13_

---

_Branch deleted on 2026-01-21 16:13_

---
