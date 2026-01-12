```yaml
number: 19050
title: "[`flake8-bugbear`] `B017` does not support non-`with` calls"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - rule
assignees: []
created_at: 2025-06-30T18:25:07Z
updated_at: 2025-07-08T19:04:56Z
url: https://github.com/astral-sh/ruff/issues/19050
synced_at: 2026-01-12T15:54:56Z
```

# [`flake8-bugbear`] `B017` does not support non-`with` calls

---

_@MeGaGiGaGon_

### Summary

I found this while working on https://github.com/astral-sh/ruff/issues/18972

[assert-raises-exception (B017)](https://docs.astral.sh/ruff/rules/assert-raises-exception/#assert-raises-exception-b017) does not lint on non-`with` calls, even though both `unittest` and `pytest` support them:
https://play.ruff.rs/c875da29-d677-4f97-ad23-2efd2fa88231
```py
import unittest

def func_that_will_raise():
    raise ValueError

class Tests(unittest.TestCase):
    def test_raises(self):
        with self.assertRaises(Exception):  # B017
            func_that_will_raise()
        self.assertRaises(Exception, func_that_will_raise)  # No error


import pytest

def test_raises():
    with pytest.raises(Exception):  # B017
        func_that_will_raise()

pytest.raises(Exception, func_that_will_raise)  # No error
```
[`unittest` docs on `assertRaises`](https://docs.python.org/3/library/unittest.html#unittest.TestCase.assertRaises)
[`pytest` docs on alternate `raises` form](https://docs.pytest.org/en/stable/how-to/assert.html#alternate-pytest-raises-form-legacy)

### Version

playground

---

_Label `rule` added by @ntBre on 2025-06-30 22:25_

---

_Closed by @ntBre on 2025-07-08 19:04_

---
