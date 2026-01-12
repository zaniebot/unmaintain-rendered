```yaml
number: 1014
title: Initialization inside a loop
type: issue
state: closed
author: MamadouSDiallo
labels: []
assignees: []
created_at: 2025-08-15T19:10:22Z
updated_at: 2025-08-15T19:14:10Z
url: https://github.com/astral-sh/ty/issues/1014
synced_at: 2026-01-12T15:54:24Z
```

# Initialization inside a loop

---

_@MamadouSDiallo_

### Summary

I can initialize the variable before the loop but I expected this code to pass as is.

```py
def test():
    
    for i in range(10):
        if i==0:
            var = 1
        else:
            var = var + 1
```

```py
error[unresolved-reference]: Name `var` used when not defined
 --> example2.py:7:19
  |
5 |             var = 1
6 |         else:
7 |             var = var + 1
  |                   ^^^
  |
info: rule `unresolved-reference` is enabled by default
```

### Version

_No response_

---

_Comment by @carljm on 2025-08-15 19:14_

This should be changed from `unresolved-reference` to `possibly-unresolved-reference` (which is disabled by default) by #232 .

Note that it's kind of dangerous to allow this code to pass without error; mypy doesn't actually understand that it will definitely be defined on the first iteration of the loop, it's just trusting you to get it right. (You can see this by changing `i == 0` to `i == 999` and mypy is still fine with it.)

---

_Closed by @carljm on 2025-08-15 19:14_

---
