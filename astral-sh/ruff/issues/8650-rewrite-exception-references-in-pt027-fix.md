```yaml
number: 8650
title: Rewrite exception references in PT027 fix
type: issue
state: closed
author: spaceone
labels:
  - bug
  - help wanted
assignees: []
created_at: 2023-11-13T09:27:15Z
updated_at: 2025-01-23T16:50:41Z
url: https://github.com/astral-sh/ruff/issues/8650
synced_at: 2026-01-12T15:54:48Z
```

# Rewrite exception references in PT027 fix

---

_@spaceone_

```python
from unittest import TestCase, main


class TestFoo(TestCase):

    def test_foo(self):
        with self.assertRaises(Exception) as exc:
            raise Exception('foo')
        assert str(exc.exception) == 'foo'


main()
```

gets converted by `ruff --fix --select PT027 --unsafe-fixes foo.py` into:
```python
from unittest import TestCase, main
import pytest


class TestFoo(TestCase):

    def test_foo(self):
        with pytest.raises(Exception) as exc:
            raise Exception('foo')
        assert str(exc.exception) == 'foo'


main()
```

causing:
```
E
======================================================================
ERROR: test_foo (__main__.TestFoo)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "foo.py", line 10, in test_foo
    assert str(exc.exception) == 'foo'
AttributeError: 'ExceptionInfo' object has no attribute 'exception'

----------------------------------------------------------------------
Ran 1 test in 0.000s

FAILED (errors=1)
```

correct would be:
```python
from unittest import TestCase, main
import pytest


class TestFoo(TestCase):

    def test_foo(self):
        with pytest.raises(Exception) as exc:
            raise Exception('foo')
        assert str(exc.value) == 'foo'


main()
```


---

_Label `bug` added by @charliermarsh on 2023-11-13 20:44_

---

_Comment by @charliermarsh on 2023-11-13 20:44_

Makes sense.

---

_Renamed from "PT027 autofix incomplete" to "Rewrite exception references in PT027 fix" by @charliermarsh on 2023-11-13 20:44_

---

_Label `help wanted` added by @zanieb on 2023-11-15 17:56_

---

_Comment by @ghost on 2023-11-27 00:53_

I'm working on a PR for the issue, just a quick question would it be helpful to include in the lint error message, the new change that's been added? So something like this,
```
test.py:6:14: PT027 [*] Use `pytest.raises` instead of unittest-style `assertRaises` and replace `exc.exception` with `exc.value`
```
Or is that too verbose? I'm guessing if we change the error message we also need to update the docs for it.

---

_Comment by @spaceone on 2023-11-27 01:16_

I don't need that addition. Probably many lint rules do a little bit more than that, e.g. adding new imports.

---

_Closed by @MichaReiser on 2025-01-23 16:50_

---
