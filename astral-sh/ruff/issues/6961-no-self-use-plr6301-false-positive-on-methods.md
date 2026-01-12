```yaml
number: 6961
title: "`no-self-use` (`PLR6301`) false positive on methods that call `super`"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-08-29T02:34:12Z
updated_at: 2023-10-14T18:55:40Z
url: https://github.com/astral-sh/ruff/issues/6961
synced_at: 2026-01-12T15:54:46Z
```

# `no-self-use` (`PLR6301`) false positive on methods that call `super`

---

_@DetachHead_

```py
from typing_extensions import override


class A:
    def foo(self):
        ...


class B(A):
    @override
    def foo(self):
        ...

    def bar(self):  # error: PLR6301
        super().foo()
```

in this case the method cannot be converted to a static method, otherwise it would break the `super` call

---

_Label `bug` added by @charliermarsh on 2023-08-29 03:12_

---

_Label `accepted` added by @charliermarsh on 2023-08-29 03:12_

---

_Comment by @LaBatata101 on 2023-08-29 21:25_

I can work on this one

---

_Comment by @charliermarsh on 2023-08-29 21:35_

Thanks!

---

_Assigned to @LaBatata101 by @charliermarsh on 2023-08-29 21:35_

---

_Closed by @charliermarsh on 2023-10-14 18:55_

---
