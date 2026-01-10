```yaml
number: 1293
title: "Accessing Enum value when values are repeated results in incorrect `unresolved-attribute` error"
type: issue
state: closed
author: MatthewCane
labels:
  - bug
  - attribute access
  - enums
assignees: []
created_at: 2025-10-01T10:25:23Z
updated_at: 2025-10-01T13:51:55Z
url: https://github.com/astral-sh/ty/issues/1293
synced_at: 2026-01-10T02:06:25Z
```

# Accessing Enum value when values are repeated results in incorrect `unresolved-attribute` error

---

_Issue opened by @MatthewCane on 2025-10-01 10:25_

### Summary

When an `Enum` class definition contains keys with repeated values, `ty` reports an incorrect `unresolved-attribute` error when accessing the value on the 3rd or greater key.

### Minimal Example:

```py
from enum import Enum


class example_enum(Enum):
    foo = "foo"
    bar = "foo"
    baz = "foo"
    boo = "foo"


example_enum.foo.value  # This is fine
example_enum.bar.value  # This is fine
example_enum.baz.value  # unresolved-attribute: Type `Literal[example_enum.bar]` has no attribute `value`
example_enum.boo.value  # unresolved-attribute: Type `Literal[example_enum.bar]` has no attribute `value`
```

https://play.ty.dev/e5e328e2-5b36-4fa9-842f-8a281db30d7b

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Label `bug` added by @AlexWaygood on 2025-10-01 10:27_

---

_Label `attribute access` added by @AlexWaygood on 2025-10-01 10:27_

---

_Label `enums` added by @AlexWaygood on 2025-10-01 10:27_

---

_Comment by @MatthewCane on 2025-10-01 11:14_

Note: I believe this bug was introduced with 0.0.1-alpha.21, it came to my attention by way of a Dependabot update failing in CI.

---

_Assigned to @sharkdp by @sharkdp on 2025-10-01 13:06_

---

_Closed by @sharkdp on 2025-10-01 13:51_

---
