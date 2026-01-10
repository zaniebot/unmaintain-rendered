---
number: 1551
title: "Using uv with internal libraries fails with `No module named 'pip'`"
type: issue
state: closed
author: wsantos
labels: []
assignees: []
created_at: 2024-02-16T23:09:35Z
updated_at: 2025-04-16T01:46:11Z
url: https://github.com/astral-sh/uv/issues/1551
synced_at: 2026-01-10T01:23:07Z
---

# Using uv with internal libraries fails with `No module named 'pip'`

---

_Issue opened by @wsantos on 2024-02-16 23:09_

We are trying to implement UV in our pip line bot we are getting this error for one of out libraries:

```
error: Failed to build editables
  Caused by: Failed to build editable: file:///Users/wsantos/projetos/xxxxxx/xxxxxxx
  Caused by: Build backend failed to build wheel through `build_editable()`:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 12, in <module>
ModuleNotFoundError: No module named 'pip'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<string>", line 6, in <module>
  File "/private/var/folders/x_/cw97nxxx13q_f1fscwrs7g9r0000gn/T/.tmphBj6Wg/.venv/lib/python3.9/site-packages/setuptools/build_meta.py", line 436, in build_editable
    return self._build_with_temp_dir(
  File "/private/var/folders/x_/cw97nxxx13q_f1fscwrs7g9r0000gn/T/.tmphBj6Wg/.venv/lib/python3.9/site-packages/setuptools/build_meta.py", line 389, in _build_with_temp_dir
    self.run_setup()
  File "/private/var/folders/x_/cw97nxxx13q_f1fscwrs7g9r0000gn/T/.tmphBj6Wg/.venv/lib/python3.9/site-packages/setuptools/build_meta.py", line 480, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/private/var/folders/x_/cw97nxxx13q_f1fscwrs7g9r0000gn/T/.tmphBj6Wg/.venv/lib/python3.9/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 14, in <module>
ModuleNotFoundError: No module named 'pip'
```

How can I fix it, I see some others issues about it but none told how we can fix it.


---

_Comment by @charliermarsh on 2024-02-16 23:13_

Thanks! Is this a library you all control? It looks like it depends on `"pip"`. Does it declare `"pip`" as a build requirement, in the `pyproject.toml`?

---

_Renamed from "Using uv with internal libraris fails with " to "Using uv with internal libraris fails with `No module named 'pip'`" by @zanieb on 2024-02-17 00:42_

---

_Comment by @wsantos on 2024-02-17 02:24_

> Thanks! Is this a library you all control? It looks like it depends on `"pip"`. Does it declare `"pip`" as a build requirement, in the `pyproject.toml`?

Yeap we control it, we don't have a `pyproject.toml` how should I do it, we do have `setup.py`

---

_Renamed from "Using uv with internal libraris fails with `No module named 'pip'`" to "Using uv with internal libraries fails with `No module named 'pip'`" by @wsantos on 2024-02-17 02:34_

---

_Comment by @ArturFormella on 2024-03-23 22:24_

This is a surprising workaround:
```
➜  uv pip install pip
Resolved 1 package in 176ms
Installed 1 package in 16ms
 + pip==24.0

```


---

_Comment by @hauntsaninja on 2024-03-23 22:35_

Add a pyproject.toml with something like:
```
[build-system]
requires = ["setuptools", "wheel", "pip"]
build-backend = "setuptools.build_meta"
```
(and keep your existing setup.py as-is)

---

_Comment by @charliermarsh on 2024-04-04 03:58_

Closing since @hauntsaninja suggestion is what you're looking for!

---

_Closed by @charliermarsh on 2024-04-04 03:58_

---

_Referenced in [astral-sh/uv#7291](../../astral-sh/uv/issues/7291.md) on 2024-09-11 14:05_

---

_Referenced in [astral-sh/uv#7327](../../astral-sh/uv/issues/7327.md) on 2025-02-01 07:38_

---

_Referenced in [holdonprojekt/hcc#1](../../holdonprojekt/hcc/pulls/1.md) on 2025-04-04 15:02_

---

_Comment by @tech-ezutil on 2025-04-16 01:46_

> Add a pyproject.toml with something like:
> 
> ```
> [build-system]
> requires = ["setuptools", "wheel", "pip"]
> build-backend = "setuptools.build_meta"
> ```
> 
> (and keep your existing setup.py as-is)

this doesn't work  for me.  there are still lots of bugs and problems with `uv pip` , instead use uv like conda : 

`uv pip install pip && pip install -r requirements.txt`

---
