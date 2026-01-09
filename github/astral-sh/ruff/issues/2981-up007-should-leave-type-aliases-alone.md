---
number: 2981
title: UP007 should leave type aliases alone
type: issue
state: closed
author: aberres
labels:
  - fixes
assignees: []
created_at: 2023-02-17T12:56:25Z
updated_at: 2023-02-24T22:40:00Z
url: https://github.com/astral-sh/ruff/issues/2981
synced_at: 2026-01-07T13:12:14-06:00
---

# UP007 should leave type aliases alone

---

_Issue opened by @aberres on 2023-02-17 12:56_

Given the following snippet, `ruff` should not throw an error. At least this is the behavior of `pyupgrade`.

There are cases where one still needs an explicit `Optional`. See https://github.com/tiangolo/typer/issues/348 / https://github.com/tiangolo/typer/issues/348#issuecomment-1337460105 for an example.

### Code

```
from __future__ import annotations

from typing import Optional

StrOrNone = Optional[str]
```

### Output

```
up007.py:6:13: UP007 [*] Use `X | Y` for type annotations
Found 1 error.
```



---

_Comment by @charliermarsh on 2023-02-17 13:43_

Ah that's interesting. I think we used to _not_ change this (following pyupgrade), but I didn't see a good reason not to, and changed it. We can revert.

Note that pyupgrade _will_ fix these cases:

```py
from __future__ import annotations

from typing import List

StrOrNone = List[str]
```


---

_Label `autofix` added by @charliermarsh on 2023-02-17 13:43_

---

_Comment by @twoertwein on 2023-02-17 14:36_

might be wrong: I think this was just a limitation of mypy<1.0

---

_Comment by @aberres on 2023-02-17 15:15_

To have some more context, the use case for me is like this:

![image](https://user-images.githubusercontent.com/20811121/219692758-215a1a37-a8f4-4ee2-87d8-3512bf9767f6.png)

Without the indirection `pyupgrade` modernizes the code as expected, and `typer` fails.

---

_Comment by @charliermarsh on 2023-02-17 23:47_

Yeah that makes sense.

Is there any sense for when https://github.com/tiangolo/typer/pull/522 would be merged? 

---

_Referenced in [astral-sh/ruff#3215](../../astral-sh/ruff/issues/3215.md) on 2023-02-24 21:26_

---

_Referenced in [astral-sh/ruff#3217](../../astral-sh/ruff/pulls/3217.md) on 2023-02-24 22:36_

---

_Closed by @charliermarsh on 2023-02-24 22:40_

---

_Referenced in [astral-sh/ruff#4108](../../astral-sh/ruff/issues/4108.md) on 2023-04-26 00:48_

---

_Referenced in [astral-sh/ruff#4170](../../astral-sh/ruff/pulls/4170.md) on 2023-05-01 17:07_

---

_Referenced in [astral-sh/ruff#4843](../../astral-sh/ruff/issues/4843.md) on 2023-06-03 23:07_

---

_Referenced in [astral-sh/ruff#10290](../../astral-sh/ruff/issues/10290.md) on 2024-08-19 09:58_

---

_Referenced in [astral-sh/ruff#13783](../../astral-sh/ruff/issues/13783.md) on 2024-10-16 20:30_

---
