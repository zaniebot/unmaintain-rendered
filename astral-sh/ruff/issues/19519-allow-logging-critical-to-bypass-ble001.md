```yaml
number: 19519
title: "Allow `logging.critical` to bypass `BLE001`"
type: issue
state: closed
author: clockback
labels:
  - rule
assignees: []
created_at: 2025-07-24T03:29:54Z
updated_at: 2025-07-25T21:52:59Z
url: https://github.com/astral-sh/ruff/issues/19519
synced_at: 2026-01-10T11:09:59Z
```

# Allow `logging.critical` to bypass `BLE001`

---

_Issue opened by @clockback on 2025-07-24 03:29_

### Summary

At present, the rule `BLE001`not be tripped by the following:
```python
import logging


try:
    print("Hello World!")
except Exception:
    logging.error("Could not run.", exc_info=True)
```
But it will be tripped by the following:
```python
import logging


try:
    print("Hello World!")
except Exception:
    logging.critical("Could not run.", exc_info=True)
```
Doesn't seem that there is any principled reason to allow the former but not the latter.

---

_Comment by @MichaReiser on 2025-07-24 12:07_

I think the examples here are the same?

ah no, there's `.error` and `critical`

---

_Label `rule` added by @ntBre on 2025-07-24 12:36_

---

_Closed by @ntBre on 2025-07-25 21:52_

---
