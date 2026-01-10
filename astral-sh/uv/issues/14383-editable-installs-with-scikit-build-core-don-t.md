---
number: 14383
title: "Editable Installs with scikit-build-core don't work"
type: issue
state: open
author: fizzxed
labels:
  - bug
  - external
assignees: []
created_at: 2025-07-01T03:09:33Z
updated_at: 2025-11-04T02:10:32Z
url: https://github.com/astral-sh/uv/issues/14383
synced_at: 2026-01-10T01:25:44Z
---

# Editable Installs with scikit-build-core don't work

---

_Issue opened by @fizzxed on 2025-07-01 03:09_

### Summary

https://github.com/astral-sh/uv/pull/7857#issuecomment-2412479901 states that scikit-build-core should work fine with build isolation. But if we try an editable rebuild it doesn't seem to work. 


To reproduce:
1. `uv init --lib --build-backend scikit hello`
2. Edit pyproject.toml adding the line `editable.rebuild = true` under `[tool.scikit-build]`
```
> cat hello/pyproject.toml
name = "hello"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "...", email = "..." }
]
requires-python = ">=3.12"
dependencies = []

[tool.scikit-build]
minimum-version = "build-system.requires"
build-dir = "build/{wheel_tag}"
editable.rebuild = true

[build-system]
requires = ["scikit-build-core>=0.10", "pybind11"]
build-backend = "scikit_build_core.build"
```
3. `uv run python -c "import hello"`
```
Running cmake --build & --install in /proj/build/cp312-cp312-linux_x86_64
[0/1] Re-running CMake...
CMake Error at CMakeLists.txt:5 (find_package):
  Could not find a package configuration file provided by "pybind11" with any
  of the following names:

    pybind11Config.cmake
    pybind11-config.cmake

  Add the installation prefix of "pybind11" to CMAKE_PREFIX_PATH or set
  "pybind11_DIR" to a directory containing one of the above files.  If
  "pybind11" provides a separate development package or SDK, be sure it has
  been installed.


-- Configuring incomplete, errors occurred!
FAILED: build.ninja

/usr/bin/cmake --regenerate-during-build -S/proj/hello -B/proj/hello/build/cp312-cp312-linux_x86_64
ninja: error: rebuilding 'build.ninja': subcommand failed
ERROR: None
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/proj/hello/src/hello/__init__.py", line 1, in <module>
    from hello._core import hello_from_bin
  File "<frozen importlib._bootstrap>", line 1360, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1322, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 1262, in _find_spec
  File "/proj/hello/.venv/lib/python3.12/site-packages/_hello_editable.py", line 149, in find_spec
    self.rebuild()
  File "/proj/hello/.venv/lib/python3.12/site-packages/_hello_editable.py", line 204, in rebuild
    result.check_returncode()
  File "/users/<USER>/.local/share/uv/python/cpython-3.12.10-linux-x86_64-gnu/lib/python3.12/subprocess.py", line 502, in check_returncode
    raise CalledProcessError(self.returncode, self.args, self.stdout,
subprocess.CalledProcessError: Command '['cmake', '--build', '.']' returned non-zero exit status 1.
```


### Platform

Linux 4.18.0-513.11.1.el8_9.x86_64 x86_64 GNU/Linux

### Version

uv 0.7.17

### Python version

Python 3.12.10

---

_Label `bug` added by @fizzxed on 2025-07-01 03:09_

---

_Renamed from "Editable Installs with scikit-build-core doesn't work" to "Editable Installs with scikit-build-core don't work" by @fizzxed on 2025-07-01 03:28_

---

_Comment by @konstin on 2025-07-01 10:18_

I can reproduce the problem using pip too, so this is a scikit-build-core bug, not a uv bug:

```
pip install -e .
python -c "import hello"
```

---

_Referenced in [scikit-build/scikit-build-core#1112](../../scikit-build/scikit-build-core/issues/1112.md) on 2025-07-01 20:45_

---

_Comment by @henryiii on 2025-07-01 20:50_

This is a general issue with all isolated builds and editable installs. If you make an isolated build, pip or uv makes an isolated environment, installs scikit-build-core and pybind11 into it, then builds the package pointing at that environment. Then it throws it away once it's done. But for an editable install, you need those requirements later to rebuild! Scikit-build-core actually doesn't need itself to build, but it does need any other packages you request, like pybind11. You'll need to do a non-editable install, and include the build requirements. This is true for all editable backends that need dependencies when rebuilding.

---

_Comment by @fizzxed on 2025-07-01 21:22_

Is this then simply an incompatibility between editable installs and isolated builds, you can only have one but not the other when you have a python project with extensions? Or can anything be done to support both?

---

_Comment by @henryiii on 2025-07-01 21:35_

If you have no build dependencies except scikit-build-core, then you can support both. If you use pybind11 as a submodule, for example, that would also work (I'd recommend using an override in that case, so you can still pull it normally for non editable installs). Otherwise, it's just a limitation of the way editable installs were designed, they are not properly compatible with isolated environments. PS: That also includes cmake and ninja; those should already in installed; if you rely on scikit-build-core to request them, then that will also break rerunning an isolated install, since they would also be gone.

(Possibly this could improve in the future in tools like uv, I could imagine uv installing the isolated environment into some not-as-temporary location for editable installs, and only removing it when making a new one, perhaps?)

---

_Comment by @fizzxed on 2025-07-01 21:48_

> (Possibly this could improve in the future in tools like uv, I could imagine uv installing the isolated environment into some not-as-temporary location for editable installs, and only removing it when making a new one, perhaps?)

I think that would be a nice feature. It would certainly make the development experience smoother for those trying to use the uv project api for Python projects that have C/C++ extensions. For now maybe just using uv's pip interface is the way to go until that hopeful future?

---

_Label `external` added by @charliermarsh on 2025-07-02 00:52_

---

_Label `bug` removed by @charliermarsh on 2025-07-02 00:52_

---

_Label `bug` added by @charliermarsh on 2025-07-02 00:52_

---

_Referenced in [astral-sh/uv#14940](../../astral-sh/uv/issues/14940.md) on 2025-07-28 13:26_

---

_Comment by @Joseph-Edwards on 2025-11-04 02:10_

A brief question following from @henryiii's comment:

> You'll need to do a non-editable install, and include the build requirements.

Is there setting I can specify in `pyproject.toml` to enforce this behaviour systematically? Something similar to setting the environment variable `UV_NO_EDITABLE`, perhaps.

I'm happy to elaborate on my specific use case, or to open a separate issue, if either would be desirable.

---

_Referenced in [openalea/stat_tool#4](../../openalea/stat_tool/issues/4.md) on 2025-11-17 16:22_

---
