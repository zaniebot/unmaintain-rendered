```yaml
number: 7857
title: Add templates for popular build backends
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
assignees: []
merged: true
base: main
head: init-supports-more-build-backends
created_at: 2024-10-02T04:03:04Z
updated_at: 2024-12-28T17:07:23Z
url: https://github.com/astral-sh/uv/pull/7857
synced_at: 2026-01-10T11:44:29Z
```

# Add templates for popular build backends

---

_Pull request opened by @samypr100 on 2024-10-02 04:03_

## Summary

Closes #6299 

This adds basic support on `uv init` to a couple additional build backends (pure and binary ones): `pdm`, `flit`, `setuptools`, `maturin`, and `scikit-build-core`.

I didn't replace the existing `--package` flag for backwards compatibility, nor did I added a "None" build backend to imply `--no-package` at this time. Instead, currently when `--app --package` or `--lib` is specified, `--build-backend` can be used to change the existing hatchling backend with a different one.

Since this also adds some build backend that support building binaries, I also added the generation of additional files needed for the ones supported in this PR inspired by https://github.com/scientific-python/cookie

This also allows room for the upcoming uv build backend (which I assume could become the default later on) in uv init while still allowing other ones to remain functional.

## Test Plan

Added additional tests for `flit`, `maturin`, and `scikit`.
For backends that build binaries, only maturin will do so in tests since we have rust dev tooling already 😆. `scikit` does not build binaries in tests as it would require additional tooling and environment configuration to be available. For these, I manually tested them on Windows and Linux.

---

_Comment by @samypr100 on 2024-10-02 04:03_

~~Still need to add docs 📝 if the changes are acceptable~~  Added initial docs

---

_Marked ready for review by @samypr100 on 2024-10-02 12:59_

---

_Comment by @bluss on 2024-10-04 20:06_

I tested the meson build, saw this, but I'm not sure what it means.

If I build twice - the second time it will have a stale/older `build/` directory in the project. The second build it fails with an error.

building twice was done something like this where the project name is `withmeson`.

```
uv sync -v --no-cache --reinstall-package withmeson
uv sync -v --no-cache --reinstall-package withmeson
```

```
FAILED: _core.cpython-311-x86_64-linux-gnu.so.p/src_main.cpp.o 
ccache c++ -I_core.cpython-311-x86_64-linux-gnu.so.p -I. -I../.. -I/home/user/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/include/python3.11 -I/tmp/.tmprmOyLK/builds-v0/.tmp9amD3K/lib/python3.11/site-packages/pybind11/include -I/install/include/python3.11 -fvisibility=hidden -fvisibility-inlines-hidden -fdiagnostics-color=always -DNDEBUG -D_FILE_OFFSET_BITS=64 -Wall -Winvalid-pch -std=c++11 -O3 -fPIC -MD -MQ _core.cpython-311-x86_64-linux-gnu.so.p/src_main.cpp.o -MF _core.cpython-311-x86_64-linux-gnu.so.p/src_main.cpp.o.d -o _core.cpython-311-x86_64-linux-gnu.so.p/src_main.cpp.o -c ../../src/main.cpp
../../src/main.cpp:1:10: fatal error: pybind11/pybind11.h: No such file or directory
    1 | #include <pybind11/pybind11.h>
      |          ^~~~~~~~~~~~~~~~~~~~~

```

I think the reason is this: the build folder has some cached stuff. And it references an include directory in `/tmp/.tmprmOyLK/builds-v0/.tmp9amD3K/lib/python3.11/site-packages/pybind11/include`  which was from a temporary build directory used in the first build. That's now gone, with the second build.

---

_Comment by @bluss on 2024-10-04 20:14_

Also meson uses ninja from /usr/bin/ninja in this case - is that an external dependency (not covered by build requirements?)

---

_Comment by @samypr100 on 2024-10-04 23:31_

> I think the reason is this: the build folder has some cached stuff. And it references an include directory in /tmp/.tmprmOyLK/builds-v0/.tmp9amD3K/lib/python3.11/site-packages/pybind11/include which was from a temporary build directory used in the first build. That's now gone, with the second build.

@bluss I noticed the same thing and assumed this was the issue as well. I briefly searched for a way to clear the config cache via mesonpy, but didn't spent too much time on it since I'm not a meson expert.

The config derived from https://github.com/scientific-python/cookie, maybe @henryiii or @rgommers have some ideas?


---

_Comment by @henryiii on 2024-10-04 23:50_

For the ninja comment: ninja is only requested if it's not on the system (or is too old). Otherwise you wouldn't be able to support all the platforms without wheels.

---

_Comment by @rgommers on 2024-10-05 20:25_

> I think the reason is this: the build folder has some cached stuff. And it references an include directory in `/tmp/.tmprmOyLK/builds-v0/.tmp9amD3K/lib/python3.11/site-packages/pybind11/include` which was from a temporary build directory used in the first build. That's now gone, with the second build.

This indicates an issue: a _fixed_ build directory (which is then reused) is pointing at a _temporary_ build environment with a build dependency that's disappearing. That is fundamentally problematic. There are only two ways to do things correctly in principle:
1. You want full rebuilds - in that case use a temporary directory for the build dir.
2. You want to use caching for efficient rebuilds - in that case ensure that both the build environment and the build directory are in fixed locations.

Different backends may be more or less tolerant of building against build dependencies located in a tmpdir (e.g., because they redo the configure stage of the build even for incremental rebuilds), but it can't be correct to do so. 

I can't see from the traceback how the environment is set up and how the build backend is invoked, but I think that's where things go wrong. 

---

_Comment by @samypr100 on 2024-10-05 22:05_

> I can't see from the traceback how the environment is set up and how the build backend is invoked, but I think that's where things go wrong.

Attached debug logs for windows (in case this helps)

<details><summary>First uv + meson build logs</summary>
<p>

