```yaml
number: 20521
title: "[ty] Fix class literal subtyping with object fallback"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: fix-class-literal-subtyping-object-fallback
created_at: 2025-09-22T21:26:41Z
updated_at: 2025-11-06T11:48:26Z
url: https://github.com/astral-sh/ruff/pull/20521
synced_at: 2026-01-12T15:57:03Z
```

# [ty] Fix class literal subtyping with object fallback

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

@ibraheemdev notes this example failed

```py
from typing import Callable

class X:
    ...

def f(callable: Callable[[], X]) -> X:
    return callable()

x = f(X)
```

Resolves https://github.com/astral-sh/ty/issues/1210

The issue was that we set the `Self` to the class type instead of the instance type of the class.

## Test Plan

Fix tests in `is_subtype_of.md`


---

_Review requested from @carljm by @MatthewMckee4 on 2025-09-22 21:26_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-09-22 21:26_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-09-22 21:26_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-09-22 21:26_

---

_Renamed from "Fix class literal subtyping with object fallback" to "[ty] Fix class literal subtyping with object fallback" by @MatthewMckee4 on 2025-09-22 21:26_

---

_Comment by @github-actions[bot] on 2025-09-22 21:28_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `ty` added by @ibraheemdev on 2025-09-22 21:38_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-09-22 21:38_

---

_Comment by @github-actions[bot] on 2025-09-22 21:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/from_thread.py:448:19: error[no-matching-overload] No overload of function `field` matches arguments
- Found 228 diagnostics
+ Found 227 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- chess/pgn.py:1857:35: error[invalid-argument-type] Argument to function `read_game` is incorrect: Expected `() -> BaseVisitor[Unknown]`, found `<class 'SkipVisitor'>`
- Found 13 diagnostics
+ Found 12 diagnostics

