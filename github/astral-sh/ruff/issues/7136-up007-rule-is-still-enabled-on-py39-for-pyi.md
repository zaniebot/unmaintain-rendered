---
number: 7136
title: "UP007: rule is still enabled on py39 for pyi"
type: issue
state: closed
author: adifelice-godaddy
labels:
  - question
assignees: []
created_at: 2023-09-04T21:41:24Z
updated_at: 2023-09-08T11:49:48Z
url: https://github.com/astral-sh/ruff/issues/7136
synced_at: 2026-01-07T13:12:15-06:00
---

# UP007: rule is still enabled on py39 for pyi

---

_Issue opened by @adifelice-godaddy on 2023-09-04 21:41_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
## Overview
Possibly duplicating: https://github.com/astral-sh/ruff/issues/3011

According to documentation, the rule `UP007` should not be enabled if `target-version` is earlier than `py310`, but this does not apply to `pyi` or stub files.

## Details
`test.py`
```python3
from typing import Any, Optional


class Test:
    p: Optional[Any]
```

```
$ ruff check --select=UP007 --target-version=py39 test.py
$
```

`test.pyi`
```python3
from typing import Any, Optional


class Test:
    p: Optional[Any]
```


```shell
$ ruff check --select=UP007 --target-version=py39 test.pyi
test.pyi:5:8: UP007 [*] Use `X | Y` for type annotations
Found 1 error.
[*] 1 potentially fixable with the --fix option.
$
```

---

_Comment by @charliermarsh on 2023-09-04 21:47_

Ah yeah, this is intentional, as `.pyi` files are treated as if `from __future__ import annotations` is enabled, and PEP 604-style unions are valid when used in such contexts. The other way to think of it is: `.pyi` files are only read by tools, and not at runtime, and so they can make use of language features that were added in later versions.

Here's another explanation with some references: https://github.com/astral-sh/ruff/issues/3851#issuecomment-1493203870.

---

_Closed by @charliermarsh on 2023-09-04 21:47_

---

_Label `question` added by @charliermarsh on 2023-09-04 21:47_

---

_Comment by @adifelice-godaddy on 2023-09-08 11:40_

We would prefer to align PROD and DEV Python versions. Is there a way to change this behavior for `pyi`? Thanks.

---

_Comment by @charliermarsh on 2023-09-08 11:49_

One option is to disable those rules for .pyi files via per-file-ignores. Something like:

```toml
[tool.ruff.per-file-ignores]
"*.pyi" = ["UP006", "UP007"]
```

You could also just disable those rules entirely via the ignore setting if you don’t want them to be applied anywhere anyway.

---
