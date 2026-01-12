```yaml
number: 1256
title: Allow setting additional attributes on functions?
type: issue
state: open
author: benglewis
labels:
  - needs-decision
  - typing semantics
assignees: []
created_at: 2025-09-25T15:32:06Z
updated_at: 2025-09-25T20:07:57Z
url: https://github.com/astral-sh/ty/issues/1256
synced_at: 2026-01-12T15:54:24Z
```

# Allow setting additional attributes on functions?

---

_@benglewis_

### Summary

You should be able to extend callable instances (e.g. functions) with attributes. Currently instead I get the above error for the following code:

```python
def a():
    a.test = 1
```

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Renamed from "Unresolved attribute `test` on type `def a(func: (...) -> Unknown) -> Unknown`.tyunresolved-attribute" to "Unresolved attribute `test` on type `def a(func: (...) -> Unknown) -> Unknown`" by @AlexWaygood on 2025-09-25 15:42_

---

_Comment by @carljm on 2025-09-25 16:02_

Thanks for the report!

I don't think it would be consistent for this to be allowed. In general, statically typed Python assumes that classes have a certain known set of attributes (with known types), and only those attributes can be set (to the correct types) on instances of those classes. Functions do not generally have a `test` attribute.

All major type checkers (mypy, pyright, pyrefly) agree with us that this code is an error. (In mypy you have to add `-> None` to the function definition, otherwise the function body isn't checked at all.)

It's true that setting arbitrary attributes on functions works at runtime, but so does this, and no type checker allows this either:

```py
class A(): pass

a = A()
a.test = 1
```

---

_Closed by @carljm on 2025-09-25 16:02_

---

_Comment by @benglewis on 2025-09-25 16:16_

Weird, because in our existing type checker, basedpyright, I am sure that this is allowed. I will check with regular old pyright and come back if it also allows this behaviour

---

_Comment by @AlexWaygood on 2025-09-25 16:20_

I have wondered in the past whether we should add a `__setattr__` method to `FunctionType` in typeshed... I do agree that it's quite common (and quite idiomatic!) to monkey-patch attributes onto functions, and that the status quo makes typing many decorators quite painful

---

_Comment by @erictraut on 2025-09-25 17:17_

In pyright, this is configurable. Pyright has a specific diagnostic category for this check: `reportFunctionMemberAccess`. This check is [disabled](https://microsoft.github.io/pyright/#/configuration?id=diagnostic-settings-defaults) in "basic" type checking mode but is enabled in "standard" and "strict".

---

_Comment by @carljm on 2025-09-25 19:00_

I'll reopen this, it seems like at some point we may want to consider how we can better support this use case.

---

_Reopened by @carljm on 2025-09-25 19:00_

---

_Label `needs-decision` added by @carljm on 2025-09-25 19:00_

---

_Label `typing semantics` added by @carljm on 2025-09-25 19:01_

---

_Renamed from "Unresolved attribute `test` on type `def a(func: (...) -> Unknown) -> Unknown`" to "Allow setting additional attributes on functions?" by @carljm on 2025-09-25 20:07_

---
