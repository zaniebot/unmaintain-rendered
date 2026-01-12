```yaml
number: 8700
title: Cannot change project python version
type: issue
state: closed
author: voodoojunk
labels: []
assignees: []
created_at: 2024-10-30T14:11:41Z
updated_at: 2024-10-30T14:17:15Z
url: https://github.com/astral-sh/uv/issues/8700
synced_at: 2026-01-12T15:59:32Z
```

# Cannot change project python version

---

_@voodoojunk_

Hi

I'm trying to change the python version for a project and it does not work.
My understanding is that updating the `requires-python` version in `pyproject.toml`  and running `uv sync` is all that is required, but it does not work and I get the following error:
```
uv sync
Using CPython 3.9.6 interpreter at: /Library/Developer/CommandLineTools/usr/bin/python3
error: The Python request from `.python-version` resolved to Python 3.9.6, which is incompatible with the project's Python requirement: `==3.11.8`. However, a workspace member (`help`) supports Python >=3.9. To install the workspace member on its own, navigate to `help`, then run `uv venv --python 3.9.6` followed by `uv pip install -e .`.
```

I also tried updating python via the `venv` subcommand and the installation appears to be successful (only after reverting to the original version in `requires-python`):
```
uv venv --python 3.11.8
Using CPython 3.11.8
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
```
But the python version being used appears to be the old one even after activating the venv:
```
uv run python --version
Python 3.9.6
```

Environment:

uv version = uv 0.4.28 (debe67ffd 2024-10-28)
macOS = 14.6

---

_Comment by @charliermarsh on 2024-10-30 14:13_

You should also remove or modify the `.python-version` file, which encodes the _specific_ version you want (as opposed to the range of supported versions).

---

_Comment by @voodoojunk on 2024-10-30 14:17_

Thanks - removing .python-version indeed solved the problem.

---

_Closed by @voodoojunk on 2024-10-30 14:17_

---
