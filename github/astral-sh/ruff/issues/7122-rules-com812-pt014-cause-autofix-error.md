---
number: 7122
title: Rules COM812,PT014 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-04T06:03:56Z
updated_at: 2023-10-01T08:34:47Z
url: https://github.com/astral-sh/ruff/issues/7122
synced_at: 2026-01-07T13:12:15-06:00
---

# Rules COM812,PT014 cause autofix error

---

_Issue opened by @qarmin on 2023-09-04 06:03_


Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select COM812,PT014 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
import pytest
@pytest.mark.parametrize(
    "pole", [0.8, 0.8 * np.exp(1j * np.pi / 4), 1.2, 1.2 * np.exp(1j * np.pi / 4)]
)
def test_streaming_match_full():
    N = 50
```

error
```
/home/rafal/test/tmp_folder/1832972.py:3:84: PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
/home/rafal/test/tmp_folder/1832972.py:3:89: PT014 Duplicate of test case at index 3 in `@pytest_mark.parametrize`
/home/rafal/test/tmp_folder/1832972.py:3:118: COM812 Trailing comma missing
Found 3 errors.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/1832972.py`, the rule codes COM812, PT014, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/12509776/python_compressed.zip)


---

_Label `bug` added by @MichaReiser on 2023-09-04 06:27_

---

_Label `fuzzer` added by @MichaReiser on 2023-09-04 06:27_

---

_Comment by @qarmin on 2023-10-01 08:34_

No longer cause problem

---

_Closed by @qarmin on 2023-10-01 08:34_

---
