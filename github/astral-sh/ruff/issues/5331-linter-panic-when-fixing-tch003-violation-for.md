---
number: 5331
title: "[Linter panic] when fixing `TCH003` violation for `typing` import"
type: issue
state: closed
author: bendoerry
labels:
  - bug
assignees: []
created_at: 2023-06-23T12:32:21Z
updated_at: 2023-12-20T16:03:03Z
url: https://github.com/astral-sh/ruff/issues/5331
synced_at: 2026-01-07T13:12:15-06:00
---

# [Linter panic] when fixing `TCH003` violation for `typing` import

---

_Issue opened by @bendoerry on 2023-06-23 12:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Ruff settings and version
Version: [`0.0.275`](https://github.com/astral-sh/ruff/releases/tag/v0.0.275)

### Settings
```toml
# ruff.toml
select = ['TCH']

[flake8-type-checking]
exempt-modules = []
```

## Description
When I try to autofix a `TCH003` violation for an import from `typing`, I get the following stack trace:
```
panicked at 'assertion failed: start.raw <= end.raw', /root/.cargo/git/checkouts/rustpython-parser-c806f105bc27c7db/f60e204/ruff_text_size/src/range.rs:48:9
Backtrace:    0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  17: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  18: <unknown>
```
I have confirmed this only occurs for imports from `typing`.

### Files
I used two files for testing, one using an import from `typing` and one from `collections.abc`:

#### `non_typing.py`
```python
# non_typing.py
from __future__ import annotations

from collections.abc import Iterable

def foo(a: Iterable[int]) -> None:
    pass
```
Running `ruff check non_typing.py --fix` works as expected

#### `typing.py`
```python
# typing.py
from __future__ import annotations

from typing import Iterable

def foo(a: Iterable[int]) -> None:
    pass
```
However if I run `ruff check typing.py --fix` I get the stacktrace above.
This also occurs for `typing` imports that aren't part of `collections.abc` as well. e.g. `Any`.

---

_Comment by @charliermarsh on 2023-06-23 12:32_

Thank you!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-23 12:48_

---

_Label `bug` added by @charliermarsh on 2023-06-23 13:00_

---

_Comment by @charliermarsh on 2023-06-23 14:02_

This might need special-casing. The problem is that we're trying to modify the `from typing` import to add `TYPE_CHECKING`, but we're also trying to remove the `Iterable` member from it.

---

_Referenced in [astral-sh/ruff#7059](../../astral-sh/ruff/issues/7059.md) on 2023-09-02 06:19_

---

_Referenced in [astral-sh/ruff#9196](../../astral-sh/ruff/issues/9196.md) on 2023-12-19 10:00_

---

_Referenced in [astral-sh/ruff#9214](../../astral-sh/ruff/pulls/9214.md) on 2023-12-20 15:49_

---

_Closed by @charliermarsh on 2023-12-20 16:03_

---
