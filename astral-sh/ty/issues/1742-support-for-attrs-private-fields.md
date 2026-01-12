```yaml
number: 1742
title: Support for attrs private fields
type: issue
state: open
author: pwuertz
labels:
  - dataclasses
  - library
assignees: []
created_at: 2025-12-03T12:38:14Z
updated_at: 2025-12-26T11:56:00Z
url: https://github.com/astral-sh/ty/issues/1742
synced_at: 2026-01-12T15:54:25Z
```

# Support for attrs private fields

---

_@pwuertz_

### Summary

In `attrs`, the generated `__init__` method takes values for private fields using their non-underscore names. Their reasoning is that the inputs to `__init__` for initializing private fields are public:

https://www.attrs.org/en/stable/init.html#private-attributes-and-aliases

> the default behavior of attrs is that if you specify a member that starts with an underscore, it will strip the underscore from the name when it creates the __init__ method signature

So this code is correct, but `ty` doesn't know that the init signature is `__init__(self, a: int)`, not `__init__(self, _a: int)`:
```python
import attrs

@attrs.define
class X:
    _a: int

# ty: error[missing-argument]: No argument provided for required parameter `_a`
# ty: error[unknown-argument]: Argument `a` does not match any known parameter
X(a=42)
```

### Version

ty 0.0.1-alpha.28

---

_Label `dataclasses` added by @sharkdp on 2025-12-03 12:53_

---

_Label `library` added by @sharkdp on 2025-12-03 12:53_

---

_Comment by @carljm on 2025-12-03 19:36_

Thanks for the report! We'll take this into consideration if or when we decide to go beyond the standardized dataclass-transform behavior and provide specialized support for some ecosystem libraries.

---

_Comment by @marchinidavide on 2025-12-26 10:40_

+1 on this one



---
