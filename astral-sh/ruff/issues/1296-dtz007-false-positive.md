```yaml
number: 1296
title: DTZ007 False positive
type: issue
state: closed
author: notypecheck
labels:
  - bug
assignees: []
created_at: 2022-12-20T06:16:01Z
updated_at: 2022-12-20T18:26:55Z
url: https://github.com/astral-sh/ruff/issues/1296
synced_at: 2026-01-12T15:54:41Z
```

# DTZ007 False positive

---

_@notypecheck_

Ruff Version: 0.0.188

Ruff correctly detects an error in this code and suggests adding `.replace`:
```py
from datetime import datetime


def parse_datetime(date: str) -> datetime:
    date_format = "%d/%m/%Y %H:%M"
    return datetime.strptime(date, date_format)
```
```
test.py:6:12: DTZ007 The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)`
```

However ruff reports the same error after `.replace` is added:
```py
from datetime import datetime, timezone


def parse_datetime(date: str) -> datetime:
    date_format = "%d/%m/%Y %H:%M"
    return datetime.strptime(date, date_format).replace(tzinfo=timezone.utc)
```
```
test.py:6:12: DTZ007 The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)`
```

---

_Comment by @charliermarsh on 2022-12-20 16:07_

Thank you! Will fix today.

---

_Label `bug` added by @charliermarsh on 2022-12-20 16:07_

---

_Closed by @charliermarsh on 2022-12-20 18:20_

---

_Comment by @charliermarsh on 2022-12-20 18:26_

Going out in [v0.0.189](https://github.com/charliermarsh/ruff/releases/tag/v0.0.189).

---
