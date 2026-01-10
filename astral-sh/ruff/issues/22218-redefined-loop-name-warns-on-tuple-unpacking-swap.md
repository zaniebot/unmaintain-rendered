---
number: 22218
title: "`redefined-loop-name` warns on tuple unpacking swap"
type: issue
state: open
author: injust
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-12-27T00:35:17Z
updated_at: 2025-12-29T15:17:08Z
url: https://github.com/astral-sh/ruff/issues/22218
synced_at: 2026-01-10T01:23:02Z
---

# `redefined-loop-name` warns on tuple unpacking swap

---

_Issue opened by @injust on 2025-12-27 00:35_

### Summary

Using tuple unpacking to swap variables is common, but PLW2901 warns because it's technically overwriting the for-loop variables:

https://play.ruff.rs/1be1a287-f128-4840-816a-21818db565bb

```python
from itertools import pairwise

def test(nums: list[int]) -> None:
    for a, b in pairwise(nums):
        if b > a:
            a, b = b, a
        print(a, b)
```

Perhaps the rule should be suppressed when variables are being swapped, as long as they were both defined in the same loop.

---

_Comment by @ntBre on 2025-12-29 15:17_

Thanks for the report! I'm not sure this is worth a general exception to the rule, but I'm open to other opinions.

cc @amyreese 

---

_Label `rule` added by @ntBre on 2025-12-29 15:17_

---

_Label `needs-decision` added by @ntBre on 2025-12-29 15:17_

---
