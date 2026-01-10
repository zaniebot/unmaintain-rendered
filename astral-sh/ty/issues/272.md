```yaml
number: 272
title: "Recognize user intent when using `--python 3.13`"
type: issue
state: open
author: sharkdp
labels:
  - cli
  - configuration
  - wish
assignees: []
created_at: 2025-05-08T13:39:49Z
updated_at: 2025-11-13T17:55:05Z
url: https://github.com/astral-sh/ty/issues/272
synced_at: 2026-01-10T02:06:24Z
```

# Recognize user intent when using `--python 3.13`

---

_Issue opened by @sharkdp on 2025-05-08 13:39_

For some reason I do this wrong all the time. I run `ty check --python 3.13` and press enter before I realize that it should be `--python-version`, not `--python`. It would be nice to detect this (directory "3.13" does not exist and it successfully parses as a python version) and to show a nice error message ("did you mean `--python-version 3.13`")?
```
▶ ty check --python 3.13  # oops
ty failed
  Cause: Invalid search path settings
  Cause: Failed to discover the site-packages directory: Invalid `--python` argument: `/home/shark/ecosystem/ami-notifications-api/3.13` could not be canonicalized
```

---

_Label `wish` added by @sharkdp on 2025-05-08 13:39_

---

_Label `cli` added by @MichaReiser on 2025-05-08 14:17_

---

_Comment by @zanieb on 2025-05-08 14:20_

We have handling for this in uv https://github.com/astral-sh/uv/blob/fa8db5a8d0bfc9eb3039e788dd7c719ffe85b030/crates/uv-python/src/discovery.rs#L1389-L1491

Note we actually overload the `--python` option to support both path and version requests (we use`--python-version` where it's okay if you resolve with a _different_  target Python version than we can actually find an interpreter for). We don't treat `3.13` as a path regardless of if it exists, you'd need to do `./3.13` for that (which I think is totally fine, given the usage). We haven't seen complaints or confusion.

---

_Comment by @sharkdp on 2025-05-08 14:24_

> Note we actually overload the `--python` option to support both path and version requests

But note that `--python` has a different meaning in ty. It's not the path to an interpreter, but instead a path to a virtual env (which I do not find very intuitive, to be honest):
```
      --python <PATH>
          Path to the Python installation from which ty resolves type information and third-party dependencies.
          
          If not specified, ty will look at the `VIRTUAL_ENV` environment variable.
          
          ty will search in the path's `site-packages` directories for type information and third-party imports.
          
          This option is commonly used to specify the path to a virtual environment.
```

---

_Comment by @zanieb on 2025-05-08 14:28_

In uv, we support either a full path to the interpreter executable or to a directory (which we will treat as a Python environment and search for the interpreter in canonical locations)

e.g.

```
❯ uv python find .venv
/Users/zb/workspace/uv/.venv/bin/python3
❯ uv python find .venv/bin/python3
/Users/zb/workspace/uv/.venv/bin/python3
❯ uv python find /usr/local
/usr/local/bin/python3
```

---

_Comment by @MichaReiser on 2025-05-08 14:31_

I don't think we want full Python discovery in ty. 

---

_Comment by @zanieb on 2025-05-08 14:35_

I don't think that's full Python discovery, those are the trivial cases (i.e., look in `bin` / `Scripts` as appropriate for the platform).

---

_Comment by @zanieb on 2025-05-08 14:37_

Just to be clear, the complicated parts of "full discovery" are searching the `PATH`, looking at registry entries  and handling Microsoft Store installs (on Windows), disambiguating `CONDA_PREFIX` from the base environment, etc.

---

_Label `configuration` added by @AlexWaygood on 2025-05-11 08:00_

---

_Comment by @Andrej730 on 2025-05-30 18:11_

What's the current way to ensure `ty` is using system environment for type checking?

What I've tried:
```
ty check test.py -vv --python L:\Software\uv\UV_PYTHON_INSTALL_DIR\cpython-3.11.12-windows-x86_64-none\python.exe`
ty check test.py -vv --python "C:\Users\Andrej\.local\bin\python.exe"
```

I've also had an assumption that something like this would work:
```
uv tool run --python 3.11 ty check L:\pomoika\test_files\1_test.py -vv
```

---

_Comment by @carljm on 2025-05-30 20:02_

@Andrej730 this is a bit off-topic for this specific issue; for better visibility you could open it as a new issue (maybe with a bit more detail about what you expected to see and what happened instead), or if you use Discord you could ask it in our Discord.

---

_Assigned to @Gankra by @Gankra on 2025-11-13 17:55_

---
