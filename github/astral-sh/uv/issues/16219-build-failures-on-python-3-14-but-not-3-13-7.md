---
number: 16219
title: Build failures on python 3.14 but not 3.13.7
type: issue
state: closed
author: killcoder
labels:
  - question
assignees: []
created_at: 2025-10-10T03:20:23Z
updated_at: 2025-10-16T05:22:10Z
url: https://github.com/astral-sh/uv/issues/16219
synced_at: 2026-01-07T13:12:19-06:00
---

# Build failures on python 3.14 but not 3.13.7

---

_Issue opened by @killcoder on 2025-10-10 03:20_

### Summary

To test python 3.14 package availability, I created a brand-new project using uv init

I added some (not all) packages to the toml file and added the hatchling build system.

With both python 3.13.7 and 3.14.0 installed, I pinned python 3.13.7 and ran uv sync

```
uv sync

Using CPython 3.13.7
Creating virtual environment at: .venv
Resolved 77 packages in 2ms
Installed 77 packages in 6.93s
```

I then pinned python version to 3.14.0 and ran uv sync again

```
uv sync

Using CPython 3.14.0
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 77 packages in 3ms
  × Failed to build `fastcrc==0.3.3`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `maturin.build_wheel` failed (exit code: 1)

      [stdout]
      Running `maturin pep517 build-wheel -i C:\Users\xxxx\AppData\Local\uv\cache\builds-v0\.tmp1TG8TJ\Scripts\python.exe --compatibility off`   
      Rust not found, installing into a temporary directory

     ....

     FileNotFoundError: [WinError 2] The system cannot find the file specified

     hint: This usually indicates a problem with the package or the build environment.
```

However this time I get build failures. I also tested using uv_build in the toml instead of hatching but it produced the same result. 

This issue is not specific to fastcrc it now supports [3.14](https://github.com/overcat/fastcrc/issues/12) and I get the same results when I uncomment geoviews. 

Is there something wrong with my build setup?

```
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "beautifulsoup4",
    "bokeh",
    "colorama",
    "dask",
    "datashader",
    # "geoviews", 
    "fastcrc",
    "holoviews",
    "hvplot",
    "lxml",
    "llvmlite==0.46.0b1",
    "numba==0.63.0b1",
    "numpy",
    "panel",
    "pandas",
    "pendulum",
    "plotly",
    # "pyarrow",
]

[dependency-groups]
dev = [
    "ruff",
    "ty",
    "pyrefly",
    "bandit",
    "pylint",
    "invoke",
    "pytest",
    # "watchfiles",
]


[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

#[build-system]
#requires = ["uv_build>=0.9.1,<0.10.0"]
#build-backend = "uv_build"

[tool.hatch.build.targets.wheel]
packages = ["src/test"]

[tool.uv.build-backend]
module-name = ["test"]
```


### Platform

Windows 11 

### Version

v0.9.1

### Python version

3.13.7, 3.14.0

---

_Label `bug` added by @killcoder on 2025-10-10 03:20_

---

_Comment by @iasx on 2025-10-12 18:03_

I faced the same issue today with `fastapi[standard]` and `watchfiles`, and managed to resolve it by installing Rust with `rustup`.

`winget install Rustlang.Rustup`

Let me know if this helps.

---

_Comment by @zanieb on 2025-10-12 22:03_

These are problems with the underlying dependencies, not uv. Presumably with 3.13 you're getting pre-built wheels. They don't provide wheels for 3.14 so you're building from source and don't have their build dependencies installed.

---

_Label `bug` removed by @zanieb on 2025-10-12 22:04_

---

_Label `question` added by @zanieb on 2025-10-12 22:04_

---

_Comment by @zanieb on 2025-10-12 22:04_

(You could test with `--no-cache --no-binary` on 3.13 to verify that the issue occurs there when you don't use a pre-built wheel as well)

---

_Comment by @killcoder on 2025-10-12 22:18_

Installing Rust (and rebooting) as mentioned by @iasx solved the issue. 

Unfortunately --no-cache fails with

```
uv sync --no-cache --no-binary --prerelease=allow

hint: A source distribution is required for `pywin32` because using pre-built wheels is disabled for all packages (i.e., with `--no-binary`)
  help: `pyviz-comms` (v3.0.6) was included because `test` (v0.1.0) depends on `panel` (v1.7.5) which depends on `pyviz-comms`
```

However removing panel from the toml did result in a successful build

```
uv sync --no-cache --no-binary --prerelease=allow

Resolved 91 packages in 593ms                                                                                                                                                                                                                                                                                 
      Built test @ file:///C:/Users/xxxx/test
      Built sniffio==1.3.1
      Built watchfiles==1.1.0
      Built anyio==4.11.0
Prepared 4 packages in 2m 44s
Uninstalled 1 package in 6ms
Installed 4 packages in 41ms
 + anyio==4.11.0
 + sniffio==1.3.1
 ~ test==0.1.0 (from file:///C:/Users/xxxx/test)
 + watchfiles==1.1.0
```

Thanks for your help. 

---

_Closed by @killcoder on 2025-10-16 05:22_

---

_Referenced in [muhammad-fiaz/logly#89](../../muhammad-fiaz/logly/issues/89.md) on 2025-10-16 05:27_

---
