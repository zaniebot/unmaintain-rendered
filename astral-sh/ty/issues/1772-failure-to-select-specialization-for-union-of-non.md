```yaml
number: 1772
title: Failure to select specialization for union of non-final generic classes
type: issue
state: closed
author: sharkdp
labels:
  - generics
assignees: []
created_at: 2025-12-05T13:37:47Z
updated_at: 2025-12-09T15:23:02Z
url: https://github.com/astral-sh/ty/issues/1772
synced_at: 2026-01-12T15:54:25Z
```

# Failure to select specialization for union of non-final generic classes

---

_@sharkdp_

We currently fail to select a useful specialization for the call to `extract_t` here:
```py
class P[T]:
    x: T

class Q[T]:
    x: T

def extract_t[T](x: P[T] | Q[T]) -> T:
    raise NotImplementedError

reveal_type(extract_t(P[int]()))  # revealed: Unknown
```
https://play.ty.dev/f21605e1-a6d1-4b08-9f8c-c608fe0cbc22

Note that there is some ambiguity here, because of multiple inheritance. But we should still be able to solve for `T`. Note that it works fine if the classes are `final`: https://play.ty.dev/a3821fe3-ace6-402d-b257-b74f1fd74238

---

_Label `generics` added by @sharkdp on 2025-12-05 13:37_

---

_Comment by @carljm on 2025-12-05 16:06_

This is similar to the case in https://github.com/astral-sh/ty/issues/1703, except in that case there are two different typevars in branches of the union, rather than the same typevar. It seems likely (but not certain) that both issues might have the same fix.

---

_Added to milestone `Beta` by @carljm on 2025-12-05 16:06_

---

_Assigned to @sharkdp by @sharkdp on 2025-12-05 16:24_

---

_Renamed from "Failure to select specialization for union on non-final generic classes" to "Failure to select specialization for union of non-final generic classes" by @sharkdp on 2025-12-08 12:46_

---

_Closed by @sharkdp on 2025-12-09 15:23_

---
