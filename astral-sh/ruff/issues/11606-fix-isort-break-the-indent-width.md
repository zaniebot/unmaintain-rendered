---
number: 11606
title: Fix isort break the indent-width
type: issue
state: closed
author: nasyxx
labels:
  - bug
  - isort
assignees: []
created_at: 2024-05-30T02:55:50Z
updated_at: 2024-05-30T04:18:09Z
url: https://github.com/astral-sh/ruff/issues/11606
synced_at: 2026-01-10T01:22:51Z
---

# Fix isort break the indent-width

---

_Issue opened by @nasyxx on 2024-05-30 02:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

ruff.toml
```toml
indent-width = 2

[lint]
select = ["I"]
```

Original

```python
# xx.py
from math import (
  sin,
  tan,
  cos,
  nan,
  pi,
)

del sin, cos, tan, pi, nan
```

Run

`ruff check --fix xx.py`

Except sorted and 2 spaces indent imports.

```python
from math import (
  cos,
  nan,
  pi,
  sin,
  tan,
)

del sin, cos, tan, pi, nan
```

Now, there are extra 2 spaces before cos, nan, ...

```python
from math import (
    cos,
    nan,
    pi,
    sin,
    tan,
)

del sin, cos, tan, pi, nan
```


```
ruff: 0.4.5
python: 3.12.2
```

---

_Comment by @charliermarsh on 2024-05-30 03:20_

I guess I'll consider this a bug; it does work as expected if you have an indented statement in the file:

```python
# xx.py
from math import (
  sin,
  tan,
  cos,
  nan,
  pi,
)

del sin, cos, tan, pi, nan

if True:
  x
```

I'm unsure if we should respect `indent-width` vs. inferring the width from the file.


---

_Label `bug` added by @charliermarsh on 2024-05-30 03:21_

---

_Label `isort` added by @charliermarsh on 2024-05-30 03:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-30 03:27_

---

_Comment by @nasyxx on 2024-05-30 03:29_

Yes. I encountered this problem when writing a `__init__` file.

---

_Referenced in [astral-sh/ruff#11608](../../astral-sh/ruff/pulls/11608.md) on 2024-05-30 03:45_

---

_Closed by @charliermarsh on 2024-05-30 04:18_

---

_Closed by @charliermarsh on 2024-05-30 04:18_

---
