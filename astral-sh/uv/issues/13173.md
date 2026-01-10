```yaml
number: 13173
title: "`uv run --with .` ignores version constrains from the script when installing package dependencies"
type: issue
state: closed
author: Luthaf
labels:
  - bug
assignees: []
created_at: 2025-04-28T09:37:35Z
updated_at: 2025-05-04T23:24:58Z
url: https://github.com/astral-sh/uv/issues/13173
synced_at: 2026-01-10T03:41:47Z
```

# `uv run --with .` ignores version constrains from the script when installing package dependencies

---

_Issue opened by @Luthaf on 2025-04-28 09:37_

### Summary

When a dependency appears both in the dependencies of a package and the dependencies of a script, it looks like the script dependencies are resolved first with the script constrains, and then the package dependencies are resolved, ignoring the script (transitive) version constrains.

Reproducing example:

```toml
# pyproject.toml
[project]
name = "uv-runs-with"
version = "0.0.0"
dependencies = [
    "numpy",
]
```

```py
# script.py

# /// script
# dependencies = ["numpy<2.0"]
# ///

import numpy as np

print("got np.__version__ =", np.__version__)
assert tuple(map(int, np.__version__.split("."))) < (2, 0, 0)
```

Output:

```
$ uv run --with . script.py
got np.__version__ = 2.2.5
Traceback (most recent call last):
  File "/Users/guillaume/code/tests/uv-run-with/script.py", line 10, in <module>
    assert tuple(map(int, np.__version__.split("."))) < (2, 0, 0)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
AssertionError
```

### Platform

macOS 15 arm64

### Version

uv 0.6.17 (Homebrew 2025-04-25)

### Python version

Python 3.12.8

---

_Label `bug` added by @Luthaf on 2025-04-28 09:37_

---

_Comment by @charliermarsh on 2025-05-04 12:41_

Makes sense, thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-04 15:02_

---

_Closed by @charliermarsh on 2025-05-04 23:24_

---