```
.\target\debug\uv.exe sync -v --no-cache
DEBUG uv 0.4.18
DEBUG Found project root: `D:\RustDev\uv\mesontest`
DEBUG Project is contained in non-workspace project: `D:\RustDev\uv`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `D:\RustDev\uv\mesontest\.python-version`
DEBUG Searching for Python 3.12 in system path or `py` launcher
DEBUG Found `cpython-3.12.7-windows-x86_64-none` at `C:\Users\Sandbox\AppData\Local\Programs\Python\Python312\python.exe` (search path)
Using CPython 3.12.7 interpreter at: C:\Users\Sandbox\AppData\Local\Programs\Python\Python312\python.exe
Creating virtual environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: mesontest @ file:///D:/RustDev/uv/mesontest
DEBUG Project is contained in non-workspace project: `D:\RustDev\uv`
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 8ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: mesontest @ file:///D:/RustDev/uv/mesontest
DEBUG Acquired lock for `C:\Users\Sandbox\AppData\Local\Temp\.tmpEsFjbV\sdists-v4\editable\3b0dc5127cb60fe2`
WARN Failed to read metadata for file: The system cannot find the file specified. (os error 2)
WARN Failed to read metadata for file: The system cannot find the file specified. (os error 2)
DEBUG Building: mesontest @ file:///D:/RustDev/uv/mesontest
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: meson-python*
DEBUG Adding direct dependency: pybind11*
DEBUG No cache entry for: https://pypi.org/simple/meson-python/
DEBUG No cache entry for: https://pypi.org/simple/pybind11/
DEBUG Searching for a compatible version of meson-python (*)
DEBUG Selecting: meson-python==0.16.0 [compatible] (meson_python-0.16.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/91/c0/104cb6244c83fe6bc3886f144cc433db0c0c78efac5dc00e409a5a08c87d/meson_python-0.16.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/13/2f/0f24b288e2ce56f51c920137620b4434a38fd80583dbbe24fc2a1656c388/pybind11-2.13.6-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for meson-python==0.16.0: meson{python_full_version >= '3.12'}>=1.2.3
DEBUG Adding transitive dependency for meson-python==0.16.0: packaging>=19.0
DEBUG Adding transitive dependency for meson-python==0.16.0: pyproject-metadata>=0.7.1
DEBUG Searching for a compatible version of pybind11 (*)
DEBUG Selecting: pybind11==2.13.6 [compatible] (pybind11-2.13.6-py3-none-any.whl)
DEBUG No cache entry for: https://pypi.org/simple/meson/
DEBUG No cache entry for: https://pypi.org/simple/packaging/
DEBUG No cache entry for: https://pypi.org/simple/pyproject-metadata/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (>=1.2.3)
DEBUG Selecting: meson==1.5.2 [compatible] (meson-1.5.2-py3-none-any.whl)
DEBUG Adding transitive dependency for meson==1.5.2: meson==1.5.2
DEBUG Adding transitive dependency for meson==1.5.2: meson{python_full_version >= '3.12'}==1.5.2
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (==1.5.2)
DEBUG Selecting: meson==1.5.2 [compatible] (meson-1.5.2-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/a6/47b9353c331318a13eb050887eacfd61eb075746285f9baf7ef7de6ae235/meson-1.5.2-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/aa/5f/bb5970d3d04173b46c9037109f7f05fc8904ff5be073ee49bb6ff00301bc/pyproject_metadata-0.8.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of meson (==1.5.2)
DEBUG Selecting: meson==1.5.2 [compatible] (meson-1.5.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=19.0)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pyproject-metadata (>=0.7.1)
DEBUG Selecting: pyproject-metadata==0.8.0 [compatible] (pyproject_metadata-0.8.0-py3-none-any.whl)
DEBUG Adding transitive dependency for pyproject-metadata==0.8.0: packaging>=19.0
DEBUG Tried 5 versions: meson 1, meson-python 1, packaging 1, pybind11 1, pyproject-metadata 1
DEBUG Split specific environment resolution took 0.276s
DEBUG Installing in meson==1.5.2, meson-python==0.16.0, packaging==24.1, pybind11==2.13.6, pyproject-metadata==0.8.0 in C:\Users\Sandbox\AppData\Local\Temp\.tmpEsFjbV\builds-v0\.tmpD9fGuR
DEBUG Identified uncached distribution: meson==1.5.2
DEBUG Identified uncached distribution: meson-python==0.16.0
DEBUG Identified uncached distribution: packaging==24.1
DEBUG Identified uncached distribution: pybind11==2.13.6
DEBUG Identified uncached distribution: pyproject-metadata==0.8.0
DEBUG Downloading and building requirements for build: meson==1.5.2, meson-python==0.16.0, packaging==24.1, pybind11==2.13.6, pyproject-metadata==0.8.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/a6/47b9353c331318a13eb050887eacfd61eb075746285f9baf7ef7de6ae235/meson-1.5.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/13/2f/0f24b288e2ce56f51c920137620b4434a38fd80583dbbe24fc2a1656c388/pybind11-2.13.6-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/91/c0/104cb6244c83fe6bc3886f144cc433db0c0c78efac5dc00e409a5a08c87d/meson_python-0.16.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/aa/5f/bb5970d3d04173b46c9037109f7f05fc8904ff5be073ee49bb6ff00301bc/pyproject_metadata-0.8.0-py3-none-any.whl
DEBUG Installing build requirements: pyproject-metadata==0.8.0, meson-python==0.16.0, packaging==24.1, pybind11==2.13.6, meson==1.5.2
DEBUG Extracting file name=PackageName("pyproject-metadata")
DEBUG Extracting file name=PackageName("meson-python")
DEBUG Extracted 6 files name=PackageName("pyproject-metadata")
DEBUG No entrypoints name=PackageName("pyproject-metadata")
DEBUG No data name=PackageName("pyproject-metadata")
DEBUG Writing extra metadata name=PackageName("pyproject-metadata")
DEBUG Writing record name=PackageName("pyproject-metadata")
DEBUG Extracted 11 files name=PackageName("meson-python")
DEBUG Extracting file name=PackageName("packaging")
DEBUG No entrypoints name=PackageName("meson-python")
DEBUG No data name=PackageName("meson-python")
DEBUG Writing extra metadata name=PackageName("meson-python")
DEBUG Writing record name=PackageName("meson-python")
DEBUG Extracted 21 files name=PackageName("packaging")
DEBUG Extracting file name=PackageName("meson")
DEBUG Extracting file name=PackageName("pybind11")
DEBUG No entrypoints name=PackageName("packaging")
DEBUG No data name=PackageName("packaging")
DEBUG Writing extra metadata name=PackageName("packaging")
DEBUG Writing record name=PackageName("packaging")
DEBUG Extracted 57 files name=PackageName("pybind11")
DEBUG Writing entrypoints name=PackageName("pybind11")
DEBUG No data name=PackageName("pybind11")
DEBUG Writing extra metadata name=PackageName("pybind11")
DEBUG Writing record name=PackageName("pybind11")
DEBUG Extracted 244 files name=PackageName("meson")
DEBUG Writing entrypoints name=PackageName("meson")
DEBUG Installing data name=PackageName("meson")
DEBUG Writing extra metadata name=PackageName("meson")
DEBUG Writing record name=PackageName("meson")
DEBUG Creating PEP 517 build environment
DEBUG Calling `mesonpy.get_requires_for_build_editable()`
DEBUG Calling `mesonpy.build_editable("C:\\Users\\Sandbox\\AppData\\Local\\Temp\\.tmpEsFjbV\\sdists-v4\\editable\\3b0dc5127cb60fe2\\2SI-UfXgTs-o2MAwpsL4m\\.tmpLXpwHC", {}, None)`
DEBUG + meson setup D:\RustDev\uv\mesontest D:\RustDev\uv\mesontest\build\cp312 -Dbuildtype=release -Db_ndebug=if-release -Db_vscrt=md --native-file=D:\RustDev\uv\mesontest\build\cp312\meson-python-native-file.ini
DEBUG The Meson build system
DEBUG Version: 1.5.2
DEBUG Source dir: D:\RustDev\uv\mesontest
DEBUG Build dir: D:\RustDev\uv\mesontest\build\cp312
DEBUG Build type: native build
DEBUG Project name: mesontest
DEBUG Project version: 0.1.0
DEBUG C++ compiler for the host machine: cl (msvc 19.39.33519 "Microsoft (R) C/C++ Optimizing Compiler Version 19.39.33519 for x64")
DEBUG C++ linker for the host machine: link link 14.39.33519.0
DEBUG Host machine cpu family: x86_64
DEBUG Host machine cpu: x86_64
DEBUG Program python found: YES (C:\Users\Sandbox\AppData\Local\Temp\.tmpEsFjbV\builds-v0\.tmpD9fGuR\Scripts\python.exe)
DEBUG Found pkg-config: YES (D:\msys64\mingw64\bin\pkg-config.exe) 2.3.0
DEBUG pybind11-config found: YES (C:\Users\Sandbox\AppData\Local\Temp\.tmpEsFjbV\builds-v0\.tmpD9fGuR\Scripts\pybind11-config.exe) 2.13.6
DEBUG Run-time dependency pybind11 found: YES 2.13.6
DEBUG Run-time dependency python found: YES 3.12
DEBUG Build targets in project: 1
DEBUG
DEBUG mesontest 0.1.0
DEBUG
DEBUG   User defined options
DEBUG     Native files: D:\RustDev\uv\mesontest\build\cp312\meson-python-native-file.ini
DEBUG     buildtype   : release
DEBUG     b_ndebug    : if-release
DEBUG     b_vscrt     : md
DEBUG
DEBUG Found ninja.exe-1.12.1 at D:\msys64\mingw64\bin\ninja.exe
DEBUG WARNING: msvc does not support C++11; attempting best effort; setting the standard to C++14
DEBUG + meson compile
DEBUG [1/2] Compiling C++ object _core.cp312-win_amd64.pyd.p/src_main.cpp.obj
DEBUG [2/2] Linking target _core.cp312-win_amd64.pyd
DEBUG    Creating library _core.cp312-win_amd64.lib and object _core.cp312-win_amd64.exp
DEBUG INFO: autodetecting backend as ninja
DEBUG INFO: calculating backend command to run: D:\msys64\mingw64\bin\ninja.exe
DEBUG Finished building: mesontest @ file:///D:/RustDev/uv/mesontest
DEBUG Released lock at `C:\Users\Sandbox\AppData\Local\Temp\.tmpEsFjbV\sdists-v4\editable\3b0dc5127cb60fe2\.lock`
Prepared 1 package in 15.89s
DEBUG Extracting file name=PackageName("mesontest")
DEBUG Extracted 6 files name=PackageName("mesontest")
DEBUG No entrypoints name=PackageName("mesontest")
DEBUG No data name=PackageName("mesontest")
DEBUG Writing extra metadata name=PackageName("mesontest")
DEBUG Writing record name=PackageName("mesontest")
Installed 1 package in 30ms
 + mesontest==0.1.0 (from file:///D:/RustDev/uv/mesontest)
```

