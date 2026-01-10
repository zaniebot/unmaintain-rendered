```yaml
number: 704
title: ty does not respect ABC inheritance
type: issue
state: closed
author: tonka3000
labels: []
assignees: []
created_at: 2025-06-25T18:59:37Z
updated_at: 2025-06-25T19:13:31Z
url: https://github.com/astral-sh/ty/issues/704
synced_at: 2026-01-10T02:07:36Z
```

# ty does not respect ABC inheritance

---

_Issue opened by @tonka3000 on 2025-06-25 18:59_

### Summary

Currently ty seems not to respect ABC inheritance.

Example:

```py
from abc import ABC
from typing import Any, Dict


class IAppStore(ABC):
    def __init__(self):
        pass

    def write(self, data: Dict[str, Any]): ...
    def read(self) -> Dict[str, Any]: ...
```

ty report `error[invalid-return-type] <filename> Function always implicitly returns `None`, which is not assignable to return type `dict[str, Any]` on the line `def read(self) -> Dict[str, Any]: ...`.

An ABC class can not be instantiated, therefore this should not be marked as issue in my opinion because the API can only be None here. 

### Version

0.0.1-alpha.12 (f7446a6ee 2025-06-25)

---

_Comment by @tonka3000 on 2025-06-25 19:02_

Closed because `@abstractmethod` is the correct solution 😄 

---

_Closed by @tonka3000 on 2025-06-25 19:02_

---

_Comment by @AlexWaygood on 2025-06-25 19:13_

I put up https://github.com/astral-sh/ruff/pull/18942 to improve our diagnostics here :-)

---
