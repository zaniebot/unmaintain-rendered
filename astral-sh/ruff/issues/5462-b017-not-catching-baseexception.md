---
number: 5462
title: "B017: Not catching BaseException"
type: issue
state: closed
author: saippuakauppias
labels:
  - rule
assignees: []
created_at: 2023-07-02T12:00:01Z
updated_at: 2023-07-03T00:18:35Z
url: https://github.com/astral-sh/ruff/issues/5462
synced_at: 2026-01-10T01:22:44Z
---

# B017: Not catching BaseException

---

_Issue opened by @saippuakauppias on 2023-07-02 12:00_

Example:
```python
from unittest import TestCase


class TestFoo(TestCase):
    def test_exception(self):
        with self.assertRaises(Exception):
            _ = 1

    def test_baseexception(self):
        with self.assertRaises(BaseException):
            _ = 1
```

Ruff output:
```shell
➜  ruff --select=B017 linters/ruff_errors/B017.py
linters/ruff_errors/B017.py:6:9: B017 `assertRaises(Exception)` should be considered evil
Found 1 error.
```

Not caught `BaseException`, although this class is the base class for all exceptions (including `Exception`): https://docs.python.org/3/library/exceptions.html#exception-hierarchy

ruff 0.0.270

---

_Comment by @FishAlchemist on 2023-07-02 17:43_

### Although I don't know if it is correct not to catch BaseException, but flake8-bugbear seems to be not caught.
![image](https://github.com/astral-sh/ruff/assets/48265002/f1be5da0-d8a9-40ad-94f1-e2567c9676d2)


---

_Comment by @charliermarsh on 2023-07-02 20:05_

I tend to agree that it should be included... Bugbear doesn't include it, and I can't find any documentation as to why. I will put up a PR and see if the ecosystem checks say flag this anywhere.

---

_Label `rule` added by @charliermarsh on 2023-07-02 20:05_

---

_Comment by @saippuakauppias on 2023-07-02 21:18_

I don't know why it's not in other linters (maybe the developers forgot that `Exception` is not the root exception class), but I think it's very important and needed here.

Ruff should be better, smarter and more accurate than other linters. Adding `BaseException` class will make it a bit better.

---

_Referenced in [astral-sh/ruff#5466](../../astral-sh/ruff/pulls/5466.md) on 2023-07-02 23:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-02 23:28_

---

_Closed by @charliermarsh on 2023-07-03 00:18_

---
