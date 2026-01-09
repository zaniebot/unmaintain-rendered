---
number: 7889
title: Add --resolution=local or similar to help projects supporting wide ranges of Python versions
type: issue
state: open
author: TormodLandet
labels:
  - question
assignees: []
created_at: 2024-10-03T08:04:35Z
updated_at: 2024-10-04T12:44:38Z
url: https://github.com/astral-sh/uv/issues/7889
synced_at: 2026-01-07T13:12:17-06:00
---

# Add --resolution=local or similar to help projects supporting wide ranges of Python versions

---

_Issue opened by @TormodLandet on 2024-10-03 08:04_

I have several projects that supports a large range of versions of Python and numpy, starting from (Python=3.7, numpy=1.0) and up to (Python=3.12, numpy=2.1). I would like to test these projects at both ends of the supported version spectrum, and I would like to use uv for that.

If my project was small enough to fit inside a single script, I could use inline script metadata and the project could be run with both `--python=3.12` and `--python=3.7` arguments to `uv run myscript.py`. The resolution used by uv is somehow "local" in this case and uv will select different supported versions of numpy depending on the version of Python supplied to `uv run`.

```python
# /// script
# python-version = ">=3.7"
# dependencies = [
#   "numpy",
# ]
# ///

import numpy as np
import sys

print(sys.version)
print(np.version.version)
```

However, if the same is defined in the `pyproject.toml` file then `uv run` will try to make a universal resolution which fails since numpy version 2.1 is available for Python 3.12 and not for Python 3.7 and vice versa with numpy version 1.25.

To test my project I must add two extra steps:

1) Test on oldest version of Python with `uv run --python=3.7 ...` (run pytest or manual tests etc)
2) *Modify* `pyproject.toml` and set `requires-python = ">=3.12"`
1) Test on newest version of Python with `uv run --python=3.12 ...` 
2) *Modify* `pyproject.toml` and set `requires-python` back to `">=3.7"`

It would be great if projects that support a wide range of Python versions (so wide that universal resolution is not possible) could still use uv to test for both the oldest and newest version of Python. Right now I must modify `pyproject.toml` which feels like a hack and if I forget to reset `requires-python` back to `">=3.7"` then I guess my wheel files will refuse to install on older versions of Python? (universal wheels, should support "all" versions of Python 3 on all platforms). 

I may have missed how to turn of universal resolution in the docs, in that case sorry! It would be nice if `uv sync` also supported something like `--python=3.12 --resolution=local` to resolve only for the current version of Python on the current platform.

Thank you so much for uv, by the way. So much faster than conda for me.

---

_Comment by @konstin on 2024-10-04 12:43_

For your case, you can get different numpy versions on different Python versions by using forking on markers (see https://docs.astral.sh/uv/reference/resolver-internals/#forking). Effectively, you declare slices of Python version ranges and the numpy range you want for those Python versions, something like:

```toml
dependencies = [
	"numpy; python_version >= '3.12'",
	"numpy>=2; python_version >= '3.9' and python_version < '3.12'",
	"numpy<2; python_version < '3.9'",
    # ...
]


---

_Label `question` added by @konstin on 2024-10-04 12:44_

---
