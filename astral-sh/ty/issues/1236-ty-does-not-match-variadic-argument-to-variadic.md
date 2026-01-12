```yaml
number: 1236
title: ty does not match variadic argument to variadic parameter
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - calls
assignees: []
created_at: 2025-09-22T06:43:16Z
updated_at: 2025-09-25T07:51:58Z
url: https://github.com/astral-sh/ty/issues/1236
synced_at: 2026-01-12T15:54:24Z
```

# ty does not match variadic argument to variadic parameter

---

_@dhruvmanila_

### Summary

In the following example:
```py
def f(x: int, *args: str) -> int:
    return 1


def _(x: list[int]):
    reveal_type(f(*x))
```

The above code should raise a type error stating that `int` cannot be assigned to `str` for the variadic parameter but ty doesn't do that.

This is because ty doesn't try to match variadic argument to the variadic parameter in certain scenarios like the above. This is due to missing logic in `ArgumentMatcher::match_variadic`.

### Version

_No response_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-09-22 06:43_

---

_Label `bug` added by @dhruvmanila on 2025-09-22 06:43_

---

_Label `calls` added by @dhruvmanila on 2025-09-22 06:43_

---

_Closed by @dhruvmanila on 2025-09-25 07:51_

---
