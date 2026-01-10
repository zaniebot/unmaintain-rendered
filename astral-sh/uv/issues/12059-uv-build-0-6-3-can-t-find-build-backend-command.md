```yaml
number: 12059
title: "uv-build 0.6.3 can't find `build-backend` command"
type: issue
state: closed
author: cthoyt
labels:
  - bug
assignees: []
created_at: 2025-03-07T22:23:08Z
updated_at: 2025-03-12T10:17:57Z
url: https://github.com/astral-sh/uv/issues/12059
synced_at: 2026-01-10T03:50:31Z
```

# uv-build 0.6.3 can't find `build-backend` command

---

_Issue opened by @cthoyt on 2025-03-07 22:23_

### Summary

I'm having a few issues with switching over to uv-build when running the following script:

```console
$ uv --preview init uvbbt --build-backend uv

$ cd uvbbt

$ cat pyproject.toml
[project]
name = "uvbbt"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "Charles Tapley Hoyt", email = "cthoyt@gmail.com" }
]
requires-python = ">=3.13"
dependencies = []

[project.scripts]
uvbbt = "uvbbt:main"

[build-system]
requires = ["uv_build>=0.6.5,<0.7"]
build-backend = "uv_build"

$ uv run uvbbt
Using CPython 3.13.1
Creating virtual environment at: .venv
      Built uvbbt @ file:///Users/cthoyt/Desktop/uvbbt
Installed 1 package in 1ms
Hello from uvbbt!

$ cat << EOF > tox.ini
[testenv:mypy]
deps =
    mypy
commands =
    mypy --ignore-missing-imports src/
EOF

# do some sneaky manual editing of the pyproject.toml
# to adjust the build backend to be `requires = ["uv_build>=0.6.3,<0.7"]`

$ uv run --with tox-uv tox -e mypy
      Built uvbbt @ file:///Users/cthoyt/Desktop/uvbbt
Uninstalled 1 package in 0.92ms
Installed 1 package in 2ms
.pkg: install_requires> /Users/cthoyt/Library/Caches/uv/archive-v0/F_Zr30UmKraoqMlEjto3B/bin/uv pip install 'uv_build<0.7,>=0.6.3'
.pkg: _optional_hooks> python /Users/cthoyt/Library/Caches/uv/archive-v0/F_Zr30UmKraoqMlEjto3B/lib/python3.13/site-packages/pyproject_api/_backend.py True uv_build
.pkg: get_requires_for_build_sdist> python /Users/cthoyt/Library/Caches/uv/archive-v0/F_Zr30UmKraoqMlEjto3B/lib/python3.13/site-packages/pyproject_api/_backend.py True uv_build
.pkg: build_sdist> python /Users/cthoyt/Library/Caches/uv/archive-v0/F_Zr30UmKraoqMlEjto3B/lib/python3.13/site-packages/pyproject_api/_backend.py True uv_build
mypy: packaging backend failed (code=1), with SystemExit: 1
Error: Unknown subcommand: build-backend (cli: ArgsOs { inner: ["/Users/cthoyt/Desktop/uvbbt/.tox/.pkg/bin/uv-build", "build-backend", "build-sdist", "/Users/cthoyt/Desktop/uvbbt/.tox/.pkg/dist"] })
Traceback (most recent call last):
  File "/Users/cthoyt/Library/Caches/uv/archive-v0/F_Zr30UmKraoqMlEjto3B/lib/python3.13/site-packages/pyproject_api/_backend.py", line 93, in run
    outcome = backend_proxy(parsed_message["cmd"], **parsed_message["kwargs"])
  File "/Users/cthoyt/Library/Caches/uv/archive-v0/F_Zr30UmKraoqMlEjto3B/lib/python3.13/site-packages/pyproject_api/_backend.py", line 34, in __call__
    return getattr(on_object, name)(*args, **kwargs)
           ~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^
  File "/Users/cthoyt/Desktop/uvbbt/.tox/.pkg/lib/python3.13/site-packages/uv_build/__init__.py", line 64, in build_sdist
    return call(args, config_settings)
  File "/Users/cthoyt/Desktop/uvbbt/.tox/.pkg/lib/python3.13/site-packages/uv_build/__init__.py", line 48, in call
    sys.exit(result.returncode)
    ~~~~~~~~^^^^^^^^^^^^^^^^^^^
SystemExit: 1
Backend: run command build_sdist with args {'sdist_directory': '/Users/cthoyt/Desktop/uvbbt/.tox/.pkg/dist', 'config_settings': None}
Backend: Wrote response {'code': 1, 'exc_type': 'SystemExit', 'exc_msg': '1'} to /var/folders/6k/r2gmmc213vl6h7bv20x624xw0000gn/T/pep517_build_sdist-p1tthnt4.json
.pkg: _exit> python /Users/cthoyt/Library/Caches/uv/archive-v0/F_Zr30UmKraoqMlEjto3B/lib/python3.13/site-packages/pyproject_api/_backend.py True uv_build
  mypy: FAIL code 1 (0.30 seconds)
  evaluation failed :( (0.33 seconds)
```

The first error because of the uv-build versioning is:

```
Using Python 3.13.1 environment at: .tox/.pkg
  × No solution found when resolving dependencies:
  ╰─▶ Because only uv-build<=0.6.3 is available and you require uv-build>=0.6.5,<0.7, we can conclude that your requirements are unsatisfiable.
.pkg: exit 1 (0.09 seconds) /Users/cthoyt/Desktop/uvbbt> /Users/cthoyt/Library/Caches/uv/archive-v0/F_Zr30UmKraoqMlEjto3B/bin/uv pip install 'uv_build<0.7,>=0.6.5' pid=25536
  mypy: FAIL code 1 (0.67 seconds)
  evaluation failed :( (0.70 seconds)
```

which can be solved by manually editing the minimum requirement to be 0.6.3 instead of 0.6.5, since I think the uv-build 0.6.5 artifact was not properly uploaded. Right now on PyPI (https://pypi.org/project/uv-build/), there's only 0.6.3. Note that I showed the output above after manually editing the pyproject.toml's build-backend to have `requires = ["uv_build>=0.6.3,<0.7"]`

Note: the issue with tox occurs both with `tox-uv` and without


### Platform

macOS 14

### Version

uv 0.6.5 (bcbcd0a1e 2025-03-06)

### Python version

Python 3.13.1

---

_Label `bug` added by @cthoyt on 2025-03-07 22:23_

---

_Renamed from "uv-build" to "Errro with new uv-build and tox" by @cthoyt on 2025-03-07 22:23_

---

_Renamed from "Errro with new uv-build and tox" to "Error with new uv-build and tox" by @cthoyt on 2025-03-07 22:23_

---

_Renamed from "Error with new uv-build and tox" to "uv-build can't find `build-backend` command" by @cthoyt on 2025-03-07 22:40_

---

_Renamed from "uv-build can't find `build-backend` command" to "uv-build 0.6.3 can't find `build-backend` command" by @cthoyt on 2025-03-07 22:40_

---

_Comment by @zanieb on 2025-03-07 23:35_

This is fixed in #12058 

---

_Comment by @cthoyt on 2025-03-12 10:17_

Can confirm that this is working in uv 0.6.6! Tested in my CI setup in https://github.com/biopragmatics/pyobo/pull/372

---

_Closed by @cthoyt on 2025-03-12 10:17_

---
