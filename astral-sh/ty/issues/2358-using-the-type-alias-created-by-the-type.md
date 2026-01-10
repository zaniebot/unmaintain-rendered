```yaml
number: 2358
title: "Using the type alias created by the type statement with `*Ts`, ty doesn't work properly, giving error"
type: issue
state: closed
author: hyperkai
labels: []
assignees: []
created_at: 2026-01-06T09:23:01Z
updated_at: 2026-01-06T09:32:41Z
url: https://github.com/astral-sh/ty/issues/2358
synced_at: 2026-01-10T01:56:41Z
```

# Using the type alias created by the type statement with `*Ts`, ty doesn't work properly, giving error

---

_Issue opened by @hyperkai on 2026-01-06 09:23_

### Summary

*Memo:
- ty check
- ty 0.0.7
- Python 3.14.0

Using the type alias created by the [type statement](https://docs.python.org/3/reference/simple_stmts.html#the-type-statement) with `*Ts`, ty doesn't work properly, giving error as shown below:

*Memo:
- mypy, pyright and pyrefly work properly, giving no error.

```python
type TA[*Ts] = tuple[*Ts]

v: TA[*tuple[int, ...]] = (0, 1, 2) # Error
```

> error[non-subscriptable]: Cannot subscript non-generic type

### Version

_No response_

---

_Comment by @AlexWaygood on 2026-01-06 09:32_

Thanks!

---

_Closed by @AlexWaygood on 2026-01-06 09:32_

---
