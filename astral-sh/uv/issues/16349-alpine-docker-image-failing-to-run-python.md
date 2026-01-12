```yaml
number: 16349
title: Alpine Docker image failing to run Python libraries due to mismatched gnu vs musl libraries
type: issue
state: closed
author: TaylorZowtuk
labels:
  - bug
assignees: []
created_at: 2025-10-17T20:22:48Z
updated_at: 2025-11-04T17:29:07Z
url: https://github.com/astral-sh/uv/issues/16349
synced_at: 2026-01-12T16:02:29Z
```

# Alpine Docker image failing to run Python libraries due to mismatched gnu vs musl libraries

---

_@TaylorZowtuk_

### Question

Many of the complexities going on here are way over my head, so I apologize if this has been directly answered in issues/PRs/the docs.

Objective: I use Docker to create a shell in a container for a development environment. I am trying to use an Alpine image to avoid bloat and to have a smaller/more secure image. I don't use `python3.x-alpine` based image (unsure if that even would resolve this issue) because it doesn't seem necessary to have a system python when using uv.

Problem: I can't get numpy to import without raising an exception

Potential cause: I think due to a mismatch between the expected platform extension suffix for compiled extensions between the installed NumPy and what the Python at runtime expects? From what I can see from checking the repo, it looks like Alpine/musl has been a problem area in the past. However, since x86_64 builds of Alpine are made available, I would have thought that this should work.

Potential related issues:
- https://github.com/astral-sh/uv/pull/12121/files#diff-b803fcb7f17ed9235f1e5cb1fcd2f5d3b2838429d4368ae4c57ce4436577f03fR740
- https://github.com/astral-sh/uv/pull/4160#issuecomment-2157850192
- (not related to UV but explains what I think is going on here too) https://stackoverflow.com/questions/68752830/numpy-from-alpine-package-repo-fails-to-import-c-extensions/69360361#69360361

Minimal reproducible example TLDR:
```shell
docker run -it --rm --name build-test ghcr.io/astral-sh/uv:alpine3.22 /bin/sh
uv python pin 3.12 --verbose
uv venv --verbose
uv pip install numpy==1.26.4 --verbose
uv run python -c "import numpy" --verbose
  ...
  ModuleNotFoundError: No module named 'numpy.core._multiarray_umath'
find / -name "*_multiarray_umath*"
  /.venv/lib/python3.12/site-packages/numpy/core/_multiarray_umath.cpython-312-x86_64-linux-musl.so
  /.venv/lib/python3.12/site-packages/numpy/_core/_multiarray_umath.py
mv /.venv/lib/python3.12/site-packages/numpy/core/_multiarray_umath.cpython-312-x86_64-linux-musl.so /.venv/lib/python3.12/site-packages/numpy/core/_multiarray_umath.cpython-312-x86_64-linux-gnu.so
uv run python -c "import numpy"
  ...
  ModuleNotFoundError: No module named 'numpy.core._multiarray_tests'
```