</p>
</details> 

<details><summary>Second uv + meson build logs (has errors)</summary>
<p>

```
.\target\debug\uv.exe sync -v --no-cache --reinstall
DEBUG uv 0.4.18
DEBUG Found project root: `D:\RustDev\uv\mesontest`
DEBUG Project is contained in non-workspace project: `D:\RustDev\uv`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `D:\RustDev\uv\mesontest\.python-version`
DEBUG Searching for Python 3.12 in system path or `py` launcher
DEBUG Found `cpython-3.12.7-windows-x86_64-none` at `C:\Users\Sandbox\AppData\Local\Programs\Python\Python312\python.exe` (search path)
Using CPython 3.12.7 interpreter at: C:\Users\Sandbox\AppData\Local\Programs\Python\Python312\python.exe
Creating virtual environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: mesontest @ file:///D:/RustDev/uv/mesontest
DEBUG Project is contained in non-workspace project: `D:\RustDev\uv`
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 8ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: mesontest @ file:///D:/RustDev/uv/mesontest
DEBUG Acquired lock for `C:\Users\Sandbox\AppData\Local\Temp\.tmpEsFjbV\sdists-v4\editable\3b0dc5127cb60fe2`
WARN Failed to read metadata for file: The system cannot find the file specified. (os error 2)
WARN Failed to read metadata for file: The system cannot find the file specified. (os error 2)
DEBUG Building: mesontest @ file:///D:/RustDev/uv/mesontest
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: meson-python*
DEBUG Adding direct dependency: pybind11*
DEBUG No cache entry for: https://pypi.org/simple/meson-python/
DEBUG No cache entry for: https://pypi.org/simple/pybind11/
DEBUG Searching for a compatible version of meson-python (*)
DEBUG Selecting: meson-python==0.16.0 [compatible] (meson_python-0.16.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/91/c0/104cb6244c83fe6bc3886f144cc433db0c0c78efac5dc00e409a5a08c87d/meson_python-0.16.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/13/2f/0f24b288e2ce56f51c920137620b4434a38fd80583dbbe24fc2a1656c388/pybind11-2.13.6-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for meson-python==0.16.0: meson{python_full_version >= '3.12'}>=1.2.3
DEBUG Adding transitive dependency for meson-python==0.16.0: packaging>=19.0
DEBUG Adding transitive dependency for meson-python==0.16.0: pyproject-metadata>=0.7.1
DEBUG Searching for a compatible version of pybind11 (*)
DEBUG Selecting: pybind11==2.13.6 [compatible] (pybind11-2.13.6-py3-none-any.whl)
DEBUG No cache entry for: https://pypi.org/simple/meson/
DEBUG No cache entry for: https://pypi.org/simple/packaging/
DEBUG No cache entry for: https://pypi.org/simple/pyproject-metadata/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (>=1.2.3)
DEBUG Selecting: meson==1.5.2 [compatible] (meson-1.5.2-py3-none-any.whl)
DEBUG Adding transitive dependency for meson==1.5.2: meson==1.5.2
DEBUG Adding transitive dependency for meson==1.5.2: meson{python_full_version >= '3.12'}==1.5.2
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (==1.5.2)
DEBUG Selecting: meson==1.5.2 [compatible] (meson-1.5.2-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/a6/47b9353c331318a13eb050887eacfd61eb075746285f9baf7ef7de6ae235/meson-1.5.2-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/aa/5f/bb5970d3d04173b46c9037109f7f05fc8904ff5be073ee49bb6ff00301bc/pyproject_metadata-0.8.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of meson (==1.5.2)
DEBUG Selecting: meson==1.5.2 [compatible] (meson-1.5.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=19.0)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pyproject-metadata (>=0.7.1)
DEBUG Selecting: pyproject-metadata==0.8.0 [compatible] (pyproject_metadata-0.8.0-py3-none-any.whl)
DEBUG Adding transitive dependency for pyproject-metadata==0.8.0: packaging>=19.0
DEBUG Tried 5 versions: meson 1, meson-python 1, packaging 1, pybind11 1, pyproject-metadata 1
DEBUG Split specific environment resolution took 0.276s
DEBUG Installing in meson==1.5.2, meson-python==0.16.0, packaging==24.1, pybind11==2.13.6, pyproject-metadata==0.8.0 in C:\Users\Sandbox\AppData\Local\Temp\.tmpEsFjbV\builds-v0\.tmpD9fGuR
DEBUG Identified uncached distribution: meson==1.5.2
DEBUG Identified uncached distribution: meson-python==0.16.0
DEBUG Identified uncached distribution: packaging==24.1
DEBUG Identified uncached distribution: pybind11==2.13.6
DEBUG Identified uncached distribution: pyproject-metadata==0.8.0
DEBUG Downloading and building requirements for build: meson==1.5.2, meson-python==0.16.0, packaging==24.1, pybind11==2.13.6, pyproject-metadata==0.8.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/a6/47b9353c331318a13eb050887eacfd61eb075746285f9baf7ef7de6ae235/meson-1.5.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/13/2f/0f24b288e2ce56f51c920137620b4434a38fd80583dbbe24fc2a1656c388/pybind11-2.13.6-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/91/c0/104cb6244c83fe6bc3886f144cc433db0c0c78efac5dc00e409a5a08c87d/meson_python-0.16.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/aa/5f/bb5970d3d04173b46c9037109f7f05fc8904ff5be073ee49bb6ff00301bc/pyproject_metadata-0.8.0-py3-none-any.whl
DEBUG Installing build requirements: pyproject-metadata==0.8.0, meson-python==0.16.0, packaging==24.1, pybind11==2.13.6, meson==1.5.2
DEBUG Extracting file name=PackageName("pyproject-metadata")
DEBUG Extracting file name=PackageName("meson-python")
DEBUG Extracted 6 files name=PackageName("pyproject-metadata")
DEBUG No entrypoints name=PackageName("pyproject-metadata")
DEBUG No data name=PackageName("pyproject-metadata")
DEBUG Writing extra metadata name=PackageName("pyproject-metadata")
DEBUG Writing record name=PackageName("pyproject-metadata")
DEBUG Extracted 11 files name=PackageName("meson-python")
DEBUG Extracting file name=PackageName("packaging")
DEBUG No entrypoints name=PackageName("meson-python")
DEBUG No data name=PackageName("meson-python")
DEBUG Writing extra metadata name=PackageName("meson-python")
DEBUG Writing record name=PackageName("meson-python")
DEBUG Extracted 21 files name=PackageName("packaging")
DEBUG Extracting file name=PackageName("meson")
DEBUG Extracting file name=PackageName("pybind11")
DEBUG No entrypoints name=PackageName("packaging")
DEBUG Writing extra metadata name=PackageName("packaging")
DEBUG Writing record name=PackageName("packaging")
DEBUG Extracted 57 files name=PackageName("pybind11")
DEBUG Writing entrypoints name=PackageName("pybind11")
DEBUG No data name=PackageName("pybind11")
DEBUG Writing extra metadata name=PackageName("pybind11")
DEBUG Writing record name=PackageName("pybind11")
DEBUG Extracted 244 files name=PackageName("meson")
DEBUG Writing entrypoints name=PackageName("meson")
DEBUG Installing data name=PackageName("meson")
DEBUG Writing extra metadata name=PackageName("meson")
DEBUG Writing record name=PackageName("meson")
DEBUG Creating PEP 517 build environment
DEBUG Calling `mesonpy.get_requires_for_build_editable()`
DEBUG Calling `mesonpy.build_editable("C:\\Users\\Sandbox\\AppData\\Local\\Temp\\.tmpEsFjbV\\sdists-v4\\editable\\3b0dc5127cb60fe2\\2SI-UfXgTs-o2MAwpsL4m\\.tmpLXpwHC", {}, None)`
DEBUG + meson setup D:\RustDev\uv\mesontest D:\RustDev\uv\mesontest\build\cp312 -Dbuildtype=release -Db_ndebug=if-release -Db_vscrt=md --native-file=D:\RustDev\uv\mesontest\build\cp312\meson-python-native-file.ini
DEBUG The Meson build system
DEBUG Version: 1.5.2
DEBUG Source dir: D:\RustDev\uv\mesontest
DEBUG Build dir: D:\RustDev\uv\mesontest\build\cp312
DEBUG Build type: native build
DEBUG Project name: mesontest
DEBUG Project version: 0.1.0
DEBUG C++ compiler for the host machine: cl (msvc 19.39.33519 "Microsoft (R) C/C++ Optimizing Compiler Version 19.39.33519 for x64")
DEBUG C++ linker for the host machine: link link 14.39.33519.0
DEBUG Host machine cpu family: x86_64
DEBUG Host machine cpu: x86_64
DEBUG Program python found: YES (C:\Users\Sandbox\AppData\Local\Temp\.tmpEsFjbV\builds-v0\.tmpD9fGuR\Scripts\python.exe)
DEBUG Found pkg-config: YES (D:\msys64\mingw64\bin\pkg-config.exe) 2.3.0
DEBUG pybind11-config found: YES (C:\Users\Sandbox\AppData\Local\Temp\.tmpEsFjbV\builds-v0\.tmpD9fGuR\Scripts\pybind11-config.exe) 2.13.6
DEBUG Run-time dependency pybind11 found: YES 2.13.6
DEBUG Run-time dependency python found: YES 3.12
DEBUG Build targets in project: 1
DEBUG
DEBUG mesontest 0.1.0
DEBUG
DEBUG   User defined options
DEBUG     Native files: D:\RustDev\uv\mesontest\build\cp312\meson-python-native-file.ini
DEBUG     buildtype   : release
DEBUG     b_ndebug    : if-release
DEBUG     b_vscrt     : md
DEBUG
DEBUG Found ninja.exe-1.12.1 at D:\msys64\mingw64\bin\ninja.exe
DEBUG WARNING: msvc does not support C++11; attempting best effort; setting the standard to C++14
DEBUG + meson compile
DEBUG [1/2] Compiling C++ object _core.cp312-win_amd64.pyd.p/src_main.cpp.obj
DEBUG [2/2] Linking target _core.cp312-win_amd64.pyd
DEBUG    Creating library _core.cp312-win_amd64.lib and object _core.cp312-win_amd64.exp
DEBUG INFO: autodetecting backend as ninja
DEBUG INFO: calculating backend command to run: D:\msys64\mingw64\bin\ninja.exe
DEBUG Finished building: mesontest @ file:///D:/RustDev/uv/mesontest
DEBUG Released lock at `C:\Users\Sandbox\AppData\Local\Temp\.tmpEsFjbV\sdists-v4\editable\3b0dc5127cb60fe2\.lock`
Prepared 1 package in 15.89s
DEBUG Extracting file name=PackageName("mesontest")
DEBUG Extracted 6 files name=PackageName("mesontest")
DEBUG No entrypoints name=PackageName("mesontest")
DEBUG No data name=PackageName("mesontest")
DEBUG Writing extra metadata name=PackageName("mesontest")
DEBUG Writing record name=PackageName("mesontest")
Installed 1 package in 30ms
 + mesontest==0.1.0 (from file:///D:/RustDev/uv/mesontest)
PS D:\RustDev\uv\mesontest> ..\target\debug\uv.exe sync -v --no-cache --reinstall
DEBUG uv 0.4.18
DEBUG Found project root: `D:\RustDev\uv\mesontest`
DEBUG Project is contained in non-workspace project: `D:\RustDev\uv`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `D:\RustDev\uv\mesontest\.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.12`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: mesontest @ file:///D:/RustDev/uv/mesontest
DEBUG Project is contained in non-workspace project: `D:\RustDev\uv`
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 5ms
DEBUG Using request timeout of 30s
DEBUG Must revalidate requirement: mesontest
DEBUG Acquired lock for `C:\Users\Sandbox\AppData\Local\Temp\.tmpRFFhNx\sdists-v4\editable\3b0dc5127cb60fe2`
WARN Failed to read metadata for file: The system cannot find the file specified. (os error 2)
WARN Failed to read metadata for file: The system cannot find the file specified. (os error 2)
DEBUG Building: mesontest @ file:///D:/RustDev/uv/mesontest
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: meson-python*
DEBUG Adding direct dependency: pybind11*
DEBUG No cache entry for: https://pypi.org/simple/meson-python/
DEBUG No cache entry for: https://pypi.org/simple/pybind11/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/13/2f/0f24b288e2ce56f51c920137620b4434a38fd80583dbbe24fc2a1656c388/pybind11-2.13.6-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of meson-python (*)
DEBUG Selecting: meson-python==0.16.0 [compatible] (meson_python-0.16.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/91/c0/104cb6244c83fe6bc3886f144cc433db0c0c78efac5dc00e409a5a08c87d/meson_python-0.16.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for meson-python==0.16.0: meson{python_full_version >= '3.12'}>=1.2.3
DEBUG Adding transitive dependency for meson-python==0.16.0: packaging>=19.0
DEBUG Adding transitive dependency for meson-python==0.16.0: pyproject-metadata>=0.7.1
DEBUG Searching for a compatible version of pybind11 (*)
DEBUG Selecting: pybind11==2.13.6 [compatible] (pybind11-2.13.6-py3-none-any.whl)
DEBUG No cache entry for: https://pypi.org/simple/meson/
DEBUG No cache entry for: https://pypi.org/simple/packaging/
DEBUG No cache entry for: https://pypi.org/simple/pyproject-metadata/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (>=1.2.3)
DEBUG Selecting: meson==1.5.2 [compatible] (meson-1.5.2-py3-none-any.whl)
DEBUG Adding transitive dependency for meson==1.5.2: meson==1.5.2
DEBUG Adding transitive dependency for meson==1.5.2: meson{python_full_version >= '3.12'}==1.5.2
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (==1.5.2)
DEBUG Selecting: meson==1.5.2 [compatible] (meson-1.5.2-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/a6/47b9353c331318a13eb050887eacfd61eb075746285f9baf7ef7de6ae235/meson-1.5.2-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/aa/5f/bb5970d3d04173b46c9037109f7f05fc8904ff5be073ee49bb6ff00301bc/pyproject_metadata-0.8.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of meson (==1.5.2)
DEBUG Selecting: meson==1.5.2 [compatible] (meson-1.5.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=19.0)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pyproject-metadata (>=0.7.1)
DEBUG Selecting: pyproject-metadata==0.8.0 [compatible] (pyproject_metadata-0.8.0-py3-none-any.whl)
DEBUG Adding transitive dependency for pyproject-metadata==0.8.0: packaging>=19.0
DEBUG Tried 5 versions: meson 1, meson-python 1, packaging 1, pybind11 1, pyproject-metadata 1
DEBUG Split specific environment resolution took 0.239s
DEBUG Installing in meson==1.5.2, meson-python==0.16.0, packaging==24.1, pybind11==2.13.6, pyproject-metadata==0.8.0 in C:\Users\Sandbox\AppData\Local\Temp\.tmpRFFhNx\builds-v0\.tmpqptYLW
DEBUG Must revalidate requirement: meson
DEBUG Must revalidate requirement: meson-python
DEBUG Must revalidate requirement: packaging
DEBUG Must revalidate requirement: pybind11
DEBUG Must revalidate requirement: pyproject-metadata
DEBUG Downloading and building requirements for build: meson==1.5.2, meson-python==0.16.0, packaging==24.1, pybind11==2.13.6, pyproject-metadata==0.8.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/a6/47b9353c331318a13eb050887eacfd61eb075746285f9baf7ef7de6ae235/meson-1.5.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/13/2f/0f24b288e2ce56f51c920137620b4434a38fd80583dbbe24fc2a1656c388/pybind11-2.13.6-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/91/c0/104cb6244c83fe6bc3886f144cc433db0c0c78efac5dc00e409a5a08c87d/meson_python-0.16.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/aa/5f/bb5970d3d04173b46c9037109f7f05fc8904ff5be073ee49bb6ff00301bc/pyproject_metadata-0.8.0-py3-none-any.whl
DEBUG Installing build requirements: pyproject-metadata==0.8.0, meson-python==0.16.0, packaging==24.1, pybind11==2.13.6, meson==1.5.2
DEBUG Extracting file name=PackageName("meson-python")
DEBUG Extracting file name=PackageName("pyproject-metadata")
DEBUG Extracted 6 files name=PackageName("pyproject-metadata")
DEBUG No entrypoints name=PackageName("pyproject-metadata")
DEBUG No data name=PackageName("pyproject-metadata")
DEBUG Writing extra metadata name=PackageName("pyproject-metadata")
DEBUG Writing record name=PackageName("pyproject-metadata")
DEBUG Extracted 11 files name=PackageName("meson-python")
DEBUG No entrypoints name=PackageName("meson-python")
DEBUG No data name=PackageName("meson-python")
DEBUG Writing extra metadata name=PackageName("meson-python")
DEBUG Writing record name=PackageName("meson-python")
DEBUG Extracting file name=PackageName("packaging")
DEBUG Extracted 21 files name=PackageName("packaging")
DEBUG No entrypoints name=PackageName("packaging")
DEBUG No data name=PackageName("packaging")
DEBUG Writing extra metadata name=PackageName("packaging")
DEBUG Writing record name=PackageName("packaging")
DEBUG Extracting file name=PackageName("meson")
DEBUG Extracting file name=PackageName("pybind11")
DEBUG Extracted 57 files name=PackageName("pybind11")
DEBUG Writing entrypoints name=PackageName("pybind11")
DEBUG No data name=PackageName("pybind11")
DEBUG Writing extra metadata name=PackageName("pybind11")
DEBUG Writing record name=PackageName("pybind11")
DEBUG Extracted 244 files name=PackageName("meson")
DEBUG Writing entrypoints name=PackageName("meson")
DEBUG Installing data name=PackageName("meson")
DEBUG Writing extra metadata name=PackageName("meson")
DEBUG Writing record name=PackageName("meson")
DEBUG Creating PEP 517 build environment
DEBUG Calling `mesonpy.get_requires_for_build_editable()`
DEBUG Calling `mesonpy.build_editable("C:\\Users\\Sandbox\\AppData\\Local\\Temp\\.tmpRFFhNx\\sdists-v4\\editable\\3b0dc5127cb60fe2\\U4fIuzClRy9QsLopv6vUY\\.tmphJXwfl", {}, None)`
DEBUG + meson setup --reconfigure D:\RustDev\uv\mesontest D:\RustDev\uv\mesontest\build\cp312 -Dbuildtype=release -Db_ndebug=if-release -Db_vscrt=md --native-file=D:\RustDev\uv\mesontest\build\cp312\meson-python-native-file.ini
DEBUG Cleaning... 0 files.
DEBUG The Meson build system
DEBUG Version: 1.5.2
DEBUG Source dir: D:\RustDev\uv\mesontest
DEBUG Build dir: D:\RustDev\uv\mesontest\build\cp312
DEBUG Build type: native build
DEBUG Project name: mesontest
DEBUG Project version: 0.1.0
DEBUG C++ compiler for the host machine: cl (msvc 19.39.33519 "Microsoft (R) C/C++ Optimizing Compiler Version 19.39.33519 for x64")
DEBUG C++ linker for the host machine: link link 14.39.33519.0
DEBUG Host machine cpu family: x86_64
DEBUG Host machine cpu: x86_64
DEBUG Program python found: YES (C:\Users\Sandbox\AppData\Local\Temp\.tmpRFFhNx\builds-v0\.tmpqptYLW\Scripts\python.exe)
DEBUG Dependency pybind11 found: YES 2.13.6 (cached)
DEBUG Run-time dependency python found: YES 3.12
DEBUG Build targets in project: 1
DEBUG
DEBUG mesontest 0.1.0
DEBUG
DEBUG   User defined options
DEBUG     Native files: D:\RustDev\uv\mesontest\build\cp312\meson-python-native-file.ini
DEBUG     buildtype   : release
DEBUG     b_ndebug    : if-release
DEBUG     b_vscrt     : md
DEBUG
DEBUG Found ninja.exe-1.12.1 at D:\msys64\mingw64\bin\ninja.exe
DEBUG WARNING: msvc does not support C++11; attempting best effort; setting the standard to C++14
DEBUG + meson compile
DEBUG [1/2] Compiling C++ object _core.cp312-win_amd64.pyd.p/src_main.cpp.obj
DEBUG FAILED: _core.cp312-win_amd64.pyd.p/src_main.cpp.obj
DEBUG "cl" "-I_core.cp312-win_amd64.pyd.p" "-I." "-I..\.." "-IC:\Users\Sandbox\AppData\Local\Programs\Python\Python312\Include" "-IC:\Users\Sandbox\AppData\Local\Temp\.tmpEsFjbV\builds-v0\.tmpD9fGuR\Lib\site-packages\pybind11\include" "-DNDEBUG" "/MD" "/nologo" "/showIncludes" "/utf-8" "/Zc:__cplusplus" "/W2" "/EHsc" "/std:c++14" "/permissive-" "/O2" "/Gw" "/Fd_core.cp312-win_amd64.pyd.p\src_main.cpp.pdb" /Fo_core.cp312-win_amd64.pyd.p/src_main.cpp.obj "/c" ../../src/main.cpp
DEBUG ../../src/main.cpp(1): fatal error C1083: Cannot open include file: 'pybind11/pybind11.h': No such file or directory
DEBUG ninja: build stopped: subcommand failed.
DEBUG INFO: autodetecting backend as ninja
DEBUG INFO: calculating backend command to run: D:\msys64\mingw64\bin\ninja.exe
DEBUG Released lock at `C:\Users\Sandbox\AppData\Local\Temp\.tmpRFFhNx\sdists-v4\editable\3b0dc5127cb60fe2\.lock`
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: mesontest @ file:///D:/RustDev/uv/mesontest
  Caused by: Build backend failed to build editable through `build_editable()` (exit code: 1)
--- stdout:
+ meson setup --reconfigure D:\RustDev\uv\mesontest D:\RustDev\uv\mesontest\build\cp312 -Dbuildtype=release -Db_ndebug=if-release -Db_vscrt=md --native-file=D:\RustDev\uv\mesontest\build\cp312\meson-python-native-file.ini
Cleaning... 0 files.
The Meson build system
Version: 1.5.2
Source dir: D:\RustDev\uv\mesontest
Build dir: D:\RustDev\uv\mesontest\build\cp312
Build type: native build
Project name: mesontest
Project version: 0.1.0
C++ compiler for the host machine: cl (msvc 19.39.33519 "Microsoft (R) C/C++ Optimizing Compiler Version 19.39.33519 for x64")
C++ linker for the host machine: link link 14.39.33519.0
Host machine cpu family: x86_64
Host machine cpu: x86_64
Program python found: YES (C:\Users\Sandbox\AppData\Local\Temp\.tmpRFFhNx\builds-v0\.tmpqptYLW\Scripts\python.exe)
Dependency pybind11 found: YES 2.13.6 (cached)
Run-time dependency python found: YES 3.12
Build targets in project: 1

mesontest 0.1.0

  User defined options
    Native files: D:\RustDev\uv\mesontest\build\cp312\meson-python-native-file.ini
    buildtype   : release
    b_ndebug    : if-release
    b_vscrt     : md

Found ninja.exe-1.12.1 at D:\msys64\mingw64\bin\ninja.exe
WARNING: msvc does not support C++11; attempting best effort; setting the standard to C++14
+ meson compile
[1/2] Compiling C++ object _core.cp312-win_amd64.pyd.p/src_main.cpp.obj
FAILED: _core.cp312-win_amd64.pyd.p/src_main.cpp.obj
"cl" "-I_core.cp312-win_amd64.pyd.p" "-I." "-I..\.." "-IC:\Users\Sandbox\AppData\Local\Programs\Python\Python312\Include" "-IC:\Users\Sandbox\AppData\Local\Temp\.tmpEsFjbV\builds-v0\.tmpD9fGuR\Lib\site-packages\pybind11\include" "-DNDEBUG" "/MD" "/nologo" "/showIncludes" "/utf-8" "/Zc:__cplusplus" "/W2" "/EHsc" "/std:c++14" "/permissive-" "/O2" "/Gw" "/Fd_core.cp312-win_amd64.pyd.p\src_main.cpp.pdb" /Fo_core.cp312-win_amd64.pyd.p/src_main.cpp.obj "/c" ../../src/main.cpp
../../src/main.cpp(1): fatal error C1083: Cannot open include file: 'pybind11/pybind11.h': No such file or directory
ninja: build stopped: subcommand failed.
INFO: autodetecting backend as ninja
INFO: calculating backend command to run: D:\msys64\mingw64\bin\ninja.exe
--- stderr:

---
```

