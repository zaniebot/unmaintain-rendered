```yaml
number: 9534
title: "False positive F401 \"imported but unused\" when use is cast() inside a lambda"
type: issue
state: closed
author: jriddy
labels:
  - bug
assignees: []
created_at: 2024-01-15T17:08:21Z
updated_at: 2024-01-16T13:09:32Z
url: https://github.com/astral-sh/ruff/issues/9534
synced_at: 2026-01-10T11:09:51Z
```

# False positive F401 "imported but unused" when use is cast() inside a lambda

---

_Issue opened by @jriddy on 2024-01-15 17:08_

I found a fun edgy edge case running this one my code base. Ruff cannot detect use of classes in calls to `cast()` inside a lambda with `from __future__ import annotations` on.

## Minimal reproducer

```python
from __future__ import annotations

from collections import Counter
from typing import cast


def get_most_common_fn(k):
    return lambda c: cast(Counter, c).most_common(k)
```

Running ruff results in an error:

```shell
❯ ruff check --isolated repro.py
repro.py:3:25: F401 [*] `collections.Counter` imported but unused
Found 1 error.
[*] 1 fixable with the `--fix` option.
```
## Expected behavior

I expected Ruff to detect the use of the imported class in the call to `cast()`, but it looks like the combination of `from __future__ import annotations` and having the cast call in a `lambda` expression obscures that use from Ruff.  Note that with the annotations future feature off this does not occur, nor does using a cast in non-lambda code seem to cause any issue.

## Info & versions

Ruff: 0.1.13
Python: 3.11.5
OS: Fedora 38

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `bug` added by @charliermarsh on 2024-01-15 17:09_

---

_Comment by @charliermarsh on 2024-01-15 17:09_

Thank you, very much a bug!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-15 20:28_

---

_Closed by @charliermarsh on 2024-01-16 01:08_

---

_Comment by @jriddy on 2024-01-16 13:09_

Thanks for the quick turnaround on this! It gives me a lot of confidence in the project

---
