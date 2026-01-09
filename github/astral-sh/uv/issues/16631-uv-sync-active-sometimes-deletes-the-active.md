---
number: 16631
title: uv sync --active sometimes deletes the active virtualenv
type: issue
state: open
author: cdb39
labels:
  - bug
assignees: []
created_at: 2025-11-07T09:21:01Z
updated_at: 2025-11-11T22:57:38Z
url: https://github.com/astral-sh/uv/issues/16631
synced_at: 2026-01-07T13:12:19-06:00
---

# uv sync --active sometimes deletes the active virtualenv

---

_Issue opened by @cdb39 on 2025-11-07 09:21_

### Summary

Running `uv sync --active` will delete and recreate the activated virtualenv when the following environment is used:
```
UV_PYTHON_INSTALL_DIR=.python-installs    # MUST be a relative path
UV_PYTHON_PREFERENCE=only-managed
UV_PYTHON_DOWNLOADS=automatic
```

This happens regardless of whether the active virtualenv already uses a uv managed python.

As the `--active` flag is designed for syncing to a virtualenv "outside of the project or script" I would never expect the `sync` command to delete it.

Example (from within a uv project directory)
```
export UV_PYTHON_INSTALL_DIR=.python-installs
export UV_PYTHON_PREFERENCE=only-managed
export UV_PYTHON_DOWNLOADS=automatic
uv venv foobar
source foobar/bin/activate
uv sync --active   # stdout tells you that uv "Removed virtual environment at: foobar"
```

### Platform

Linux 6.8.0-85-generic x86_64 GNU/Linux

### Version

uv 0.9.7

### Python version

_No response_

---

_Label `bug` added by @cdb39 on 2025-11-07 09:21_

---

_Comment by @zanieb on 2025-11-07 14:42_

Duplicate of https://github.com/astral-sh/uv/issues/15603

---

_Comment by @cdb39 on 2025-11-07 15:34_

Ah thanks.  Apologies for not managing to find the already open issue.

Reading through the other issue, it feels like there is still a separate (potential) bug here.

With that set of environment variables, `uv sync` _always_ treats the venv python version as mismatching, e.g. if I run `uv sync` twice, the second invocation will also recreate the active venv.

---

_Comment by @zanieb on 2025-11-07 16:29_

Oh, can you share the logs of `uv sync -vv` up to the recreated environment please?

---

_Comment by @cdb39 on 2025-11-11 22:40_

