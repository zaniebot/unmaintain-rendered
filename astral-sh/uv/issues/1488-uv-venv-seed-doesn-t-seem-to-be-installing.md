```yaml
number: 1488
title: "`uv venv --seed` doesn't seem to be installing `setuptools` for pypy"
type: issue
state: closed
author: bendoerry
labels:
  - bug
  - virtualenv
  - pypy
assignees: []
created_at: 2024-02-16T13:00:52Z
updated_at: 2024-03-01T17:44:28Z
url: https://github.com/astral-sh/uv/issues/1488
synced_at: 2026-01-10T05:40:31Z
```

# `uv venv --seed` doesn't seem to be installing `setuptools` for pypy

---

_Issue opened by @bendoerry on 2024-02-16 13:00_

Not sure whether title is "correct" here, but it presents that way.

Alembic is illustrative here, I've also had this occur with `psycogreen`, `psycopg2cffi`, `flask-migrate`, `docopt` (this is a non-exhaustive list)

With CPython (using rye to ensure same python version)
```console
> uv venv --python <HOME>/.rye/py/cpython@3.9.16/install/bin/python3 --seed
Using Python 3.9.16 interpreter at <HOME>/.rye/py/cpython@3.9.16/install/bin/python3.9
Creating virtualenv at: .venv
 + setuptools==69.1.0
 + pip==24.0
 + wheel==0.42.0

> source .venv/bin/activate
> python3 --version
Python 3.9.16

> uv pip install alembic==1.0.11
Resolved 9 packages in 12ms
Installed 9 packages in 8ms
 + alembic==1.0.11
 + greenlet==3.0.3
 + mako==1.3.2
 + markupsafe==2.1.5
 + python-dateutil==2.8.2
 + python-editor==1.0.4
 + six==1.16.0
 + sqlalchemy==2.0.27
 + typing-extensions==4.9.0
```

With PyPy (This also occurs with system PyPy, not just rye managed)
```console
> uv venv --python <HOME>/.rye/py/pypy@3.9.16/bin/python3 --seed
Using Python 3.9.16 interpreter at <HOME>/.rye/py/pypy@3.9.16/bin/pypy3.9
Creating virtualenv at: .venv
 + setuptools==69.1.0
 + pip==24.0
 + wheel==0.42.0

> source .venv/bin/activate
> python3 --version
Python 3.9.16 (feeb267ead3e6771d3f2f49b83e1894839f64fb7, Dec 29 2022, 14:23:21)
[PyPy 7.3.11 with GCC 10.2.1 20210130 (Red Hat 10.2.1-11)]

> uv pip install alembic==1.0.11
Resolved 9 packages in 11ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: greenlet==3.0.3
  Caused by: Failed to build: greenlet==3.0.3
  Caused by: Build backend failed to determine extra requires with `build_wheel()`:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 4, in <module>
ModuleNotFoundError: No module named 'setuptools'
---

> # Next one included since error is slightly different
> uv pip install alembic==1.0.11 --index-url <index with prebuilt pypy wheels>
error: Failed to download and build: alembic==1.0.11
  Caused by: Failed to build: alembic==1.0.11
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel`:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 4, in <module>
ModuleNotFoundError: No module named 'setuptools'
---
```

---

_Label `bug` added by @zanieb on 2024-02-16 14:37_

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:38_

---

_Assigned to @BurntSushi by @BurntSushi on 2024-02-19 16:11_

---

_Comment by @edgarrmondragon on 2024-02-20 22:32_

I think this is a general issue with build requirements in PyPy. See also https://github.com/tox-dev/tox-uv/issues/15.

---

_Label `pypy` added by @konstin on 2024-02-21 16:02_

---

_Comment by @BurntSushi on 2024-02-29 21:02_

With https://github.com/astral-sh/uv/pull/2094 merged, I was able to run your original set of steps successfully. Could you try again when the next release is out? (Or build from `main` if you want to try earlier.) If it still doesn't work, it might help to share a bit more about your environment. Linux? macOS? And show the output of `RUST_LOG=trace uv pip install -v ...`.

---

_Comment by @charliermarsh on 2024-02-29 21:15_

This actually might be fixed... Let's see.

---

_Comment by @bendoerry on 2024-03-01 17:42_

So this does seem to work for us now, we're able to install all our dependencies on PyPy and so far our tests seem to pass (haven't had time to run the full test suite though).

---

_Comment by @charliermarsh on 2024-03-01 17:44_

Okay cool -- closing for now but if you run into other PyPy issues, please file.

---

_Closed by @charliermarsh on 2024-03-01 17:44_

---
