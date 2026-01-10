---
number: 2359
title: "F401 triggered if there's a class property with the same name"
type: issue
state: closed
author: hofrob
labels: []
assignees: []
created_at: 2023-01-30T18:30:32Z
updated_at: 2023-01-31T12:26:23Z
url: https://github.com/astral-sh/ruff/issues/2359
synced_at: 2026-01-10T01:22:40Z
---

# F401 triggered if there's a class property with the same name

---

_Issue opened by @hofrob on 2023-01-30 18:30_

Running `ruff --select F401 foo.py` on this file will trigger rule F401.

```py
import typing

if typing.TYPE_CHECKING:
    import datetime
    import uuid


class Foo:
    first: "datetime.datetime"
    uuid: "uuid.UUID"

# result: F401 `uuid` imported but unused
```

The reason is the name of the class property is the same as that of the imported package. If I change it, the command is successful.

Versions:
* python 3.11
* ruff 0.0.237

---

_Comment by @charliermarsh on 2023-01-30 20:03_

I think this is a duplicate of #1401, but let me know if I'm wrong!

---

_Closed by @charliermarsh on 2023-01-30 20:03_

---

_Comment by @hofrob on 2023-01-31 08:45_

Yes of course. I forgot to search, sorry about that! :bow: 

---

_Comment by @charliermarsh on 2023-01-31 12:26_

No problem!

---
