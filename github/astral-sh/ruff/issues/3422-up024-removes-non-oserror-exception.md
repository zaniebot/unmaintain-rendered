---
number: 3422
title: "`UP024` removes non-`OSError` exception"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-03-09T19:13:33Z
updated_at: 2023-03-11T20:24:13Z
url: https://github.com/astral-sh/ruff/issues/3422
synced_at: 2026-01-07T13:12:14-06:00
---

# `UP024` removes non-`OSError` exception

---

_Issue opened by @charliermarsh on 2023-03-09 19:13_

Found a false positive one:

My code:

```python
import socket
...
from kombu import Connection, exceptions

...

try:
    conn = Connection(settings.CELERY_BROKER_URL)
    conn.ensure_connection(max_retries=2)
    conn._close()
except (socket.error, exceptions.OperationalError):
    return HttpResponseServerError("cache: cannot connect to broker.")
```

their MROs:

```python
exceptions.OperationalError.__mro__
(<class 'kombu.exceptions.OperationalError'>, <class 'kombu.exceptions.KombuError'>, <class 'Exception'>, <class 'BaseException'>, <class 'object'>)

socket.error.__mro__
(<class 'OSError'>, <class 'Exception'>, <class 'BaseException'>, <class 'object'>)
```


ruff suggests:

```python
try:
    conn = Connection(settings.CELERY_BROKER_URL)
    conn.ensure_connection(max_retries=2)
    conn._close()
except OSError:
    return HttpResponseServerError("cache: cannot connect to broker.")
```

Seems like ruff ignores the second one's MRO. Thank you!

_Originally posted by @deliro in https://github.com/charliermarsh/ruff/issues/1568#issuecomment-1462531574_


---

_Label `bug` added by @charliermarsh on 2023-03-10 05:30_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-11 19:08_

---

_Referenced in [astral-sh/ruff#3451](../../astral-sh/ruff/pulls/3451.md) on 2023-03-11 20:12_

---

_Closed by @charliermarsh on 2023-03-11 20:24_

---
