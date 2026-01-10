```yaml
number: 595
title: Used when not defined
type: issue
state: closed
author: Ryang20718
labels: []
assignees: []
created_at: 2025-06-06T21:41:58Z
updated_at: 2025-06-06T21:47:42Z
url: https://github.com/astral-sh/ty/issues/595
synced_at: 2026-01-10T02:34:10Z
```

# Used when not defined

---

_Issue opened by @Ryang20718 on 2025-06-06 21:41_

### Summary

To repro
```
def test_retry_func():
    c = 0

    def inner():
        nonlocal c
        c += 1

```

### Version

0.0.1a8

---

_Closed by @AlexWaygood on 2025-06-06 21:47_

---
