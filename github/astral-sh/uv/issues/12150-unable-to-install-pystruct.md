---
number: 12150
title: Unable to install pystruct
type: issue
state: closed
author: sizhky
labels:
  - question
assignees: []
created_at: 2025-03-13T11:33:39Z
updated_at: 2025-03-14T00:27:44Z
url: https://github.com/astral-sh/uv/issues/12150
synced_at: 2026-01-07T13:12:18-06:00
---

# Unable to install pystruct

---

_Issue opened by @sizhky on 2025-03-13 11:33_

### Summary

```bash
uv pip install pystruct 
```

returns

```bash
error: Build backend failed to determine requirements with `build_wheel()` (exit status: 1)

[stderr]
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/paperspace/.cache/uv/builds-v0/.tmp0rZvfR/lib/python3.7/site-packages/setuptools/build_meta.py", line 341, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=['wheel'])
  File "/home/paperspace/.cache/uv/builds-v0/.tmp0rZvfR/lib/python3.7/site-packages/setuptools/build_meta.py", line 323, in _get_build_requires
    self.run_setup()
  File "/home/paperspace/.cache/uv/builds-v0/.tmp0rZvfR/lib/python3.7/site-packages/setuptools/build_meta.py", line 488, in run_setup
    self).run_setup(setup_script=setup_script)
  File "/home/paperspace/.cache/uv/builds-v0/.tmp0rZvfR/lib/python3.7/site-packages/setuptools/build_meta.py", line 338, in run_setup
    exec(code, locals())
  File "<string>", line 5, in <module>
ModuleNotFoundError: No module named 'numpy'
```

There's an `import numpy` in the `setup.py` of pystruct. But numpy was installed in the uv environment before I tried pip installing pystruct.

### Platform

Linux 6.2.0-37-generic x86_64 GNU/Linux

### Version

0.5.1

### Python version

Python 3.11.10

---

_Label `bug` added by @sizhky on 2025-03-13 11:33_

---

_Comment by @charliermarsh on 2025-03-13 12:55_

You can try running with `--no-build-isolation`. This looks like a problem with the package. See #2252.

---

_Label `bug` removed by @charliermarsh on 2025-03-13 12:55_

---

_Label `question` added by @charliermarsh on 2025-03-13 12:55_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-13 12:55_

---

_Closed by @charliermarsh on 2025-03-14 00:27_

---
