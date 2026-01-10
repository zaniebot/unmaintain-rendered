```yaml
number: 662
title: "`TypeVar` bounds do not work with `__class__`"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - generics
assignees: []
created_at: 2025-06-16T18:23:00Z
updated_at: 2025-11-28T08:20:26Z
url: https://github.com/astral-sh/ty/issues/662
synced_at: 2026-01-10T01:58:59Z
```

# `TypeVar` bounds do not work with `__class__`

---

_Issue opened by @MeGaGiGaGon on 2025-06-16 18:23_

### Summary

This code does not type check [playground](https://play.ty.dev/143c2d06-491d-486f-abdf-77776d2f0215)
```py
from typing import TypeVar

_B = TypeVar("_B", bound="Bound")
class Bound:
    def copy(self: _B) -> _B:
        new = self.__class__()
        reveal_type(new)
        return new
```
`ty` gives
```
Revealed type: `Bound` (revealed-type) [Ln 7, Col 21]
Return type does not match returned value: expected `_B`, found `Bound` (invalid-return-type) [Ln 8, Col 16]
```
While `basedpyright` gives no errors and `Type of "new" is "_B@copy"` [playground](https://basedpyright.com/?typeCheckingMode=all&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAqiApgGoCGIAsAFC0D6AQlALyEkUgAUAREzwBooAIzABXFABMWPRuKk8AlLQDGAG3IBnTVDkTJALlpQTUScWBQVYBHC6bia4AahNFUALQA%2BV4yM1TQKgUYgB3VigHJwA6enp1LU04rmUAoJMQYgA3YnI1engEYi4Q0NT0jOIYMRAUYLCgA)
And `mypy` gives this [playground](https://mypy-play.net/?mypy=latest&python=3.13&gist=9f6fd8bbadc87087b1b44ad9b2cdf988)
```
main.py:7: note: Revealed type is "_B`-1"
Success: no issues found in 1 source file
```

The same error happens if you use `type` instead of `__class__`

### Version

ty 0.0.1-alpha.10 (15bae14c2 2025-06-13) + playground

---

_Label `generics` added by @carljm on 2025-06-17 01:49_

---

_Comment by @carljm on 2025-06-17 01:50_

This requires that we support `type[]` of a TypeVar, I think.

---

_Added to milestone `Stable` by @carljm on 2025-11-14 15:06_

---

_Closed by @sharkdp on 2025-11-28 08:20_

---
