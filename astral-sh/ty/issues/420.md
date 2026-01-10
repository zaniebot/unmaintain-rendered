```yaml
number: 420
title: "[possibly-unbound-attribute] for NamedTuple"
type: issue
state: closed
author: varchasgopalaswamy
labels: []
assignees: []
created_at: 2025-05-16T07:16:41Z
updated_at: 2025-05-16T07:40:49Z
url: https://github.com/astral-sh/ty/issues/420
synced_at: 2026-01-10T02:34:09Z
```

# [possibly-unbound-attribute] for NamedTuple

---

_Issue opened by @varchasgopalaswamy on 2025-05-16 07:16_

### Summary

Asserting that a member of a namedtuple has a certain type is not reflected in subsequent usage. Not sure if this is unique to `NamedTuple` or true of other container classes as well. Works as expected with pylance.

Reproducer: 

```python
from typing import NamedTuple

class Test:
    def f(self) -> int:
        return 1

class Test2(NamedTuple):
    inst: Test | None = None

test = Test2(Test())
assert test.inst is not None
print(test.inst.f())

```

```bash 

ty check test.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
warning[possibly-unbound-attribute]: Attribute `f` on type `Test | None` is possibly unbound
  --> test.py:21:7
   |
19 | test = Test2(Test())
20 | assert test.inst is not None
21 | print(test.inst.f())
   |       ^^^^^^^^^^^
   |
info: rule `possibly-unbound-attribute` is enabled by default

```



### Version

ty 0.0.1-alpha.3 (144a26d44 2025-05-15)

---

_Closed by @sharkdp on 2025-05-16 07:40_

---

_Comment by @sharkdp on 2025-05-16 07:40_

Thank you for reporthing this. This is not supported yet, see #164

---
