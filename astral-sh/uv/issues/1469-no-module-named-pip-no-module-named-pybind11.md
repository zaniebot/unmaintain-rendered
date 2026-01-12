```yaml
number: 1469
title: "No module named pip / No module named 'pybind11'"
type: issue
state: closed
author: starenka
labels: []
assignees: []
created_at: 2024-02-16T10:05:28Z
updated_at: 2024-05-24T06:53:14Z
url: https://github.com/astral-sh/uv/issues/1469
synced_at: 2026-01-12T15:58:28Z
```

# No module named pip / No module named 'pybind11'

---

_@starenka_

```
starenka /tmp % uv venv tmp                     
Using Python 3.11.8 interpreter at /usr/bin/python3.11
Creating virtualenv at: tmp
starenka /tmp % source tmp/bin/activate
(tmp) starenka /tmp % python -V
Python 3.11.8
(tmp) starenka /tmp % pip --version                              
pip 24.0 from /usr/lib/python3/dist-packages/pip (python 3.11)
(tmp) starenka /tmp % uv pip install 'fasttext==0.9.2'
error: Failed to download and build: fasttext==0.9.2
  Caused by: Failed to build: fasttext==0.9.2
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel`:
--- stdout:

--- stderr:
/tmp/.tmpXuExdC/.venv/bin/python: No module named pip
Traceback (most recent call last):
  File "<string>", line 38, in __init__
ModuleNotFoundError: No module named 'pybind11'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<string>", line 10, in <module>
  File "/tmp/.tmpXuExdC/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 366, in prepare_metadata_for_build_wheel
    self.run_setup()
  File "/tmp/.tmpXuExdC/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 480, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/tmp/.tmpXuExdC/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 72, in <module>
  File "<string>", line 41, in __init__
RuntimeError: pybind11 install failed.
---
```

---

_Comment by @starenka on 2024-02-16 10:26_

it seems it has problems via pip too, feel free to close this. sorry

---

_Closed by @starenka on 2024-02-16 12:21_

---

_Comment by @yxtay on 2024-05-24 06:53_

Seems like it's fixed by installing from github. [#1446](https://github.com/astral-sh/uv/issues/1446#issuecomment-1956644900)

---