<details>
  <summary>w/ verbose output:</summary>
  
  ```shell
  blah@blahip:~/recommender-systems$ docker run -it --rm --name build-test ghcr.io/astral-sh/uv:alpine3.22 /bin/sh
  / # uv python pin 3.12 --verbose
  DEBUG uv 0.9.3
  DEBUG Acquired shared lock for `/root/.cache/uv`
  DEBUG Failed to discover virtual project: No `pyproject.toml` found in current directory or any parent directory
  DEBUG No Python version file found in ancestors of working directory: /
  DEBUG Searching for Python 3.12 in managed installations or search path
  DEBUG Searching for managed installations at `/root/.local/share/uv/python`
  DEBUG Acquired lock for `/root/.local/share/uv/python`
  DEBUG Using request timeout of 30s
  INFO Fetching requested Python...
  DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20251014/cpython-3.12.12%2B20251014-x86_64-unknown-linux-musl-install_only_stripped.tar.gz
  DEBUG Extracting cpython-3.12.12-20251014-x86_64-unknown-linux-musl-install_only_stripped.tar.gz to temporary location: /root/.local/share/uv/python/.temp/.tmpkWnSjp
  Downloading cpython-3.12.12-linux-x86_64-musl (download) (25.8MiB)
   Downloading cpython-3.12.12-linux-x86_64-musl (download)
  DEBUG Moving /root/.local/share/uv/python/.temp/.tmpkWnSjp/python to /root/.local/share/uv/python/cpython-3.12.12-linux-x86_64-musl
  DEBUG Released lock at `/root/.local/share/uv/python/.lock`
  DEBUG Writing Python versions to `/.python-version`
  Pinned `/.python-version` to `3.12`
  DEBUG Released lock at `/root/.cache/uv/.lock`
  / # uv venv --verbose
  DEBUG uv 0.9.3
  DEBUG Acquired shared lock for `/root/.cache/uv`
  DEBUG Reading Python requests from version file at `/.python-version`
  DEBUG Using Python request `3.12` from version file at `/.python-version`
  DEBUG Searching for Python 3.12 in managed installations or search path
  DEBUG Searching for managed installations at `/root/.local/share/uv/python`
  DEBUG Found managed installation `cpython-3.12.12-linux-x86_64-musl`
  DEBUG Found `cpython-3.12.12-linux-x86_64-musl` at `/root/.local/share/uv/python/cpython-3.12.12-linux-x86_64-musl/bin/python3.12` (managed installations)
  Using CPython 3.12.12
  Creating virtual environment at: .venv
  DEBUG Assessing Python executable as base candidate: /root/.local/share/uv/python/cpython-3.12.12-linux-x86_64-musl/bin/python3.12
  DEBUG Using base executable for virtual environment: /root/.local/share/uv/python/cpython-3.12.12-linux-x86_64-musl/bin/python3.12
  DEBUG Detected parent process ID: 1
  DEBUG Parent process executable: /bin/busybox
  DEBUG Parent process comm: sh
  DEBUG Could not determine shell from parent process
  DEBUG Released lock at `/root/.cache/uv/.lock`
  / # uv pip install numpy==1.26.4 --verbose
  DEBUG uv 0.9.3
  DEBUG Acquired shared lock for `/root/.cache/uv`
  DEBUG Searching for default Python interpreter in virtual environments
  DEBUG Found `cpython-3.12.12-linux-x86_64-musl` at `/.venv/bin/python3` (virtual environment)
  DEBUG Using Python 3.12.12 environment at: /.venv
  DEBUG Acquired lock for `/.venv`
  DEBUG At least one requirement is not satisfied: numpy==1.26.4
  DEBUG Using request timeout of 30s
  DEBUG Solving with installed Python version: 3.12.12
  DEBUG Solving with target Python version: >=3.12.12
  DEBUG Adding direct dependency: numpy>=1.26.4, <1.26.4+
  DEBUG No cache entry for: https://pypi.org/simple/numpy/
  WARN Skipping file for numpy: numpy-1.0.1.dev3460.win32-py2.4.exe
  WARN Skipping file for numpy: numpy-1.3.0.win32-py2.5.exe
  WARN Skipping file for numpy: numpy-1.5.0.win32-py2.6.exe
  WARN Skipping file for numpy: numpy-1.5.1.win32-py2.5-nosse.exe
  WARN Skipping file for numpy: numpy-1.5.1.win32-py2.6-nosse.exe
  WARN Skipping file for numpy: numpy-1.5.1.win32-py2.7-nosse.exe
  WARN Skipping file for numpy: numpy-1.5.1.win32-py3.1-nosse.exe
  WARN Skipping file for numpy: numpy-1.6.0.win32-py2.5.exe
  WARN Skipping file for numpy: numpy-1.6.0.win32-py2.6.exe
  WARN Skipping file for numpy: numpy-1.6.0.win32-py2.7.exe
  WARN Skipping file for numpy: numpy-1.6.0.win32-py3.1.exe
  WARN Skipping file for numpy: numpy-1.6.0.win32-py3.2.exe
  WARN Skipping file for numpy: numpy-1.6.1.win32-py2.5.exe
  WARN Skipping file for numpy: numpy-1.6.1.win32-py2.6.exe
  WARN Skipping file for numpy: numpy-1.6.1.win32-py2.7.exe
  WARN Skipping file for numpy: numpy-1.6.1.win32-py3.1.exe
  WARN Skipping file for numpy: numpy-1.6.1.win32-py3.2.exe
  WARN Skipping file for numpy: numpy-1.6.2.win32-py2.5.exe
  WARN Skipping file for numpy: numpy-1.6.2.win32-py2.6.exe
  WARN Skipping file for numpy: numpy-1.6.2.win32-py2.7.exe
  WARN Skipping file for numpy: numpy-1.6.2.win32-py3.1.exe
  WARN Skipping file for numpy: numpy-1.6.2.win32-py3.2.exe
  WARN Skipping file for numpy: numpy-1.7.0.win32-py2.5.exe
  WARN Skipping file for numpy: numpy-1.7.0.win32-py2.6.exe
  WARN Skipping file for numpy: numpy-1.7.0.win32-py2.7.exe
  WARN Skipping file for numpy: numpy-1.7.0.win32-py3.1.exe
  WARN Skipping file for numpy: numpy-1.7.0.win32-py3.2.exe
  WARN Skipping file for numpy: numpy-1.7.0.win32-py3.3.exe
  WARN Skipping file for numpy: numpy-1.7.1.win32-py2.5.exe
  WARN Skipping file for numpy: numpy-1.7.1.win32-py2.6.exe
  WARN Skipping file for numpy: numpy-1.7.1.win32-py2.7.exe
  WARN Skipping file for numpy: numpy-1.7.1.win32-py3.1.exe
  WARN Skipping file for numpy: numpy-1.7.1.win32-py3.2.exe
  WARN Skipping file for numpy: numpy-1.7.1.win32-py3.3.exe
  DEBUG Searching for a compatible version of numpy (>=1.26.4, <1.26.4+)
  DEBUG Selecting: numpy==1.26.4 [compatible] (numpy-1.26.4-cp312-cp312-musllinux_1_1_x86_64.whl)
  DEBUG No cache entry for: https://files.pythonhosted.org/packages/76/8c/2ba3902e1a0fc1c74962ea9bb33a534bb05984ad7ff9515bf8d07527cadd/numpy-1.26.4-cp312-cp312-musllinux_1_1_x86_64.whl.metadata
  DEBUG Tried 1 versions: numpy 1
  DEBUG marker environment resolution took 0.034s
  Resolved 1 package in 34ms
  DEBUG Identified uncached distribution: numpy==1.26.4
  DEBUG No cache entry for: https://files.pythonhosted.org/packages/76/8c/2ba3902e1a0fc1c74962ea9bb33a534bb05984ad7ff9515bf8d07527cadd/numpy-1.26.4-cp312-cp312-musllinux_1_1_x86_64.whl
  Downloading numpy (17.0MiB)
   Downloading numpy
  Prepared 1 package in 224ms
  Installed 1 package in 13ms
   + numpy==1.26.4
  DEBUG Released lock at `/.venv/.lock`
  DEBUG Released lock at `/root/.cache/uv/.lock`
  / # uv run python -c "import numpy" --verbose
  Traceback (most recent call last):
    File "/.venv/lib/python3.12/site-packages/numpy/core/__init__.py", line 24, in <module>
      from . import multiarray
    File "/.venv/lib/python3.12/site-packages/numpy/core/multiarray.py", line 10, in <module>
      from . import overrides
    File "/.venv/lib/python3.12/site-packages/numpy/core/overrides.py", line 8, in <module>
      from numpy.core._multiarray_umath import (
  ModuleNotFoundError: No module named 'numpy.core._multiarray_umath'
  
  During handling of the above exception, another exception occurred:
  
  Traceback (most recent call last):
    File "/.venv/lib/python3.12/site-packages/numpy/__init__.py", line 130, in <module>
      from numpy.__config__ import show as show_config
    File "/.venv/lib/python3.12/site-packages/numpy/__config__.py", line 4, in <module>
      from numpy.core._multiarray_umath import (
    File "/.venv/lib/python3.12/site-packages/numpy/core/__init__.py", line 50, in <module>
      raise ImportError(msg)
  ImportError: 
  
  IMPORTANT: PLEASE READ THIS FOR ADVICE ON HOW TO SOLVE THIS ISSUE!
  
  Importing the numpy C-extensions failed. This error can happen for
  many reasons, often due to issues with your setup or how NumPy was
  installed.
  
  We have compiled some common reasons and troubleshooting tips at:
  
      https://numpy.org/devdocs/user/troubleshooting-importerror.html
  
  Please note and check the following:
  
    * The Python version is: Python3.12 from "/.venv/bin/python3"
    * The NumPy version is: "1.26.4"
  
  and make sure that they are the versions you expect.
  Please carefully study the documentation linked above for further help.
  
  Original error was: No module named 'numpy.core._multiarray_umath'
  
  
  The above exception was the direct cause of the following exception:
  
  Traceback (most recent call last):
    File "<string>", line 1, in <module>
    File "/.venv/lib/python3.12/site-packages/numpy/__init__.py", line 135, in <module>
      raise ImportError(msg) from e
  ImportError: Error importing numpy: you should not try to import numpy from
          its source directory; please exit the numpy source tree, and relaunch
          your python interpreter from there.
  / # find / -name "*_multiarray_umath*"
  /root/.cache/uv/archive-v0/EdwIXTxo2PosNPAW4l-F4/numpy/core/_multiarray_umath.cpython-312-x86_64-linux-musl.so
  /root/.cache/uv/archive-v0/EdwIXTxo2PosNPAW4l-F4/numpy/_core/_multiarray_umath.py
  /.venv/lib/python3.12/site-packages/numpy/core/_multiarray_umath.cpython-312-x86_64-linux-musl.so
  /.venv/lib/python3.12/site-packages/numpy/_core/_multiarray_umath.py
  / # mv /.venv/lib/python3.12/site-packages/numpy/core/_multiarray_umath.cpython-312-x86_64-linux-musl.so /.venv/lib/python3.1
  2/site-packages/numpy/core/_multiarray_umath.cpython-312-x86_64-linux-gnu.so
  / # uv run python -c "import numpy" --verbose
  Traceback (most recent call last):
    File "/.venv/lib/python3.12/site-packages/numpy/__init__.py", line 130, in <module>
      from numpy.__config__ import show as show_config
    File "/.venv/lib/python3.12/site-packages/numpy/__config__.py", line 4, in <module>
      from numpy.core._multiarray_umath import (
    File "/.venv/lib/python3.12/site-packages/numpy/core/__init__.py", line 100, in <module>
      from . import _add_newdocs
    File "/.venv/lib/python3.12/site-packages/numpy/core/_add_newdocs.py", line 4973, in <module>
      add_newdoc('numpy.core._multiarray_tests', 'format_float_OSprintf_g',
    File "/.venv/lib/python3.12/site-packages/numpy/core/function_base.py", line 543, in add_newdoc
      new = getattr(__import__(place, globals(), {}, [obj]), obj)
                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ModuleNotFoundError: No module named 'numpy.core._multiarray_tests'
  
  The above exception was the direct cause of the following exception:
  
  Traceback (most recent call last):
    File "<string>", line 1, in <module>
    File "/.venv/lib/python3.12/site-packages/numpy/__init__.py", line 135, in <module>
      raise ImportError(msg) from e
  ImportError: Error importing numpy: you should not try to import numpy from
          its source directory; please exit the numpy source tree, and relaunch
          your python interpreter from there.
  ```
  
