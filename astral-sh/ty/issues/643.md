```yaml
number: 643
title: Associated literal should narrow type (tagged unions)
type: issue
state: closed
author: aslpavel
labels:
  - narrowing
assignees: []
created_at: 2025-06-12T14:18:52Z
updated_at: 2025-11-13T07:25:41Z
url: https://github.com/astral-sh/ty/issues/643
synced_at: 2026-01-10T02:06:24Z
```

# Associated literal should narrow type (tagged unions)

---

_Issue opened by @aslpavel on 2025-06-12 14:18_

### Summary

When you have union with literal attribute it should be possible to narrow based on this attribute
[playground](https://play.ty.dev/8f4c1419-a0d9-4e68-848e-df8b5054ebd2)
```python
# tagged.py
from dataclasses import dataclass
from typing import Literal, assert_type


@dataclass
class A:
    tag: Literal["A"]
    value: int


@dataclass
class B:
    tag: Literal["B"]
    value: str


type AB = A | B


def test(ab: AB) -> None:
    if ab.tag == "A":
        assert_type(ab.value, int)
```

```
$ ty check tagged.py 
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[type-assertion-failure]: Argument does not have asserted type `int`
  --> tagged.py:22:9
   |
20 | def test(ab: AB) -> None:
21 |     if ab.tag == "A":
22 |         assert_type(ab.value, int)
   |         ^^^^^^^^^^^^--------^^^^^^
   |                     |
   |                     Inferred type of argument is `int | str`
   |
info: `int` and `int | str` are not equivalent types
info: rule `type-assertion-failure` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1-alpha.9

---

_Label `narrowing` added by @carljm on 2025-06-12 14:47_

---

_Closed by @carljm on 2025-11-13 07:25_

---
