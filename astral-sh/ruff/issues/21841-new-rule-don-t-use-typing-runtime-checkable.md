---
number: 21841
title: "new rule: don't use `typing.runtime_checkable`"
type: issue
state: open
author: KotlinIsland
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-12-08T08:03:43Z
updated_at: 2025-12-08T18:37:00Z
url: https://github.com/astral-sh/ruff/issues/21841
synced_at: 2026-01-10T01:23:02Z
---

# new rule: don't use `typing.runtime_checkable`

---

_Issue opened by @KotlinIsland on 2025-12-08 08:03_

### Summary

this feature isn't sound, and is often not what the user expects. i have seen people fall for it

```py
from typing import Protocol, runtime_checkable


@runtime_checkable
class P(Protocol):
    a: int

class A:
    a: str = "a"


def f(a: object):
    if isinstance(a, P):
        print(a.a + 1)  # WHAT!

f(A())
```

things like pydantic, beartype and optype do a better job at runtime validation than the stdlib, but people would probably assume that the stdlib works properly

---

_Comment by @ntBre on 2025-12-08 14:38_

I think for a blanket ban like this, [banned-api (TID251)](https://docs.astral.sh/ruff/rules/banned-api) might work well.

https://play.ruff.rs/49f830d7-8d90-4120-8493-20779d25b76d

---

_Label `question` added by @ntBre on 2025-12-08 14:38_

---

_Comment by @KotlinIsland on 2025-12-08 15:21_

sure, this is good. but ruff rules are about discoverability, it doesn't help the 99% of people that don't know that `runtime_checkable` doesn't work properly

---

_Label `rule` added by @ntBre on 2025-12-08 18:36_

---

_Label `needs-decision` added by @ntBre on 2025-12-08 18:36_

---

_Label `question` removed by @ntBre on 2025-12-08 18:37_

---
