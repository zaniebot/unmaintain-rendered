```yaml
number: 11288
title: "`uv run --python <path> <tool>` always recreates virtualenvironment"
type: issue
state: closed
author: LordAro
labels:
  - bug
assignees: []
created_at: 2025-02-06T19:06:17Z
updated_at: 2025-04-09T16:36:40Z
url: https://github.com/astral-sh/uv/issues/11288
synced_at: 2026-01-10T03:41:47Z
```

# `uv run --python <path> <tool>` always recreates virtualenvironment

---

_Issue opened by @LordAro on 2025-02-06 19:06_

### Summary

Running `uv run --python <path> <tool>`  appears to always remove and recreate the virtual environment every time any command is run:

```
[project]
name = "foo"
requires-python = "~=3.12.0"
version = "0.0"

dependencies = [
"gcovr>=8.3",
]
```

```
$ # my custom python
$ wget https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.12.7%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
$ tar xf cpython-3.12.7+20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
$ mv python my-custom-python-here
$ # uv
$ uv run --python my-custom-python-here gcovr --version
Using CPython 3.12.7 interpreter at: python/bin/python3
Creating virtual environment at: .venv
Installed 6 packages in 14ms
gcovr 8.3

Copyright (c) 2013-2025 the gcovr authors
Copyright (c) 2013 Sandia Corporation.
Under the terms of Contract DE-AC04-94AL85000 with Sandia Corporation,
the U.S. Government retains certain rights in this software.
lordaro@Artemis:~/uv-test$ uv run --python my-custom-python-here gcovr --version
Using CPython 3.12.7 interpreter at: python/bin/python3
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Installed 6 packages in 15ms
gcovr 8.3

Copyright (c) 2013-2025 the gcovr authors
Copyright (c) 2013 Sandia Corporation.
Under the terms of Contract DE-AC04-94AL85000 with Sandia Corporation,
the U.S. Government retains certain rights in this software.
```

Output with --verbose shows that it is using this custom python install and the venv is created accordingly, but it seems to be always "unsatisfied" with its version:

```
...
DEBUG Discovered project `foo` at: /home/lordaro/uv-test
DEBUG Acquired lock for `/home/lordaro/uv-test`
DEBUG Using Python request `python` from explicit request
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version does not satisfy `python`
DEBUG Checking for Python interpreter in directory `python`
Using CPython 3.12.7 interpreter at: python/bin/python3
Removed virtual environment at: .venv
Creating virtual environment at: .venv
...
```

This is also always the case with an absolute path as well as relative.

`uv run --python 3.12` does _not_ exhibit this behaviour

### Platform

Ubuntu 24.04 (WSL)

### Version

0.5.29

### Python version

N/A

---

_Label `bug` added by @LordAro on 2025-02-06 19:06_

---

_Comment by @zanieb on 2025-02-06 19:14_

I can't reproduce this

```
❯ uv run -v --python /Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13 python --version
DEBUG uv 0.5.29 (ca73c4754 2025-02-05)
DEBUG Found project root: `/Users/zb/workspace/uv/example`
DEBUG Project is contained in non-workspace project: `/Users/zb/workspace/uv`
DEBUG No workspace root found, using project root
DEBUG Discovered project `example` at: /Users/zb/workspace/uv/example
DEBUG Acquired lock for `/Users/zb/workspace/uv/example`
DEBUG Using Python request `/Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13` from explicit request
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `/Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13`
...
```


---

_Comment by @zanieb on 2025-02-06 19:17_

Here's one where the environment is invalidated (because I changed Python versions), then re-used

