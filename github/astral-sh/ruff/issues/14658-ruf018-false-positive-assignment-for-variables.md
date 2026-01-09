---
number: 14658
title: "`RUF018` false positive. Assignment for variables only used in other asserts"
type: issue
state: closed
author: Skylion007
labels:
  - bug
assignees: []
created_at: 2024-11-28T15:52:20Z
updated_at: 2024-11-29T13:37:01Z
url: https://github.com/astral-sh/ruff/issues/14658
synced_at: 2026-01-07T13:12:16-06:00
---

# `RUF018` false positive. Assignment for variables only used in other asserts

---

_Issue opened by @Skylion007 on 2024-11-28 15:52_

`RUF018` flags any asserts which assigns values in them, which is sensible. However, there are two cases where value assignment is actually fine, and even makes the code more readable. This rule is to prevent code breakages when asserts are disabled. I ran it on the sympy code and found two use cases that make sense.

```python
    assert (t:=cancel((F, G))) == (1, P, Q)
    assert isinstance(t, tuple)
```

Here this is using a variable which is ONLY referenced in other assert expressions. There is no clean way to write this compound assert without knowing what assert failed, without storing the variable t in a temporary variable. Furthermore, the`t:=` may be expensive construct, we may only want to assign this variable when asserts are enabled.

The other less useful, but interesting case is using the assert expression in the error message of the assert. See here:
```python
     assert (g:=solve(groebner(eqs, s), dict=True)) == sol, g
```
without this, the value has to be repeated to know what `g` actually is in this context, or it needs to be assigned to a temporary variable which may be constructed when asserts are also disabled. `g` should be cast to a string, but that is a fix for another rule`

---

_Label `bug` added by @AlexWaygood on 2024-11-28 16:57_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-11-28 17:07_

---

_Referenced in [astral-sh/ruff#14661](../../astral-sh/ruff/pulls/14661.md) on 2024-11-28 19:08_

---

_Closed by @AlexWaygood on 2024-11-29 13:37_

---

_Referenced in [astral-sh/ruff#12990](../../astral-sh/ruff/issues/12990.md) on 2024-12-09 22:26_

---
