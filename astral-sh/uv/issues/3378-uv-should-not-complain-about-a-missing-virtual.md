---
number: 3378
title: "`uv`should not complain about a missing virtual env when called from a non activated virtual environment."
type: issue
state: closed
author: gotcha
labels: []
assignees: []
created_at: 2024-05-05T19:52:04Z
updated_at: 2024-05-05T22:53:26Z
url: https://github.com/astral-sh/uv/issues/3378
synced_at: 2026-01-10T01:23:27Z
---

# `uv`should not complain about a missing virtual env when called from a non activated virtual environment.

---

_Issue opened by @gotcha on 2024-05-05 19:52_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

This should not fail to locate the virtualenv - (`uv` should find out that the working dir is a virtualenv):
```bash
gotcha@ubuntu:~$ python3 -m venv myvenv
gotcha@ubuntu:~$ cd myvenv/
gotcha@ubuntu:~/myvenv$ bin/pip install uv
Collecting uv
  Obtaining dependency information for uv from https://files.pythonhosted.org/packages/da/3f/63f17edf42f5408b4b6dd19c79c94e9ca2c3d95aaf43a6b7c11ba83c7b50/uv-0.1.39-py3-none-manylinux_2_28_aarch64.whl.metadata
  Using cached uv-0.1.39-py3-none-manylinux_2_28_aarch64.whl.metadata (29 kB)
Using cached uv-0.1.39-py3-none-manylinux_2_28_aarch64.whl (11.6 MB)
Installing collected packages: uv
Successfully installed uv-0.1.39
gotcha@ubuntu:~/myvenv$ bin/uv pip install zope.interface
error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.
```
Beware: this happens when the virtual environment has NOT been activated.

(I personally never activate my virtual environments in order to discriminate between my virtualenvs and the system python.
By always `cd`ing into the venv and prefixing my calls to the local executables with `bin/` like above in `bin/uv pip install`, I ensure 1) to avoid mixing with the global installation and 2) to be very specific about the environment that I mutate.)

I use uv 0.1.39.


---

_Referenced in [astral-sh/uv#3379](../../astral-sh/uv/pulls/3379.md) on 2024-05-05 20:02_

---

_Closed by @charliermarsh on 2024-05-05 22:53_

---
