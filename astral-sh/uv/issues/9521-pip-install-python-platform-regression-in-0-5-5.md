```yaml
number: 9521
title: pip install --python-platform regression in 0.5.5
type: issue
state: closed
author: schlamar
labels:
  - bug
assignees: []
created_at: 2024-11-29T12:47:46Z
updated_at: 2024-11-29T17:26:26Z
url: https://github.com/astral-sh/uv/issues/9521
synced_at: 2026-01-12T15:59:52Z
```

# pip install --python-platform regression in 0.5.5

---

_@schlamar_

**Expected behavior**

Using `uv pip install --python-platform X`, uv should prefer a wheel with platform tags (for example *-manylinux_2_17_x86_64.manylinux2014_x86_64.whl).

**Actual behavior**

Since uv 0.5.5 it seems that uv is prefering a wheel without platform tags (*-none-any).

**Steps to reproduce** 

```
> uv pip install -v --python-platform linux --target temp --only-binary=:all: --python-version 3.10 --no-deps "sqlalchemy>=2,<3"
DEBUG uv 0.5.4 (c62c83c37 2024-11-20)
...
DEBUG Selecting: sqlalchemy==2.0.36 [compatible] (SQLAlchemy-2.0.36-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
```

```
> uv pip install -v --python-platform linux --target temp --only-binary=:all: --python-version 3.10 --no-deps "sqlalchemy>=2,<3"
DEBUG uv 0.5.5 (95cd8b8b3 2024-11-27)
...
DEBUG Selecting: sqlalchemy==2.0.36 [compatible] (SQLAlchemy-2.0.36-py3-none-any.whl)
```

This is only reproducible if you create a new virtual environment and install the specific uv version. If you upgrade or downgrade uv in an existing virtual environment, the issue is not reproducible. I guess there is some caching involved in this case.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-29 17:00_

---

_Comment by @charliermarsh on 2024-11-29 17:02_

This does look like a regression, let me take a look...

---

_Label `bug` added by @charliermarsh on 2024-11-29 17:02_

---

_Comment by @charliermarsh on 2024-11-29 17:08_

Ah I see, ok. This probably only applies to `--python-platform` and not to invocations on _actual_ Linux machines, so that's good at least.

---

_Closed by @charliermarsh on 2024-11-29 17:26_

---
