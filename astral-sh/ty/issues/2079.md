```yaml
number: 2079
title: "Infer correct `Protocol` adherence when mixing explicit args and `ParamSpec`"
type: issue
state: closed
author: oliverlambson
labels: []
assignees: []
created_at: 2025-12-18T16:46:51Z
updated_at: 2025-12-18T18:53:44Z
url: https://github.com/astral-sh/ty/issues/2079
synced_at: 2026-01-10T01:54:00Z
```

# Infer correct `Protocol` adherence when mixing explicit args and `ParamSpec`

---

_Issue opened by @oliverlambson on 2025-12-18 16:46_

### Summary

When mixing explicit args and a ParamSpec in a protocol, functions that correctly implement the protocol are reported as not adhering to it.

Minimal reproduction:

```python
"""ty doesn't infer correct protocol adherence when mixing paramspec with additional args"""

from typing import Any, Protocol


class MyFuncProto[T, **P](Protocol):
    def __call__(self, ctx: dict[str, Any], *args: P.args, **kwargs: P.kwargs) -> T: ...


def handle_func[T, **P](
    fn: MyFuncProto[T, P],
    ctx: dict[str, Any] | None = None,
    *args: P.args,
    **kwargs: P.kwargs,
) -> T:
    if ctx is None:
        ctx = {}
    return fn(ctx, *args, **kwargs)


def foo_basic(ctx: dict[str, Any]) -> None:
    pass


handle_func(foo_basic)


def foo_arg(ctx: dict[str, Any], name: str) -> None:
    pass


handle_func(foo_arg, name="test")


def foo_kwarg(ctx: dict[str, Any], name: str = "default") -> None:
    pass


handle_func(foo_kwarg)
handle_func(foo_kwarg, name="test")
```

MyPy validates this as passing, but ty produces the following errors:

```
error[invalid-argument-type]: Argument to function `handle_func` is incorrect
  --> repro.py:25:13
   |
25 | handle_func(foo_basic)
   |             ^^^^^^^^^ Expected `MyFuncProto[Unknown, (...)]`, found `def foo_basic(ctx: dict[str, Any]) -> None`
   |
info: Function defined here
  --> repro.py:10:5
   |
10 | def handle_func[T, **P](
   |     ^^^^^^^^^^^
11 |     fn: MyFuncProto[T, P],
   |     --------------------- Parameter declared here
12 |     ctx: dict[str, Any] | None = None,
13 |     *args: P.args,
   |
info: rule `invalid-argument-type` is enabled by default

error[invalid-argument-type]: Argument to function `handle_func` is incorrect
  --> repro.py:32:13
   |
32 | handle_func(foo_arg, name="test")
   |             ^^^^^^^ Expected `MyFuncProto[Unknown, (...)]`, found `def foo_arg(ctx: dict[str, Any], name: str) -> None`
   |
info: Function defined here
  --> repro.py:10:5
   |
10 | def handle_func[T, **P](
   |     ^^^^^^^^^^^
11 |     fn: MyFuncProto[T, P],
   |     --------------------- Parameter declared here
12 |     ctx: dict[str, Any] | None = None,
13 |     *args: P.args,
   |
info: rule `invalid-argument-type` is enabled by default

error[invalid-argument-type]: Argument to function `handle_func` is incorrect
  --> repro.py:39:13
   |
39 | handle_func(foo_kwarg)
   |             ^^^^^^^^^ Expected `MyFuncProto[Unknown, (...)]`, found `def foo_kwarg(ctx: dict[str, Any], name: str = "default") -> None`
40 | handle_func(foo_kwarg, name="test")
   |
info: Function defined here
  --> repro.py:10:5
   |
10 | def handle_func[T, **P](
   |     ^^^^^^^^^^^
11 |     fn: MyFuncProto[T, P],
   |     --------------------- Parameter declared here
12 |     ctx: dict[str, Any] | None = None,
13 |     *args: P.args,
   |
info: rule `invalid-argument-type` is enabled by default

error[invalid-argument-type]: Argument to function `handle_func` is incorrect
  --> repro.py:40:13
   |
39 | handle_func(foo_kwarg)
40 | handle_func(foo_kwarg, name="test")
   |             ^^^^^^^^^ Expected `MyFuncProto[Unknown, (...)]`, found `def foo_kwarg(ctx: dict[str, Any], name: str = "default") -> None`
   |
info: Function defined here
  --> repro.py:10:5
   |
10 | def handle_func[T, **P](
   |     ^^^^^^^^^^^
11 |     fn: MyFuncProto[T, P],
   |     --------------------- Parameter declared here
12 |     ctx: dict[str, Any] | None = None,
13 |     *args: P.args,
   |
info: rule `invalid-argument-type` is enabled by default

Found 4 diagnostics
```

### Version

0.0.3

---

_Comment by @AlexWaygood on 2025-12-18 17:09_

Thanks for the report! I think this is https://github.com/astral-sh/ty/issues/1714

---

_Closed by @AlexWaygood on 2025-12-18 17:09_

---

_Comment by @oliverlambson on 2025-12-18 18:53_

@AlexWaygood thanks that looks good, sorry I missed it

---
