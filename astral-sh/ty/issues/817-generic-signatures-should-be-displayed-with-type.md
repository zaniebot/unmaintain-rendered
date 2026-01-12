```yaml
number: 817
title: Generic signatures should be displayed with type parameters
type: issue
state: closed
author: InSyncWithFoo
labels:
  - help wanted
  - generics
assignees: []
created_at: 2025-07-12T01:14:15Z
updated_at: 2025-08-05T23:35:10Z
url: https://github.com/astral-sh/ty/issues/817
synced_at: 2026-01-12T15:54:24Z
```

# Generic signatures should be displayed with type parameters

---

_@InSyncWithFoo_

Currently this is how a generic function is displayed:

```python
def f[T](v: T) -> T: ...

reveal_type(f)  # def f(v: T) -> T
```

It ought to be:

```python
reveal_type(f)  # def f[T](v: T) -> T
```

Same with pre-PEP-695 syntax:

```python
T = TypeVar('T')
def f(v: T) -> T: ...

reveal_type(f)  # def f[T](v: T) -> T
```

If the function is defined using PEP 695 syntax, then the order of its type parameters must be exactly the same as defined. Otherwise, ty should collect them in a first-used order and add them to the signature accordingly.

This was first discussed [in #17294](https://github.com/astral-sh/ruff/pull/17294#discussion_r2038730962) (or earlier?).

---

_Label `generics` added by @MichaReiser on 2025-07-12 16:31_

---

_Label `help wanted` added by @carljm on 2025-07-23 23:25_

---

_Added to milestone `Beta` by @carljm on 2025-07-23 23:25_

---

_Closed by @carljm on 2025-08-05 23:35_

---
