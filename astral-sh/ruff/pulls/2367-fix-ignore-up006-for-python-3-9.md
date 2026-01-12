```yaml
number: 2367
title: "fix: ignore UP006 for Python < 3.9"
type: pull_request
state: closed
author: spaceone
labels:
  - question
assignees: []
base: main
head: up006-py3.9
created_at: 2023-01-30T21:51:41Z
updated_at: 2023-01-30T22:49:33Z
url: https://github.com/astral-sh/ruff/pull/2367
synced_at: 2026-01-12T04:52:00Z
```

# fix: ignore UP006 for Python < 3.9

---

_Pull request opened by @spaceone on 2023-01-30 21:51_

UP006 transforms `typing.Dict[str, str]` into `dict[str, str]` which is a `TypeError: 'type' object is not subscriptable` in Python < 3.9: https://peps.python.org/pep-0585/

The test should ignore it if the target version doesn't match.

---

_Comment by @burgholzer on 2023-01-30 22:00_

The transformation is perfectly fine if
```python
from __future__ import annotations
```
is present. See https://peps.python.org/pep-0563/ which works with Python 3.7+.

Edit: I should have been clearer. It works as a type annotation with Python 3.7+

---

_Comment by @spaceone on 2023-01-30 22:11_

> The transformation is perfectly fine if
> 
> ```python
> from __future__ import annotations
> ```
> 
> is present. See https://peps.python.org/pep-0563/ which works with Python 3.7+.

no:
```bash
$ cat foo.py
from __future__ import annotations
dict[str, str]
$ python3 --version
Python 3.7.3
$ python3 foo.py
Traceback (most recent call last):
  File "foo.py", line 2, in <module>
    dict[str, str]
TypeError: 'type' object is not subscriptable
```


---

_Comment by @charliermarsh on 2023-01-30 22:18_

We already guard against this in `src/checkers/ast.rs`, at the call-site for this function! Can you include a code snippet that's causing an error, along with your `pyproject.toml` and the command you ran?

---

_Comment by @charliermarsh on 2023-01-30 22:20_

Like, if you have this file:

```py
import typing

typing.Dict[str, str]
```

Ruff doesn't change this if you have:

```toml
[tool.ruff]
target-version = "py37"
```

We don't change it if you have `from __future__ import annotations` either.

---

_Label `question` added by @charliermarsh on 2023-01-30 22:20_

---

_Comment by @spaceone on 2023-01-30 22:49_

hm, ok. function annotations are stored as strings:

```
$ cat foo.py
#!/usr/bin/python3

from __future__ import annotations
from typing import List

def bar() -> List[foo]:
    return []

$ ruff --isolated --target-version py37 --select UP006 --fix - < foo.py
#!/usr/bin/python3

from __future__ import annotations
from typing import List

def bar() -> list[foo]:
    return []
```

---

_Closed by @spaceone on 2023-01-30 22:49_

---