</p>
</details>

<details><summary>meson.build</summary>
<p>

```meson
project(
    'mesontest',
    'cpp',
    version: '0.1.0',
    meson_version: '>= 1.2.3',
    default_options: [
        'cpp_std=c++11',
        'python.install_env=venv',
    ],
)

py = import('python').find_installation(pure: false)
pybind11_dep = dependency('pybind11')

py.extension_module('_core',
    'src/main.cpp',
    subdir: 'mesontest',
    install: true,
    dependencies : [pybind11_dep],
)

install_subdir('src/mesontest', install_dir: py.get_install_dir() / 'mesontest', strip_directory: true)
```

</p>
</details> 

---

_Comment by @samypr100 on 2024-10-05 23:35_

@bluss digging around meson docs for a bit, I found that `--clearcache` allows it to work with uv sync on second runs as expected. I'll update the PR to include this so that it works with uv a bit better.

```
[tool.meson-python.args]
setup = ["--clearcache"]
```

---

_@rgommers reviewed on 2024-10-06 07:34_

---

_Review comment by @rgommers on `crates/uv/src/commands/project/init.rs`:907 on 2024-10-06 07:34_

This line should be removed. A regular package shouldn't have to know what the name is of the environment it's going to be installed in.

Other than that, the `meson.build` file produced looks good to me.

