---
number: 6411
title: "[Autofix error] Failed to format"
type: issue
state: closed
author: gaborbernat
labels: []
assignees: []
created_at: 2023-08-08T01:42:32Z
updated_at: 2023-08-08T01:45:11Z
url: https://github.com/astral-sh/ruff/issues/6411
synced_at: 2026-01-10T01:22:45Z
---

# [Autofix error] Failed to format

---

_Issue opened by @gaborbernat on 2023-08-08 01:42_

```
from __future__ import annotations

from json import JSONDecodeError


def _as():
    try:
        pass
    except (JSONDecodeError, JSONDecodeError, TypeError):
        raise ValueError("")

```

```
❯ ruff check --fix magic.py

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `magic.py`, the rule codes B014, EM101, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

magic.py:1:1: D100 Missing docstring in public module
magic.py:6:5: ANN202 Missing return type annotation for private function `_as`
magic.py:9:12: B014 Exception handler with duplicate exception: `JSONDecodeError`
magic.py:10:9: B904 Within an `except` clause, raise exceptions with `raise ... from err` or `raise ... from None` to distinguish them from errors in exception handling
magic.py:10:9: TRY200 Use `raise from` to specify exception cause
magic.py:10:26: EM101 Exception must not use a string literal, assign to variable first
Found 6 errors.
```

```
❯ ruff --version
ruff 0.0.281
```

---

_Closed by @gaborbernat on 2023-08-08 01:45_

---
