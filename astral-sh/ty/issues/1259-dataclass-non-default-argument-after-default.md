```yaml
number: 1259
title: "dataclass: Non-default argument after default argument"
type: issue
state: closed
author: rayansostenes
labels:
  - dataclasses
assignees: []
created_at: 2025-09-26T07:19:38Z
updated_at: 2025-09-26T07:22:15Z
url: https://github.com/astral-sh/ty/issues/1259
synced_at: 2026-01-12T15:54:24Z
```

# dataclass: Non-default argument after default argument

---

_@rayansostenes_

### Summary

```python
from dataclasses import dataclass


@dataclass
class A:
    a: str | None = None
    b: str
    # The line above should produce and error, it will fail at runtime with:
    # >>> TypeError: non-default argument 'b' follows default argument
```
> https://play.ty.dev/5b8a6931-5da1-4d14-9db9-1b7a7bf5791b






### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Comment by @sharkdp on 2025-09-26 07:22_

Thank you for reporting this. We already track this as a sub-item in #111.

---

_Closed by @sharkdp on 2025-09-26 07:22_

---

_Label `dataclasses` added by @sharkdp on 2025-09-26 07:22_

---
