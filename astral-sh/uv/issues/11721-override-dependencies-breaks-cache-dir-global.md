```yaml
number: 11721
title: override-dependencies breaks cache-dir global setting
type: issue
state: closed
author: Winand
labels:
  - bug
assignees: []
created_at: 2025-02-23T09:21:29Z
updated_at: 2025-02-24T12:42:44Z
url: https://github.com/astral-sh/uv/issues/11721
synced_at: 2026-01-12T16:00:44Z
```

# override-dependencies breaks cache-dir global setting

---

_@Winand_

**upd: see comment below**

### Summary

I have a separate [Windows Dev Drive](https://learn.microsoft.com/en-us/windows/dev-drive/) on `D:`.
I've added a file `D:\uv.toml` with contents:
```
cache-dir = "D:/.uv/cache"
```
to address issues #6613, #10595

Then I create a new project in `D:\folder\subfolder\prj`:
```toml
[project]
name = "prj"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "boltons>=25.0.0",
]

# [tool.uv]
# override-dependencies = [
#     "numpy",
# ]
```

If I run `uv sync --verbose` without `override-dependencies` settings I get the following output:
<details>
<summary>Acquired lock for D:\.uv\cache\simple-v15\pypi\boltons.lock</summary>

```
DEBUG uv 0.6.2 (6d3614eec 2025-02-19)
DEBUG Found project root: `D:\folder\subfolder\prj`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `D:\folder\subfolder\prj`
DEBUG Reading Python requests from version file at `D:\folder\subfolder\prj\.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.12`
DEBUG Released lock at `C:\Users\user\AppData\Local\Temp\uv-3b01b79bfea41645.lock`
DEBUG Using request timeout of 30s
DEBUG Ignoring existing lockfile due to mismatched overrides:
  Requested: {}
  Existing: {Requirement { name: PackageName("numpy"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
DEBUG Found static `pyproject.toml` for: prj @ file:///D:/folder/subfolder/prj
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: prj*
DEBUG Searching for a compatible version of prj @ file:///D:/folder/subfolder/prj (*)
DEBUG Adding direct dependency: boltons>=25.0.0
DEBUG Acquired lock for `D:\.uv\cache\simple-v15\pypi\boltons.lock`
DEBUG Found fresh response for: https://pypi.org/simple/boltons/
DEBUG Released lock at `D:\.uv\cache\simple-v15\pypi\boltons.lock`
DEBUG Searching for a compatible version of boltons (>=25.0.0)
DEBUG Acquired lock for `D:\.uv\cache\wheels-v4\pypi\boltons\boltons-25.0.0-py3-none-any.lock`
DEBUG Selecting: boltons==25.0.0 [preference] (boltons-25.0.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/45/7f/0e961cf3908bc4c1c3e027de2794f867c6c89fb4916fc7dba295a0e80a2d/boltons-25.0.0-py3-none-any.whl.metadata
DEBUG Released lock at `D:\.uv\cache\wheels-v4\pypi\boltons\boltons-25.0.0-py3-none-any.lock`
DEBUG Tried 2 versions: boltons 1, prj 1
DEBUG all marker environments resolution took 0.004s
Resolved 2 packages in 8ms
DEBUG Using request timeout of 30s
DEBUG Requirement already installed: boltons==25.0.0
Audited 1 package in 0.29ms
```
</details>

But with `override-dependencies` uncommented uv doesn't use `cache-dir` specified in `D:\uv.toml`:
<details>
<summary>Acquired lock for `C:\Users\user\AppData\Local\uv\cache\simple-v15\pypi\boltons.lock`</summary>

```
DEBUG uv 0.6.2 (6d3614eec 2025-02-19)
DEBUG Found project root: `D:\folder\subfolder\prj`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `D:\folder\subfolder\prj`
DEBUG Reading Python requests from version file at `D:\folder\subfolder\prj\.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.12`
DEBUG Released lock at `C:\Users\user\AppData\Local\Temp\uv-3b01b79bfea41645.lock`
DEBUG Using request timeout of 30s
DEBUG Ignoring existing lockfile due to mismatched overrides:
  Requested: {Requirement { name: PackageName("numpy"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
  Existing: {}
DEBUG Found static `pyproject.toml` for: prj @ file:///D:/folder/subfolder/prj
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: prj*
DEBUG Searching for a compatible version of prj @ file:///D:/folder/subfolder/prj (*)
DEBUG Adding direct dependency: boltons>=25.0.0
DEBUG Acquired lock for `C:\Users\user\AppData\Local\uv\cache\simple-v15\pypi\boltons.lock`
DEBUG No cache entry for: https://pypi.org/simple/boltons/
DEBUG Released lock at `C:\Users\user\AppData\Local\uv\cache\simple-v15\pypi\boltons.lock`
DEBUG Searching for a compatible version of boltons (>=25.0.0)
DEBUG Selecting: boltons==25.0.0 [preference] (boltons-25.0.0-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\user\AppData\Local\uv\cache\wheels-v4\pypi\boltons\boltons-25.0.0-py3-none-any.lock`
DEBUG No cache entry for: https://files.pythonhosted.org/packages/45/7f/0e961cf3908bc4c1c3e027de2794f867c6c89fb4916fc7dba295a0e80a2d/boltons-25.0.0-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\user\AppData\Local\uv\cache\wheels-v4\pypi\boltons\boltons-25.0.0-py3-none-any.lock`
DEBUG Tried 2 versions: boltons 1, prj 1
DEBUG all marker environments resolution took 0.289s
Resolved 2 packages in 293ms
DEBUG Using request timeout of 30s
DEBUG Requirement already installed: boltons==25.0.0
Audited 1 package in 0.24ms
```
</details>


### Platform

Windows 11 x64

### Version

0.6.2

### Python version

3.12.5

---

_Label `bug` added by @Winand on 2025-02-23 09:21_

---

_Comment by @Winand on 2025-02-23 09:32_

Ah! I see now that I was wrong, sorry.

What really breaks my setup is `[tool.uv]` section in my `pyproject.toml`. I think uv doesn't read `D:\uv.toml` if it finds that `[tool.uv]` section, [as said in docs](https://docs.astral.sh/uv/configuration/files/).

I think as of now it's not possible to put some preferences in _pyproject.toml_ and others in _uv.toml_. It's a pity that it's not possible to tell uv that settings in pyproject.toml should override settings in uv.toml, so it doesn't stop its search for settings on pyproject.toml file.

---

_Comment by @charliermarsh on 2025-02-23 23:55_

It is possible. You can put settings like `uv.toml` in the user- or system-level `uv.toml` file. See: https://docs.astral.sh/uv/configuration/files/.

---

_Comment by @charliermarsh on 2025-02-23 23:56_

Specifically, you want `%SYSTEMDRIVE%\ProgramData\uv\uv.toml` on Windows. We just don't look for arbitrary `uv.toml` files in the parent directories.

---

_Closed by @charliermarsh on 2025-02-23 23:56_

---

_Comment by @Winand on 2025-02-24 06:08_

@charliermarsh 
> We just don't look for arbitrary uv.toml files in the parent directories.

Well, if there's no `[tool.uv]` section in project.toml then uv _does_ look for `uv.toml` in parent directories.

I know about `%SYSTEMDRIVE%\ProgramData\uv\uv.toml` but it's not possible to set one _cache-dir_ per drive using system-level configuration file. But ok, I can live with that.

---
