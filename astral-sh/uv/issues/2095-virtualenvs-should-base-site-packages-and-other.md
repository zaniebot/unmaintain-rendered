```yaml
number: 2095
title: "virtualenvs should base `site-packages` and other directories on `sysconfig` paths"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - pypy
assignees: []
created_at: 2024-02-29T20:45:50Z
updated_at: 2024-03-05T21:13:25Z
url: https://github.com/astral-sh/uv/issues/2095
synced_at: 2026-01-10T05:40:32Z
```

# virtualenvs should base `site-packages` and other directories on `sysconfig` paths

---

_Issue opened by @charliermarsh on 2024-02-29 20:45_

Right now, we assume a specific structure for the virtualenvs that we create, and it's necessary (I think?) that this structure matches the `sysconfig.get_paths()` returned by the interpreter in the virtual environment later. The paths seem to be correct for CPython, but they're not quite right for PyPy, and could be wrong _in general_ for unusual distros.

If you look at [virtualenv](https://github.com/pypa/virtualenv/blob/5cd543fdf8047600ff2737babec4a635ad74d169/src/virtualenv/discovery/py_info.py#L119C14-L119C28), my understanding is that it does something like...

- Call `sysconfig.get_path()` without expanding variables.
- Grab the value for each variable.
- [Override specific variables to the current working directory](https://github.com/pypa/virtualenv/blob/5cd543fdf8047600ff2737babec4a635ad74d169/src/virtualenv/discovery/py_info.py#L161).


---

_Label `bug` added by @charliermarsh on 2024-02-29 20:45_

---

_Label `pypy` added by @charliermarsh on 2024-02-29 20:50_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-05 00:19_

---

_Closed by @charliermarsh on 2024-03-05 21:13_

---
