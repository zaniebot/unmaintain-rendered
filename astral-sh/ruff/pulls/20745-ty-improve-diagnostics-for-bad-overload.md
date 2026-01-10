```yaml
number: 20745
title: "[ty] Improve diagnostics for bad `@overload` definitions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/overload-diagnostics
created_at: 2025-10-07T14:40:53Z
updated_at: 2025-10-07T21:52:58Z
url: https://github.com/astral-sh/ruff/pull/20745
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Improve diagnostics for bad `@overload` definitions

---

_Pull request opened by @AlexWaygood on 2025-10-07 14:40_

## Summary

This PR attempts to address some of the confusion in https://github.com/astral-sh/ty/issues/1316 by improving our diagnostics around bad `@overload` definitions:
- If a series of `@overload`-decorated functions is not followed by a non-`@overload`-decorated function, the diagnostic message is improved. Specifically, the diagnostic message no longer uses the term "implementation function", which is a little jargon-y for users (and was misunderstood in <https://github.com/astral-sh/ty/issues/1316>). Instead, it now uses the term "non-`@overload`-decorated function"
- A new warning-level diagnostic is added that is triggered if you provide a nontrivial body for an `@overload`-decorated function. While there's nothing _wrong_ with this in principle, the body will be completely discarded at runtime, so it strongly suggests that the user has misunderstood the purpose of the `@overload` decorator. I think it will probably help users quite a bit to have this pointed out to them.

On this file:

```py
from typing import overload

@overload
def foo(x: int) -> int:
    return x

@overload
def foo(x: str) -> None:
    """Docstring"""
    pass
    print("oh no, a string")
```

We now emit these diagnostics:

<img width="2718" height="1452" alt="image" src="https://github.com/user-attachments/assets/29d2c0ea-665b-46c6-87d0-6f513dc8ba61" />

## Test Plan

Mdtests and snapshots


---

_Review requested from @carljm by @AlexWaygood on 2025-10-07 14:40_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-07 14:40_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-07 14:40_

---

_Label `ty` added by @AlexWaygood on 2025-10-07 14:40_

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-07 14:40_

---

_Comment by @github-actions[bot] on 2025-10-07 14:43_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-07 21:50:12.141597704 +0000
+++ new-output.txt	2025-10-07 21:50:15.430625513 +0000
@@ -680,8 +680,8 @@
 narrowing_typeis.py:191:18: error[invalid-argument-type] Argument to function `takes_int_typeis` is incorrect: Expected `(object, /) -> TypeIs[int]`, found `def bool_typeis(val: object) -> TypeIs[bool]`
 overloads_basic.py:39:1: error[invalid-argument-type] Method `__getitem__` of type `Overload[(__i: int, /) -> int, (__s: slice[Any, Any, Any], /) -> bytes]` cannot be called with key of type `Literal[""]` on object of type `Bytes`
 overloads_definitions.py:20:5: error[invalid-overload] Overloaded function `func1` requires at least two overloads
-overloads_definitions.py:33:5: error[invalid-overload] Overloaded non-stub function `func2` must have an implementation
-overloads_definitions.py:63:9: error[invalid-overload] Overloaded non-stub function `not_abstract` must have an implementation
+overloads_definitions.py:33:5: error[invalid-overload] Overloads for function `func2` must be followed by a non-`@overload`-decorated implementation function
+overloads_definitions.py:63:9: error[invalid-overload] Overloads for function `not_abstract` must be followed by a non-`@overload`-decorated implementation function
 overloads_definitions.py:81:9: error[invalid-overload] Overloaded function `func5` does not use the `@staticmethod` decorator consistently
 overloads_definitions.py:94:9: error[invalid-overload] Overloaded function `func6` does not use the `@classmethod` decorator consistently
 overloads_definitions.py:117:45: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | str`
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-07 14:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/fixtures.py:1782:9: warning[useless-overload-body] Useless body for `@overload`-decorated function `parsefactories`: This statement will never be executed
+ src/_pytest/fixtures.py:1790:9: warning[useless-overload-body] Useless body for `@overload`-decorated function `parsefactories`: This statement will never be executed
- src/_pytest/mark/structures.py:482:13: error[invalid-overload] Overloaded non-stub function `__call__` must have an implementation
+ src/_pytest/mark/structures.py:482:13: error[invalid-overload] Overloads for function `__call__` must be followed by a non-`@overload`-decorated implementation function
- src/_pytest/mark/structures.py:497:13: error[invalid-overload] Overloaded non-stub function `__call__` must have an implementation
+ src/_pytest/mark/structures.py:497:13: error[invalid-overload] Overloads for function `__call__` must be followed by a non-`@overload`-decorated implementation function
- Found 489 diagnostics
+ Found 491 diagnostics

