```yaml
number: 508
title: "Add subdiagnostic hint if async context manager is used in non-async `with` statement"
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-05-25T11:47:07Z
updated_at: 2025-05-26T19:34:48Z
url: https://github.com/astral-sh/ty/issues/508
synced_at: 2026-01-10T02:34:10Z
```

# Add subdiagnostic hint if async context manager is used in non-async `with` statement

---

_Issue opened by @AlexWaygood on 2025-05-25 11:47_

CPython just added a really nice hint to its diagnostic message if you use an async context manager in a non-async `with` statement:

```pycon
Python 3.14.0a7+ (heads/main:fac41f56d4b, May 25 2025, 12:27:36) [Clang 17.0.0 (clang-1700.0.13.3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> class Foo:
...     async def __aenter__(self): ...
...     async def __aexit__(self, *args): ...
...     
>>> with Foo():
...     ...
...     
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    with Foo():
         ~~~^^
TypeError: 'Foo' object does not support the context manager protocol (missed __exit__ method) but it supports the asynchronous context manager protocol. Did you mean to use 'async with'?
```

We should add a subdiagnostic to our equivalent error message that suggests using `async with` instead of `with`, so that it looks something like:

```
error: Object of type `Foo` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
note: Objects of type `Foo` *can* be used as async context managers
note: Consider using `async with` here
```

(Here's a playground link to show our current error message: https://play.ty.dev/00fecd74-416e-46d1-8368-a2b87cbb84cd.)

---

_Label `help wanted` added by @AlexWaygood on 2025-05-25 11:47_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-25 11:47_

---

_Comment by @AlexWaygood on 2025-05-25 12:11_

@lipefree -- this should be fairly easy to do, if you're still looking for easy issues :-)

- Here's an example of an existing diagnostic where we add a subdiagnostic giving additional information: https://github.com/astral-sh/ruff/blob/83a036960b8e7d31856a5f10d208b9eac894a187/crates/ty_python_semantic/src/types/infer.rs#L5751-L5755
- The area of the code you'd need to edit is here: https://github.com/astral-sh/ruff/blob/83a036960b8e7d31856a5f10d208b9eac894a187/crates/ty_python_semantic/src/types.rs#L6099-L6104
- We'd only want to add the subdiagnostic if `context_expression.try_call_dunder(db, "__aenter__", CallArgumentTypes::none())` and `context_expression.try_call_dunder(db, "__aexit__", CallArgumentTypes::positional([Type::none(db), Type::none(db), Type::none(db)]))` both return `Ok()` variants

---

_Comment by @lipefree on 2025-05-25 17:15_

Hey @AlexWaygood ! I will try to look into it tonight then and see if I can come up with something !

---

_Comment by @AlexWaygood on 2025-05-25 18:21_

(Tests should be added using [mdtest snapshots](https://github.com/astral-sh/ruff/tree/main/crates/ty_test#diagnostic-snapshotting) to https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/with/sync.md)

---

_Closed by @sharkdp on 2025-05-26 19:34_

---
