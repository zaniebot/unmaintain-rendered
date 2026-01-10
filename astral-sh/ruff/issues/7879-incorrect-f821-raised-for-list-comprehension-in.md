---
number: 7879
title: "Incorrect F821 raised for list comprehension in `Annotated`"
type: issue
state: closed
author: Harry-Lees
labels:
  - bug
assignees: []
created_at: 2023-10-09T21:31:36Z
updated_at: 2023-10-10T03:51:01Z
url: https://github.com/astral-sh/ruff/issues/7879
synced_at: 2026-01-10T01:22:47Z
---

# Incorrect F821 raised for list comprehension in `Annotated`

---

_Issue opened by @Harry-Lees on 2023-10-09 21:31_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


Using Ruff `0.0.292` on the following code snippet

```python
from typing import Annotated

foo = [1, 2, 3, 4, 5]

class Bar:
    baz: Annotated[
        str,
        [qux for qux in foo],
    ]
```

The following error is raised:

```
test.py:8:10: F821 Undefined name `qux`
Found 1 error.
```

The minimal reproducable example looks a little bit weird, but I came across this when writing a [pydantic validator](https://docs.pydantic.dev/latest/concepts/validators/#annotated-validators) on some (IMO) reasonable pydantic code. A more sensible example can be seen below.

```python
from typing import Annotated

from pydantic import BaseModel
from pydantic.functional_validators import BeforeValidator

foo = [1, 2, 3, 4, 5]

class Bar(BaseModel):
    baz: Annotated[
        list[int], BeforeValidator(lambda values: [value**2 for value in values])
    ]

qux = Bar(baz=foo)
```


```
test.py:10:52: F821 Undefined name `value`
Found 1 error.
```

---

_Comment by @charliermarsh on 2023-10-09 23:29_

Thanks!

---

_Label `bug` added by @charliermarsh on 2023-10-09 23:29_

---

_Comment by @charliermarsh on 2023-10-09 23:29_

The cause here isn't immediately obvious to me, but definitely looks like a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-10 03:34_

---

_Comment by @charliermarsh on 2023-10-10 03:34_

I see the error here.

---

_Referenced in [astral-sh/ruff#7885](../../astral-sh/ruff/pulls/7885.md) on 2023-10-10 03:42_

---

_Closed by @charliermarsh on 2023-10-10 03:51_

---
