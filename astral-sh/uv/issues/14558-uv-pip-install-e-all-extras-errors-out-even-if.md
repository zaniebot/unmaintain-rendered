```yaml
number: 14558
title: "`uv pip install -e . --all-extras` errors out even if pyproject.toml is present"
type: issue
state: closed
author: akx
labels:
  - question
assignees: []
created_at: 2025-07-11T06:37:23Z
updated_at: 2025-07-20T22:45:26Z
url: https://github.com/astral-sh/uv/issues/14558
synced_at: 2026-01-10T03:32:45Z
```

# `uv pip install -e . --all-extras` errors out even if pyproject.toml is present

---

_Issue opened by @akx on 2025-07-11 06:37_

### Summary

When developing a package that might have many optional dependencies (as extras), it would be useful to be able to install all of them.

I would have expected `uv pip install -e . --all-extras` to be able to install `extras-example`, `django` and `pydantic`, but it fails with an error message that doesn't seem to be quite relevant (as `pyproject.toml` is very present):

```
$ ls
pyproject.toml
$ cat pyproject.toml
[project]
name = "extras-example"
version = "0.1.0"

[project.optional-dependencies]
django = ["django>=4.2"]
pydantic = ["pydantic>=2.0"]
$ uv --version
uv 0.7.20 (251420396 2025-07-09)
$ uv venv
Using CPython 3.12.11
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate.fish
$ uv pip install -e . --all-extras
error: Requesting extras requires a `pyproject.toml`, `setup.cfg`, or `setup.py` file. Use `<dir>[extra]` syntax or `-r <file>` instead.
```
Installing the package sans extras works:
```
$ uv pip install -e .
Resolved 1 package in 2ms
      Built extras-example @ ...
Prepared 1 package in 429ms
Installed 1 package in 1ms
 + extras-example==0.1.0 (from ...)
```
as does specifying the extras by hand (as the error message tells you to do):
```
$ uv pip install -e .[django,pydantic]
Resolved 9 packages in 408ms
      Built extras-example @ ...
Prepared 4 packages in 641ms
Uninstalled 1 package in 0.62ms
Installed 9 packages in 68ms
 + annotated-types==0.7.0
 + asgiref==3.9.1
 + django==5.2.4
 ~ extras-example==0.1.0 (from ...)
 + pydantic==2.11.7
 + pydantic-core==2.33.2
 + sqlparse==0.5.3
 + typing-extensions==4.14.1
 + typing-inspection==0.4.1
```

In other words – why does the error message say it requires a `pyproject.toml`, when one is certainly present, or why can it not read the extras from there? (On the other hand, wouldn't it be enough to build the package, read the built package's metadata to find the extras, and install them, then proceed with regular editable installation?)



### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.20 (251420396 2025-07-09)

### Python version

CPython 3.12.11

---

_Label `bug` added by @akx on 2025-07-11 06:37_

---

_Comment by @zanieb on 2025-07-11 12:52_

`uv pip` won't read dependency metadata from the `pyproject.toml` unless it's provided, e.g., `uv pip install -r pyproject.toml --all-extras`. The `--extra` flags are present in `uv pip install` because there's no other way to request extras from a `pyproject.toml`, unlike when doing `uv pip install <package>[extra, ...]`.

---

_Label `bug` removed by @zanieb on 2025-07-11 12:52_

---

_Label `question` added by @zanieb on 2025-07-11 12:52_

---

_Comment by @zanieb on 2025-07-11 12:53_

Related https://github.com/astral-sh/uv/issues/11198

---

_Closed by @charliermarsh on 2025-07-20 22:45_

---
