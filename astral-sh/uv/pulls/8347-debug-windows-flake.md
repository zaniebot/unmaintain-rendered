```yaml
number: 8347
title: "DEBUG: Windows flake"
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/win-debug-ugh
created_at: 2024-10-18T19:33:28Z
updated_at: 2024-10-20T18:37:48Z
url: https://github.com/astral-sh/uv/pull/8347
synced_at: 2026-01-12T16:08:16Z
```

# DEBUG: Windows flake

---

_@zanieb_

Debugging #6940 

---

_Comment by @zanieb on 2024-10-18 22:52_

Got a snapshot without filters

```
[crates\uv\tests\it\common\mod.rs:1152:5] &snapshot = "success: false
exit_code: 2
----- stdout -----

----- stderr -----
warning: `VIRTUAL_ENV=E:\\uv-tmp\\.tmpHTBFBl\\temp\\.venv` does not match the project environment path `.venv` and will be ignored
Using CPython 3.12.7 interpreter at: C:\\hostedtoolcache\\windows\\Python\\3.12.7\\x64\\python.exe
Creating virtual environment at: .venv
Resolved 8 packages in 229ms
error: Failed to prepare distributions
  Caused by: Failed to build `seeds @ file:///E:/uv-tmp/.tmpHTBFBl/temp/albatross-virtual-workspace/packages/seeds`
  Caused by: Build backend failed to build editable through `build_editable` (exit code: 1)

[stderr]
Traceback (most recent call last):
  File \"<string>\", line 11, in <module>
  File \"E:\\uv-tmp\\.tmpHTBFBl\\cache\\builds-v0\\.tmpavQDxb\\Lib\\site-packages\\hatchling\\build.py\", line 83, in build_editable
    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File \"E:\\uv-tmp\\.tmpHTBFBl\\cache\\builds-v0\\.tmpavQDxb\\Lib\\site-packages\\hatchling\\builders\\plugin\\interface.py\", line 90, in build
    self.metadata.validate_fields()
  File \"E:\\uv-tmp\\.tmpHTBFBl\\cache\\builds-v0\\.tmpavQDxb\\Lib\\site-packages\\hatchling\\metadata\\core.py\", line 260, in validate_fields
    self.core.validate_fields()
  File \"E:\\uv-tmp\\.tmpHTBFBl\\cache\\builds-v0\\.tmpavQDxb\\Lib\\site-packages\\hatchling\\metadata\\core.py\", line 1352, in validate_fields
    getattr(self, attribute)
  File \"E:\\uv-tmp\\.tmpHTBFBl\\cache\\builds-v0\\.tmpavQDxb\\Lib\\site-packages\\hatchling\\metadata\\core.py\", line 1223, in dependencies
    self._dependencies = list(self.dependencies_complex)
                              ^^^^^^^^^^^^^^^^^^^^^^^^^
  File \"E:\\uv-tmp\\.tmpHTBFBl\\cache\\builds-v0\\.tmpavQDxb\\Lib\\site-packages\\hatchling\\metadata\\core.py\", line 1174, in dependencies_complex
    from packaging.requirements import InvalidRequirement, Requirement
  File \"E:\\uv-tmp\\.tmpHTBFBl\\cache\\builds-v0\\.tmpavQDxb\\Lib\\site-packages\\packaging\\requirements.py\", line 7, in <module>
    from ._parser import parse_requirement as _parse_requirement
  File \"<frozen importlib._bootstrap>\", line 1360, in _find_and_load
  File \"<frozen importlib._bootstrap>\", line 1331, in _find_and_load_unlocked
  File \"<frozen importlib._bootstrap>\", line 935, in _load_unlocked
  File \"<frozen importlib._bootstrap_external>\", line 991, in exec_module
  File \"<frozen importlib._bootstrap_external>\", line 1128, in get_code
  File \"<frozen importlib._bootstrap_external>\", line 1186, in get_data
PermissionError: [Errno 13] Permission denied: 'E:\\\\uv-tmp\\\\.tmpHTBFBl\\\\cache\\\\builds-v0\\\\.tmpavQDxb\\\\Lib\\\\site-packages\\\\packaging\\\\_parser.py'
"
```

---

_Comment by @zanieb on 2024-10-19 00:14_

With [56d5cc4](https://github.com/astral-sh/uv/pull/8347/commits/56d5cc4d5ac73f1ffd24d8bfd24b0f43fee1e68d) it hasn't reproduced in ~60m

---

_Closed by @zanieb on 2024-10-20 18:37_

---
