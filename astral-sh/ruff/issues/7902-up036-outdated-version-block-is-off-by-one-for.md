```yaml
number: 7902
title: UP036 (outdated-version-block) is off-by-one for the target version
type: issue
state: closed
author: oprypin
labels:
  - bug
assignees: []
created_at: 2023-10-10T18:59:29Z
updated_at: 2023-10-11T19:33:45Z
url: https://github.com/astral-sh/ruff/issues/7902
synced_at: 2026-01-12T15:54:47Z
```

# UP036 (outdated-version-block) is off-by-one for the target version

---

_@oprypin_

If my `target-version` is `py38`, I expect the condition `if sys.version_info >= (3, 8):` to be dropped but it isn't.
It gets dropped for `py37`, though.

**This is a regression introduced in #7284 while fixing #7258.**

### A minimal code snippet that reproduces the bug.

```python
import sys

if sys.version_info >= (3, 8):
    print('oh no')
```

expected:

```python
print('oh no')
```

actual: (no lint and the file doesn't get fixed)

### The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

```bash
ruff check test.py
```

### The current Ruff settings (any relevant sections from your `pyproject.toml`).

```toml
[tool.ruff]
target-version = "py38"
select = ["UP036"]
```

### The current Ruff version (`ruff --version`)

```
ruff 0.0.292
```

---

_Label `bug` added by @charliermarsh on 2023-10-10 19:00_

---

_Comment by @tdulcet on 2023-10-11 14:23_

Another related issue here is that indexing/slicing does not work:
```py
import sys

if sys.version_info[:2] < (3, 7):
    print('oh no')
```
```
ruff --target-version py37 --select UP036 --isolated test.py
```
This if block should be removed, but nothing changes. Even the [Ruff documentation](https://docs.astral.sh/ruff/rules/sys-version-slice3/) suggests using this slice syntax here, so it should work as expected.

---

_Comment by @zanieb on 2023-10-11 14:54_

Thanks for the reports!

---

_Closed by @zanieb on 2023-10-11 19:33_

---