If I run with a relative path in `UV_PYTHON_INSTALL_DIR`.  This happens on any subsequent syncs as well.
```
DEBUG uv 0.8.14
DEBUG Found project root: `/home/cbh/tmp/venv-deletes`
DEBUG No workspace root found, using project root
TRACE Checking lock for `/home/cbh/tmp/venv-deletes` at `/tmp/uv-ec9844c115760107.lock`
DEBUG Acquired lock for `/home/cbh/tmp/venv-deletes`
DEBUG Reading Python requests from version file at `/home/cbh/tmp/venv-deletes/.python-version`
DEBUG Using Python request `3.10` from version file at `.python-version`
DEBUG Using active virtual environment `foobar` instead of project environment `.venv`
DEBUG Checking for Python environment at: `foobar`
TRACE Found cached interpreter info for Python 3.10.18, skipping query of: foobar/bin/python3
DEBUG The project environment's Python version satisfies the request: `Python 3.10`
TRACE The project environment's Python version meets the Python requirement: `>=3.10`
DEBUG Ignoring Python interpreter at `/home/cbh/tmp/venv-deletes/foobar/bin/python3`: only managed interpreters allowed
DEBUG The project environment's Python interpreter does not meet the Python preference: `only managed`
DEBUG Searching for Python 3.10 in managed installations
DEBUG Searching for managed installations at `.python-installs`
TRACE Found `ld` path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "Usage: ld.so [OPTION]... EXECUTABLE-FILE [ARGS-FOR-PROGRAM...]\nYou have invoked `ld.so', the helper program for shared library executables.\nThis program usually lives in the file `/lib/ld.so', and special directives\nin executable files using ELF shared libraries tell the system's program\nloader to load the helper program from this file.  This helper program loads\nthe shared libraries needed by the program executable, prepares the program\nto run, and runs it.  You may invoke this helper program directly from the\ncommand line to load and run an ELF executable file; this is like executing\nthat file itself, but always uses this helper program from the file you\nspecified, instead of the helper program file specified in the executable\nfile you run.  This is mostly of use for maintainers to test new versions\nof this helper program; chances are you did not intend to run this program.\n\n  --list                list all dependencies and how they are resolved\n  --verify              verify that given object really is a dynamically linked\n\t\t\tobject we can handle\n  --inhibit-cache       Do not use /etc/ld.so.cache\n  --library-path PATH   use given PATH instead of content of the environment\n\t\t\tvariable LD_LIBRARY_PATH\n  --inhibit-rpath LIST  ignore RUNPATH and RPATH information in object names\n\t\t\tin LIST\n  --audit LIST          use objects named in LIST as auditors\n  --preload LIST        preload objects named in LIST\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
DEBUG Found managed installation `cpython-3.10.18-linux-x86_64-gnu`
TRACE Found cached interpreter info for Python 3.10.18, skipping query of: .python-installs/cpython-3.10.18-linux-x86_64-gnu/bin/python3.10
DEBUG Found `cpython-3.10.18-linux-x86_64-gnu` at `/home/cbh/tmp/venv-deletes/.python-installs/cpython-3.10.18-linux-x86_64-gnu/bin/python3.10` (managed installations)
Using CPython 3.10.18
DEBUG Using active virtual environment `foobar` instead of project environment `.venv`
Removed virtual environment at: foobar
Creating virtual environment at: foobar
DEBUG Assessing Python executable as base candidate: /home/cbh/tmp/venv-deletes/.python-installs/cpython-3.10.18-linux-x86_64-gnu/bin/python3.10
DEBUG Using base executable for virtual environment: /home/cbh/tmp/venv-deletes/.python-installs/cpython-3.10.18-linux-x86_64-gnu/bin/python3.10
DEBUG Released lock at `/tmp/uv-ec9844c115760107.lock`
TRACE Checking lock for `foobar` at `foobar/.lock`
DEBUG Acquired lock for `foobar`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: venv-deletes @ file:///home/cbh/tmp/venv-deletes
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 3ms
DEBUG Using request timeout of 30s
Audited in 0.08ms
DEBUG Released lock at `/home/cbh/tmp/venv-deletes/foobar/.lock`
```

If I run with an absolute path
```
DEBUG uv 0.8.14
DEBUG Found project root: `/home/cbh/tmp/venv-deletes`
DEBUG No workspace root found, using project root
TRACE Checking lock for `/home/cbh/tmp/venv-deletes` at `/tmp/uv-ec9844c115760107.lock`
DEBUG Acquired lock for `/home/cbh/tmp/venv-deletes`
DEBUG Reading Python requests from version file at `/home/cbh/tmp/venv-deletes/.python-version`
DEBUG Using Python request `3.10` from version file at `.python-version`
DEBUG Using active virtual environment `foobar` instead of project environment `.venv`
DEBUG Checking for Python environment at: `foobar`
TRACE Found cached interpreter info for Python 3.10.18, skipping query of: foobar/bin/python3
DEBUG The project environment's Python version satisfies the request: `Python 3.10`
TRACE The project environment's Python version meets the Python requirement: `>=3.10`
TRACE The virtual environment's Python interpreter meets the Python preference: `only managed`
DEBUG Released lock at `/tmp/uv-ec9844c115760107.lock`
TRACE Checking lock for `foobar` at `foobar/.lock`
DEBUG Acquired lock for `foobar`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: venv-deletes @ file:///home/cbh/tmp/venv-deletes
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 2ms
DEBUG Using request timeout of 30s
Audited in 0.00ms
DEBUG Released lock at `/home/cbh/tmp/venv-deletes/foobar/.lock`
```

I wonder if it's because the `home` in `pyvenv.cfg` ends up being the resolved path, so it doesn't match as being within the python install directory since the path strings are different, even though the resolved paths are the same.

---

_Comment by @zanieb on 2025-11-11 22:57_

Hm I presume it's somehow breaking the `is_managed` check?

https://github.com/astral-sh/uv/blob/60a811e715d1d1919afd73b0e4dec4c4be8aa3f9/crates/uv-python/src/interpreter.rs#L301-L317

https://github.com/astral-sh/uv/blob/caf49f845f1112a242f93f6860438e8ada324d6c/crates/uv-python/src/managed.rs#L129-L147

---
