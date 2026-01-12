```yaml
number: 1232
title: report invalid overload implementation when implementation is unannotated
type: issue
state: open
author: KotlinIsland
labels:
  - wish
  - overloads
assignees: []
created_at: 2025-09-22T02:03:32Z
updated_at: 2025-11-18T16:10:38Z
url: https://github.com/astral-sh/ty/issues/1232
synced_at: 2026-01-12T15:54:24Z
```

# report invalid overload implementation when implementation is unannotated

---

_@KotlinIsland_

### Summary

```py
from typing import overload

@overload
def f(a: int) -> int: ...

@overload
def f(a: str) -> str: ...

def f(a):
    return 1  # expect error about this implementation being invalid
```

### Version

_No response_

---

_Comment by @dhruvmanila on 2025-09-22 04:36_

Is this similar to #128 and #103 ?

---

_Comment by @KotlinIsland on 2025-09-22 05:25_

> Is this similar to #128 and #103 ?

no, seems very different. although perhaps it would depend on return type inference

i know the impl in my egg isn't annotated, but the signatures are annotated, this is the true source of truth in an overload impl

this issue is regarding that the body of the impl conforms to the signature declarations, i.e. here the impl does not conform to signature 2

---

_Label `overloads` added by @sharkdp on 2025-09-22 07:31_

---

_Renamed from "report invalid overload implmentation" to "report invalid overload implementation" by @AlexWaygood on 2025-09-22 12:32_

---

_Renamed from "report invalid overload implementation" to "report invalid overload implementation when implementation is unannotated" by @AlexWaygood on 2025-09-22 12:33_

---

_Label `wish` added by @AlexWaygood on 2025-09-22 12:33_

---

_Comment by @carljm on 2025-09-22 21:46_

Does any existing type checker implement this?

It seems quite difficult to implement in full generality.

It would be easy to tell if the return type of the implementation is compatible with no overload, but the feature requested here seems like it would at least require re-checking the implementation body once per overload.

---

_Comment by @KotlinIsland on 2025-09-22 23:35_

> it would at least require re-checking the implementation body once per overload.

something along those lines, yes

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
