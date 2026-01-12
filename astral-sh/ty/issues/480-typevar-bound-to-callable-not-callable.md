```yaml
number: 480
title: TypeVar bound to Callable not callable
type: issue
state: closed
author: danielhollas
labels:
  - bug
  - generics
assignees: []
created_at: 2025-05-21T21:08:12Z
updated_at: 2025-05-30T19:01:53Z
url: https://github.com/astral-sh/ty/issues/480
synced_at: 2026-01-12T15:54:23Z
```

# TypeVar bound to Callable not callable

---

_@danielhollas_

### Summary

```python
import typing as t

C = t.TypeVar('C', bound=t.Callable[..., t.Any])

def callable_typevar(f: C):
    return f()
```

```console
❯ uvx ty check test.py 
error[call-non-callable]: Object of type `C` is not callable
 --> test.py:6:12
  |
5 | def callable_typevar(f: C):
6 |     return f()
  |            ^^^
  |
info: rule `call-non-callable` is enabled by default

Found 1 diagnostic
```

https://play.ty.dev/7bb16762-80fd-4ca6-b8f2-e4922c210160

Perhaps related to #95?

### Version

0.0.1-alpha.6

---

_Label `generics` added by @AlexWaygood on 2025-05-21 21:37_

---

_Label `bug` added by @AlexWaygood on 2025-05-21 21:37_

---

_Renamed from "Bound typevar to Callable not callable" to "TypeVar bound to Callable not callable" by @danielhollas on 2025-05-22 00:45_

---

_Comment by @karlicoss on 2025-05-26 21:41_

I think the same issue is causing `pytest.skip()` calls not to type check 
```
error[call-non-callable]: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
 --> test_pytest_skip.py:2:1
  |
1 | import pytest
2 | pytest.skip("whatever")
  | ^^^^^^^^^^^^^^^^^^^^^^^
  |
info: rule `call-non-callable` is enabled by default
```

Here's where it's defined and one of generic variables is a `Callable` https://github.com/pytest-dev/pytest/blob/83536b4b0074ca35d90933d3ad46cb6efe7f5145/src/_pytest/outcomes.py#L83-L98

Just leaving it here so it pops up when people search for `pytest.skip` in issue tracker

---

_Comment by @AlexWaygood on 2025-05-26 21:48_

@karlicoss no, that's a subtly different issue, and something of a tricky one to fix (it's debatable whether ty is "incorrect" here -- though even if it _is_ actually "correct", we still might need to change something, since there are a lot of real-world annotations like this!). See https://github.com/astral-sh/ruff/pull/17832#issuecomment-2855425767, and also https://discuss.python.org/t/when-should-we-assume-callable-types-are-method-descriptors/92938/1

---

_Assigned to @carljm by @carljm on 2025-05-30 17:43_

---

_Closed by @carljm on 2025-05-30 19:01_

---