---

_Comment by @rgommers on 2024-10-06 08:00_

> I found that `--clearcache` allows it to work with uv sync on second runs as expected.

I think this is not going to help beyond being a band aid for this specific scenario. For one, editable installs will be invoking `ninja`, and you happen to have it installed at `D:\msys64\mingw64\bin\ninja.exe`. But if the user doesn't have it, it's going to be in the temporary build env and then discarded, so even importing an editable install won't work then. See the docs at https://mesonbuild.com/meson-python/how-to-guides/editable-installs.html#build-dependencies

For regular installs things look fine from the logs as far as I can tell. For editable installs, build isolation should be disabled for things to work well. This probably requires a bit of context, so here it is:

Editable installs were not designed well for dealing with packages with compiled code. The `pip` defaults completely ignore compiled code and assume what `setuptools` does: force the user to manually rebuild the package with either `pip install` or `python setup.py build_ext -i` (which is a bad experience). `meson-python` is primarily used for projects with compiled code (as is `scikit-build-core`, which has similar capabilities to `meson-python`) and hence tries to do better - it will auto-rebuild whenever one touches compiled code so that the experience of working on C/C++/Cython code is very similar to working on pure Python code. For that to happen, one needs to have build dependencies available in the dev env, or at least in a fixed location. For `pip`, it's up to the user of the editable install to add `--no-build-isolation` to the install command for things to work as intended with auto-rebuilds. Hopefully `uv` has the chance to offer a better experience here.

