```yaml
number: 7620
title: PTH123 suggest to use pathlib.Path.open() which does not implement opener
type: issue
state: closed
author: M5oul
labels:
  - bug
assignees: []
created_at: 2023-09-23T18:09:33Z
updated_at: 2023-09-26T09:07:37Z
url: https://github.com/astral-sh/ruff/issues/7620
synced_at: 2026-01-10T11:09:49Z
```

# PTH123 suggest to use pathlib.Path.open() which does not implement opener

---

_Issue opened by @M5oul on 2023-09-23 18:09_

### Issue
In case an `opener` is used into `open()` function, for creating a file with certain permission for instance, ruff `PTH123` suggests to use `pathlib.Path.open()` which does not implement the `opener`.

```py
import os
dir_fd = os.open('somedir', os.O_RDONLY)
def opener(path, flags):
    return os.open(path, flags, dir_fd=dir_fd)

with open('spamspam.txt', 'w', opener=opener) as f:
    print('This will be written to somedir/spamspam.txt', file=f)
``` 

- ruff version range: v0.0.276 - v0.0.291

### Reference
- [ruff PTH123](https://docs.astral.sh/ruff/rules/builtin-open/)
- [python `open()`](https://docs.python.org/3/library/functions.html#open)
- [python `pathlib.Path.open()`](https://docs.python.org/3/library/pathlib.html#pathlib.Path.open)

### Solution
- Ruff can stop reporting it in case an `opener` is defined
- Using `# noqa: PTH123`
- …

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `bug` added by @konstin on 2023-09-25 08:28_

---

_Closed by @konstin on 2023-09-26 09:07_

---
