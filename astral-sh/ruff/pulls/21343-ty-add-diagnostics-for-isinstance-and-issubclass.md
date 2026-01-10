```yaml
number: 21343
title: "[ty] Add diagnostics for `isinstance()` and `issubclass()` calls that use invalid PEP-604 unions for their second argument"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/isinstance-validation
created_at: 2025-11-08T20:03:35Z
updated_at: 2025-11-10T08:46:33Z
url: https://github.com/astral-sh/ruff/pull/21343
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Add diagnostics for `isinstance()` and `issubclass()` calls that use invalid PEP-604 unions for their second argument

---

_Pull request opened by @AlexWaygood on 2025-11-08 20:03_

## Summary

This PR adds extra validation for `isinstance()` and `issubclass()` calls that use `UnionType` instances for their second argument. According to typeshed's annotations, any `UnionType` is accepted for the second argument, but this isn't true at runtime: at runtime, all elements in the `UnionType` must either be class objects or be `None` in order for the `isinstance()` or `issubclass()` call to reliably succeed:

```pycon
% uvx python3.14                            
Python 3.14.0 (main, Oct 10 2025, 12:54:13) [Clang 20.1.4 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from typing import LiteralString
>>> import types
>>> type(LiteralString | int) is types.UnionType
True
>>> isinstance(42, LiteralString | int)
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    isinstance(42, LiteralString | int)
    ~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/Library/Application Support/uv/python/cpython-3.14.0-macos-aarch64-none/lib/python3.14/typing.py", line 559, in __instancecheck__
    raise TypeError(f"{self} cannot be used with isinstance()")
TypeError: typing.LiteralString cannot be used with isinstance()
```

## Test Plan

Added mdtests/snapshots


---

_Label `ty` added by @AlexWaygood on 2025-11-08 20:03_

---

_Comment by @astral-sh-bot[bot] on 2025-11-08 20:05_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-08 20:06_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Marked ready for review by @AlexWaygood on 2025-11-08 20:07_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-08 20:07_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-08 20:07_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-08 20:07_

---

_@sharkdp approved on 2025-11-10 08:44_

Thanks!

---

_Merged by @AlexWaygood on 2025-11-10 08:46_

---

_Closed by @AlexWaygood on 2025-11-10 08:46_

---

_Branch deleted on 2025-11-10 08:46_

---
