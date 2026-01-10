```yaml
number: 556
title: "Support pointing to a system-installation Python executable with `--python` on Windows"
type: issue
state: closed
author: Andrej730
labels:
  - bug
  - windows
  - imports
assignees: []
created_at: 2025-05-31T07:39:11Z
updated_at: 2025-06-17T17:56:52Z
url: https://github.com/astral-sh/ty/issues/556
synced_at: 2026-01-10T02:08:20Z
```

# Support pointing to a system-installation Python executable with `--python` on Windows

---

_Issue opened by @Andrej730 on 2025-05-31 07:39_

### Summary

_Creating a separate issue based on https://github.com/astral-sh/ty/issues/272#issuecomment-2923071089_

I think there is an expectation from user pov that by default when you do `ty check xxx.py`, if there's no virtual environment around, that it would use system python environment. It serves well for type checking simple scripts or for other cases when there is not venv used. 

E.g. currently `python` in my system is Python 3.11, but running `ty check xxx.py` is using Python 3.13 (`Python version: Python 3.13, platform: win32` from `-vv`) and it doesn't detect the system installed dependencies.

What would be current way to accomplish this?

Currently it's focused only on venvs (https://github.com/astral-sh/ty/blob/main/docs/README.md#python-environment). There is an option to provide path to executable as `--python`, but it's only for for venvs too.

What I've tried (don't mind `UV_PYTHON_INSTALL_DIR`, I do use `uv` for system python's dependency management, but it behaves the same as usual system python, also tried with usual system Python from python.org).
```sh
ty check test.py -vv --python L:\Software\uv\UV_PYTHON_INSTALL_DIR\cpython-3.11.12-windows-x86_64-none\python.exe
ty check test.py -vv --python L:\Software\Python311\python.exe
# That probably would be too advanced
ty check test.py -vv --python "C:\Users\Andrej\.local\bin\python.exe"
```

Trying things out I've also noticed there could be an expectation that something like this would work:
```sh
uv tool run --python 3.11 ty check test.py -vv
```


The only alternative I've found is to create a "system venv" and point to it, which does work for my odd setup with `uv` managed system python that has system venv either way, but it's probably not a good general solution.
```
ty check test.py --python L:\Software\uv\systemvenv\.venv\Scripts\python.exe
ty check test.py --python L:\Software\uv\systemvenv\.venv
```





### Version

ty 0.0.1-alpha.7 (afb20f6fe 2025-05-26)

---

_Label `imports` added by @carljm on 2025-05-31 14:07_

---

_Comment by @zanieb on 2025-05-31 15:11_

You can use `--python` with a system path, e.g.:

```
❯ uvx ty check --python /usr/local -vv example.py
2025-05-31 10:07:32.697717 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
2025-05-31 10:07:32.704935 DEBUG Version: 0.0.1-alpha.7 (afb20f6fe 2025-05-26)
2025-05-31 10:07:32.705019 DEBUG Architecture: aarch64, OS: macos, case-sensitive: case-insensitive
2025-05-31 10:07:32.705041 DEBUG Searching for a project in '/Users/zb/workspace/uv'
2025-05-31 10:07:32.705236 DEBUG Resolving requires-python constraint: `>=3.8`
2025-05-31 10:07:32.705248 DEBUG Resolved requires-python constraint to: 3.8
2025-05-31 10:07:32.705273 DEBUG Project without `tool.ty` section: '/Users/zb/workspace/uv'
2025-05-31 10:07:32.705286 DEBUG Searching for a user-level configuration at `/Users/zb/.config/ty/ty.toml`
2025-05-31 10:07:32.705305 INFO Defaulting to python-platform `darwin`
2025-05-31 10:07:32.705316 DEBUG Defaulting `src.root` to `.`
2025-05-31 10:07:32.705327 INFO Python version: Python 3.8, platform: darwin
2025-05-31 10:07:32.705336 DEBUG Adding first-party search path '/Users/zb/workspace/uv'
2025-05-31 10:07:32.705361 DEBUG Using vendored stdlib
2025-05-31 10:07:32.705725 DEBUG Resolving `--python` argument: /usr/local
2025-05-31 10:07:32.705752 DEBUG Attempting to parse virtual environment metadata at '/usr/local/pyvenv.cfg'
2025-05-31 10:07:32.705761 DEBUG Searching for site-packages directory in `sys.prefix` path `/usr/local`
2025-05-31 10:07:32.705871 DEBUG Resolved site-packages directories for this environment are: ["/usr/local/lib/python3.14/site-packages"]
2025-05-31 10:07:32.705881 DEBUG Adding site-packages search path '/usr/local/lib/python3.14/site-packages'
...
```

We don't infer the Python version from there yet, but we could in the future (as in https://github.com/astral-sh/ruff/pull/18057).

I think we could tweak the logic for `--python <executable>` to work in more cases, too.

The general idea is that robust system Python environment detection is quite complicated, and we'll leverage uv for that functionality eventually. It's intentionally minimal right now.

---

_Comment by @Andrej730 on 2025-05-31 15:47_

Oh, I see using a directory path (not an executable path) works.
```sh
ty check test.py -vv --python L:\Software\uv\UV_PYTHON_INSTALL_DIR\cpython-3.11.12-windows-x86_64-none
ty check test.py -vv --python L:\Software\Python311
```
I'll use a shortcut for now that adds `--python xxx\yyy` to `ty check` call to solve the problem with the system dependencies.

---

_Comment by @AlexWaygood on 2025-05-31 17:59_

Ah, using `--python` to point to an executable should work as well, but I think our support for that right now might be buggy on Windows, looking at it! On Unix, the Python executable for a system or virtual environment is located at `<sys.prefix>/bin/python`. But on Windows, the Python executable is at a different location for a system environment than it is for a virtual environment. For a virtual environment, the executable on Windows will be located at `<sys.prefix>\Scripts\python.exe`, but for a system environment, the executable will be located at `<sys.prefix>\python.exe`.

If I'm correct here, I think the logic here needs to be updated for Windows: https://github.com/astral-sh/ruff/blob/aa1fad61e0f9fe0c7faac876f2ef55cd3817fc6c/crates/ty_python_semantic/src/module_resolver/resolver.rs#L260-L278

---

_Label `windows` added by @AlexWaygood on 2025-05-31 17:59_

---

_Renamed from "ty to recognize system python and it's packages" to "Support pointing to a Python executable with `--python` on Windows" by @AlexWaygood on 2025-05-31 18:00_

---

_Renamed from "Support pointing to a Python executable with `--python` on Windows" to "Support pointing to a system-installation Python executable with `--python` on Windows" by @AlexWaygood on 2025-05-31 18:00_

---

_Label `bug` added by @AlexWaygood on 2025-05-31 18:06_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-06-01 18:11_

---

_Closed by @AlexWaygood on 2025-06-05 07:19_

---

_Comment by @Andrej730 on 2025-06-17 17:56_

Thank you, can confirm as fixed.

---
