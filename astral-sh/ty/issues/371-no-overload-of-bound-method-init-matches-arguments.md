```yaml
number: 371
title: "No overload of bound method `__init__` matches arguments"
type: issue
state: closed
author: Ryang20718
labels: []
assignees: []
created_at: 2025-05-13T23:18:24Z
updated_at: 2025-05-13T23:24:06Z
url: https://github.com/astral-sh/ty/issues/371
synced_at: 2026-01-12T15:54:23Z
```

# No overload of bound method `__init__` matches arguments

---

_@Ryang20718_

### Summary

To repro this false positive error

https://play.ty.dev/5ef5a8a6-c627-4128-be1f-54939f9a19cb

```
import tempfile

with tempfile.TemporaryDirectory() as tmp:
    print("HELLO")

### Version

ty 0.0.0-alpha.8

---

_Closed by @AlexWaygood on 2025-05-13 23:24_

---
