```yaml
number: 16257
title: Install current project as an editable dependency
type: issue
state: open
author: cpita-work
labels:
  - question
assignees: []
created_at: 2025-10-12T18:37:23Z
updated_at: 2025-10-22T12:10:51Z
url: https://github.com/astral-sh/uv/issues/16257
synced_at: 2026-01-12T16:02:27Z
```

# Install current project as an editable dependency

---

_@cpita-work_

### Question

I'm working on a project named `meli`, which has a helper python package in `./util` and many jupyter notebooks in `./notebooks`. I can install the current project with `uv pip install -e .` so that the notebooks may import stuff from util, but:
- It's an extra step using a different tool (pip instead of using normal uv dependency management)
- Each time I run `uv sync` it's reverted.

I tried `uv add --dev .` which sets `workspace = true` for the dependency. But after `uv sync` nothing is added to the venv.

Another attempt that didn't work:

```
[dependency-groups]
dev = [
    "meli",
]

[tool.uv.sources]
meli = { path = ".", editable = true }
```

So how shall I proceed?

Thanks!

NOTE: this flag to `uv sync` confuses me since AFAICS the current project is not being installed into the venv at all (editable or not):

> [--no-install-project](https://docs.astral.sh/uv/reference/cli/#uv-sync--no-install-project)
> Do not install the current project.
>
> By default, the current project is installed into the environment with all of its dependencies. The --no-install-project option allows the project to be excluded, but all of its dependencies are still installed. This is particularly useful in situations like building Docker images where installing the project separately from its dependencies allows optimal layer caching.

### Platform

macOS 15.5

### Version

0.9.2

---

_Label `question` added by @cpita-work on 2025-10-12 18:37_

---

_Comment by @cpita-work on 2025-10-12 20:08_

I found out that the uv_build backend may be using to achieve what I want:

```
[build-system]
requires = ["uv_build>=0.9.2,<0.10.0"]
build-backend = "uv_build"

[tool.uv.build-backend]
module-name = "util"
module-root = ""
```

Then in `uv.lock`:

```
[[package]]
name = "meli"
version = "0.1.0"
source = { editable = "." }
```

instead of virtual as previously.

Anyway, I not quite understand why my previous attempt failed and would like to know if this is the simplest and/or expected way to achieve it.

---

_Comment by @konstin on 2025-10-13 10:13_

What do the verbose logs of `uv sync -v` say? If the logs don't clear it up, can you share a minimal reproducible example with `pyproject.toml` files?

---

_Comment by @cpita-work on 2025-10-14 12:31_

As the following series of examples shows, the problem is the missing build backend spec in pyproject.toml. So I guess the way to get the current project as an editable dependency will be backend dependant. I know how to do it for the uv backend alone. And since the current project is just another installable project, perhaps uv could have done it even without a backend spec, just like it does with every other dependency?

```bash
uv init test
cd test
touch util/__init__.py
touch util/foo.py
```

pyproject.toml
```toml
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []
```

```
uv sync -v

DEBUG uv 0.9.2 (Homebrew 2025-10-10)
DEBUG Acquired shared lock for `/Users/carlos/.cache/uv`
DEBUG Found project root: `/private/tmp/test`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/private/tmp/test`
DEBUG Reading Python requests from version file at `/private/tmp/test/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG Searching for Python 3.13 in managed installations or search path
DEBUG Searching for managed installations at `/Users/carlos/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.5-macos-aarch64-none`
DEBUG Found `cpython-3.13.5-macos-aarch64-none` at `/Users/carlos/.local/share/uv/python/cpython-3.13-macos-aarch64-none/bin/python3.13` (managed installations)
Using CPython 3.13.5
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /Users/carlos/.local/share/uv/python/cpython-3.13-macos-aarch64-none/bin/python3.13
DEBUG Using base executable for virtual environment: /Users/carlos/.local/share/uv/python/cpython-3.13-macos-aarch64-none/bin/python3.13
DEBUG Released lock at `/var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/uv-a0e9bb00a68d9699.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test @ file:///private/tmp/test
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.5
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: test*
DEBUG Searching for a compatible version of test @ file:///private/tmp/test (*)
DEBUG Tried 1 versions: test 1
DEBUG all marker environments resolution took 0.002s
Resolved 1 package in 20ms
DEBUG Using request timeout of 30s
Audited in 0.06ms
DEBUG Released lock at `/private/tmp/test/.venv/.lock`
DEBUG Released lock at `/Users/carlos/.cache/uv/.lock`
```

```
ls -l .venv/lib/python3.13/site-packages/

total 12
-rw-r--r-- 1 carlos wheel   18 Oct 14 09:10 _virtualenv.pth
-rw-r--r-- 1 carlos wheel 4342 Oct 14 09:10 _virtualenv.py
```

uv.lock
```toml
version = 1
revision = 3
requires-python = ">=3.13"

[[package]]
name = "test"
version = "0.1.0"
source = { virtual = "." }
```

---

Change pyproject.toml to:

```toml
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "util"
]

