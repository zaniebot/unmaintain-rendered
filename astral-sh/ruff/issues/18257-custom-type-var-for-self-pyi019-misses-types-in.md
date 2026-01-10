```yaml
number: 18257
title: "custom-type-var-for-self (PYI019): misses types in quotes `cls: \"type[_S]\"`"
type: issue
state: closed
author: collinanderson
labels:
  - rule
assignees: []
created_at: 2025-05-22T17:13:41Z
updated_at: 2025-06-16T14:47:18Z
url: https://github.com/astral-sh/ruff/issues/18257
synced_at: 2026-01-10T11:09:58Z
```

# custom-type-var-for-self (PYI019): misses types in quotes `cls: "type[_S]"`

---

_Issue opened by @collinanderson on 2025-05-22 17:13_

### Summary

[custom-type-var-for-self (PYI019)](https://docs.astral.sh/ruff/rules/custom-type-var-for-self/) catches `cls: type[_S]` but misses `cls: "type[_S]"`

```
from typing import TypeVar

_S = TypeVar("_S", bound="Foo")

class Foo:
    @classmethod
    def bar(cls: "type[_S]", arg: int) -> _S: ...
```

### Version

ruff 0.11.10

---

_Comment by @close2code-palm on 2025-05-25 16:46_

Hello! Found the way to resolve hidden fix, working on this 😌

---

_Label `rule` added by @ntBre on 2025-05-28 12:44_

---

_Closed by @ntBre on 2025-06-16 14:47_

---
