---
number: 3936
title: Support flake8-pep585 for accesses from typing after import
type: issue
state: closed
author: wookie184
labels:
  - rule
assignees: []
created_at: 2023-04-11T16:58:11Z
updated_at: 2025-06-11T12:21:20Z
url: https://github.com/astral-sh/ruff/issues/3936
synced_at: 2026-01-07T13:12:14-06:00
---

# Support flake8-pep585 for accesses from typing after import

---

_Issue opened by @wookie184 on 2023-04-11 16:58_

Ruff catches all of these as `UP035` violations:
```python
from typing import (
    Container,
    Callable,
    AsyncIterable,
    Mapping,
    Coroutine,
    OrderedDict,
)
```
However, none of these trigger that rule:
```python
import typing

a: typing.Container
b: typing.Callable
c: typing.AsyncIterable
d: typing.Mapping
e: typing.Coroutine
f: typing.OrderedDict

```

This isn't something supported by pyupgrade (as the autofix would be quite complicated), but it seems like it would be worth a warning. `flake8-pep585` catches these.

On ruff playground:
https://play.ruff.rs/#N4KABGBEDOCmA2sDGAXSAuMBtSBVACpALoA04UKAhgE4DmsKAtAG6zXQCWA9gHYZQAHAJ4BmAIxjIIAL4ASeRwC2ArtRRgUQgRx60QISpk3bdAOgDCvKjrYgARka07aFyvHiU7iEEkcmXAILQQjxIAJIobJ7eACZ+zqYAspQC-iCw8WaW1FwArig2IABmmS4A8tQxbLAxACIcqPpFOYoaTrpgSipqYAAU5BCWPNY8bGQQg24eXrDjE0Eh4ZHU0bMDYMmpznODqnkFoztgFVXUNfWoZACUQA

Note that the names I listed are just examples, there are probably more pep 585 names with the same issue. 


---

_Referenced in [python-discord/bot#2523](../../python-discord/bot/pulls/2523.md) on 2023-04-11 17:30_

---

_Label `rule` added by @charliermarsh on 2023-04-11 18:15_

---

_Comment by @charliermarsh on 2023-04-11 18:18_

I agree that we should flag these. They can probably go under the existing `pyupgrade` rules (even if we can't fix them right now). We do have most of the machinery we'd need to fix these, but that can be a separate effort.

---

_Referenced in [astral-sh/ruff#5454](../../astral-sh/ruff/pulls/5454.md) on 2023-06-30 13:15_

---

_Referenced in [astral-sh/ruff#7526](../../astral-sh/ruff/issues/7526.md) on 2024-07-11 14:54_

---

_Referenced in [astral-sh/ruff#10955](../../astral-sh/ruff/issues/10955.md) on 2024-07-11 14:55_

---

_Closed by @MichaReiser on 2024-12-30 11:21_

---

_Comment by @oren0e on 2025-06-11 12:21_

I have something similar with the latest version - I used a library which has moved some modules into different sections and ruff did not catch it while my editor (vim with pyright) explicitly showed an error.

---

_Referenced in [astral-sh/ruff#21314](../../astral-sh/ruff/issues/21314.md) on 2025-11-07 14:05_

---
