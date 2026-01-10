---
number: 12286
title: Add rule for using PEP696 default type params
type: issue
state: closed
author: janosh
labels:
  - good first issue
  - rule
  - accepted
  - python313
assignees: []
created_at: 2024-07-11T01:15:32Z
updated_at: 2024-07-18T17:52:13Z
url: https://github.com/astral-sh/ruff/issues/12286
synced_at: 2026-01-10T01:22:52Z
---

# Add rule for using PEP696 default type params

---

_Issue opened by @janosh on 2024-07-11 01:15_

[PEP 696](https://peps.python.org/pep-0696) allows cleaning up code like

```py
from typing import Generator

def gen() -> Generator[int, None, None]:  # bad (on 3.13+)
    yield 42

def gen() -> Generator[int]:  # good
    yield 42
```

would be great if `ruff` had an auto-fix for that if the target version is at least 3.13

---

_Comment by @dhruvmanila on 2024-07-11 03:24_

Seems like a good idea. Thanks for raising this request. Do you know if there are any other types like `Generator` that has been changed? I think `AsyncGenerator` can also be considered.

---

_Label `rule` added by @dhruvmanila on 2024-07-11 03:25_

---

_Label `python313` added by @dhruvmanila on 2024-07-11 03:25_

---

_Label `good first issue` added by @charliermarsh on 2024-07-12 12:40_

---

_Label `accepted` added by @charliermarsh on 2024-07-12 12:40_

---

_Comment by @charliermarsh on 2024-07-12 12:40_

PR welcome, we can put it under the `UP` category.

---

_Referenced in [astral-sh/ruff#12371](../../astral-sh/ruff/pulls/12371.md) on 2024-07-17 17:16_

---

_Comment by @janosh on 2024-07-18 17:51_

looks like this can be closed since https://github.com/astral-sh/ruff/pull/12371 was implemented and merged in record time as usual. thanks! 👍 

---

_Closed by @janosh on 2024-07-18 17:52_

---

_Comment by @charliermarsh on 2024-07-18 17:52_

Good call, thanks @janosh.

---
