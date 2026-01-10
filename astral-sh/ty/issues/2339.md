---
number: 2339
title: Losing type information in itertools.chain
type: issue
state: closed
author: AndBoyS
labels: []
assignees: []
created_at: 2026-01-05T11:24:06Z
updated_at: 2026-01-05T11:31:55Z
url: https://github.com/astral-sh/ty/issues/2339
synced_at: 2026-01-10T01:51:14Z
---

# Losing type information in itertools.chain

---

_Issue opened by @AndBoyS on 2026-01-05 11:24_

### Summary

When converting chain to list, generic info is lost

```python
from typing_extensions import reveal_type
from itertools import chain

x: list[list[str]] = []

reveal_type(chain(*x))  # chain[str]
for el in chain(*x):
    reveal_type(el)  # str
reveal_type(list(chain(*x)))  # list[Unknown]
```

Python version is 3.10

### Version

0.0.8

---

_Comment by @AlexWaygood on 2026-01-05 11:31_

Thanks! This is #1714

---

_Closed by @AlexWaygood on 2026-01-05 11:31_

---
