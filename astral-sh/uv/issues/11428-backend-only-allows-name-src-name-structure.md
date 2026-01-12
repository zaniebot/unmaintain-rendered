```yaml
number: 11428
title: "Backend only allows `<name>/src/<name>` structure"
type: issue
state: closed
author: chrisrodrigue
labels:
  - enhancement
assignees: []
created_at: 2025-02-11T21:41:33Z
updated_at: 2025-03-07T14:20:01Z
url: https://github.com/astral-sh/uv/issues/11428
synced_at: 2026-01-12T16:00:36Z
```

# Backend only allows `<name>/src/<name>` structure

---

_@chrisrodrigue_

### Summary

Trying to convert [pyserial](https://github.com/pyserial/pyserial/) to an in-project dependency (as either workspace member or path dependency under `albatross/packages`) is tricky.

Although the package name is `pyserial`, it actually installs a package named `serial` into the virtual environment. There is no way to indicate this to the uv backend, which enforces a strict `<name>/src/<name>` for installation.

[setup.py](https://github.com/pyserial/pyserial/blob/7aeea35429d15f3eefed10bbb659674638903e3a/setup.py#L60) permits specifying an arbitrary package layout, but this does not seem to be supported in the `pyproject.toml` spec or any `tool.uv` settings. Nested workspaces are not supported either.

Sample subpackage `pyproject.toml`:

```toml
[build-system]
requires = ["uv>=0.5,<0.6"]
build-backend = "uv"

[project]
name = "pyserial"
version = "3.5"
requires-python = ">=3.12"
dependencies = []
```

Sample root `pyproject.toml`:

```toml
[build-system]
requires = ["uv>=0.5,<0.6"]
build-backend = "uv"

[project]
name = "albatross"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  "pyserial==3.5",
]

[tool.uv.sources]
pyserial = { path = "packages/pyserial" }
```

### Error

```cmd
  × Failed to build `pyserial @ file:///C:/Users/user/dev/albatross/packages/pyserial`
  ╰─▶ Expected a Python module with an `__init__.py` at: `packages\pyserial\src\pyserial`
  help: `pyserial` was included because `albatross` (v0.1.0) depends on `pyserial`
```

In the case of pyserial, you cannot actually `import pyserial`. You can only do `import serial`, `import  serial.tools`, `import serial.urlhandler`, or `import serial.threaded`. 

To support these types of projects, there could be a way to specify a different module name and subpackages to the builder. 

i.e.
```toml
[build-system]
requires = ["uv>=0.5,<0.6"]
build-backend = "uv"

[project]
name = "pyserial"
version = "3.5"
requires-python = ">=3.12"
dependencies = []

[tool.uv.build-backend]
module-name = "serial" # module path is now pyserial/src/serial
packages = ["serial", "serial.tools", "serial.urlhandler",  "serial.threaded"]
```
### Platform

Windows 11 x86_64

### Version

uv 0.5.30

### Python version

Python 3.13.1

---

_Label `bug` added by @chrisrodrigue on 2025-02-11 21:41_

---

_Comment by @chrisrodrigue on 2025-02-11 21:54_

Main tracking issue: #8779

---

_Renamed from "Backend expects `<package-name>/src/<package-name>` structure for in-project packages" to "Backend only allows `<name>/src/<name>` structure" by @chrisrodrigue on 2025-02-12 17:45_

---

_Label `bug` removed by @konstin on 2025-03-04 14:49_

---

_Label `enhancement` added by @konstin on 2025-03-04 14:49_

---

_Closed by @konstin on 2025-03-07 14:20_

---

_Closed by @konstin on 2025-03-07 14:20_

---
