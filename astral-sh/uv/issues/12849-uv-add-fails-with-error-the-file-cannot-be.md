```yaml
number: 12849
title: "uv add fails with error : The file cannot be accessed by the system. (os error 1920)"
type: issue
state: open
author: aprd21
labels:
  - bug
assignees: []
created_at: 2025-04-12T02:27:25Z
updated_at: 2025-04-18T03:10:56Z
url: https://github.com/astral-sh/uv/issues/12849
synced_at: 2026-01-12T16:01:14Z
```

# uv add fails with error : The file cannot be accessed by the system. (os error 1920)

---

_@aprd21_

### Summary

I'm unable to run uv add pandas-profiling, and encounter the following error. 
```
uv add pandas-profiling
  x Failed to download and build `htmlmin==0.1.12`
  |-> Failed to create temporary virtualenv
  `-> failed to copy file from
      C:\Users\xyz\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\python.exe
      to
      C:\Users\xyz\AppData\Local\uv\cache\builds-v0\.tmp3YDyRY\Scripts\python.exe:
      The file cannot be accessed by the system. (os error 1920)
  help: If you want to add the package regardless of the failed resolution,
        provide the `--frozen` flag to skip locking and syncing.
```
Other uv commands haven't given me problem so far, and this is the first time I'm stuck. For eg, I ran uv add matplotlib, and it worked. The pyproject file below is before adding matplotlib. 

I'm also using pycharm IDE in case it matters.

<details><summary>Pyproject.toml file</summary>
<p>
pyproject.toml

[project]
name = "redacted"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "django>=5.1.7",
    "notebook>=7.3.3",
    "numpy>=2.2.4",
    "pandas>=2.2.3",
    "pre-commit>=4.2.0",
    "pyrate-limiter>=3.7.0",
    "pytest>=8.3.5",
    "requests>=2.32.3",
    "ruff>=0.11.2",
]
</p>
</details> 

### Platform

Windows 11 x86_64

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

### Python version

Python 3.12.10

---

_Label `bug` added by @aprd21 on 2025-04-12 02:27_

---

_Comment by @aprd21 on 2025-04-12 02:30_

If it helps, I was also facing this issue prior to upgrading uv , with version 0.6.11

---

_Comment by @yanghanlin on 2025-04-17 13:42_

I've experienced the same issue with Windows 11 and uv 0.6.14 (a4cec56dc 2025-04-09). For context, I'm also using the Python distribution from the Microsoft Store, with Windows Defender enabled. A workaround that has been effective for me is to make uv download a separate copy of the Python interpreter instead of relying on the system Python:

```powershell
# Remove the virtual environment
Remove-Item -Force -Recurse .venv
# Force uv to re-create the virtual environment with uv-managed Python
uv sync --managed-python
```

---

_Comment by @aprd21 on 2025-04-18 03:10_

The above workaround also worked for me. Thanks!



---
