```yaml
number: 2334
title: Another TRY201 false positive
type: issue
state: closed
author: LefterisJP
labels: []
assignees: []
created_at: 2023-01-30T00:45:32Z
updated_at: 2023-01-30T02:16:20Z
url: https://github.com/astral-sh/ruff/issues/2334
synced_at: 2026-01-12T15:54:42Z
```

# Another TRY201 false positive

---

_@LefterisJP_

This is about a TRY201 false positive and unlike https://github.com/charliermarsh/ruff/issues/2164 I think it's a bug.

Consider this
```python
a = 1
b = 2

def foo(arg) -> None:
    raise TypeError

try:
    a + b
except (ValueError, KeyError) as e:
    if isinstance(e, ValueError):
        try:
            foo()
        except TypeError:  # now raise ValueError e
            raise e from None 
```

Run it with `ruff 0.0.237` and `TRY201` enabled and you will get `test.py:14:19: TRY201 Use `raise` without specifying exception name`

This is a false positive as doing `raise` alone would raise the `TypeError`.

---

_Closed by @charliermarsh on 2023-01-30 02:16_

---