</details>

Note: what supports the idea that the Python installation is looking for `-linux-gnu.so` libraries instead of `-linux-musl.so` is that when we rename the `_multiarray_umath` library, it seems to get past the issue and fails instead on the next improperly linked library (now `_multiarray_tests`).

I appreciate any direction on how to resolve this issue, and I apologize if this is just user error on my end!

### Platform

Linux 6.8.0-1040-aws x86_64 Linux

### Version

uv 0.9.3

---

_Label `question` added by @TaylorZowtuk on 2025-10-17 20:22_

---

_Comment by @konstin on 2025-10-19 09:27_

This is a bug of the numpy musl build for Python 3.12, I can reproduce this with numpy 2.3.4 and Python 3.12, but not with numpy 2.3.4 and Python 3.13 and 3.14. Please report this to numpy, this isn't something we can fix on the uv side.

For context, we can confirm that we have the correct musl Python interpreter with `ldd`:

```
$ ldd /.venv/bin/python
	/lib/ld-musl-x86_64.so.1 (0x7a211bab1000)
	libc.so => /lib/ld-musl-x86_64.so.1 (0x7a211bab1000)
```

The logs also confirm we're installing the musl numpy build:

```
DEBUG Selecting: numpy==2.3.4 [compatible] (numpy-2.3.4-cp312-cp312-musllinux_1_2_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b0/e7/b106253c7c0d5dc352b9c8fab91afd76a93950998167fa3e5afe4ef3a18f/numpy-2.3.4-cp312-cp312-musllinux_1_2_x86_64.whl.metadata
```

