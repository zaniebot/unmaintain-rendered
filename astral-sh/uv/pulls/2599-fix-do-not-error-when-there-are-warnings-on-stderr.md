```yaml
number: 2599
title: "fix: do not error when there are warnings on stderr"
type: pull_request
state: merged
author: wolfv
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-error-uv
created_at: 2024-03-21T21:11:24Z
updated_at: 2024-03-22T07:07:07Z
url: https://github.com/astral-sh/uv/pull/2599
synced_at: 2026-01-12T16:05:07Z
```

# fix: do not error when there are warnings on stderr

---

_@wolfv_

## Summary

We had some users report bugs because the Python querying failed due to warnings in `stderr`. I don't think this should fail on any `stderr` output.

E.g.

```
  × Querying Python at `USER/.pixi/envs/default/bin/python3.10` failed with status exit status: 0 with exit status: 0
  │ --- stdout:
  │ {"markers": {"implementation_name": "cpython", "implementation_version": "3.10.0", "os_name": "posix", "platform_machine": "x86_64", "platform_python_implementation": "CPython", "platform_release": "5.15.146.1-microsoft-standard-WSL2",
  │ "platform_system": "Linux", "platform_version": "#1 SMP Thu Jan 11 04:09:03 UTC 2024", "python_full_version": "3.10.0", "python_version": "3.10", "sys_platform": "linux"}, "base_prefix": "USER/.pixi/
  │ envs/default", "base_exec_prefix": "USER/.pixi/envs/default", "prefix": "USER/.pixi/envs/default", "base_executable": "USER/.pixi/envs/default/
  │ bin/python3.10", "sys_executable": "USER/.pixi/envs/default/bin/python3.10", "stdlib": "USER/.pixi/envs/default/lib/python3.10", "scheme": {"platlib": "/home/mvanniekerk/
  │ code/vice-python/.pixi/envs/default/lib/python3.10/site-packages", "purelib": "USER/.pixi/envs/default/lib/python3.10/site-packages", "include": "USER/.pixi/envs/default/
  │ include/python3.10", "scripts": "USER/.pixi/envs/default/bin", "data": "USER/.pixi/envs/default"}, "virtualenv": {"purelib": "lib/python3.10/site-packages", "platlib": "lib/
  │ python3.10/site-packages", "include": "include/site/python3.10", "scripts": "bin", "data": ""}}
  │ --- stderr:
  │ [03/21/24 15:59:48] WARNING  pyproject.toml does not contain a setuptools.py:119
  │                              tool.setuptools_scm section
  │ ---
```

---

_Comment by @charliermarsh on 2024-03-21 21:14_

Do you know what version they're on? That warning feels like it might be indicative of a real issue in uv. (We shouldn't be reading pyproject.toml in there.)

---

_Comment by @charliermarsh on 2024-03-21 21:14_

It would be related to this: https://github.com/astral-sh/uv/pull/2305

---

_Comment by @charliermarsh on 2024-03-21 21:15_

(I'm fine not to fail on stderr though.)

---

_@zanieb reviewed on 2024-03-21 21:21_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/interpreter.rs`:390 on 2024-03-21 21:21_

Let's update this comment too

---

_Comment by @wolfv on 2024-03-21 21:32_

We are in fact a few versions behind with `uv` (0.16 right now I think) - will update to latest release tomorrow. Indeed sounds like #2305 might help!

I'll fix the comment now. I do think that failing on content in stderr is not quite right.

---

_Comment by @zanieb on 2024-03-21 21:37_

Yeah we talked about this same problem previously in #1885 

---

_Merged by @charliermarsh on 2024-03-21 23:23_

---

_Closed by @charliermarsh on 2024-03-21 23:23_

---

_Label `bug` added by @charliermarsh on 2024-03-21 23:23_

---

_Comment by @charliermarsh on 2024-03-21 23:23_

Thx!

---

_Branch deleted on 2024-03-22 07:07_

---
