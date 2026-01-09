---
number: 3686
title: "Hint to \"try: `--prerelease=allow`\" when I already am"
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
  - error messages
assignees: []
created_at: 2024-05-21T04:51:18Z
updated_at: 2024-10-15T01:04:31Z
url: https://github.com/astral-sh/uv/issues/3686
synced_at: 2026-01-07T13:12:17-06:00
---

# Hint to "try: `--prerelease=allow`" when I already am

---

_Issue opened by @adamtheturtle on 2024-05-21 04:51_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Apologies for not minimising this reproduction - I do not have the time right now and I hope that this report is still useful.
I am trying to restore an old project, https://github.com/adamtheturtle/dcos-e2e.

I try to install dependencies:

```shell
$ uv --version
uv 0.1.45
$ uv pip install --exclude-newer 2018-10-08 --prerelease=allow --verbose .
INFO Found a virtualenv through VIRTUAL_ENV at: /Users/adam/.virtualenvs/dcos-e2e
DEBUG Cached interpreter info for Python 3.12.3, skipping probing: /Users/adam/.virtualenvs/dcos-e2e/bin/python
DEBUG Using Python 3.12.3 environment at /Users/adam/.virtualenvs/dcos-e2e/bin/python
DEBUG Trying to lock if free: /Users/adam/.virtualenvs/dcos-e2e/.lock
DEBUG At least one requirement is not satisfied: file:///Users/adam/Documents/forks/dcos-e2e
DEBUG Using registry request timeout of 30s
DEBUG Trying to lock if free: /Users/adam/Library/Caches/uv/built-wheels-v3/path/ffcc5eebe36891de/.lock
DEBUG Preparing metadata for: file:///Users/adam/Documents/forks/dcos-e2e
DEBUG No static `PKG-INFO` available for: file:///Users/adam/Documents/forks/dcos-e2e (MissingPkgInfo)
DEBUG No static `pyproject.toml` available for: file:///Users/adam/Documents/forks/dcos-e2e (MissingPyprojectToml)
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG No compatible version found for: setuptools
error: Failed to build: `file:///Users/adam/Documents/forks/dcos-e2e`
  Caused by: Failed to install requirements from setup.py build (resolve)
  Caused by: No solution found when resolving: setuptools>=40.8.0
  Caused by: Because only setuptools<40.8.0 is available and you require setuptools>=40.8.0, we can conclude that the requirements are unsatisfiable.

hint: Pre-releases are available for setuptools in the requested range (e.g., 63.0.0b1), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

I would not expect the `hint` at the bottom - I am already using `--prerelease=allow`.

---

_Label `error messages` added by @konstin on 2024-05-21 09:55_

---

_Comment by @charliermarsh on 2024-05-21 11:44_

Thanks.

---

_Label `bug` added by @zanieb on 2024-05-21 14:41_

---

_Comment by @charliermarsh on 2024-05-21 18:41_

Mmm we don't propagate pre-releases to source distribution builds IIRC.

---

_Comment by @charliermarsh on 2024-05-21 18:58_

Not sure what to do here. I guess: don't show this at all for source builds?

---

_Referenced in [pyprojectx/pyprojectx#114](../../pyprojectx/pyprojectx/issues/114.md) on 2024-08-22 07:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-15 00:33_

---

_Referenced in [astral-sh/uv#8192](../../astral-sh/uv/pulls/8192.md) on 2024-10-15 00:46_

---

_Closed by @charliermarsh on 2024-10-15 01:04_

---
