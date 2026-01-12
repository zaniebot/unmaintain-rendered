```yaml
number: 2224
title: "UP014 introduces E999 IndentationError: expected an indented block"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-01-26T21:07:33Z
updated_at: 2023-01-26T21:24:23Z
url: https://github.com/astral-sh/ruff/issues/2224
synced_at: 2026-01-12T15:54:42Z
```

# UP014 introduces E999 IndentationError: expected an indented block

---

_@spaceone_

```
$ cat foo.py
#!/usr/bin/python3
try:
    from typing import Any, IO, Dict, Iterator, List, Optional, Tuple, Type, NamedTuple  # noqa: F401                                                                       
    from types import TracebackType  # noqa: F401
    from argparse import Namespace  # noqa: F401
    Transaction = NamedTuple("Transaction", [("tid", int), ("dn", str), ("command", str)])                                                                                  
except ImportError:
    Transaction = namedtuple("Transaction", ["tid", "dn", "command"])  # type: ignore
$ ruff --select UP014 --fix foo.py
Found 1 error (1 fixed, 0 remaining).
$ cat foo.py
#!/usr/bin/python3
try:
    from typing import Any, IO, Dict, Iterator, List, Optional, Tuple, Type, NamedTuple  # noqa: F401
    from types import TracebackType  # noqa: F401
    from argparse import Namespace  # noqa: F401
    class Transaction(NamedTuple):
    tid: int
    dn: str
    command: str
except ImportError:
    Transaction = namedtuple("Transaction", ["tid", "dn", "command"])  # type: ignore
$ python3 foo.py
  File "foo.py", line 7
    tid: int
      ^
IndentationError: expected an indented block
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-26 21:09_

---

_Comment by @charliermarsh on 2023-01-26 21:09_

Fixing, thank you.

---

_Label `bug` added by @charliermarsh on 2023-01-26 21:12_

---

_Comment by @charliermarsh on 2023-01-26 21:24_

For now, turning these fixes off when the code is indented. Will come back with a proper fix.

---

_Closed by @charliermarsh on 2023-01-26 21:24_

---
