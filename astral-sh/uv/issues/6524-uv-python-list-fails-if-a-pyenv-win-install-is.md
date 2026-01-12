```yaml
number: 6524
title: "`uv python list` fails if a pyenv-win install is included in the windows registry"
type: issue
state: closed
author: DavidCEllis
labels:
  - bug
  - help wanted
  - windows
  - uv python
assignees: []
created_at: 2024-08-23T14:56:29Z
updated_at: 2024-08-29T20:48:23Z
url: https://github.com/astral-sh/uv/issues/6524
synced_at: 2026-01-12T15:59:05Z
```

# `uv python list` fails if a pyenv-win install is included in the windows registry

---

_@DavidCEllis_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

If a version of python is installed via pyenv with the `--register` flag then subsequently `uv` will fail to list python installs as it attempts to query metadata of a file that does not exist.

It looks like this is caused because `uv` makes the assumption that `py --list-paths` will return paths with only `<major>.<minor>` version information while pyenv installs are shown with `<major>.<minor>.<micro>`[^1]. The regex is then capturing the `<micro>` part of the version as part of the path to the python executable.

https://github.com/astral-sh/uv/blob/bbcd10d3cc7710b8c33955b91915e8463c2b6b9b/crates/uv-python/src/py_launcher.rs#L36-L39

Commands:
```
pyenv install 3.12.5 --register
uv python list
```

Full output:

```
C:\>uv --version
uv 0.3.2 (c5440001c 2024-08-23)

C:\>py --list
No installed Pythons found!

C:\>uv python list
cpython-3.12.5-windows-x86_64-none     <download available>
cpython-3.11.9-windows-x86_64-none     <download available>
cpython-3.10.14-windows-x86_64-none    <download available>
cpython-3.9.19-windows-x86_64-none     <download available>
cpython-3.8.19-windows-x86_64-none     <download available>
pypy-3.7.13-windows-x86_64-none        <download available>

C:\>pyenv install 3.12.5 --register
:: [Info] ::  Mirror: https://www.python.org/ftp/python
:: [Installing] ::  3.12.5 ...
:: [Info] :: completed! 3.12.5

C:\>py --list-paths
 -V:3.12.5 *      C:\Users\ducks\.pyenv\pyenv-win\versions\3.12.5\python.exe

C:\>uv python list
error: Failed to query Python interpreter
  Caused by: failed to query metadata of file `C:\.5 *      C:\Users\ducks\.pyenv\pyenv-win\versions\3.12.5\python.exe`
  Caused by: The filename, directory name, or volume label syntax is incorrect. (os error 123)
```

Example of `py --list-paths` output with multiple versions of python installed and registered with pyenv and some regular installs.[^2]

```
 -V:3.13.0rc1 *   C:\Users\ducks\.pyenv\pyenv-win\versions\3.13.0rc1\python.exe
 -V:3.12.5        C:\Users\ducks\.pyenv\pyenv-win\versions\3.12.5\python.exe
 -V:3.12.5-win32  C:\Users\ducks\.pyenv\pyenv-win\versions\3.12.5-win32\python.exe
 -V:3.12          C:\Users\ducks\AppData\Local\Programs\Python\Python312\python.exe
 -V:3.12-32       C:\Users\ducks\AppData\Local\Programs\Python\Python312-32\python.exe
 -V:3.11.9        C:\Users\ducks\.pyenv\pyenv-win\versions\3.11.9\python.exe
 -V:3.10.11       C:\Users\ducks\.pyenv\pyenv-win\versions\3.10.11\python.exe
 -V:3.9.13        C:\Users\ducks\.pyenv\pyenv-win\versions\3.9.13\python.exe
 -V:3.8.10        C:\Users\ducks\.pyenv\pyenv-win\versions\3.8.10\python.exe
```

[^1]: It looks like `py` is just using the key names to get the version rather than the value of `Version` or `SysVersion` in the registry.
[^2]: This also lead to me finding out that pyenv-win registers 32bit python installs as 64bit which I've reported as a bug there.

---

_Renamed from "'uv python list' fails if a pyenv-win install is included in the windows registry" to "`uv python list` fails if a pyenv-win install is included in the windows registry" by @DavidCEllis on 2024-08-23 14:56_

---

_Label `bug` added by @charliermarsh on 2024-08-23 14:58_

---

_Label `help wanted` added by @charliermarsh on 2024-08-23 14:59_

---

_Label `windows` added by @charliermarsh on 2024-08-23 14:59_

---

_Comment by @charliermarsh on 2024-08-23 14:59_

Thank you!

---

_Label `uv python` added by @zanieb on 2024-08-23 15:04_

---

_Assigned to @konstin by @konstin on 2024-08-28 11:50_

---

_Closed by @konstin on 2024-08-29 20:48_

---