> Objective: I use Docker to create a shell in a container for a development environment. I am trying to use an Alpine image to avoid bloat and to have a smaller/more secure image. I don't use `python3.x-alpine` based image (unsure if that even would resolve this issue) because it doesn't seem necessary to have a system python when using uv.

While what you reported is a bug, please note that alpine isn't just a smaller image, musl as a libc is fundamentally different from glibc, which requires dedicated support by packages and can change runtime characteristics (https://pythonspeed.com/articles/alpine-docker-python/).

---

_Label `question` removed by @konstin on 2025-10-19 09:28_

---

_Label `external` added by @konstin on 2025-10-19 09:28_

---

_Comment by @TaylorZowtuk on 2025-10-19 17:33_

@konstin thank you so much for pointing me in the correct direction. I will open an issue in the numpy repo.

> and can change runtime characteristics

I actually did see this, and paired with being blocked by this issue, I decided to switch to using the Trixie-slim base image instead. I appreciate you pointing out this additional gotcha for me.

---

_Closed by @TaylorZowtuk on 2025-10-19 17:33_

---

_Comment by @mattip on 2025-10-20 15:56_

I am not sure there is something else going on. Where is uv getting the python it uses? When I check the extension suffixes used by that Python, there is something fishy with Python3.12 that changes when I use Python3.13. When I use the python3.12 from `apk add python3`, it uses the `*musl` extension, not `*gnu`.

