```yaml
number: 705
title: The return type should not be detected as None
type: issue
state: closed
author: tonka3000
labels: []
assignees: []
created_at: 2025-06-25T19:12:53Z
updated_at: 2025-06-25T19:14:15Z
url: https://github.com/astral-sh/ty/issues/705
synced_at: 2026-01-10T02:07:36Z
```

# The return type should not be detected as None

---

_Issue opened by @tonka3000 on 2025-06-25 19:12_

### Summary

In the below scenario ty report that the return type is `Version|None` but the code can only be `Version`.
```py
class Version:
    def __init__(self):
        self.foo = 1


_ver: Version | None = None


def get_version() -> Version:
    global _ver
    if _ver is not None:
        return _ver # <=== ty error here
    _ver = Version()
    return _ver
```

ty report **error[invalid-return-type] <filename>: Return type does not match returned value: expected `Version`, found `Version | None`**


### Version

0.0.1-alpha.12 (f7446a6ee 2025-06-25)

---

_Comment by @AlexWaygood on 2025-06-25 19:14_

Thanks! This is #311

---

_Closed by @AlexWaygood on 2025-06-25 19:14_

---
