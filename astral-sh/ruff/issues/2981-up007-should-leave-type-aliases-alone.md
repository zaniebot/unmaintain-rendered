```yaml
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
synced_at: 2026-01-12T15:54:43Z
```

# UP007 should leave type aliases alone

---

_@aberres_

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

_Closed by @charliermarsh on 2023-02-24 22:40_

---
