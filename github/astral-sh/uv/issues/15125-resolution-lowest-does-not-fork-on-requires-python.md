---
number: 15125
title: "`--resolution=lowest` does not fork on requires-python"
type: issue
state: open
author: ego-thales
labels:
  - bug
assignees: []
created_at: 2025-08-07T08:18:14Z
updated_at: 2025-12-18T20:41:41Z
url: https://github.com/astral-sh/uv/issues/15125
synced_at: 2026-01-07T13:12:19-06:00
---

# `--resolution=lowest` does not fork on requires-python

---

_Issue opened by @ego-thales on 2025-08-07 08:18_

### Summary

Hi everyone,

### The problem

When using `--resolution=lowest`, it seems that `uv` does not try to find "the lowest compatible version", it simply takes the lower bound I wrote in `pyproject.toml` and uses it blindly. It leads to failure when using an incompatible python version:
```console
$ uv run --resolution=lowest -p313 pytest
Using CPython 3.13.3
Removed virtual environment at: .venv
Creating virtual environment at: .venv
error: Distribution `torch==2.3.0 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're using CPython 3.13 (`cp313`), but `torch` (v2.3.0) only has wheels with the following Python ABI tag: `cp312`
```

### Expected behaviour

I would expect `uv` to figure out by itself the lowest compatible version with my python versions.

### Relevant context

```toml
# pyproject.toml
requires-python = ">=3.12, <3.14"
dependencies = [
    "torch>=2.3",
    # Adjust indirect dependencies lower bound
    "filelock>=3.13", # torch<=2.7.1
    "fsspec>=2023.10", # torch<=2.7.1
    "networkx>=3.2", # torch<=2.7.1
    "nvidia-nvjitlink-cu12>=12.1", # torch > nvidia-cusolver-cu12 > nvidia-cusparse-cu12
    "sympy>=1.9", # torch<=2.7.1
    "typing-extensions>=4.12", # torch<=2.7.1
]
```

I'm not sure if this is a bug or if I missed the expected behaviour of `uv`. In the latter case, could you please help me understand what I should do?

Thanks in advance!
Élie

### Platform

Ubuntu 22.04

### Version

0.8.5

### Python version

3.12/3.13

---

_Label `bug` added by @ego-thales on 2025-08-07 08:18_

---

_Renamed from "`--resolution=lowest` does not resolve?" to "`--resolution=lowest` does not fork on requires-python" by @konstin on 2025-08-07 09:52_

---

_Comment by @konstin on 2025-08-07 09:57_

This missing inverted logic for `--resolution=lowest`: For the default resolution, when we see different `requires-python` value for versions, we split the resolution into two and would solve once for `>=3.13` and once for `<3.13,>=3.12`, selecting two different versions and having wheel for both Python versions. We do this by selecting a _lower_ torch version for `<3.13,>=3.12`. But we're missing the inverted lookup for `--resolution=lowest`: We already picked the lowest version, now we need to check for a _higher_ torch version for `>=3.13`.

As a workaround, you can set the following:

```toml
[tool.uv]
required-environments = [
  "python_version >= '3.12' and python_version < '3.12'",
  "python_version >= '3.13'"
]
```

---

_Comment by @ego-thales on 2025-08-08 08:01_

Thanks for your time and answer! Your workaround also works fine (without the typoed `3.12`).
I also realized that indirect dependencies lower bound adjustments should be made in `constraint-dependencies`, not in denepdencies directly. For future reference:
```toml
[tool.uv]
# Until github.com/astral-sh/uv/issues/15125 is fixed
required-environments = [
  "python_version >= '3.12' and python_version < '3.13'",
  "python_version >= '3.13'"
]
# Adjust `torch` indirect dependencies lower bound for example
constraint-dependencies = [
    "filelock>=3.13", # torch<=2.7.1
    "fsspec>=2023.10", # torch<=2.7.1
    "networkx>=3.2", # torch<=2.7.1
    "nvidia-nvjitlink-cu12>=12.1", # torch > nvidia-cusolver-cu12 > nvidia-cusparse-cu12
    "sympy>=1.9", # torch<=2.7.1
    "typing-extensions>=4.12", # torch<=2.7.1
]
```

For those wondering, `constraint-dependencies` is necessary because otherwise, `uv` would try to install `nvidia-nvjitlink-cu12` for example, even on macOS for which it does not exist.

All the best!


---

_Closed by @ego-thales on 2025-08-08 08:01_

---

_Reopened by @ego-thales on 2025-08-08 08:34_

---

_Referenced in [ThalesGroup/scio#9](../../ThalesGroup/scio/issues/9.md) on 2025-08-08 09:51_

---

_Comment by @ego-thales on 2025-08-08 09:56_

Sorry I accidentally closed while the bug is still there, even if the workaround works.

I also mentioned that there seems to be [other issues](https://github.com/ThalesGroup/scio/issues/9) related to `--resolution=lowest`, but I'm not sure whether I should open a separate issue, or if it even is a bug or a misconfig from my part.


---

_Comment by @konstin on 2025-08-19 12:02_

For that case, it sounds correct to have to use `tool.uv.required-environments`, at least for universal resolution. For testing `uv pip install --resolution=lowest` instead, this should not be a problem.

---

_Comment by @juntyr on 2025-12-18 20:41_

I encountered a variant of this problem when combining a lower bound `numpy~=2.3` in my pyproject.toml dependencies with `uv sync --resolution lowest-direct —-only-binary` in a Python 3.14 venv, which results in uv trying to install numpy 2.3.0, which doesn’t have 3.14 wheels (2.3.2 does). Interestingly, `uv pip install numpy~=2.3 --resolution lowest-direct —-only-binary` in a Python 3.14 venv works as expected and installs 2.3.2. The above mentioned workaround of adding required environments doesn’t work for me.

I’m on uv version 0.9.18.

---
