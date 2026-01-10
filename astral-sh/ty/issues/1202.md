---
number: 1202
title: "Add support for `functools.total_ordering`"
type: issue
state: closed
author: JimDabell
labels:
  - runtime semantics
assignees: []
created_at: 2025-09-18T10:06:07Z
updated_at: 2026-01-06T03:47:05Z
url: https://github.com/astral-sh/ty/issues/1202
synced_at: 2026-01-10T01:51:14Z
---

# Add support for `functools.total_ordering`

---

_Issue opened by @JimDabell on 2025-09-18 10:06_

### Summary

Classes can define `__eq__()` and one of `__lt__()`, `__le__()`, `__gt__()`, or `__ge__()`, then use the `@functools.total_ordering` decorator to fill in the rest of the comparison dunder methods.

ty does not see these implementations and incorrectly reports `unsupported-operator`.

```python
import functools


@functools.total_ordering
class Comparable:
	def __init__(self, value: int):
		self.value = value

	def __eq__(self, other) -> bool:
		return self.value == other.value

	def __lt__(self, other) -> bool:
		return self.value < other.value

	# Uncommenting this implementation satisfies ty
	# def __le__(self, other) -> bool:
	# 	return self.value <= other.value


a = Comparable(1)
b = Comparable(2)

print(a <= b)
```

This may be related to #391.

Neither Pyright nor Mypy flag this as an error.

### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Renamed from "`unsupported-operator` does not understand `functools.total_ordering()`" to "Add support for `functools.total_ordering()`" by @AlexWaygood on 2025-09-18 10:18_

---

_Comment by @AlexWaygood on 2025-09-18 10:20_

Thanks for the report!

Mypy and pyright both apply special handling for this decorator, which monkey-patches methods onto a class in a similar way to the `@dataclass` decorator. We'll need to apply special handling too in order to support the decorator.

---

_Label `runtime semantics` added by @AlexWaygood on 2025-09-18 10:20_

---

_Renamed from "Add support for `functools.total_ordering()`" to "Add support for `functools.total_ordering`" by @AlexWaygood on 2025-10-23 08:54_

---

_Added to milestone `GA` by @AlexWaygood on 2025-10-23 13:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-24 17:23_

---

_Closed by @charliermarsh on 2026-01-06 03:47_

---
