---
number: 9230
title: "F821 Undefined name `item` when assigning a set comprehension in an if-clause"
type: issue
state: closed
author: kurellajunior
labels:
  - bug
assignees: []
created_at: 2023-12-21T12:10:07Z
updated_at: 2023-12-22T18:06:41Z
url: https://github.com/astral-sh/ruff/issues/9230
synced_at: 2026-01-07T13:12:15-06:00
---

# F821 Undefined name `item` when assigning a set comprehension in an if-clause

---

_Issue opened by @kurellajunior on 2023-12-21 12:10_

```python item.py
import logging
from itertools import groupby


class MonocausalTransformer:
    for key, tuples in groupby([("a",1),("a", 2),("b",3)], lambda t: t[0]):
        if len(replacements := {item[1] for item in tuples}) > 1:
            logging.error("Multiple replacements for %s: %s", key, replacements)

```
When running `ruff . --fix` on the whole project it gives this error message: ```F821 Undefined name `item` ```
But it works just as expected when executed
And extracting the set to a separate variable a line above works also fine

```
$ ruff --version
ruff 0.1.7
$ ruff --fix item.py 
item.py:7:33: F821 Undefined name `item`
Found 1 error.
```
```toml
[tool.ruff]
target-version = "py311"
line-length = 120
select = ["ALL"]
ignore = [
    "A003",
    "ARG002",  # unused method arguments, e.g. when implementing an interface
    "D10",  # TODO: Add doc messages to public api, or make private
    "D203",  # Conflicting with D211
    "D213",  # Conflicting with D212
    "TD002", "TD003", "FIX002",  # TODO: Fix all todo comments
    "PLC1901",  # We have to distinguish between None and ""
    "RUF012",  # Ignored already for pydantic models, but we use our own base class
    "S311",  # No cryptography happening here
]
```
https://play.ruff.rs/27d1104c-b86e-45dc-98c6-fc6eac195bcb


---

_Label `bug` added by @charliermarsh on 2023-12-21 19:49_

---

_Comment by @charliermarsh on 2023-12-21 19:49_

Thanks! Does look like a bug.

---

_Comment by @charliermarsh on 2023-12-21 21:08_

Is this the complete snippet? I'm unable to reproduce in the playground: https://play.ruff.rs/dd91b36b-c177-4e84-b190-368de569f6a4

---

_Label `question` added by @charliermarsh on 2023-12-21 21:08_

---

_Comment by @kurellajunior on 2023-12-21 22:57_

> Is this the complete snippet? I'm unable to reproduce in the playground: [play.ruff.rs/dd91b36b-c177-4e84-b190-368de569f6a4](https://play.ruff.rs/dd91b36b-c177-4e84-b190-368de569f6a4)

You are right, I extracted only a small snippet, thought it was enough. I extracted now a larger snippet, stripped it down to the actual minimum: https://play.ruff.rs/27d1104c-b86e-45dc-98c6-fc6eac195bcb
This now gives on my console, in my poetry environment with ruff 0.1.7 the documented error:
```bash
$ ruff --fix item.py 
item.py:26:33: F821 Undefined name `item`
Found 1 error.
```

As soon as I remove the class context, the error is gone

Also, when I put everything inside `__init__()` the error is gone

Also updated the original post

---

_Comment by @charliermarsh on 2023-12-21 23:06_

Thanks! Assume this has to do with accessing from within a class scope. We will fix.

---

_Label `question` removed by @charliermarsh on 2023-12-21 23:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-22 15:50_

---

_Referenced in [astral-sh/ruff#9248](../../astral-sh/ruff/pulls/9248.md) on 2023-12-22 17:59_

---

_Closed by @charliermarsh on 2023-12-22 18:06_

---
