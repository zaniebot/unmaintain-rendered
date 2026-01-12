```yaml
number: 2114
title: "`contextmanager` decorator has inconsistencies when combined with `staticmethod`"
type: issue
state: closed
author: mflova
labels: []
assignees: []
created_at: 2025-12-19T16:26:43Z
updated_at: 2025-12-24T03:35:11Z
url: https://github.com/astral-sh/ty/issues/2114
synced_at: 2026-01-12T15:54:26Z
```

# `contextmanager` decorator has inconsistencies when combined with `staticmethod`

---

_@mflova_

### Summary

More specifically, it does not work only when the context manager is called within the instance itself. As you can see from the snippet below, same calls are valid or not valid depending on where the context manager is called from

```py
from collections.abc import Iterator
from contextlib import contextmanager


class A:
    @staticmethod
    @contextmanager
    def my_context(num: int) -> Iterator[None]:
        yield

    def method(self) -> None:
        with self.my_context(10):  # Too many positional arguments: expected 0, got 1
            pass

with A.my_context(5):  # OK
    ...

with A().my_context(5):  # Too many positional arguments: expected 0, got 1
    ...
```

### Version

0.0.4

---

_Comment by @carljm on 2025-12-20 17:02_

Thanks for the report!

---

_Added to milestone `Stable` by @carljm on 2025-12-20 17:03_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-21 14:45_

---

_Closed by @charliermarsh on 2025-12-24 03:35_

---
