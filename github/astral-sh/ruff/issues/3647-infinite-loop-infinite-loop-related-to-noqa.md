---
number: 3647
title: "[Infinite loop] Infinite loop related to noqa"
type: issue
state: closed
author: qarmin
labels: []
assignees: []
created_at: 2023-03-21T15:16:25Z
updated_at: 2023-03-21T15:32:33Z
url: https://github.com/astral-sh/ruff/issues/3647
synced_at: 2026-01-07T13:12:14-06:00
---

# [Infinite loop] Infinite loop related to noqa

---

_Issue opened by @qarmin on 2023-03-21 15:16_

Ruff 33394e4a693077ffe966af772fd5fec8853f786e
```
ruff . --fix
```
on
```
from io import SEEK_CUR, SEEK_END, BytesIO, open  # noqa
class KaitaiStruct:
    @classmethod
    def from_file(cls, filename):
        f = open(filename, "rb")
```
shows
```
error: Failed to converge after 100 iterations.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `Desktop/RunEveryCommand/Ruff/Broken/848376472PY_FILE_TEST_1124330.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

Desktop/RunEveryCommand/Ruff/Broken/848376472PY_FILE_TEST_1124330.py:1:51: PGH004 Use specific rule codes when using `noqa`
Desktop/RunEveryCommand/Ruff/Broken/848376472PY_FILE_TEST_1124330.py:2:7: D101 Missing docstring in public class
Desktop/RunEveryCommand/Ruff/Broken/848376472PY_FILE_TEST_1124330.py:4:9: ANN206 Missing return type annotation for classmethod `from_file`
Desktop/RunEveryCommand/Ruff/Broken/848376472PY_FILE_TEST_1124330.py:4:9: D102 Missing docstring in public method
Desktop/RunEveryCommand/Ruff/Broken/848376472PY_FILE_TEST_1124330.py:4:19: ANN102 Missing type annotation for `cls` in classmethod
Desktop/RunEveryCommand/Ruff/Broken/848376472PY_FILE_TEST_1124330.py:4:24: ANN001 Missing type annotation for function argument `filename`
Desktop/RunEveryCommand/Ruff/Broken/848376472PY_FILE_TEST_1124330.py:5:9: UP020 Use builtin `open`
Found 107 errors (100 fixed, 7 remaining).

```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-21 15:24_

---

_Referenced in [astral-sh/ruff#3650](../../astral-sh/ruff/pulls/3650.md) on 2023-03-21 15:25_

---

_Closed by @charliermarsh on 2023-03-21 15:32_

---
