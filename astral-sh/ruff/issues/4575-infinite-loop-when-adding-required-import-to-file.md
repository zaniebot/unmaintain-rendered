```yaml
number: 4575
title: "[Infinite loop] When adding required import to file with `# isort: off` as first line"
type: issue
state: closed
author: bendoerry
labels:
  - bug
  - isort
assignees: []
created_at: 2023-05-22T11:31:08Z
updated_at: 2023-05-22T15:49:05Z
url: https://github.com/astral-sh/ruff/issues/4575
synced_at: 2026-01-10T11:09:47Z
```

# [Infinite loop] When adding required import to file with `# isort: off` as first line

---

_Issue opened by @bendoerry on 2023-05-22 11:31_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
## Summary
If I add required imports to a python file where the very first line is `# isort: off` ruff will raise an error and add the required import multiple times. This does not happen if the `# isort: off` directive is not the first line

### Ruff Version:
[0.0.269](https://github.com/charliermarsh/ruff/releases/tag/v0.0.269)

### Ruff Config:
```toml
[isort]
required-imports = [
  "from __future__ import annotations",
]
```

### Ruff Command: 
```shell
ruff check --fix --select I002 unsorted.py
```

## `isort: off` directive on first line
### Python File: `unsorted.py`
```python
# isort: off
from json import dumps
from base64 import b64encode

# isort: on
from collections import defaultdict
from functools import wraps
```

### Ruff Output
```shell
error: Failed to converge after 100 iterations.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `unsorted.py`, the rule codes I002, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

unsorted.py:1:1: I002 Missing required import: `from __future__ import annotations`
Found 101 errors (100 fixed, 1 remaining).
```
### Modified Python File
```python
# isort: off
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from __future__ import annotations
from json import dumps
from base64 import b64encode

# isort: on
from collections import defaultdict
from functools import wraps
```

## Other comment as first line
### Python File: `unsorted.py`
```python
# No Issue
# isort: off
from json import dumps
from base64 import b64encode

# isort: on
from collections import defaultdict
from functools import wraps
```

### Ruff Output
```shell
Found 1 error (1 fixed, 0 remaining).
```

### Modified Python File
```python
# No Issue
from __future__ import annotations
# isort: off
from json import dumps
from base64 import b64encode

# isort: on
from collections import defaultdict
from functools import wraps
```
Ideally I would have an extra line between the `future` import and the `# isort: off` comment.

## Blank first line
### Python File: `unsorted.py`
```python

# isort: off
from json import dumps
from base64 import b64encode

# isort: on
from collections import defaultdict
from functools import wraps
```

### Ruff Output
```shell
Found 1 error (1 fixed, 0 remaining).
```

### Modified Python File
```python
from __future__ import annotations

# isort: off
from json import dumps
from base64 import b64encode

# isort: on
from collections import defaultdict
from functools import wraps
```

---

_Label `bug` added by @charliermarsh on 2023-05-22 13:12_

---

_Label `isort` added by @charliermarsh on 2023-05-22 13:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-22 15:09_

---

_Closed by @charliermarsh on 2023-05-22 15:49_

---
