```yaml
number: 2456
title: "PT009 changes `assertFalse()` into `assert … is False`"
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-02-01T22:10:36Z
updated_at: 2023-02-02T16:24:44Z
url: https://github.com/astral-sh/ruff/issues/2456
synced_at: 2026-01-12T15:54:42Z
```

# PT009 changes `assertFalse()` into `assert … is False`

---

_@spaceone_

```sh
$ cat foo.py
import unittest


class TestFoo(unittest.TestCase):

    def test_foo(self):
        self.assertFalse(None)


unittest.main()
$ ruff --select PT009 --fix foo.py
Found 1 error (1 fixed, 0 remaining).
$ cat foo.py
import unittest


class TestFoo(unittest.TestCase):

    def test_foo(self):
        assert None is False


unittest.main()
$ python3 foo.py
F
======================================================================
FAIL: test_foo (__main__.TestFoo)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "foo.py", line 7, in test_foo
    assert None is False
AssertionError

----------------------------------------------------------------------
Ran 1 test in 0.000s

FAILED (failures=1)
```

---

_Comment by @charliermarsh on 2023-02-01 23:10_

Is this incorrect? :)

---

_Comment by @edgarrmondragon on 2023-02-01 23:20_

I guess `assertFalse` is really checking for _falsy_ values. Maybe the right fix should be

```diff
-         self.assertFalse(None)
+         not None
```

---

_Comment by @spaceone on 2023-02-02 10:07_

https://github.com/python/cpython/blob/main/Lib/unittest/case.py#L705-L709
I'll do a PR.

---

_Comment by @charliermarsh on 2023-02-02 16:24_

Fixed by #2476.

---

_Closed by @charliermarsh on 2023-02-02 16:24_

---
