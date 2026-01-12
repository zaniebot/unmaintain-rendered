```yaml
number: 16352
title: Respect requires-python from dependencies when running scripts with inline metadata
type: issue
state: open
author: jdumas
labels:
  - question
assignees: []
created_at: 2025-10-18T01:40:27Z
updated_at: 2025-11-01T21:27:14Z
url: https://github.com/astral-sh/uv/issues/16352
synced_at: 2026-01-12T16:02:29Z
```

# Respect requires-python from dependencies when running scripts with inline metadata

---

_@jdumas_

### Summary

Consider the following script:

```python
#!/usr/bin/env -S uv run --exact --script

# /// script
# dependencies = ["uvtest"]
# [tool.uv.sources]
# uvtest = { path = "." }
# ///

import sys

def main():
    print("Python version:", sys.version)

if __name__ == "__main__":
    main()
```

With the following `pyproject.toml`:

```toml
[project]
name = "uvtest"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13,<3.14"
dependencies = []
```

Run the script with uv:
```
> uv run --exact --script main.py
Python version: 3.14.0 (main, Oct  7 2025, 16:07:00) [Clang 20.1.4 ]
```

As we can see, if we do not provide a `requires-python` directly in the script metadata, `uv` will simply install whatever it wants. It seems to simply ignore whatever `requires-python` I have in the local `pyproject.toml` that my script depend on.

Usually I wouldn't put an upper limit, but the way I hit this issue is I was trying to bump my `pyproject.toml` from 3.13 to 3.14, and I an issue where the cached Python version used in the script venv wasn't compatible anymore with the `pyproject.toml` Python requirement:

```
  × No solution found when resolving script dependencies:
  ╰─▶ Because the current Python version (3.13) does not satisfy Python>=3.14 and uvtest==0.1.0 depends on Python>=3.14, we can conclude that uvtest==0.1.0 cannot be used.
      And because only uvtest==0.1.0 is available and you require uvtest, we can conclude that your requirements are unsatisfiable.
```

It should be easy to reproduce this by following these steps:
1. Set `# requires-python = ">=3.13,<3.14"` in the script metadata, and set `requires-python = ">=3.13"` in the `pyproject.toml`
2. Run `run --exact --script main.py`. `uv` will install and use Python 3.13.
3. Remove `# require-python = ...` from the script metadata, and bump `requires-python = ">=3.14"` in the `pyproject.toml`
4. Run `run --exact --script main.py`. `uv` will now issue the error message above.

No amount of `--refresh`, `--reinstall`, `--upgrade` was able to solve this issue for me.

### Platform

macOS (Darwin 23.5.0 arm64)

### Version

0.9.3

### Python version

N/A

---

_Label `bug` added by @jdumas on 2025-10-18 01:40_

---

_Comment by @konstin on 2025-10-20 10:09_

uv does only look at lower bounds in `requires-python`, not upper bounds, for somewhat convoluted reasons, see https://docs.astral.sh/uv/reference/internals/resolver/#requires-python for details, which is the case you're running into.

---

_Label `bug` removed by @konstin on 2025-10-20 10:09_

---

_Label `question` added by @konstin on 2025-10-20 10:09_

---

_Comment by @jdumas on 2025-10-20 14:35_

Oof. It's not great that the upper bound is ignored entirely. But this is not the source the bug here. If you follow the repro steps I outlined at the end of my message, the upper-bound is only used in step 1 to force the installation of Python 3.13. When the error message shows up in step 4, the upper-bound has been removed already in step 3.

---

_Comment by @charliermarsh on 2025-11-01 21:07_

I thought we'd attempt to re-solve here with a different Python version. I think this is intended to work.

---

_Comment by @charliermarsh on 2025-11-01 21:27_

Oh sorry, what I'm describing is only enforced for tool installation, not scripts.

---