```
# uv run --python 3.12 python
Python 3.12.12 (main, Oct 14 2025, 21:17:41) [Clang 14.0.3 ] on linux
>>> import _imp
>>> _imp.extension_suffixes()
['.cpython-312-x86_64-linux-gnu.so', '.abi3.so', '.so']

# uv run --python 3.13 python
Python 3.13.9 (main, Oct 14 2025, 21:18:37) [Clang 14.0.3 ] on linux
>>> import _imp              
>>> _imp.extension_suffixes()
['.cpython-313-x86_64-linux-musl.so', '.abi3.so', '.so']
```



---

_Label `external` removed by @konstin on 2025-10-20 15:59_

---

_Label `bug` added by @konstin on 2025-10-20 15:59_

---

_Comment by @konstin on 2025-10-20 16:00_

We're using Astral's Python distribution (https://github.com/astral-sh/python-build-standalone). CC @geofft That's a PBS bug I think?

---

_Reopened by @konstin on 2025-10-20 16:02_

---

_Closed by @konstin on 2025-10-20 16:02_

---

_Reopened by @konstin on 2025-10-20 16:02_

---

_Comment by @TaylorZowtuk on 2025-10-20 16:06_

Ah hang on, I think this is a known issue; https://github.com/astral-sh/python-build-standalone/issues/724


---

_Comment by @samypr100 on 2025-10-21 01:04_

> docker run -it --rm --name build-test ghcr.io/astral-sh/uv:alpine3.22 /bin/sh

@TaylorZowtuk You could use `docker run -it --rm --name build-test ghcr.io/astral-sh/uv:python3.12-alpine /bin/sh` for the time being and it won't use the PBS build and the image is Alpine 3.22 based upstream from official Python docker images. The container size ends up being relatively similar as PBS would get downloaded on the pure alpine one.

---

_Comment by @geofft on 2025-10-29 21:31_

This should be fixed with the Python versions in the latest version of uv (0.9.6). Please do
```
uv self update
uv python install --reinstall
```
and see if that fixes it!

---

_Comment by @TaylorZowtuk on 2025-11-04 17:29_

> This should be fixed with the Python versions in the latest version of uv (0.9.6)
Following the same steps in my minimal reproducible example no longer results in an error when we get to the step attempting to import numpy.

Commands executed:
```shell
$ docker run -it --rm --name build-test ghcr.io/astral-sh/uv:alpine3.22 /bin/sh
/ # uv python pin 3.12 --verbose
/ # uv venv --verbose
/ # uv pip install numpy==1.26.4 --verbose
/ # uv run python -c "import numpy" --verbose

/ # uv run python -c "import numpy as np;a=np.array([[1,2],[3,4]]);b=np.array([[5,6],[7,8]]);print(np.dot(a,b));"
[[19 22]
 [43 50]]
/ # uv --version
uv 0.9.7

/ # uv run --python 3.12 python
Python 3.12.12 (main, Oct 28 2025, 11:59:18) [Clang 14.0.3 ] on linux
>>> import _imp
>>> _imp.extension_suffixes()
['.cpython-312-x86_64-linux-musl.so', '.abi3.so', '.so']
```

Thanks for the help/fix everyone.

---

_Closed by @TaylorZowtuk on 2025-11-04 17:29_

---