[tool.uv.sources]
util = { path = ".", editable = true }
```

```
uv sync -v

DEBUG uv 0.9.2 (Homebrew 2025-10-10)
DEBUG Acquired shared lock for `/Users/carlos/.cache/uv`
DEBUG Found project root: `/private/tmp/test`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/private/tmp/test`
DEBUG Reading Python requests from version file at `/private/tmp/test/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.13`
DEBUG Released lock at `/var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/uv-a0e9bb00a68d9699.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test @ file:///private/tmp/test
DEBUG No workspace root found, using project root
DEBUG Resolving despite existing lockfile due to mismatched requirements for: `test==0.1.0`
  Requested: {Requirement { name: PackageName("util"), extras: [], groups: [], marker: true, source: Directory { install_path: "/private/tmp/test", editable: Some(true), virtual: Some(false), url: VerbatimUrl { url: DisplaySafeUrl { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/private/tmp/test", query: None, fragment: None }, given: None } }, origin: None }}
  Existing: {Requirement { name: PackageName("utilx"), extras: [], groups: [], marker: true, source: Directory { install_path: "/private/tmp/test/", editable: Some(true), virtual: Some(false), url: VerbatimUrl { url: DisplaySafeUrl { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/private/tmp/test", query: None, fragment: None }, given: None } }, origin: None }}
DEBUG Solving with installed Python version: 3.13.5
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: test*
DEBUG Searching for a compatible version of test @ file:///private/tmp/test (*)
DEBUG Adding direct dependency: util*
DEBUG Searching for a compatible version of util @ file:///private/tmp/test (*)
DEBUG Adding transitive dependency for util==0.1.0: util*
DEBUG Tried 2 versions: test 1, util 1
DEBUG all marker environments resolution took 0.000s
Resolved 2 packages in 8ms
DEBUG Using request timeout of 30s
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1760444102, tv_nsec: 419759870 })), None, None, {}, {"src": None}
DEBUG Identified uncached distribution: util @ file:///private/tmp/test
DEBUG Acquired lock for `/Users/carlos/.cache/uv/sdists-v9/editable/0a7393f02a8a20f2`
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1760444102, tv_nsec: 419759870 })), None, None, {}, {"src": None}
DEBUG Cached revision does not match expected cache info for: util @ file:///private/tmp/test
   Building util @ file:///private/tmp/test
DEBUG Building: util @ file:///private/tmp/test
DEBUG Not using uv build backend direct build for source tree `util @ file:///private/tmp/test`, failed to parse pyproject.toml: TOML parse error at line 1, column 1
  |
1 | [project]
  | ^
missing field `build-system`

DEBUG Assessing Python executable as base candidate: /Users/carlos/.local/share/uv/python/cpython-3.13-macos-aarch64-none/bin/python3.13
DEBUG Reusing existing build environment for: util @ file:///private/tmp/test
DEBUG Assessing Python executable as base candidate: /Users/carlos/.local/share/uv/python/cpython-3.13-macos-aarch64-none/bin/python3.13
DEBUG Using base executable for virtual environment: /Users/carlos/.local/share/uv/python/cpython-3.13-macos-aarch64-none/bin/python3.13
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.5
DEBUG Solving with target Python version: >=3.13.5
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==80.9.0 [compatible] (setuptools-80.9.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG marker environment resolution took 0.003s
DEBUG Installing in setuptools==80.9.0 in /Users/carlos/.cache/uv/builds-v0/.tmp60gJrs
DEBUG Registry requirement already cached: setuptools==80.9.0
DEBUG Installing build requirement: setuptools==80.9.0
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_editable()`
DEBUG running egg_info
DEBUG writing test.egg-info/PKG-INFO
DEBUG writing dependency_links to test.egg-info/dependency_links.txt
DEBUG writing requirements to test.egg-info/requires.txt
DEBUG writing top-level names to test.egg-info/top_level.txt
DEBUG reading manifest file 'test.egg-info/SOURCES.txt'
DEBUG writing manifest file 'test.egg-info/SOURCES.txt'
DEBUG No workspace root found, using project root
DEBUG Locking the source tree for setuptools
DEBUG Acquired lock for `/private/tmp/test`
DEBUG Calling `setuptools.build_meta:__legacy__.build_editable("/Users/carlos/.cache/uv/builds-v0/.tmpFpuIl0", {}, None)`
DEBUG running editable_wheel
DEBUG creating /Users/carlos/.cache/uv/builds-v0/.tmpFpuIl0/.tmp-bn4w7m7y/test.egg-info
DEBUG writing /Users/carlos/.cache/uv/builds-v0/.tmpFpuIl0/.tmp-bn4w7m7y/test.egg-info/PKG-INFO
DEBUG writing dependency_links to /Users/carlos/.cache/uv/builds-v0/.tmpFpuIl0/.tmp-bn4w7m7y/test.egg-info/dependency_links.txt
DEBUG writing requirements to /Users/carlos/.cache/uv/builds-v0/.tmpFpuIl0/.tmp-bn4w7m7y/test.egg-info/requires.txt
DEBUG writing top-level names to /Users/carlos/.cache/uv/builds-v0/.tmpFpuIl0/.tmp-bn4w7m7y/test.egg-info/top_level.txt
DEBUG writing manifest file '/Users/carlos/.cache/uv/builds-v0/.tmpFpuIl0/.tmp-bn4w7m7y/test.egg-info/SOURCES.txt'
DEBUG reading manifest file '/Users/carlos/.cache/uv/builds-v0/.tmpFpuIl0/.tmp-bn4w7m7y/test.egg-info/SOURCES.txt'
DEBUG writing manifest file '/Users/carlos/.cache/uv/builds-v0/.tmpFpuIl0/.tmp-bn4w7m7y/test.egg-info/SOURCES.txt'
DEBUG creating '/Users/carlos/.cache/uv/builds-v0/.tmpFpuIl0/.tmp-bn4w7m7y/test-0.1.0.dist-info'
DEBUG creating /Users/carlos/.cache/uv/builds-v0/.tmpFpuIl0/.tmp-bn4w7m7y/test-0.1.0.dist-info/WHEEL
DEBUG running build_py
DEBUG running egg_info
DEBUG creating /var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/tmpp_bmltfc.build-temp/test.egg-info
DEBUG writing /var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/tmpp_bmltfc.build-temp/test.egg-info/PKG-INFO
DEBUG writing dependency_links to /var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/tmpp_bmltfc.build-temp/test.egg-info/dependency_links.txt
DEBUG writing requirements to /var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/tmpp_bmltfc.build-temp/test.egg-info/requires.txt
DEBUG writing top-level names to /var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/tmpp_bmltfc.build-temp/test.egg-info/top_level.txt
DEBUG writing manifest file '/var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/tmpp_bmltfc.build-temp/test.egg-info/SOURCES.txt'
DEBUG reading manifest file '/var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/tmpp_bmltfc.build-temp/test.egg-info/SOURCES.txt'
DEBUG writing manifest file '/var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/tmpp_bmltfc.build-temp/test.egg-info/SOURCES.txt'
DEBUG Editable install will be performed using a meta path finder.
DEBUG 
DEBUG Options like `package-data`, `include/exclude-package-data` or
DEBUG `packages.find.exclude/include` may have no effect.
DEBUG 
DEBUG adding '__editable___test_0_1_0_finder.py'
DEBUG adding '__editable__.test-0.1.0.pth'
DEBUG creating '/Users/carlos/.cache/uv/builds-v0/.tmpFpuIl0/.tmp-bn4w7m7y/test-0.1.0-0.editable-py3-none-any.whl' and adding '/var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/tmpcz80tb_0test-0.1.0-0.editable-py3-none-any.whl' to it
DEBUG adding 'test-0.1.0.dist-info/METADATA'
DEBUG adding 'test-0.1.0.dist-info/WHEEL'
DEBUG adding 'test-0.1.0.dist-info/top_level.txt'
DEBUG adding 'test-0.1.0.dist-info/RECORD'
DEBUG /Users/carlos/.cache/uv/builds-v0/.tmp60gJrs/lib/python3.13/site-packages/setuptools/command/editable_wheel.py:351: InformationOnly: Editable installation.
DEBUG !!
DEBUG 
DEBUG         ********************************************************************************
DEBUG         Please be careful with folders in your working directory with the same
DEBUG         name as your package as they may take precedence during imports.
DEBUG         ********************************************************************************
DEBUG 
DEBUG !!
DEBUG   with strategy, WheelFile(wheel_path, "w") as wheel_obj:
DEBUG Released lock at `/var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/uv-setuptools-a0e9bb00a68d9699.lock`
DEBUG Released lock at `/Users/carlos/.cache/uv/sdists-v9/editable/0a7393f02a8a20f2/.lock`
  × Failed to build `util @ file:///private/tmp/test`
  ╰─▶ Package metadata name `test` does not match given name `util`
  help: `util` was included because `test` (v0.1.0) depends on `util`
DEBUG Released lock at `/private/tmp/test/.venv/.lock`
DEBUG Released lock at `/Users/carlos/.cache/uv/.lock`
```

---

Change pyproject.toml to:

```toml
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
]

[tool.uv.build-backend]
module-name = "util"
module-root = ""
```

```
uv sync -v

DEBUG uv 0.9.2 (Homebrew 2025-10-10)
DEBUG Acquired shared lock for `/Users/carlos/.cache/uv`
DEBUG Found project root: `/private/tmp/test`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/private/tmp/test`
DEBUG Reading Python requests from version file at `/private/tmp/test/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.13`
DEBUG Released lock at `/var/folders/zm/s9zmhlzs0hz23m57twr6d0lw0000gn/T/uv-a0e9bb00a68d9699.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Resolving despite existing lockfile due to mismatched source: `test` (unexpected: `virtual`)
DEBUG Found static `pyproject.toml` for: test @ file:///private/tmp/test
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.5
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: test*
DEBUG Searching for a compatible version of test @ file:///private/tmp/test (*)
DEBUG Tried 1 versions: test 1
DEBUG all marker environments resolution took 0.002s
Resolved 1 package in 21ms
DEBUG Using request timeout of 30s
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1760444998, tv_nsec: 527521903 })), None, None, {}, {"src": None}
DEBUG Identified uncached distribution: test @ file:///private/tmp/test
DEBUG Acquired lock for `/Users/carlos/.cache/uv/sdists-v9/editable/0a7393f02a8a20f2`
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1760444998, tv_nsec: 527521903 })), None, None, {}, {"src": None}
DEBUG Cached revision does not match expected cache info for: test @ file:///private/tmp/test
   Building test @ file:///private/tmp/test
