---
number: 21345
title: "Using `ThreadPoolExecutor_context.map()` without iterating over it"
type: issue
state: open
author: DeflateAwning
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-11-09T01:29:36Z
updated_at: 2025-11-18T16:56:51Z
url: https://github.com/astral-sh/ruff/issues/21345
synced_at: 2026-01-10T01:23:02Z
---

# Using `ThreadPoolExecutor_context.map()` without iterating over it

---

_Issue opened by @DeflateAwning on 2025-11-09 01:29_

### Summary

The following example should be a warning, because exceptions from `copy_object()` never actually get raised. This is very bad, as it allows errors to go by unnoticed. 

The correct approach is to wrap the map part in a `list()`: `list(executor.map(copy_object, all_objects))`

```python
from concurrent.futures import ThreadPoolExecutor

def main():
        with ThreadPoolExecutor(max_workers=40) as executor:
            executor.map(copy_object, all_objects)
```

---

_Label `rule` added by @ntBre on 2025-11-10 14:27_

---

_Label `needs-decision` added by @ntBre on 2025-11-10 14:27_

---

_Comment by @tmct on 2025-11-18 16:56_

We just experienced a pernicious bug that this would check have caught easily! +1

---
