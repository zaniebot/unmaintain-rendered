```yaml
number: 2042
title: Using optional Self in method signatures fails
type: issue
state: closed
author: alexmacnorthstar
labels: []
assignees: []
created_at: 2025-12-17T21:33:56Z
updated_at: 2025-12-17T21:38:43Z
url: https://github.com/astral-sh/ty/issues/2042
synced_at: 2026-01-12T15:54:26Z
```

# Using optional Self in method signatures fails

---

_@alexmacnorthstar_

### Summary

```py
from typing import Self

class Foo:
	bar: Self | None

	def __init__(self, bar: Self | None):
		self.bar = bar

x = Foo(None)
```
Gives
```
error[invalid-assignment]: Object of type `Self@__init__ | None` is not assignable to attribute `bar` of type `<special-form 'typing.Self'> | None`
 --> test.py:7:3
  |
6 |     def __init__(self, bar: Self | None):
7 |         self.bar = bar
  |         ^^^^^^^^
  |
info: rule `invalid-assignment` is enabled by default
```

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Comment by @AlexWaygood on 2025-12-17 21:38_

Thanks!

---

_Closed by @AlexWaygood on 2025-12-17 21:38_

---
