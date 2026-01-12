```yaml
number: 7945
title: TCH false positive to generic runtime base classes
type: issue
state: closed
author: iurisilvio
labels:
  - bug
assignees: []
created_at: 2023-10-13T14:29:05Z
updated_at: 2023-10-13T19:44:18Z
url: https://github.com/astral-sh/ruff/issues/7945
synced_at: 2026-01-12T15:54:47Z
```

# TCH false positive to generic runtime base classes

---

_@iurisilvio_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

My branch with a broken example: https://github.com/astral-sh/ruff/compare/main...iurisilvio:test-7945

```python
from __future__ import annotations

from pathlib import Path
from collections import Sequence


class A(Sequence[int]):
    path: Path

# This works as expected
# class A(Sequence):
#     path: Path
``` 

```
[tool.ruff.flake8-type-checking]
runtime-evaluated-base-classes = ["collections.Sequence"]
```

Running master ruff with `ruff check --select TCH file.py` give me a `TCH003`:

```console
file.py:3:21: TCH003 Move standard library import `pathlib.Path` into a type-checking block
```

It is a false positive because ruff don't understand the base class is `collections.Sequence`. Removing the generics part `[int]` works.

---

_Comment by @iurisilvio on 2023-10-13 15:25_

I did some investigation but was unable to fix for now. The `semantic.resolve_call_path(...)` for the `Sequence` node returns `None` instead of `collections.Sequence` and this is probably the reason it doesn't work.

https://github.com/astral-sh/ruff/blob/8255e4ed6c6dbd2eefb14160ef88fc7753649866/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L43

---

_Label `bug` added by @charliermarsh on 2023-10-13 19:31_

---

_Comment by @charliermarsh on 2023-10-13 19:31_

Ah yup, makes sense -- thank you!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-13 19:32_

---

_Closed by @charliermarsh on 2023-10-13 19:44_

---
