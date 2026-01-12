```yaml
number: 21776
title: "[ty] Fix flow of associated member states during star imports"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/fix-1355
created_at: 2025-12-03T15:48:38Z
updated_at: 2025-12-03T16:52:34Z
url: https://github.com/astral-sh/ruff/pull/21776
synced_at: 2026-01-12T15:57:33Z
```

# [ty] Fix flow of associated member states during star imports

---

_@sharkdp_

## Summary

Star-imports can not just affect the state of symbols that they pull in, they can also affect the state of members that are associated with those symbols. For example, if `obj.attr` was previously narrowed from `int | None` to `int`, and a star-import now overwrites `obj`, then the narrowing on `obj.attr` should be "reset".

This PR keeps track of the state of associated members during star imports and properly models the flow of their corresponding state through the control flow structure that we artificially create for star-imports.

See [this comment](https://github.com/astral-sh/ty/issues/1355#issuecomment-3607125005) for an explanation why this caused ty to see certain `asyncio` symbols as not being accessible on Python 3.14.

closes https://github.com/astral-sh/ty/issues/1355

## Ecosystem impact

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- src/async_utils/bg_loop.py:115:31: error[invalid-argument-type] Argument to bound method `set_task_factory` is incorrect: Expected `_TaskFactory | None`, found `def eager_task_factory[_T_co](loop: AbstractEventLoop | None, coro: Coroutine[Any, Any, _T_co@eager_task_factory], *, name: str | None = None, context: Context | None = None) -> Task[_T_co@eager_task_factory]`
- Found 30 diagnostics
+ Found 29 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ mitmproxy/utils/asyncio_utils.py:96:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/conftest.py:37:31: error[invalid-argument-type] Argument to bound method `set_task_factory` is incorrect: Expected `_TaskFactory | None`, found `def eager_task_factory[_T_co](loop: AbstractEventLoop | None, coro: Coroutine[Any, Any, _T_co@eager_task_factory], *, name: str | None = None, context: Context | None = None) -> Task[_T_co@eager_task_factory]`
```

All of these seem to be correct, they give us a different type for `asyncio` symbols that are now imported from different `sys.version_info` branches (where we previously failed to recognize some of these as statically true/false).

```diff
dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/asyncio/patch.py:39:12: error[invalid-argument-type] Argument to function `unwrap` is incorrect: Expected `WrappedFunction`, found `def create_task[_T](self, coro: Coroutine[Any, Any, _T@create_task] | Generator[Any, None, _T@create_task], *, name: object = None) -> Task[_T@create_task]`
+ ddtrace/contrib/internal/asyncio/patch.py:39:12: error[invalid-argument-type] Argument to function `unwrap` is incorrect: Expected `WrappedFunction`, found `def create_task[_T](self, coro: Generator[Any, None, _T@create_task] | Coroutine[Any, Any, _T@create_task], *, name: object = None) -> Task[_T@create_task]`
```

Similar, but only results in a diagnostic change.

## Test Plan

Added a regression test


---

_Label `bug` added by @sharkdp on 2025-12-03 15:48_

---

_Label `ty` added by @sharkdp on 2025-12-03 15:48_

---

_Comment by @astral-sh-bot[bot] on 2025-12-03 15:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-03 15:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- src/async_utils/bg_loop.py:115:31: error[invalid-argument-type] Argument to bound method `set_task_factory` is incorrect: Expected `_TaskFactory | None`, found `def eager_task_factory[_T_co](loop: AbstractEventLoop | None, coro: Coroutine[Any, Any, _T_co@eager_task_factory], *, name: str | None = None, context: Context | None = None) -> Task[_T_co@eager_task_factory]`
- Found 30 diagnostics
+ Found 29 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ mitmproxy/utils/asyncio_utils.py:96:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/conftest.py:37:31: error[invalid-argument-type] Argument to bound method `set_task_factory` is incorrect: Expected `_TaskFactory | None`, found `def eager_task_factory[_T_co](loop: AbstractEventLoop | None, coro: Coroutine[Any, Any, _T_co@eager_task_factory], *, name: str | None = None, context: Context | None = None) -> Task[_T_co@eager_task_factory]`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/asyncio/patch.py:39:12: error[invalid-argument-type] Argument to function `unwrap` is incorrect: Expected `WrappedFunction`, found `def create_task[_T](self, coro: Coroutine[Any, Any, _T@create_task] | Generator[Any, None, _T@create_task], *, name: object = None) -> Task[_T@create_task]`
+ ddtrace/contrib/internal/asyncio/patch.py:39:12: error[invalid-argument-type] Argument to function `unwrap` is incorrect: Expected `WrappedFunction`, found `def create_task[_T](self, coro: Generator[Any, None, _T@create_task] | Coroutine[Any, Any, _T@create_task], *, name: object = None) -> Task[_T@create_task]`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @sharkdp on 2025-12-03 16:08_

---

_Review requested from @carljm by @sharkdp on 2025-12-03 16:08_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-03 16:08_

---

_Review requested from @dcreager by @sharkdp on 2025-12-03 16:08_

---

_@AlexWaygood approved on 2025-12-03 16:24_

Brilliant. Thank you!!

---

_Merged by @sharkdp on 2025-12-03 16:52_

---

_Closed by @sharkdp on 2025-12-03 16:52_

---

_Branch deleted on 2025-12-03 16:52_

---
