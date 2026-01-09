---
number: 6455
title: PYI055 suggestion causes TypeError at runtime
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
assignees: []
created_at: 2023-08-09T20:16:00Z
updated_at: 2023-08-11T16:12:56Z
url: https://github.com/astral-sh/ruff/issues/6455
synced_at: 2026-01-07T13:12:15-06:00
---

# PYI055 suggestion causes TypeError at runtime

---

_Issue opened by @adamtheturtle on 2023-08-09 20:16_

ruff 0.0.283 (error is new in this version)
Python 3.11.4

I get an error from ruff which proposes a solution, and applying that proposal causes an error.

I have the following file:

```
# example.py
#
# pip install requests_mock httpretty

import httpretty
import requests_mock

item: type[requests_mock.Mocker] | type[httpretty] = requests_mock.Mocker
```

* `python example.py` has no output
* `ruff --select=PYI --isolated example.py` shows:

```
example.py:42:7: PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[requests_mock.Mocker | httpretty]`.
```

The ruff documentation at https://beta.ruff.rs/docs/rules/unnecessary-type-union/ suggests that this change is purely cosmetic:

> The type built-in function accepts unions, and it is clearer to explicitly specify them as a single type.

However, if I make the change suggested, and then run `python example.py`, I get a `TypeError`:

```
Traceback (most recent call last):
  File "/Users/adam/Desktop/mypy-ruff-example/example.py", line 38, in <module>
    item: type[requests_mock.Mocker | httpretty] = requests_mock.Mocker
               ~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~
TypeError: unsupported operand type(s) for |: 'type' and 'module'
```

---

_Label `bug` added by @charliermarsh on 2023-08-09 20:21_

---

_Comment by @charliermarsh on 2023-08-09 20:21_

Thanks.

---

_Comment by @charliermarsh on 2023-08-09 20:30_

(I think you mean `PYI055`.)

---

_Renamed from "PYI030 suggestion causes TypeError at runtime" to "PYI055 suggestion causes TypeError at runtime" by @zanieb on 2023-08-09 20:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-09 20:32_

---

_Referenced in [astral-sh/ruff#6457](../../astral-sh/ruff/pulls/6457.md) on 2023-08-09 20:36_

---

_Closed by @charliermarsh on 2023-08-09 20:46_

---

_Comment by @adamtheturtle on 2023-08-11 16:12_

Thank you for fixing this @charliermarsh . As it seems you can detect this case, perhaps it would be good to have this case be a `ruff` error? (though maybe that's more of a job for `mypy` which also has no problem with it).

---
