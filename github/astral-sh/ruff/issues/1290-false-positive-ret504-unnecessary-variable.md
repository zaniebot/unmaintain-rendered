---
number: 1290
title: "False-positive RET504 \"Unnecessary variable assignment before `return` statement\""
type: issue
state: closed
author: actionless
labels:
  - bug
assignees: []
created_at: 2022-12-19T12:57:51Z
updated_at: 2022-12-20T18:27:00Z
url: https://github.com/astral-sh/ruff/issues/1290
synced_at: 2026-01-07T13:12:14-06:00
---

# False-positive RET504 "Unnecessary variable assignment before `return` statement"

---

_Issue opened by @actionless on 2022-12-19 12:57_

ruff 0.0.187

```python
import sys
from time import time


def func() -> float:
    result = time()
    sys.stdout.write('log smth\n')
    return result
```

```console
$ ruff test_ruff_ret_504.py
Found 1 error(s).
test_ruff_ret_504.py:8:12: RET504 Unnecessary variable assignment before `return` statement
```

---

_Renamed from "False-positive RET504" to "False-positive RET504 "Unnecessary variable assignment before `return` statement"" by @actionless on 2022-12-19 12:59_

---

_Comment by @squiddy on 2022-12-19 15:08_

See also https://github.com/charliermarsh/ruff/issues/1233, which was closed recently.

> I'm torn on this! Almost any statement can be side-effectful -- even something like a = b + c could actually involve operator overloading under-the-hood and thus make use of bar. So, I guess the suggestion is that we make this check much more conservative, and only flag when there are no expressions between the assignment and the return?

Which is not to say that this report/request is invalid, the question on how to deal with that sensibly is still open.

---

_Comment by @actionless on 2022-12-19 15:50_

i think it's reasonable to assume that any statement is side-effectful - at least because it takes some time to execute:

``` python
def func():
    time_before = time()
    result = long_operation()
    sys.stdout.write(f"took {time() - time_before} seconds\n")
    return result
 ```

---

_Comment by @charliermarsh on 2022-12-19 17:12_

Alright, maybe we just change this to only allow obviously non-effectful statements (like `pass`) between assignment and return.

---

_Label `bug` added by @charliermarsh on 2022-12-19 21:41_

---

_Referenced in [astral-sh/ruff#1294](../../astral-sh/ruff/pulls/1294.md) on 2022-12-20 00:47_

---

_Closed by @charliermarsh on 2022-12-20 00:48_

---

_Comment by @charliermarsh on 2022-12-20 18:27_

Going out in [v0.0.189](https://github.com/charliermarsh/ruff/releases/tag/v0.0.189).

---
