```yaml
number: 11023
title: "`invalid-length-return-type` (`PLE0303`) - false positive when `__len__` raises an exception"
type: issue
state: closed
author: DetachHead
labels:
  - bug
assignees: []
created_at: 2024-04-19T03:28:41Z
updated_at: 2024-04-19T03:39:25Z
url: https://github.com/astral-sh/ruff/issues/11023
synced_at: 2026-01-10T11:09:53Z
```

# `invalid-length-return-type` (`PLE0303`) - false positive when `__len__` raises an exception

---

_Issue opened by @DetachHead on 2024-04-19 03:28_

```py
class Foo:
    def __len__(): # error: `__len__` does not return a non-negative integer
        raise Exception
```
[playground](https://play.ruff.rs/ebb68cde-b0fe-4b8f-92dd-c9843cd80da4)

# use case

i have a subclass of a type from a 3rd party module that uses `__len__` in a very misleading way, so i want to ban people from using it on my subclass:
```py
from typing import override, Never

class Bar(Foo):
    @override
    def __len__() -> Never:
        raise Exception("__len__ is banned on this class")
```

---

_Comment by @charliermarsh on 2024-04-19 03:39_

I believe this is fixed on `main`.

---

_Comment by @charliermarsh on 2024-04-19 03:39_

Via #11017. Sorry about that.

---

_Closed by @charliermarsh on 2024-04-19 03:39_

---

_Label `bug` added by @charliermarsh on 2024-04-19 03:39_

---
