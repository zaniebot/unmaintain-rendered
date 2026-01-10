```yaml
number: 1139
title: TypedDict used as kwargs reports
type: issue
state: closed
author: strangemonad
labels: []
assignees: []
created_at: 2025-09-06T01:35:27Z
updated_at: 2025-09-06T17:54:57Z
url: https://github.com/astral-sh/ty/issues/1139
synced_at: 2026-01-10T02:06:25Z
```

# TypedDict used as kwargs reports

---

_Issue opened by @strangemonad on 2025-09-06 01:35_

### Summary

Not sure if this is either covered by #154 or if it might be an additional valid test case for it but I'd expect the following to work

https://play.ty.dev/02d82d0d-2d4b-4ea7-8d21-bfd11df43028

```python
from typing import TypedDict

class MyService:
    def __init__(
        self,
        db_url: str,
        task_queue: str,
    ): ...

class MyServiceConfig(TypedDict, total=False):
    db_url: str
    task_queue: str

def create_my_service_explicit__works(config: MyServiceConfig):
    return MyService(
        db_url=config["db_url"],
        task_queue=config["task_queue"],
    )

def create_my_service(config: MyServiceConfig):
    return MyService(**config)

def create_my_service_splat__error(**kwargs: MyServiceConfig):
    return MyService(**kwargs)
```

### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @sharkdp on 2025-09-06 17:54_

Thank you for reporting this.

This is related to #247.

---

_Closed by @sharkdp on 2025-09-06 17:54_

---
