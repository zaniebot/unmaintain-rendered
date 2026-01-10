---
number: 2956
title: "PT009: `--fix` ignores custom assertion messages"
type: issue
state: closed
author: bastimeyer
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-02-16T16:09:03Z
updated_at: 2023-02-17T23:44:15Z
url: https://github.com/astral-sh/ruff/issues/2956
synced_at: 2026-01-10T01:22:41Z
---

# PT009: `--fix` ignores custom assertion messages

---

_Issue opened by @bastimeyer on 2023-02-16 16:09_

before
```py
import unittest


class TestFoo(unittest.TestCase):
    def test_foo(self):
        self.assertTrue(unittest, "The module exists")
```

lint & fix
```
$ ruff --version
ruff 0.0.247

$ ruff --select PT009 PT009.py 
PT009.py:6:9: PT009 [*] Use a regular `assert` instead of unittest-style `assertTrue`
Found 1 error.
[*] 1 potentially fixable with the --fix option.

$ ruff --select PT009 --fix PT009.py 
Found 1 error (1 fixed, 0 remaining).
```

after
```py
import unittest


class TestFoo(unittest.TestCase):
    def test_foo(self):
        assert unittest
```

expected
```py
import unittest


class TestFoo(unittest.TestCase):
    def test_foo(self):
        assert unittest, "The module exists"
```

----

Related to `PT009`, `PT027` (`self.assertRaises`) is missing in ruff from the rules defined by upstream `flake8-pytest-style`:
https://github.com/m-burst/flake8-pytest-style/blob/master/docs/rules/PT027.md

---

_Label `bug` added by @charliermarsh on 2023-02-16 16:45_

---

_Label `autofix` added by @charliermarsh on 2023-02-16 16:45_

---

_Comment by @charliermarsh on 2023-02-17 22:16_

It does the right thing if you use `self.assertTrue(unittest, msg="The module exists")`, but we should fix it to respect the positional.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-17 22:16_

---

_Referenced in [astral-sh/ruff#3002](../../astral-sh/ruff/pulls/3002.md) on 2023-02-17 23:41_

---

_Closed by @charliermarsh on 2023-02-17 23:44_

---