mypy (https://github.com/python/mypy)
- mypyc/test-data/fixtures/ir.py:65:9: error[invalid-overload] Overloaded non-stub function `__init__` must have an implementation
+ mypyc/test-data/fixtures/ir.py:65:9: error[invalid-overload] Overloads for function `__init__` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:94:9: error[invalid-overload] Overloaded non-stub function `__init__` must have an implementation
+ mypyc/test-data/fixtures/ir.py:94:9: error[invalid-overload] Overloads for function `__init__` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:107:9: error[invalid-overload] Overloaded non-stub function `__getitem__` must have an implementation
+ mypyc/test-data/fixtures/ir.py:107:9: error[invalid-overload] Overloads for function `__getitem__` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:168:9: error[invalid-overload] Overloaded non-stub function `__init__` must have an implementation
+ mypyc/test-data/fixtures/ir.py:168:9: error[invalid-overload] Overloads for function `__init__` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:177:9: error[invalid-overload] Overloaded non-stub function `__getitem__` must have an implementation
+ mypyc/test-data/fixtures/ir.py:177:9: error[invalid-overload] Overloads for function `__getitem__` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:188:9: error[invalid-overload] Overloaded non-stub function `__init__` must have an implementation
+ mypyc/test-data/fixtures/ir.py:188:9: error[invalid-overload] Overloads for function `__init__` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:199:9: error[invalid-overload] Overloaded non-stub function `__and__` must have an implementation
- mypyc/test-data/fixtures/ir.py:203:9: error[invalid-overload] Overloaded non-stub function `__or__` must have an implementation
- mypyc/test-data/fixtures/ir.py:207:9: error[invalid-overload] Overloaded non-stub function `__xor__` must have an implementation
- mypyc/test-data/fixtures/ir.py:214:9: error[invalid-overload] Overloaded non-stub function `__getitem__` must have an implementation
+ mypyc/test-data/fixtures/ir.py:199:9: error[invalid-overload] Overloads for function `__and__` must be followed by a non-`@overload`-decorated implementation function
+ mypyc/test-data/fixtures/ir.py:203:9: error[invalid-overload] Overloads for function `__or__` must be followed by a non-`@overload`-decorated implementation function
+ mypyc/test-data/fixtures/ir.py:207:9: error[invalid-overload] Overloads for function `__xor__` must be followed by a non-`@overload`-decorated implementation function
+ mypyc/test-data/fixtures/ir.py:214:9: error[invalid-overload] Overloads for function `__getitem__` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:221:9: error[invalid-overload] Overloaded non-stub function `__add__` must have an implementation
+ mypyc/test-data/fixtures/ir.py:221:9: error[invalid-overload] Overloads for function `__add__` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:232:9: error[invalid-overload] Overloaded non-stub function `__getitem__` must have an implementation
+ mypyc/test-data/fixtures/ir.py:232:9: error[invalid-overload] Overloads for function `__getitem__` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:244:9: error[invalid-overload] Overloaded non-stub function `__add__` must have an implementation
+ mypyc/test-data/fixtures/ir.py:244:9: error[invalid-overload] Overloads for function `__add__` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:264:9: error[invalid-overload] Overloaded non-stub function `__init__` must have an implementation
+ mypyc/test-data/fixtures/ir.py:264:9: error[invalid-overload] Overloads for function `__init__` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:276:9: error[invalid-overload] Overloaded non-stub function `update` must have an implementation
+ mypyc/test-data/fixtures/ir.py:276:9: error[invalid-overload] Overloads for function `update` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:366:5: error[invalid-overload] Overloaded non-stub function `sum` must have an implementation
+ mypyc/test-data/fixtures/ir.py:366:5: error[invalid-overload] Overloads for function `sum` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:377:5: error[invalid-overload] Overloaded non-stub function `next` must have an implementation
+ mypyc/test-data/fixtures/ir.py:377:5: error[invalid-overload] Overloads for function `next` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:388:5: error[invalid-overload] Overloaded non-stub function `zip` must have an implementation
+ mypyc/test-data/fixtures/ir.py:388:5: error[invalid-overload] Overloads for function `zip` must be followed by a non-`@overload`-decorated implementation function
- mypyc/test-data/fixtures/ir.py:394:5: error[invalid-overload] Overloaded non-stub function `divmod` must have an implementation
- mypyc/test-data/fixtures/ir.py:400:5: error[invalid-overload] Overloaded non-stub function `pow` must have an implementation
+ mypyc/test-data/fixtures/ir.py:394:5: error[invalid-overload] Overloads for function `divmod` must be followed by a non-`@overload`-decorated implementation function
+ mypyc/test-data/fixtures/ir.py:400:5: error[invalid-overload] Overloads for function `pow` must be followed by a non-`@overload`-decorated implementation function

trio (https://github.com/python-trio/trio)
- src/trio/_subprocess.py:1154:19: error[invalid-overload] Overloaded non-stub function `open_process` must have an implementation
+ src/trio/_subprocess.py:1154:19: error[invalid-overload] Overloads for function `open_process` must be followed by a non-`@overload`-decorated implementation function
- src/trio/_subprocess.py:1172:19: error[invalid-overload] Overloaded non-stub function `run_process` must have an implementation
+ src/trio/_subprocess.py:1172:19: error[invalid-overload] Overloads for function `run_process` must be followed by a non-`@overload`-decorated implementation function

altair (https://github.com/vega/altair)
- altair/vegalite/v6/schema/channels.py:499:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
+ altair/vegalite/v6/schema/channels.py:499:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:501:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:505:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:535:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:539:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:547:9: error[invalid-overload] Overloaded non-stub function `legend` must have an implementation
- altair/vegalite/v6/schema/channels.py:656:9: error[invalid-overload] Overloaded non-stub function `scale` must have an implementation
- altair/vegalite/v6/schema/channels.py:725:9: error[invalid-overload] Overloaded non-stub function `sort` must have an implementation
- altair/vegalite/v6/schema/channels.py:738:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
- altair/vegalite/v6/schema/channels.py:748:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:501:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:505:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:535:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:539:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:547:9: error[invalid-overload] Overloads for function `legend` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:656:9: error[invalid-overload] Overloads for function `scale` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:725:9: error[invalid-overload] Overloads for function `sort` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:738:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:748:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:750:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:750:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
- altair/vegalite/v6/schema/channels.py:908:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:925:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
+ altair/vegalite/v6/schema/channels.py:908:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:925:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:929:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:929:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:931:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
+ altair/vegalite/v6/schema/channels.py:931:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:1072:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:1317:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
+ altair/vegalite/v6/schema/channels.py:1072:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1317:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1319:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1323:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1353:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1359:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1367:9: error[invalid-overload] Overloads for function `legend` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1476:9: error[invalid-overload] Overloads for function `scale` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1545:9: error[invalid-overload] Overloads for function `sort` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1558:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:1319:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:1323:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:1353:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:1359:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:1367:9: error[invalid-overload] Overloaded non-stub function `legend` must have an implementation
- altair/vegalite/v6/schema/channels.py:1476:9: error[invalid-overload] Overloaded non-stub function `scale` must have an implementation
- altair/vegalite/v6/schema/channels.py:1545:9: error[invalid-overload] Overloaded non-stub function `sort` must have an implementation
- altair/vegalite/v6/schema/channels.py:1558:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
- altair/vegalite/v6/schema/channels.py:1568:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
- altair/vegalite/v6/schema/channels.py:1570:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
+ altair/vegalite/v6/schema/channels.py:1568:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1570:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:1730:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:1747:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
+ altair/vegalite/v6/schema/channels.py:1730:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1747:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:1751:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:1751:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1753:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:1895:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2124:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:1753:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
- altair/vegalite/v6/schema/channels.py:1895:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:2124:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
- altair/vegalite/v6/schema/channels.py:2128:9: error[invalid-overload] Overloaded non-stub function `align` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2128:9: error[invalid-overload] Overloads for function `align` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:2130:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:2134:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2130:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2134:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:2149:9: error[invalid-overload] Overloaded non-stub function `center` must have an implementation
- altair/vegalite/v6/schema/channels.py:2153:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:2161:9: error[invalid-overload] Overloaded non-stub function `header` must have an implementation
- altair/vegalite/v6/schema/channels.py:2222:9: error[invalid-overload] Overloaded non-stub function `sort` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2149:9: error[invalid-overload] Overloads for function `center` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2153:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2161:9: error[invalid-overload] Overloads for function `header` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2222:9: error[invalid-overload] Overloads for function `sort` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2230:9: error[invalid-overload] Overloads for function `spacing` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2238:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:2230:9: error[invalid-overload] Overloaded non-stub function `spacing` must have an implementation
- altair/vegalite/v6/schema/channels.py:2238:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
- altair/vegalite/v6/schema/channels.py:2248:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2248:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2250:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2506:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:2250:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
- altair/vegalite/v6/schema/channels.py:2506:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
- altair/vegalite/v6/schema/channels.py:2510:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:2514:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:2544:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:2550:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:2573:9: error[invalid-overload] Overloaded non-stub function `format` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2510:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2514:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2544:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2550:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2573:9: error[invalid-overload] Overloads for function `format` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:2575:9: error[invalid-overload] Overloaded non-stub function `formatType` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2575:9: error[invalid-overload] Overloads for function `formatType` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:2583:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2583:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:2593:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2593:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:2595:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2595:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:2748:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:2927:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2748:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2927:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:2931:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:2935:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:2952:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:2964:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2931:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2935:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2952:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:2964:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:2974:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2974:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:2976:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
+ altair/vegalite/v6/schema/channels.py:2976:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:3255:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
- altair/vegalite/v6/schema/channels.py:3259:9: error[invalid-overload] Overloaded non-stub function `align` must have an implementation
+ altair/vegalite/v6/schema/channels.py:3255:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3259:9: error[invalid-overload] Overloads for function `align` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:3266:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:3270:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
+ altair/vegalite/v6/schema/channels.py:3266:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3270:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:3285:9: error[invalid-overload] Overloaded non-stub function `bounds` must have an implementation
- altair/vegalite/v6/schema/channels.py:3289:9: error[invalid-overload] Overloaded non-stub function `center` must have an implementation
+ altair/vegalite/v6/schema/channels.py:3285:9: error[invalid-overload] Overloads for function `bounds` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3289:9: error[invalid-overload] Overloads for function `center` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:3293:9: error[invalid-overload] Overloaded non-stub function `columns` must have an implementation
- altair/vegalite/v6/schema/channels.py:3297:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:3305:9: error[invalid-overload] Overloaded non-stub function `header` must have an implementation
- altair/vegalite/v6/schema/channels.py:3366:9: error[invalid-overload] Overloaded non-stub function `sort` must have an implementation
- altair/vegalite/v6/schema/channels.py:3376:9: error[invalid-overload] Overloaded non-stub function `spacing` must have an implementation
- altair/vegalite/v6/schema/channels.py:3386:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
+ altair/vegalite/v6/schema/channels.py:3293:9: error[invalid-overload] Overloads for function `columns` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3297:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3305:9: error[invalid-overload] Overloads for function `header` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3366:9: error[invalid-overload] Overloads for function `sort` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3376:9: error[invalid-overload] Overloads for function `spacing` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3386:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3396:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:3396:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
- altair/vegalite/v6/schema/channels.py:3398:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
- altair/vegalite/v6/schema/channels.py:3682:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
+ altair/vegalite/v6/schema/channels.py:3398:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3682:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:3684:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:3688:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:3718:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:3724:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:3732:9: error[invalid-overload] Overloaded non-stub function `legend` must have an implementation
- altair/vegalite/v6/schema/channels.py:3841:9: error[invalid-overload] Overloaded non-stub function `scale` must have an implementation
- altair/vegalite/v6/schema/channels.py:3910:9: error[invalid-overload] Overloaded non-stub function `sort` must have an implementation
- altair/vegalite/v6/schema/channels.py:3923:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
+ altair/vegalite/v6/schema/channels.py:3684:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3688:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3718:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3724:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3732:9: error[invalid-overload] Overloads for function `legend` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3841:9: error[invalid-overload] Overloads for function `scale` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3910:9: error[invalid-overload] Overloads for function `sort` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3923:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:3933:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:3933:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
- altair/vegalite/v6/schema/channels.py:3935:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
+ altair/vegalite/v6/schema/channels.py:3935:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:4095:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:4112:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
+ altair/vegalite/v6/schema/channels.py:4095:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4112:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4116:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:4116:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
- altair/vegalite/v6/schema/channels.py:4118:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
- altair/vegalite/v6/schema/channels.py:4260:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:4506:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
+ altair/vegalite/v6/schema/channels.py:4118:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4260:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4506:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:4510:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:4514:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:4544:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:4550:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:4558:9: error[invalid-overload] Overloaded non-stub function `legend` must have an implementation
- altair/vegalite/v6/schema/channels.py:4667:9: error[invalid-overload] Overloaded non-stub function `scale` must have an implementation
- altair/vegalite/v6/schema/channels.py:4736:9: error[invalid-overload] Overloaded non-stub function `sort` must have an implementation
- altair/vegalite/v6/schema/channels.py:4749:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
+ altair/vegalite/v6/schema/channels.py:4510:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4514:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4544:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4550:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4558:9: error[invalid-overload] Overloads for function `legend` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4667:9: error[invalid-overload] Overloads for function `scale` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4736:9: error[invalid-overload] Overloads for function `sort` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4749:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:4759:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:4759:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:4761:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
+ altair/vegalite/v6/schema/channels.py:4761:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:4921:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:4938:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
+ altair/vegalite/v6/schema/channels.py:4921:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4938:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:4942:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:4942:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
- altair/vegalite/v6/schema/channels.py:4944:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
- altair/vegalite/v6/schema/channels.py:5085:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:5304:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
+ altair/vegalite/v6/schema/channels.py:4944:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5085:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5304:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5306:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5310:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5340:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5344:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5367:9: error[invalid-overload] Overloads for function `format` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:5306:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:5310:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:5340:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:5344:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:5367:9: error[invalid-overload] Overloaded non-stub function `format` must have an implementation
- altair/vegalite/v6/schema/channels.py:5369:9: error[invalid-overload] Overloaded non-stub function `formatType` must have an implementation
- altair/vegalite/v6/schema/channels.py:5377:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
- altair/vegalite/v6/schema/channels.py:5387:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:5369:9: error[invalid-overload] Overloads for function `formatType` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5377:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5387:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:5389:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
+ altair/vegalite/v6/schema/channels.py:5389:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:5542:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:5719:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
+ altair/vegalite/v6/schema/channels.py:5542:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5719:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:5721:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:5725:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:5742:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:5754:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
+ altair/vegalite/v6/schema/channels.py:5721:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5725:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5742:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5754:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5764:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:5764:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:5766:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5959:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:5766:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
- altair/vegalite/v6/schema/channels.py:5959:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
- altair/vegalite/v6/schema/channels.py:5963:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
+ altair/vegalite/v6/schema/channels.py:5963:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:5965:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:5969:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:5981:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
+ altair/vegalite/v6/schema/channels.py:5965:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5969:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5981:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:5991:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:5991:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
- altair/vegalite/v6/schema/channels.py:5993:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
+ altair/vegalite/v6/schema/channels.py:5993:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6127:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6127:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6129:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6129:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6131:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6131:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6247:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6247:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6251:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6251:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:6253:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:6257:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:6269:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6253:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:6257:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:6269:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
- altair/vegalite/v6/schema/channels.py:6279:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6279:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:6411:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6411:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:6413:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6413:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6415:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6415:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6617:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6617:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6621:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6621:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6623:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:6627:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:6639:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6623:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:6627:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:6639:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6649:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6649:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:6651:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6651:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
- altair/vegalite/v6/schema/channels.py:6785:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6785:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6787:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6787:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6789:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6789:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6905:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6905:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:6909:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6909:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:6911:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:6915:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:6927:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6911:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:6915:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:6927:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:6937:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:6937:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:7069:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
+ altair/vegalite/v6/schema/channels.py:7069:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:7071:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:7071:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
- altair/vegalite/v6/schema/channels.py:7073:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
- altair/vegalite/v6/schema/channels.py:7344:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
+ altair/vegalite/v6/schema/channels.py:7073:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:7344:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:7348:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:7352:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:7382:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:7388:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
- altair/vegalite/v6/schema/channels.py:7396:9: error[invalid-overload] Overloaded non-stub function `legend` must have an implementation
- altair/vegalite/v6/schema/channels.py:7505:9: error[invalid-overload] Overloaded non-stub function `scale` must have an implementation
- altair/vegalite/v6/schema/channels.py:7574:9: error[invalid-overload] Overloaded non-stub function `sort` must have an implementation
- altair/vegalite/v6/schema/channels.py:7587:9: error[invalid-overload] Overloaded non-stub function `timeUnit` must have an implementation
+ altair/vegalite/v6/schema/channels.py:7348:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:7352:9: error[invalid-overload] Overloads for function `bin` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:7382:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:7388:9: error[invalid-overload] Overloads for function `field` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:7396:9: error[invalid-overload] Overloads for function `legend` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:7505:9: error[invalid-overload] Overloads for function `scale` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:7574:9: error[invalid-overload] Overloads for function `sort` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:7587:9: error[invalid-overload] Overloads for function `timeUnit` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:7597:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:7597:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:7599:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:7599:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
- altair/vegalite/v6/schema/channels.py:7757:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:7774:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
+ altair/vegalite/v6/schema/channels.py:7757:9: error[invalid-overload] Overloads for function `bandPosition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:7774:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:7778:9: error[invalid-overload] Overloaded non-stub function `title` must have an implementation
+ altair/vegalite/v6/schema/channels.py:7778:9: error[invalid-overload] Overloads for function `title` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:7780:9: error[invalid-overload] Overloaded non-stub function `type` must have an implementation
+ altair/vegalite/v6/schema/channels.py:7780:9: error[invalid-overload] Overloads for function `type` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:7921:9: error[invalid-overload] Overloaded non-stub function `condition` must have an implementation
- altair/vegalite/v6/schema/channels.py:8098:9: error[invalid-overload] Overloaded non-stub function `aggregate` must have an implementation
+ altair/vegalite/v6/schema/channels.py:7921:9: error[invalid-overload] Overloads for function `condition` must be followed by a non-`@overload`-decorated implementation function
+ altair/vegalite/v6/schema/channels.py:8098:9: error[invalid-overload] Overloads for function `aggregate` must be followed by a non-`@overload`-decorated implementation function
- altair/vegalite/v6/schema/channels.py:8100:9: error[invalid-overload] Overloaded non-stub function `bandPosition` must have an implementation
- altair/vegalite/v6/schema/channels.py:8104:9: error[invalid-overload] Overloaded non-stub function `bin` must have an implementation
- altair/vegalite/v6/schema/channels.py:8121:9: error[invalid-overload] Overloaded non-stub function `field` must have an implementation
+ altair/vegalite/v6/schema/channels.py:8100:9: error[invalid-overload] Overload...*[Comment body truncated]*

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-07 14:46_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-10-07 14:48_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-10-07 14:56_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-07 14:56_

---

_Comment by @github-actions[bot] on 2025-10-07 15:01_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-overload` | 0 | 0 | 524 |
| `useless-overload-body` | 2 | 0 | 0 |
| **Total** | **2** | **0** | **524** |

**[Full report with detailed diff](https://alex-overload-diagnostics.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-overload-diagnostics.ecosystem-663.pages.dev/timing))


---

_Comment by @AlexWaygood on 2025-10-07 15:02_

The two added diagnostics both look like true positives!

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/overloads.md_-_Overloads_-_Invalid_-_Overload_without_an_…_-_Regular_modules_(5c8e81664d1c7470).snap`:40 on 2025-10-07 15:17_

This is also part of the concise message, and I think it it's more confusing than helpful? We could add a "will raise `TypeError` at runtime" message to a lot of lints…

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/overloads.md_-_Overloads_-_Invalid_-_Overload_without_an_…_-_Regular_modules_(5c8e81664d1c7470).snap`:45 on 2025-10-07 15:17_

:+1: 

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:979 on 2025-10-07 15:26_

Maybe
```suggestion
    ///     return x + 1  # will never be executed
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:983 on 2025-10-07 15:26_

```suggestion
    ///     return "Oh no, got a string"  # will never be executed
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:2212 on 2025-10-07 15:29_

There's probably an alternative implementation here where you would collect all remaining statements and use the full range for emitting the diagnostic, but I don't think it's worth the additional complexity.

---

_@sharkdp approved on 2025-10-07 15:29_

Very nice!

---

_@carljm reviewed on 2025-10-07 15:56_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:2209 on 2025-10-07 15:56_

Nit: "overridden" doesn't feel like the right term here. That term is usually applied to subclass overrides, not to subsequent shadowing definitions. In general I don't think it's necessarily clear to the reader what "overridden at runtime" means.

I would probably just say something like "... are solely to provide a signature for type checkers; their bodies are unused"

---

_@AlexWaygood reviewed on 2025-10-07 21:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/overloads.md_-_Overloads_-_Invalid_-_Overload_without_an_…_-_Regular_modules_(5c8e81664d1c7470).snap`:40 on 2025-10-07 21:33_

> We could add a "will raise `TypeError` at runtime" message to a lot of lints…

heh, and I like to, when it's true! I think it's useful to be clear when something is a theoretical soundness complaint that might or might not cause issues, and when something will actually, definitely, 100% of the time cause a `TypeError` to be raised (assuming our inference is correct, of course). Subclassing a `@final` class is the former category; this is the latter category! There's no way the user could possibly want this behaviour

---

_@AlexWaygood reviewed on 2025-10-07 21:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/overloads.md_-_Overloads_-_Invalid_-_Overload_without_an_…_-_Regular_modules_(5c8e81664d1c7470).snap`:40 on 2025-10-07 21:37_

Good point about it being part of the concise message, though. That's too long for a supposedly single-line diagnostic.

---

_@AlexWaygood reviewed on 2025-10-07 21:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:2212 on 2025-10-07 21:40_

Diagnostics with very long ranges can also be rendered oddly. And can interact in surprising ways with suppression comments.

---

_@AlexWaygood reviewed on 2025-10-07 21:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:2209 on 2025-10-07 21:49_

switched to "overwritten", which I think is more precise, and avoids the confusion about "overidden" usually being applied to subclass overrides

---

_Merged by @AlexWaygood on 2025-10-07 21:52_

---

_Closed by @AlexWaygood on 2025-10-07 21:52_

---

_Branch deleted on 2025-10-07 21:52_

---
