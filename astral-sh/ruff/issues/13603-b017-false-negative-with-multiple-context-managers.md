```yaml
number: 13603
title: B017 false-negative with multiple context managers
type: issue
state: closed
author: samueljsb
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-10-02T11:22:35Z
updated_at: 2024-10-03T10:05:06Z
url: https://github.com/astral-sh/ruff/issues/13603
synced_at: 2026-01-12T15:54:53Z
```

# B017 false-negative with multiple context managers

---

_@samueljsb_

```console
$ cat t.py
import contextlib

import pytest

def test_():
    with pytest.raises(Exception):
        pass

    with (
        contextlib.nullcontext(),
        pytest.raises(Exception),
    ):
        pass

$ ruff check --select B017 t.py
t.py:6:10: B017 `pytest.raises(Exception)` should be considered evil
  |
5 | def test_():
6 |     with pytest.raises(Exception):
  |          ^^^^^^^^^^^^^^^^^^^^^^^^ B017
7 |         pass
  |

Found 1 error.
```

This should report both occurrences of `pytest.raises(Exception)`, but the second one is missing.

---

```console
$ python --version --version
Python 3.12.5 (v3.12.5:ff3bc82f7c9, Aug  7 2024, 05:32:06) [Clang 13.0.0 (clang-1300.0.29.30)]

$ ruff --version
ruff 0.6.8
```

---

_Label `bug` added by @AlexWaygood on 2024-10-02 14:02_

---

_Label `help wanted` added by @AlexWaygood on 2024-10-02 14:02_

---

_Closed by @dhruvmanila on 2024-10-03 10:05_

---

_Closed by @dhruvmanila on 2024-10-03 10:05_

---
