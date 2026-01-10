```yaml
number: 2004
title: TypeIs fails to narrow when used as a method
type: issue
state: closed
author: EnzoAgosta
labels: []
assignees: []
created_at: 2025-12-17T12:32:46Z
updated_at: 2025-12-17T12:34:19Z
url: https://github.com/astral-sh/ty/issues/2004
synced_at: 2026-01-10T01:55:00Z
```

# TypeIs fails to narrow when used as a method

---

_Issue opened by @EnzoAgosta on 2025-12-17 12:32_

### Summary

When a generic TypeIs narrowing helper is defined as an instance method it stops narrowing; the identical logic in a standalone function works correctly.

I used all defaults settings by running `uvx ty check`, my pyproject.toml has no ty related settings yet as I am evaluating changing to ty.

Minimal reproducer
```python
from typing import Any, TypeIs, reveal_type


class Foo:
  def method_narrow_type[S](self, obj: Any, expected_type: type[S]) -> TypeIs[S]:
    return isinstance(obj, expected_type)


def function_narrow_type[S](obj: Any, expected_type: type[S]) -> TypeIs[S]:
  return isinstance(obj, expected_type)


def return_any() -> Any:
  pass


unknown = return_any()
foo = Foo()

if foo.method_narrow_type(unknown, int):
  reveal_type(unknown)  # Any
if function_narrow_type(unknown, int):
  reveal_type(unknown)  # Any & int
```

I also tested it with Pyright and Mypy and both narrow the type to int correctly in both cases. Pyrefly however has the same behavior as Ty.

playground link: https://play.ty.dev/ecbc66c4-b832-4bbf-bd68-55ce0e8ca664

### Version

ty 0.0.2

---

_Closed by @AlexWaygood on 2025-12-17 12:34_

---

_Comment by @AlexWaygood on 2025-12-17 12:34_

Thanks! Please see #1569

---
