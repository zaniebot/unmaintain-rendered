```yaml
number: 1330
title: "`FBT003 Boolean positional value in function call` triggered in unittest assert methods"
type: issue
state: closed
author: actionless
labels:
  - bug
assignees: []
created_at: 2022-12-22T08:08:55Z
updated_at: 2022-12-22T13:40:23Z
url: https://github.com/astral-sh/ruff/issues/1330
synced_at: 2026-01-12T15:54:41Z
```

# `FBT003 Boolean positional value in function call` triggered in unittest assert methods

---

_@actionless_

for example, 

```python
from unittest import TestCase


def do_stuff() -> bool:
    return True


class TestStuff(TestCase):

    def test_stuff_works(self) -> None:
        self.assertEqual(do_stuff(), True)
```

```console
$ ruff test_ruff_FBT003.py
Found 1 error(s).
test_ruff_FBT003.py:11:38: FBT003 Boolean positional value in function call
```

---

_Label `bug` added by @charliermarsh on 2022-12-22 13:27_

---

_Comment by @charliermarsh on 2022-12-22 13:27_

Will add this to the allowlist.

---

_Closed by @charliermarsh on 2022-12-22 13:40_

---