---

_Review comment by @samypr100 on `crates/uv/src/commands/project/init.rs`:907 on 2024-10-06 14:56_

> A regular package shouldn't have to know what the name is of the environment it's going to be installed in.

I see, I made the change per suggestion from https://mesonbuild.com/Python-module.html / https://mesonbuild.com/Builtin-options.html#python-module as I was having some issues at first locally with meson. I'll remove it.

---

_@samypr100 reviewed on 2024-10-06 14:56_

---

_@samypr100 reviewed on 2024-10-06 14:58_

---

_Review comment by @samypr100 on `crates/uv/src/commands/project/init.rs`:907 on 2024-10-06 14:58_

6794774

---

_Comment by @samypr100 on 2024-10-06 16:07_

> > I found that `--clearcache` allows it to work with uv sync on second runs as expected.
> 
> I think this is not going to help beyond being a band aid for this specific scenario. For one, editable installs will be invoking `ninja`, and you happen to have it installed at `D:\msys64\mingw64\bin\ninja.exe`. But if the user doesn't have it, it's going to be in the temporary build env and then discarded, so even importing an editable install won't work then. See the docs at https://mesonbuild.com/meson-python/how-to-guides/editable-installs.html#build-dependencies
> 
> For regular installs things look fine from the logs as far as I can tell. For editable installs, build isolation should be disabled for things to work well. This probably requires a bit of context, so here it is:
> 
> Editable installs were not designed well for dealing with packages with compiled code. The `pip` defaults completely ignore compiled code and assume what `setuptools` does: force the user to manually rebuild the package with either `pip install` or `python setup.py build_ext -i` (which is a bad experience). `meson-python` is primarily used for projects with compiled code (as is `scikit-build-core`, which has similar capabilities to `meson-python`) and hence tries to do better - it will auto-rebuild whenever one touches compiled code so that the experience of working on C/C++/Cython code is very similar to working on pure Python code. For that to happen, one needs to have build dependencies available in the dev env, or at least in a fixed location. For `pip`, it's up to the user of the editable install to add `--no-build-isolation` to the install command for things to work as intended with auto-rebuilds. Hopefully `uv` has the chance to offer a better experience here.

