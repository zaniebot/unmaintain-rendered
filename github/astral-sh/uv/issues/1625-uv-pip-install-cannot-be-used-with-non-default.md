---
number: 1625
title: "`uv pip install` cannot be used with non-default venv names created by `uv venv` without venv activation"
type: issue
state: closed
author: SnoopJ
labels:
  - duplicate
assignees: []
created_at: 2024-02-18T04:49:42Z
updated_at: 2024-02-18T08:12:54Z
url: https://github.com/astral-sh/uv/issues/1625
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv pip install` cannot be used with non-default venv names created by `uv venv` without venv activation

---

_Issue opened by @SnoopJ on 2024-02-18 04:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

`uv pip install` does not have a workflow for installing directly into a venv previously created by `uv venv` (and there is no `uv` entrypoint in the venv that could be used instead).

This is inconvenient when using a workflow that benefits from _not_ activating the venv whose site is being manipulated (e.g. multiple Python versions/target environments).

uv version: (0.1.4 @ https://github.com/astral-sh/uv/commit/fef1956c627483d2496b14a0a6a6ac2fc2826c2f)
Platform: Ubuntu 20.04
Python version: 3.9.16 provided by pyenv

## Reproduction

NOTE: this invocation points directly at the `uv` entrypoint as a workaround for #1623.

```
$ ~/.pyenv/versions/3.9.16/bin/uv venv test_venv
Using Python 3.9.16 interpreter at /home/snoopjedi/.pyenv/versions/3.9.16/bin/python3
Creating virtualenv at: test_venv
$ ~/.pyenv/versions/3.9.16/bin/uv pip install numpy
error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.
$ ~/.pyenv/versions/3.9.16/bin/uv pip install --help | grep venv  # --help does not mention any venv functionality
$ source test_venv/bin/activate
$ ~/.pyenv/versions/3.9.16/bin/uv pip install numpy  # once venv is activated, install works as expected
Resolved 1 package in 41ms
Installed 1 package in 35ms
 + numpy==1.26.4
$ ls test_venv/lib/python3.9/site-packages/
__pycache__  _virtualenv.pth  _virtualenv.py  numpy  numpy-1.26.4.dist-info  numpy.libs
```


---

_Comment by @zanieb on 2024-02-18 08:05_

Related
- https://github.com/astral-sh/uv/issues/1422
- https://github.com/astral-sh/uv/issues/1598#issuecomment-1950259889
- https://github.com/astral-sh/uv/issues/1501
- https://github.com/astral-sh/uv/issues/1396
- https://github.com/astral-sh/uv/issues/1632

---

_Comment by @zanieb on 2024-02-18 08:06_

Closing in favor of https://github.com/astral-sh/uv/issues/1632 and https://github.com/astral-sh/uv/issues/1422

---

_Closed by @zanieb on 2024-02-18 08:06_

---

_Label `duplicate` added by @zanieb on 2024-02-18 08:07_

---

_Comment by @zanieb on 2024-02-18 08:07_

Thanks for the clear issue by the way! It's nicely written

---

_Referenced in [astral-sh/uv#1632](../../astral-sh/uv/issues/1632.md) on 2024-02-18 18:01_

---
