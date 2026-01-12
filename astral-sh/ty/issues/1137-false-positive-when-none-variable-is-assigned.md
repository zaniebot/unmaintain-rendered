```yaml
number: 1137
title: False positive when None variable is assigned - possibly-unbound-attribute
type: issue
state: closed
author: JoanPuig
labels: []
assignees: []
created_at: 2025-09-05T19:40:09Z
updated_at: 2025-09-07T14:14:41Z
url: https://github.com/astral-sh/ty/issues/1137
synced_at: 2026-01-12T15:54:24Z
```

# False positive when None variable is assigned - possibly-unbound-attribute

---

_@JoanPuig_

### Summary

Given this code:

```
def my_function(new_value, my_set: set | None = None):
    if my_set is None:
        my_set = set()

    def my_add(v):
        my_set.add(v)

    my_add(new_value)
```

I think for all cases by the time `my_add` is called, `my_set` is not None, bur I get this report instead:

```
warning[possibly-unbound-attribute]: Attribute `add` on type `set[Unknown] | None` is possibly unbound
 --> src\main.py:6:9
  |
5 |     def my_add(v):
6 |         my_set.add(v)
  |         ^^^^^^^^^^
7 |
8 |     my_add(new_value)
  |
info: rule `possibly-unbound-attribute` is enabled by default
```

### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @carljm on 2025-09-05 21:30_

Thanks for the report! This is a duplicate of #559 and will be fixed by https://github.com/astral-sh/ruff/pull/19932

---

_Closed by @carljm on 2025-09-05 21:30_

---

_Comment by @JoanPuig on 2025-09-07 11:32_

Thanks @carljm, sorry I was not able to identify the duplicate.

---

_Comment by @carljm on 2025-09-07 14:14_

No worries at all!

---