All great points, thanks for the links and meson context. Maybe we could consider having a default section for meson in particular with ```[tool.uv] no-build-isolation = true``` in the `pyproject.toml` to support editable installs better? Although that will give a user an error on their first experience with the uv meson scaffolding.

As a starting point, I was originally of the mindset that having to use `--reinstall` with `uv` for editable cases in the binary build backends was acceptable given the goal of the `init` command. Of course, this has led to the issues aformentioned with meson. Alternatively, the recommendation for using `uv sync / uv run` with meson could be to not support editable installs by using `--no-editable` instead (an get rid of the `--clearcache` from meson setup args).

> Hopefully uv has the chance to offer a better experience here.

I think there's definitely further work to improve on this area as this PR mainly is a starting cookiecutter that introduces leveraging other build backends with uv. 



---

_Comment by @henryiii on 2024-10-06 17:09_

If uv didn't use a different random path every time, but cached the build environment, then that would be solved, I think. Might be something to look into? It would be a big time savings for both meson-python and scikit-build-core, as they could take advantage of caching then. Meson-python should also detect a moving build environment and rerun from scratch if it's detected, a feature I recently added to scikit-build-core.

---

_Comment by @samypr100 on 2024-10-06 23:26_

> Meson-python should also detect a moving build environment and rerun from scratch if it's detected, a feature I recently added to scikit-build-core.

