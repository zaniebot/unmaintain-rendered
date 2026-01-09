---
number: 7258
title: UP036 (outdated-version-block) is off-by-one for the target version
type: issue
state: closed
author: oprypin
labels:
  - bug
assignees: []
created_at: 2023-09-10T14:07:46Z
updated_at: 2023-09-11T23:02:25Z
url: https://github.com/astral-sh/ruff/issues/7258
synced_at: 2026-01-07T13:12:15-06:00
---

# UP036 (outdated-version-block) is off-by-one for the target version

---

_Issue opened by @oprypin on 2023-09-10 14:07_

If my `target-version` is `py37`, I expect the code block guarded by `sys.version_info < (3, 7)` to be dropped but it isn't.
It gets dropped for `py38`, though.

### A minimal code snippet that reproduces the bug.

```python
import sys

if sys.version_info < (3, 7):
    print('oh no')
```

### The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

```bash
ruff check test.py
```

### The current Ruff settings (any relevant sections from your `pyproject.toml`).

```toml
[tool.ruff]
target-version = "py37"
select = ["UP036"]
```

### The current Ruff version (`ruff --version`)

```
ruff 0.0.287
```

---

_Label `bug` added by @charliermarsh on 2023-09-10 16:32_

---

_Comment by @charliermarsh on 2023-09-10 16:32_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-11 22:31_

---

_Referenced in [astral-sh/ruff#7284](../../astral-sh/ruff/pulls/7284.md) on 2023-09-11 22:51_

---

_Closed by @charliermarsh on 2023-09-11 23:02_

---

_Referenced in [astral-sh/ruff#7902](../../astral-sh/ruff/issues/7902.md) on 2023-10-10 18:59_

---

_Referenced in [algorandfoundation/algokit-cli#359](../../algorandfoundation/algokit-cli/pulls/359.md) on 2023-11-22 03:33_

---
