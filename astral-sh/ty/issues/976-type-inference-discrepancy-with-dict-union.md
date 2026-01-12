```yaml
number: 976
title: "Type inference discrepancy with dict union operator: Ty returns dict[Unknown, Unknown] | Unknown while mypy correctly infers dict[str, str]"
type: issue
state: closed
author: VanyaDNDZ
labels: []
assignees: []
created_at: 2025-08-13T11:42:27Z
updated_at: 2025-08-13T11:44:51Z
url: https://github.com/astral-sh/ty/issues/976
synced_at: 2026-01-12T15:54:24Z
```

# Type inference discrepancy with dict union operator: Ty returns dict[Unknown, Unknown] | Unknown while mypy correctly infers dict[str, str]

---

_@VanyaDNDZ_

### Summary

There's a type inference discrepancy between Ty's type checker and mypy when using the dict union operator (|).
```
from typing import reveal_type
def update_headers(headers: dict[str, str]):
    update: dict[str, str] = {
        "x-wolt-web-clientid": "some_id",
        "w-wolt-session-id": "some_id",
        "Client-Version": "1.0.0",
    } 
    headers_inner1 = headers.copy() | update # Revealed type: `Unknown`
    headers_inner2 = update | headers.copy()  # Revealed type: `dict[Unknown, Unknown]` 
    reveal_type(headers_inner1)
    reveal_type(headers_inner2)

```
also reproduces on [playground](https://play.ty.dev/cb06327b-cc5e-4cfa-9faa-e48f95adb407)

Expected behavior:
In both cases revealed type should be `dict[str,str]`


### Version

ty 0.0.1-alpha.17 (1712284c8 2025-08-06)

---

_Comment by @AlexWaygood on 2025-08-13 11:44_

Thanks! Please see #543

---

_Closed by @AlexWaygood on 2025-08-13 11:44_

---