That would be great (if possible).

I've removed `--clearcache` for the time being and documented the observations with meson-python.

---

_Assigned to @zanieb by @zanieb on 2024-10-08 19:18_

---

_Assigned to @konstin by @zanieb on 2024-10-08 19:18_

---

_Review comment by @konstin on `crates/uv/src/commands/project/init.rs`:669 on 2024-10-12 14:46_

nit: move inside if block, since it is only used inside the if block

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:2466 on 2024-10-12 14:52_

The second sentence is redundant

---

_Review comment by @konstin on `crates/uv/src/commands/project/init.rs`:978 on 2024-10-12 15:06_

Currently, this function both writes some files (effectful) and returns the contents of a file (pure). It should either return all files or write all files, so it's clearly to the reader that this is either just a template-filling function or an effectful function: When I read `generate_package_script` in `init_application`, I assumed it did only generate the file it returned and not also write other files.

---

_Review comment by @konstin on `crates/uv/src/commands/project/init.rs`:800 on 2024-10-12 15:09_

Do these build backend have forever stability guarantees (somewhere in their docs/github issues where we can link them?) or do we need to put version bounds on them?

---

_Review comment by @konstin on `crates/uv/src/commands/project/init.rs`:916 on 2024-10-12 15:13_

we're just in time to switch to 3.9 as starter default minimum version (https://devguide.python.org/versions/)

---

_@konstin reviewed on 2024-10-12 15:14_

---

_Review comment by @samypr100 on `crates/uv/src/commands/project/init.rs`:978 on 2024-10-12 15:45_

Agreed, it was a last minute add

---

_@samypr100 reviewed on 2024-10-12 15:45_

---

_@samypr100 reviewed on 2024-10-12 15:46_

---

_Review comment by @samypr100 on `crates/uv/src/commands/project/init.rs`:800 on 2024-10-12 15:46_

Good question, we currently don't put bounds on hatchling either, do we want to upper bound all of them?  I follow each's build backend suggested version constraints I saw on their guides.

---

_@samypr100 reviewed on 2024-10-12 15:46_

---

_Review comment by @samypr100 on `crates/uv/src/commands/project/init.rs`:916 on 2024-10-12 15:46_

♥️

---

_Comment by @konstin on 2024-10-12 16:49_

Hi!

Adding more build backend is a great idea and the code looks good, i left some mostly style comments.

I certainly see the struggle of native build backends with the current constraints, PEP 517 left a lot missing for non-pure Python projects. From uv's perspective though, it's important that build backends work correctly as long as a valid PEP 517 environment is given (https://peps.python.org/pep-0517/#build-environment). `--no-build-isolation` in particular breaks uv's workflows. I'm hesistant to include meson if it fails with changing venv paths and meson and scikit-build-core given they recommend deactivating build isolation.

---

_@samypr100 reviewed on 2024-10-12 17:30_

---

_Review comment by @samypr100 on `crates/uv/src/commands/project/init.rs`:669 on 2024-10-12 17:30_

✔️ a090e8f286e41bb2be8d81999d19eb3571fc4bda

---

_@samypr100 reviewed on 2024-10-12 17:30_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:2466 on 2024-10-12 17:30_

✔️ a090e8f286e41bb2be8d81999d19eb3571fc4bda

---

_@samypr100 reviewed on 2024-10-12 17:30_

---

_Review comment by @samypr100 on `crates/uv/src/commands/project/init.rs`:978 on 2024-10-12 17:30_

✔️ a090e8f286e41bb2be8d81999d19eb3571fc4bda

---

_@samypr100 reviewed on 2024-10-12 17:30_

---

_Review comment by @samypr100 on `crates/uv/src/commands/project/init.rs`:916 on 2024-10-12 17:30_

✔️ a090e8f286e41bb2be8d81999d19eb3571fc4bda

---

_Comment by @samypr100 on 2024-10-14 22:15_

> Hi!
> 
> Adding more build backend is a great idea and the code looks good, i left some mostly style comments.
> 
> I certainly see the struggle of native build backends with the current constraints, PEP 517 left a lot missing for non-pure Python projects. From uv's perspective though, it's important that build backends work correctly as long as a valid PEP 517 environment is given (https://peps.python.org/pep-0517/#build-environment). `--no-build-isolation` in particular breaks uv's workflows. I'm hesistant to include meson if it fails with changing venv paths and meson and scikit-build-core given they recommend deactivating build isolation.

Sounds good, should I remove `meson-python` as well as `scikit-build-core`? `scikit-build-core` seems to work with build isolation, so I'd prefer to keep it but I'll let others chime in.

---

_Comment by @henryiii on 2024-10-14 23:00_

`scikit-build-core` works fine with build isolation.

(and I’ll look into meson-python soon)

---

_Comment by @konstin on 2024-10-15 07:35_

Then let's start with `scikit-build-core` and add `meson-python` in a follow-up PR once the bug is fixed.

---

_Comment by @konstin on 2024-10-15 08:18_

@henryiii What's the stability policy for `scikit-build-core` and `pybind11`? I.e., if we put the following in a published package, do they guarantee that there's never a breaking change released for those projects that may break a publish source dist, or should we add bounds? 

```toml
[build-system]
requires = ["scikit-build-core", "pybind11"]
build-backend = "scikit_build_core.build"
```

---

_Comment by @samypr100 on 2024-10-15 21:53_

> Then let's start with `scikit-build-core` and add `meson-python` in a follow-up PR once the bug is fixed.

Done

---

_Label `enhancement` added by @konstin on 2024-10-16 11:44_

---

_Renamed from "feat(init): support different init build backends" to "Add templates for popular build backends" by @konstin on 2024-10-16 11:44_

---

_@konstin approved on 2024-10-16 12:13_

---

_Merged by @konstin on 2024-10-16 12:19_

---

_Closed by @konstin on 2024-10-16 12:19_

---

_Branch deleted on 2024-10-16 12:31_

---

_Comment by @lucascolley on 2024-12-26 11:36_

> I think this is not going to help beyond being a band aid for this specific scenario. For one, editable installs will be invoking ninja, and you happen to have it installed at D:\msys64\mingw64\bin\ninja.exe. But if the user doesn't have it, it's going to be in the temporary build env and then discarded, so even importing an editable install won't work then.

I just hit this trying to develop scikit-learn with uv. Is this issue on your radar @konstin @zanieb ? Or should I open an issue on the tracker?

---

_Comment by @zanieb on 2024-12-28 17:07_

@lucascolley I wasn't following this, but now I am :)

A dedicated issue seems nice since we've had a couple links to this pull request already. I'm not entirely sure what needs to happen, i.e., if there needs to be a change in meson or uv.

---
