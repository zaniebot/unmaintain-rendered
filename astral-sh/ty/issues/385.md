```yaml
number: 385
title: trio.open_memory_channel Generic Type Parameters Not Recognized
type: issue
state: closed
author: rowillia
labels:
  - bug
  - calls
assignees: []
created_at: 2025-05-14T15:00:55Z
updated_at: 2025-05-14T15:14:20Z
url: https://github.com/astral-sh/ty/issues/385
synced_at: 2026-01-10T02:34:09Z
```

# trio.open_memory_channel Generic Type Parameters Not Recognized

---

_Issue opened by @rowillia on 2025-05-14 15:00_

### Summary

## Description

ty incorrectly reports that `trio.open_memory_channel` with generic type parameters is not callable. This is a standard pattern in trio for creating type-safe memory channels.

## Minimal Reproduction

Create a file `test_trio.py`:

```python
from typing import Dict, Any
import trio


async def test_memory_channel():
    # ty reports: "Object of type `GenericAlias` is not callable"
    send, receive = trio.open_memory_channel[Dict[str, Any]](100)
    
    # Same error with other types
    int_send, int_receive = trio.open_memory_channel[int](10)
    str_send, str_receive = trio.open_memory_channel[str](5)
```

Run `ty check --python /path/to/python test_trio.py`

## Expected Behavior

trio's `open_memory_channel` is designed to accept generic type parameters to specify the type of objects that can be sent through the channel (See https://github.com/python-trio/trio-typing/blob/master/trio-stubs/__init__.pyi#L388-L393)

### Version

ty 0.0.1-alpha.1 (12f466e46 2025-05-13)

---

_Comment by @sharkdp on 2025-05-14 15:14_

Thank you for reporting this. We do not yet correctly model cases where the return type of `__new__` is not an instance of the class. See #281.

---

_Closed by @sharkdp on 2025-05-14 15:14_

---

_Label `bug` added by @sharkdp on 2025-05-14 15:14_

---

_Label `calls` added by @sharkdp on 2025-05-14 15:14_

---