kornia (https://github.com/kornia/kornia)
- kornia/config.py:68:36: error[no-matching-overload] No overload of function `field` matches arguments
- Found 784 diagnostics
+ Found 783 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/client.py:2642:1: error[invalid-assignment] Object of type `<class 'SubprocessSSHVendor'>` is not assignable to `() -> SSHVendor`
- Found 180 diagnostics
+ Found 179 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_vendor/rich/progress.py:980:20: error[no-matching-overload] No overload of function `field` matches arguments
- Found 435 diagnostics
+ Found 434 diagnostics

rich (https://github.com/Textualize/rich)
- rich/progress.py:980:20: error[no-matching-overload] No overload of function `field` matches arguments
- Found 310 diagnostics
+ Found 309 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/auths.py:127:37: error[no-matching-overload] No overload of function `field` matches arguments
- Found 273 diagnostics
+ Found 272 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_entry_queue.py:45:43: error[invalid-argument-type] Argument to function `Factory` is incorrect: Expected `() -> RLock`, found `<class 'RLock'>`
- Found 674 diagnostics
+ Found 673 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 45 diagnostics
+ Found 44 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-shell/prefect_shell/commands.py:265:9: error[invalid-argument-type] Argument to function `PrivateAttr` is incorrect: Expected `() -> AsyncExitStack[bool | None]`, found `<class 'AsyncExitStack'>`
- Found 3041 diagnostics
+ Found 3040 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/tests/test_deconstruct.py:92:12: error[no-matching-overload] No overload of function `get_callable_args` matches arguments
- Found 1359 diagnostics
+ Found 1358 diagnostics

jax (https://github.com/google/jax)
- jax/_src/pallas/mosaic/interpret.py:403:26: error[no-matching-overload] No overload of function `field` matches arguments
- jax/_src/pallas/mosaic/interpret.py:419:26: error[no-matching-overload] No overload of function `field` matches arguments
- jax/_src/pallas/mosaic/interpret.py:582:26: error[no-matching-overload] No overload of function `field` matches arguments
- Found 2252 diagnostics
+ Found 2249 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/internal/rate_limiter.py:203:29: error[no-matching-overload] No overload of function `field` matches arguments
- Found 6669 diagnostics
+ Found 6668 diagnostics

CPython (cases_generator) (https://github.com/python/cpython)
- Lib/_pyrepl/_threading_handler.py:35:32: error[no-matching-overload] No overload of function `field` matches arguments
- Lib/test/test_urllib2.py:1870:26: error[invalid-argument-type] Argument to function `build_opener` is incorrect: Expected `BaseHandler | (() -> BaseHandler)`, found `<class 'FooHandler'>`
- Lib/test/test_urllib2.py:1870:38: error[invalid-argument-type] Argument to function `build_opener` is incorrect: Expected `BaseHandler | (() -> BaseHandler)`, found `<class 'BarHandler'>`
- Lib/test/test_urllib2.py:1875:26: error[invalid-argument-type] Argument to function `build_opener` is incorrect: Expected `BaseHandler | (() -> BaseHandler)`, found `<class 'FooHandler'>`
- Found 23908 diagnostics
+ Found 23904 diagnostics

CPython (peg_generator) (https://github.com/python/cpython)
- Lib/_pyrepl/_threading_handler.py:35:32: error[no-matching-overload] No overload of function `field` matches arguments
- Lib/test/test_urllib2.py:1870:26: error[invalid-argument-type] Argument to function `build_opener` is incorrect: Expected `BaseHandler | (() -> BaseHandler)`, found `<class 'FooHandler'>`
- Lib/test/test_urllib2.py:1870:38: error[invalid-argument-type] Argument to function `build_opener` is incorrect: Expected `BaseHandler | (() -> BaseHandler)`, found `<class 'BarHandler'>`
- Lib/test/test_urllib2.py:1875:26: error[invalid-argument-type] Argument to function `build_opener` is incorrect: Expected `BaseHandler | (() -> BaseHandler)`, found `<class 'FooHandler'>`
- Found 23907 diagnostics
+ Found 23903 diagnostics

CPython (Argument Clinic) (https://github.com/python/cpython)
- Lib/_pyrepl/_threading_handler.py:35:32: error[no-matching-overload] No overload of function `field` matches arguments
- Lib/test/test_urllib2.py:1870:26: error[invalid-argument-type] Argument to function `build_opener` is incorrect: Expected `BaseHandler | (() -> BaseHandler)`, found `<class 'FooHandler'>`
- Lib/test/test_urllib2.py:1870:38: error[invalid-argument-type] Argument to function `build_opener` is incorrect: Expected `BaseHandler | (() -> BaseHandler)`, found `<class 'BarHandler'>`
- Lib/test/test_urllib2.py:1875:26: error[invalid-argument-type] Argument to function `build_opener` is incorrect: Expected `BaseHandler | (() -> BaseHandler)`, found `<class 'FooHandler'>`
- Found 23908 diagnostics
+ Found 23904 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-09-22 21:43_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `no-matching-overload` | 0 | 10 | 0 |
| `invalid-argument-type` | 0 | 3 | 0 |
| `invalid-assignment` | 0 | 1 | 0 |
| **Total** | **0** | **14** | **0** |


---

_Comment by @MatthewMckee4 on 2025-09-22 21:44_

All of the 
```
error[no-matching-overload] No overload of function `field` matches arguments
```
 diagnostics look good, seems like they are mostly for something of the form:

```py
a: A = field(default_factory=A)
```

and the other diagnostics basically saying
```
error[invalid-argument-type] Argument to function ... is incorrect: Expected `() -> A`, found `<class 'A'>`
```

---

_@ibraheemdev approved on 2025-09-22 22:03_

---

_@carljm approved on 2025-09-23 00:26_

Great find, thank you!

---

_Merged by @carljm on 2025-09-23 00:26_

---

_Closed by @carljm on 2025-09-23 00:26_

---

_Branch deleted on 2025-11-06 11:48_

---