```
❯ uv run -v --python /Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12 python --version
DEBUG uv 0.5.29 (ca73c4754 2025-02-05)
DEBUG Found project root: `/Users/zb/workspace/uv/example`
DEBUG Project is contained in non-workspace project: `/Users/zb/workspace/uv`
DEBUG No workspace root found, using project root
DEBUG Discovered project `example` at: /Users/zb/workspace/uv/example
DEBUG Acquired lock for `/Users/zb/workspace/uv/example`
DEBUG Using Python request `/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12` from explicit request
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version does not satisfy `/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12`
DEBUG Checking for Python interpreter at path `/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12`
Using CPython 3.12.6 interpreter at: /Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12
Removed virtual environment at: .venv
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12
DEBUG Using base executable for virtual environment: /Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12
DEBUG Released lock at `/var/folders/6p/k5sd5z7j31b31pq4lhn0l8d80000gn/T/uv-cff78d0b34cfcb51.lock`
DEBUG Using request timeout of 30s
DEBUG Ignoring existing lockfile due to change in Python requirement: `>=3.12` vs. `>=3.12, <4`
DEBUG Found static `pyproject.toml` for: example @ file:///Users/zb/workspace/uv/example
DEBUG Project is contained in non-workspace project: `/Users/zb/workspace/uv`
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12, <4
DEBUG Adding direct dependency: example*
DEBUG Searching for a compatible version of example @ file:///Users/zb/workspace/uv/example (*)
DEBUG Adding direct dependency: ruff>=0.9.4
DEBUG Found stale response for: https://pypi.org/simple/ruff/
DEBUG Sending revalidation request for: https://pypi.org/simple/ruff/
DEBUG Found not-modified response for: https://pypi.org/simple/ruff/
DEBUG Searching for a compatible version of ruff (>=0.9.4)
DEBUG Selecting: ruff==0.9.4 [preference] (ruff-0.9.4-py3-none-linux_armv6l.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b6/f8/3fafb7804d82e0699a122101b5bee5f0d6e17c3a806dcbc527bb7d3f5b7a/ruff-0.9.4-py3-none-linux_armv6l.whl.metadata
DEBUG Tried 2 versions: example 1, ruff 1
DEBUG all marker environments resolution took 0.050s
Resolved 2 packages in 59ms
DEBUG Using request timeout of 30s
DEBUG Requirement already cached: ruff==0.9.4
Installed 1 package in 2ms
 + ruff==0.9.4
DEBUG Using Python 3.12.6 interpreter at: /Users/zb/workspace/uv/example/.venv/bin/python
DEBUG Running `python --version`
DEBUG Spawned child 45184 in process group 45183
Python 3.12.6
DEBUG Command exited with code: 0
❯ uv run -v --python /Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12 python --version
DEBUG uv 0.5.29 (ca73c4754 2025-02-05)
DEBUG Found project root: `/Users/zb/workspace/uv/example`
DEBUG Project is contained in non-workspace project: `/Users/zb/workspace/uv`
DEBUG No workspace root found, using project root
DEBUG Discovered project `example` at: /Users/zb/workspace/uv/example
DEBUG Acquired lock for `/Users/zb/workspace/uv/example`
DEBUG Using Python request `/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12` from explicit request
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12`
DEBUG Released lock at `/var/folders/6p/k5sd5z7j31b31pq4lhn0l8d80000gn/T/uv-cff78d0b34cfcb51.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: example @ file:///Users/zb/workspace/uv/example
DEBUG Project is contained in non-workspace project: `/Users/zb/workspace/uv`
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 2 packages in 0.83ms
DEBUG Using request timeout of 30s
DEBUG Requirement already installed: ruff==0.9.4
Audited 1 package in 0.03ms
DEBUG Using Python 3.12.6 interpreter at: /Users/zb/workspace/uv/example/.venv/bin/python3
DEBUG Running `python --version`
DEBUG Spawned child 45252 in process group 45250
Python 3.12.6
DEBUG Command exited with code: 0
```

---

_Label `needs-mre` added by @zanieb on 2025-02-06 19:17_

---

_Comment by @LordAro on 2025-02-06 19:20_

Ah, you're using the python executable as the argument to `--python`, I'm just using the containing directory (which is also valid?)

---

_Comment by @zanieb on 2025-02-06 19:23_

Ah interesting, yes I can reproduce with that

```
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version does not satisfy `/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none`
DEBUG Checking for Python interpreter in directory `/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none`
Using CPython 3.12.6 interpreter at: /Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3
```

That _might_ be a bug? Let me see why that's happening.

---

_Label `needs-mre` removed by @zanieb on 2025-02-06 19:23_

---

_Comment by @zanieb on 2025-02-06 19:25_

The logic we use is

https://github.com/astral-sh/uv/blob/ec480bd3ee43da2f5f15e7c6383c334e78894c31/crates/uv-python/src/discovery.rs#L1479-L1480

Which definitely would return `false` in this case. The `--python <dir>` request is generally intended to target the environment in that directory (e.g., in `uv pip install`), but in projects we don't ever do that — we resolve the interpreter and use it to create the project environment.

---

_Comment by @LordAro on 2025-02-07 08:16_

Some justification for using the folder rather than the executable - I'm looking at having a cross platform command that works for both Windows & Linux - Windows puts the executable in the python root, Linux has a `bin` folder

---

_Closed by @zanieb on 2025-02-12 18:46_

---

_Closed by @zanieb on 2025-02-12 18:46_

---

_Comment by @LordAro on 2025-04-09 16:36_

I'm still seeing this as an issue with uv 0.6.13 on Windows (MSYS2/UCRT64)

```
$ uv run -vvv --python auxd/python -- pip --version
DEBUG uv 0.6.13 (a0f5c7250 2025-04-07)
DEBUG Found project root: `C:\Users\cpigott\dev\foobar`
DEBUG No workspace root found, using project root
DEBUG Discovered project `foobar` at: C:\Users\cpigott\dev\foobar
TRACE Checking lock for `C:\Users\cpigott\dev\foobar` at `C:\Users\cpigott\AppData\Local\Temp\uv-dcf159be3df1a52e.lock`
DEBUG Acquired lock for `C:\Users\cpigott\dev\foobar`
DEBUG Using Python request `C:/Users/cpigott/dev/foobar/auxd/python` from explicit request
DEBUG Checking for Python environment at `.venv`
TRACE Cached interpreter info for Python 3.12.9, skipping probing: .venv\Scripts\python.exe
DEBUG The virtual environment's Python version does not satisfy `C:/Users/cpigott/dev/foobar/auxd/python`
DEBUG Checking for Python interpreter in directory `auxd/python`
TRACE Cached interpreter info for Python 3.12.9, skipping probing: auxd/python\python.exe
Using CPython 3.12.9 interpreter at: auxd\python\python.exe
Removed virtual environment at: .venv
Creating virtual environment at: .venv
```

.venv/pyvenv.cfg contains `home = C:\Users\cpigott\dev\foobar\auxd\python` as I'd expect

I've tried with and without absolute path on the commandline, no difference

---
