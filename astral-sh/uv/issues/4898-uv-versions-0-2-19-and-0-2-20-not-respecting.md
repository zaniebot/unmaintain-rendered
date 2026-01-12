```yaml
number: 4898
title: uv versions 0.2.19 and 0.2.20 not respecting wheel dependecies
type: issue
state: closed
author: atrbgithub
labels:
  - bug
  - resolver
assignees: []
created_at: 2024-07-08T14:26:57Z
updated_at: 2024-07-08T16:32:10Z
url: https://github.com/astral-sh/uv/issues/4898
synced_at: 2026-01-12T15:58:52Z
```

# uv versions 0.2.19 and 0.2.20 not respecting wheel dependecies

---

_@atrbgithub_

Given the following:

```
python --version
Python 3.8.19

uv --version
uv 0.2.20
```

The error is:

```
uv pip install dvc==3.42.0
error: No virtual environment found
sh-5.1$ uv venv
Using Python 3.8.19 interpreter at: /some/location/bin/python3
Creating virtualenv at: .venv
sh-5.1$ uv pip install dvc==3.42.0
error: Failed to download and build `voluptuous==0.15.1`
  Caused by: Failed to build: `voluptuous==0.15.1`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/a_home_dir/.cache/uv/environments-v0/.tmpvwToiO/lib/python3.8/site-packages/setuptools/build_meta.py", line 327, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
  File "/home/a_home_dir/.cache/uv/environments-v0/.tmpvwToiO/lib/python3.8/site-packages/setuptools/build_meta.py", line 297, in _get_build_requires
    self.run_setup()
  File "/home/a_home_dir/.cache/uv/environments-v0/.tmpvwToiO/lib/python3.8/site-packages/setuptools/build_meta.py", line 497, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/a_home_dir/.cache/uv/environments-v0/.tmpvwToiO/lib/python3.8/site-packages/setuptools/build_meta.py", line 313, in run_setup
    exec(code, locals())
  File "<string>", line 7, in <module>
  File "/home/a_home_dir/.cache/uv/built-wheels-v3/pypi/voluptuous/0.15.1/MQJft_QmRPeFdMipU2XVd/voluptuous-0.15.1.tar.gz/./voluptuous/__init__.py", line 78, in <module>
    from voluptuous.schema_builder import *
  File "/home/a_home_dir/.cache/uv/built-wheels-v3/pypi/voluptuous/0.15.1/MQJft_QmRPeFdMipU2XVd/voluptuous-0.15.1.tar.gz/./voluptuous/schema_builder.py", line 13, in <module>
    from functools import cache, wraps
ImportError: cannot import name 'cache' from 'functools' (/some/location/lib/python3.8/functools.py)
---
```

This also fails with uv `0.2.19`

It works with `0.2.18`, which installs the correct supported package:

```
 + voluptuous==0.14.2
```

Rather than attempting to install:

```
voluptuous==0.15.1
```

As this does not support python 3.8. 







---

_Comment by @zanieb on 2024-07-08 14:29_

Thanks for the report. We'll look into this.

---

_Label `bug` added by @zanieb on 2024-07-08 14:29_

---

_Label `resolver` added by @zanieb on 2024-07-08 14:29_

---

_Comment by @zanieb on 2024-07-08 14:30_

I'm a bit suspicious of https://github.com/astral-sh/uv/pull/4705 / #4707. Probably related to https://github.com/astral-sh/uv/issues/4885.

---

_Comment by @charliermarsh on 2024-07-08 14:35_

I haven't been able to repro this yet with `echo "dvc==3.42.0" | uv pip compile - --python-version 3.8`.

---

_Comment by @zanieb on 2024-07-08 14:41_

I reproduced with

```
uv venv --python 3.8 --preview
uv pip install dvc==3.42.0
```

[log.txt](https://github.com/user-attachments/files/16130720/log.txt)


---

_Comment by @charliermarsh on 2024-07-08 14:41_

Okay, I think this is a prefetching problem. We're no longer filtering by `Requires-Python` in the prefetching codepath.

---

_Comment by @charliermarsh on 2024-07-08 14:41_

Because it was removed from the version map but not "re-added" there.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-08 14:59_

---

_Closed by @charliermarsh on 2024-07-08 16:32_

---