DEBUG Building: test @ file:///private/tmp/test
DEBUG Performing direct build for test @ file:///private/tmp/test
DEBUG Writing wheel at /Users/carlos/.cache/uv/builds-v0/.tmphiXaXZ/test-0.1.0-py3-none-any.whl
DEBUG Adding pth file to /Users/carlos/.cache/uv/builds-v0/.tmphiXaXZ/test-0.1.0-py3-none-any.whl
DEBUG Source root: .
DEBUG Module path: util
DEBUG Adding metadata files to: /Users/carlos/.cache/uv/builds-v0/.tmphiXaXZ/test-0.1.0-py3-none-any.whl
DEBUG Finished building: test @ file:///private/tmp/test
      Built test @ file:///private/tmp/test
DEBUG Released lock at `/Users/carlos/.cache/uv/sdists-v9/editable/0a7393f02a8a20f2/.lock`
Prepared 1 package in 10ms
Installed 1 package in 4ms
 + test==0.1.0 (from file:///private/tmp/test)
DEBUG Released lock at `/private/tmp/test/.venv/.lock`
DEBUG Released lock at `/Users/carlos/.cache/uv/.lock`
```

```
ls -l .venv/lib/python3.13/site-packages/

total 16
-rw-r--r--  1 carlos wheel   18 Oct 14 09:10 _virtualenv.pth
-rw-r--r--  1 carlos wheel 4342 Oct 14 09:10 _virtualenv.py
drwxr-xr-x 10 carlos wheel  320 Oct 14 09:30 test-0.1.0.dist-info
-rw-r--r--  1 carlos wheel   18 Oct 14 09:30 test.pth
```

uv.lock

```toml
version = 1
revision = 3
requires-python = ">=3.13"

[[package]]
name = "test"
version = "0.1.0"
source = { editable = "." }
```

---

_Comment by @konstin on 2025-10-14 12:37_

For any project that you want to install, you should specify a `[build-system]` table in its `pyproject.toml`. All major build backends support editable installation, and uv install workspace members and path dependencies with `editable = true` as editable by default.

---

_Comment by @cpita-work on 2025-10-14 12:41_

Ok, I get that. The remaining question is: shouldn't uv install the current project as editable given a spec like this? (even if no build backend is set).

```
[dependency-groups]
dev = [
    "xxx",
]

[tool.uv.sources]
xxx = { path = ".", editable = true }
```

---

_Comment by @konstin on 2025-10-22 12:10_

It does install as editable with this spec for me; I do recommend however setting a build system, it leads to a much more consistent experience. A `path = "."` source should never be required in project setup.

```
$ uv sync
warning: `VIRTUAL_ENV=/home/konsti/projects/uv/.venv` does not match the project environment path `.venv` and will be ignored; use `--active` to target the active environment instead
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.14`.
Resolved 1 package in 0.66ms
Installed 1 package in 3ms
 + debug==0.0.0 (from file:///home/konsti/projects/uv/debug)
$ ls .venv/lib/python3.14/site-packages/
debug-0.0.0.dist-info  __editable__.debug-0.0.0.pth  _virtualenv.pth  _virtualenv.py
```

---
