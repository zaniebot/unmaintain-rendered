```yaml
number: 1401
title: Ty accepts invalid override and abstract class instantiation (false negative)
type: issue
state: closed
author: benedekaibas
labels: []
assignees: []
created_at: 2025-10-20T03:08:19Z
updated_at: 2025-10-20T07:28:34Z
url: https://github.com/astral-sh/ty/issues/1401
synced_at: 2026-01-12T15:54:25Z
```

# Ty accepts invalid override and abstract class instantiation (false negative)

---

_@benedekaibas_

### Summary

Ty currently allows a subclass that violates standard type-checking rules.
In this example, the subclass both narrows the parameter type of an overridden method (which breaks the Liskov substitution principle) and instantiates an abstract base class without providing implementations for all of its required abstract methods.

Other type checkers like `mypy`, `zuban`, and `pyrefly` correctly detect these problems, but `Ty` reports no issues.

- Code to reproduce:

```python
from typing import ClassVar, Final
from typing import AbstractSet
from abc import ABC, abstractmethod

class MySet(ABC):
    SIZE: ClassVar[Final[int]] = 10
    @abstractmethod
    def __contains__(self, item: object) -> bool:
        ...

class Sub(MySet, AbstractSet[int]):
    def __contains__(self, item: int) -> bool:
        return item < self.SIZE

if __name__ == "__main__":
    s = Sub()          # Should be an error (abstract class)
    print(3 in s)
```

Expected output:

```text
ERROR Class member `Sub.__contains__` overrides parent class `AbstractSet` in an inconsistent manner
ERROR Class member `Sub.__contains__` overrides parent class `MySet` in an inconsistent manner
```
Ty currently outputs:

```text
All checks passed!
```

I have run the newest version of `ty` on this code. Even though I know `ty` is in preview and not ready for production use, I think this bug should be corrected to make `ty` more reliable.

### Version

ty 0.0.1-alpha.23

---

_Comment by @sharkdp on 2025-10-20 07:28_

Thank you for reporting this, we currently don't support Liskov checks yet, but will hopefully add this soon. See #166 

---

_Closed by @sharkdp on 2025-10-20 07:28_

---
