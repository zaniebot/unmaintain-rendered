```yaml
number: 8966
title: "uv doesn't play well with meson-python build backend and pytest"
type: issue
state: open
author: hacksparr0w
labels:
  - needs-mre
assignees: []
created_at: 2024-11-09T11:39:11Z
updated_at: 2024-11-10T00:13:02Z
url: https://github.com/astral-sh/uv/issues/8966
synced_at: 2026-01-12T15:59:39Z
```

# uv doesn't play well with meson-python build backend and pytest

---

_@hacksparr0w_

I'm having problems running tests in my project using the meson-python build backend. Whenever I run pytest using `uv run pytest`, the command fails due to a module resolution issue.

The exact commands to reproduce this with their output are below.

```bash
root@b0407d055a7e:/app# uv run --verbose pytest
DEBUG uv 0.5.1
DEBUG Found project root: `/app`
DEBUG No workspace root found, using project root
DEBUG Discovered project `argon2` at: /app
DEBUG Reading Python requests from version file at `/app/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found `cpython-3.12.7-linux-x86_64-gnu` at `/usr/local/bin/python` (search path)
Using CPython 3.12.7 interpreter at: /usr/local/bin/python
Creating virtual environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: argon2 @ file:///app
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 10 packages in 3ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: argon2 @ file:///app
DEBUG Identified uncached distribution: iniconfig==2.0.0
DEBUG Identified uncached distribution: nodeenv==1.9.1
DEBUG Identified uncached distribution: packaging==24.2
DEBUG Identified uncached distribution: pluggy==1.5.0
DEBUG Identified uncached distribution: pyright==1.1.388
DEBUG Identified uncached distribution: pytest==8.3.3
DEBUG Identified uncached distribution: ruff==0.7.2
DEBUG Identified uncached distribution: typing-extensions==4.12.2
DEBUG Acquired lock for `/root/.cache/uv/sdists-v6/editable/df64fc0b914aef95`
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d2/1d/1b658dbd2b9fa9c4c9f32accbfc0205d532c8c6194dc0f2a4c0428e7128a/nodeenv-1.9.1-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a0/57/4642e57484d80d274750dcc872ea66655bbd7e66e986fede31e1865b463d/ruff-0.7.2-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6b/77/7440a06a8ead44c7757a64362dd22df5760f9b12dc5f11b6188cd2fc27a0/pytest-8.3.3-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/03/57/7fb00363b7f267a398c5bdf4f55f3e64f7c2076b2e7d2901b3373d52b6ff/pyright-1.1.388-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl
DEBUG Building: argon2 @ file:///app
DEBUG No workspace root found, using project root
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: meson-python*
DEBUG No cache entry for: https://pypi.org/simple/meson-python/
DEBUG Searching for a compatible version of meson-python (*)
DEBUG Selecting: meson-python==0.17.1 [compatible] (meson_python-0.17.1-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7d/ec/40c0ddd29ef4daa6689a2b9c5ced47d5b58fa54ae149b19e9a97f4979c8c/meson_python-0.17.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for meson-python==0.17.1: meson{python_full_version >= '3.12'}>=1.2.3
DEBUG Adding transitive dependency for meson-python==0.17.1: packaging>=19.0
DEBUG Adding transitive dependency for meson-python==0.17.1: pyproject-metadata>=0.7.1
DEBUG No cache entry for: https://pypi.org/simple/meson/
DEBUG No cache entry for: https://pypi.org/simple/packaging/
DEBUG No cache entry for: https://pypi.org/simple/pyproject-metadata/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e8/61/9dd3e68d2b6aa40a5fc678662919be3c3a7bf22cba5a6b4437619b77e156/pyproject_metadata-0.9.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (>=1.2.3)
DEBUG Selecting: meson==1.6.0 [compatible] (meson-1.6.0-py3-none-any.whl)
DEBUG Adding transitive dependency for meson==1.6.0: meson==1.6.0
DEBUG Adding transitive dependency for meson==1.6.0: meson{python_full_version >= '3.12'}==1.6.0
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (==1.6.0)
DEBUG Selecting: meson==1.6.0 [compatible] (meson-1.6.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/76/73/3dc4edc855c9988ff05ea5590f5c7bda72b6e0d138b2ddc1fab92a1f242f/meson-1.6.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of meson (==1.6.0)
DEBUG Selecting: meson==1.6.0 [compatible] (meson-1.6.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=19.0)
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Searching for a compatible version of pyproject-metadata (>=0.7.1)
DEBUG Selecting: pyproject-metadata==0.9.0 [compatible] (pyproject_metadata-0.9.0-py3-none-any.whl)
DEBUG Adding transitive dependency for pyproject-metadata==0.9.0: packaging>=19.0
DEBUG Tried 4 versions: meson 1, meson-python 1, packaging 1, pyproject-metadata 1
DEBUG marker environment resolution took 1.076s
DEBUG Installing in meson==1.6.0, meson-python==0.17.1, packaging==24.2, pyproject-metadata==0.9.0 in /root/.cache/uv/builds-v0/.tmpkZMobh
DEBUG Identified uncached distribution: meson==1.6.0
DEBUG Identified uncached distribution: meson-python==0.17.1
DEBUG Requirement already cached: packaging==24.2
DEBUG Identified uncached distribution: pyproject-metadata==0.9.0
DEBUG Downloading and building requirements for build: meson==1.6.0, meson-python==0.17.1, pyproject-metadata==0.9.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/76/73/3dc4edc855c9988ff05ea5590f5c7bda72b6e0d138b2ddc1fab92a1f242f/meson-1.6.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7d/ec/40c0ddd29ef4daa6689a2b9c5ced47d5b58fa54ae149b19e9a97f4979c8c/meson_python-0.17.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e8/61/9dd3e68d2b6aa40a5fc678662919be3c3a7bf22cba5a6b4437619b77e156/pyproject_metadata-0.9.0-py3-none-any.whl
DEBUG Installing build requirements: pyproject-metadata==0.9.0, meson-python==0.17.1, meson==1.6.0, packaging==24.2
DEBUG Creating PEP 517 build environment
DEBUG Calling `mesonpy.get_requires_for_build_editable()`
DEBUG No workspace root found, using project root
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: meson-python*
DEBUG Adding direct dependency: ninja>=1.8.2
DEBUG Adding direct dependency: patchelf>=0.11.0
DEBUG Searching for a compatible version of meson-python (*)
DEBUG Selecting: meson-python==0.17.1 [compatible] (meson_python-0.17.1-py3-none-any.whl)
DEBUG Adding transitive dependency for meson-python==0.17.1: meson{python_full_version >= '3.12'}>=1.2.3
DEBUG Adding transitive dependency for meson-python==0.17.1: packaging>=19.0
DEBUG Adding transitive dependency for meson-python==0.17.1: pyproject-metadata>=0.7.1
DEBUG No cache entry for: https://pypi.org/simple/patchelf/
DEBUG No cache entry for: https://pypi.org/simple/ninja/
DEBUG Searching for a compatible version of ninja (>=1.8.2)
DEBUG Selecting: ninja==1.11.1.1 [compatible] (ninja-1.11.1.1-py2.py3-none-manylinux1_x86_64.manylinux_2_5_x86_64.whl)        
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6d/92/8d7aebd4430ab5ff65df2bfee6d5745f95c004284db2d8ca76dcbfd9de47/ninja-1.11.1.1-py2.py3-none-manylinux1_x86_64.manylinux_2_5_x86_64.whl.metadata
DEBUG Searching for a compatible version of patchelf (>=0.11.0)
DEBUG Searching for a compatible version of patchelf (>=0.11.0, <0.18.0.0 | >0.18.0.0)
DEBUG Selecting: patchelf==0.17.2.1 [compatible] (patchelf-0.17.2.1-py2.py3-none-manylinux_2_5_x86_64.manylinux1_x86_64.musllinux_1_1_x86_64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c6/73/c3105c973dd2afcdc5d946ee211d5c4ecdf9d27bb54ae835b144e706e86d/patchelf-0.17.2.1-py2.py3-none-manylinux_2_5_x86_64.manylinux1_x86_64.musllinux_1_1_x86_64.whl.metadata
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (>=1.2.3)
DEBUG Selecting: meson==1.6.0 [compatible] (meson-1.6.0-py3-none-any.whl)
DEBUG Adding transitive dependency for meson==1.6.0: meson==1.6.0
DEBUG Adding transitive dependency for meson==1.6.0: meson{python_full_version >= '3.12'}==1.6.0
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (==1.6.0)
DEBUG Selecting: meson==1.6.0 [compatible] (meson-1.6.0-py3-none-any.whl)
DEBUG Searching for a compatible version of meson (==1.6.0)
DEBUG Selecting: meson==1.6.0 [compatible] (meson-1.6.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=19.0)
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Searching for a compatible version of pyproject-metadata (>=0.7.1)
DEBUG Selecting: pyproject-metadata==0.9.0 [compatible] (pyproject_metadata-0.9.0-py3-none-any.whl)
DEBUG Adding transitive dependency for pyproject-metadata==0.9.0: packaging>=19.0
DEBUG Tried 7 versions: patchelf 2, meson 1, meson-python 1, ninja 1, packaging 1, pyproject-metadata 1
DEBUG marker environment resolution took 1.910s
DEBUG Installing in meson==1.6.0, meson-python==0.17.1, ninja==1.11.1.1, packaging==24.2, patchelf==0.17.2.1, pyproject-metadata==0.9.0 in /root/.cache/uv/builds-v0/.tmpkZMobh
DEBUG Requirement already installed: meson==1.6.0
DEBUG Requirement already installed: meson-python==0.17.1
DEBUG Identified uncached distribution: ninja==1.11.1.1
DEBUG Requirement already installed: packaging==24.2
DEBUG Identified uncached distribution: patchelf==0.17.2.1
DEBUG Requirement already installed: pyproject-metadata==0.9.0
DEBUG Downloading and building requirements for build: ninja==1.11.1.1, patchelf==0.17.2.1
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c6/73/c3105c973dd2afcdc5d946ee211d5c4ecdf9d27bb54ae835b144e706e86d/patchelf-0.17.2.1-py2.py3-none-manylinux_2_5_x86_64.manylinux1_x86_64.musllinux_1_1_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6d/92/8d7aebd4430ab5ff65df2bfee6d5745f95c004284db2d8ca76dcbfd9de47/ninja-1.11.1.1-py2.py3-none-manylinux1_x86_64.manylinux_2_5_x86_64.whl
DEBUG Installing build requirements: ninja==1.11.1.1, patchelf==0.17.2.1
DEBUG Calling `mesonpy.build_editable("/root/.cache/uv/builds-v0/.tmpGpfFXh", {}, None)`
DEBUG + meson setup /app /app/build/cp312 -Dbuildtype=release -Db_ndebug=if-release -Db_vscrt=md --native-file=/app/build/cp312/meson-python-native-file.ini
DEBUG The Meson build system
DEBUG Version: 1.6.0
DEBUG Source dir: /app
DEBUG Build dir: /app/build/cp312
DEBUG Build type: native build
DEBUG Project name: argon2
DEBUG Project version: undefined
DEBUG C compiler for the host machine: cc (gcc 12.2.0 "cc (Debian 12.2.0-14) 12.2.0")
DEBUG C linker for the host machine: cc ld.bfd 2.40
DEBUG Host machine cpu family: x86_64
DEBUG Host machine cpu: x86_64
DEBUG Program python found: YES (/root/.cache/uv/builds-v0/.tmpkZMobh/bin/python)
DEBUG Build targets in project: 1
DEBUG
DEBUG argon2 undefined
DEBUG
DEBUG   User defined options
DEBUG     Native files: /app/build/cp312/meson-python-native-file.ini
DEBUG     b_ndebug    : if-release
DEBUG     b_vscrt     : md
DEBUG     buildtype   : release
DEBUG
DEBUG Found ninja-1.11.1.git.kitware.jobserver-1 at /root/.cache/uv/builds-v0/.tmpkZMobh/bin/ninja
DEBUG + /root/.cache/uv/builds-v0/.tmpkZMobh/bin/ninja
DEBUG [1/7] Compiling C object libargon2.so.p/libargon2_src_thread.c.o
DEBUG [2/7] Compiling C object libargon2.so.p/libargon2_src_argon2.c.o
DEBUG [3/7] Compiling C object libargon2.so.p/libargon2_src_blake2_blake2b.c.o
DEBUG [4/7] Compiling C object libargon2.so.p/libargon2_src_encoding.c.o
DEBUG [5/7] Compiling C object libargon2.so.p/libargon2_src_core.c.o
DEBUG [6/7] Compiling C object libargon2.so.p/libargon2_src_ref.c.o
DEBUG [7/7] Linking target libargon2.so
DEBUG Finished building: argon2 @ file:///app
DEBUG Released lock at `/root/.cache/uv/sdists-v6/editable/df64fc0b914aef95/.lock`
Prepared 9 packages in 18.46s
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/nodeenv.py` to `/root/.cache/uv/archive-v0/tb1-SzQbw2gLSJjc4j8tW/nodeenv.py`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/_pytest/stepwise.py` to `/root/.cache/uv/archive-v0/bQblO6m7lBX3GUgKeDzQS/_pytest/stepwise.py`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/iniconfig/exceptions.py` to `/root/.cache/uv/archive-v0/nMi_CadvDuxEzmak5E5fb/iniconfig/exceptions.py`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/argon2-0.1.0.dist-info/WHEEL` to `/root/.cache/uv/archive-v0/-m5PSXPoKQxLcLeVVY9-1/argon2-0.1.0.dist-info/WHEEL`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/pluggy-1.5.0.dist-info/WHEEL` to `/root/.cache/uv/archive-v0/faIc_ZsGuZ0iy0NLu0S-W/pluggy-1.5.0.dist-info/WHEEL`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/typing_extensions-4.12.2.dist-info/WHEEL` to `/root/.cache/uv/archive-v0/8QKP1TBx-BFdcFUSZGw3y/typing_extensions-4.12.2.dist-info/WHEEL`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/packaging/_parser.py` to `/root/.cache/uv/archive-v0/r7vFqOyFZIe77AgSV7vCI/packaging/_parser.py`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/pyright-1.1.388.dist-info/WHEEL` to `/root/.cache/uv/archive-v0/iT4B5aCPns2GBCprf4n3R/pyright-1.1.388.dist-info/WHEEL`, attempting to copy files as a fallback
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/ruff/__init__.py` to `/root/.cache/uv/archive-v0/-FcV2FEOje4TR6jV_BSnU/ruff/__init__.py`, attempting to copy files as a fallback
Installed 9 packages in 1.06s
 + argon2==0.1.0 (from file:///app)
 + iniconfig==2.0.0
 + nodeenv==1.9.1
 + packaging==24.2
 + pluggy==1.5.0
 + pyright==1.1.388
 + pytest==8.3.3
 + ruff==0.7.2
 + typing-extensions==4.12.2
DEBUG Using Python 3.12.7 interpreter at: /app/.venv/bin/python
DEBUG Running `pytest`
==================================================== test session starts =====================================================
platform linux -- Python 3.12.7, pytest-8.3.3, pluggy-1.5.0
rootdir: /app
configfile: pyproject.toml
collected 0 items / 1 error

=========================================================== ERRORS ===========================================================
____________________________________________ ERROR collecting tests/test_kats.py _____________________________________________
tests/test_kats.py:3: in <module>
    import argon2
<frozen importlib._bootstrap>:1360: in _find_and_load
    ???
<frozen importlib._bootstrap>:1322: in _find_and_load_unlocked
    ???
<frozen importlib._bootstrap>:1262: in _find_spec
    ???
.venv/lib/python3.12/site-packages/_argon2_editable_loader.py:311: in find_spec
    tree = self._rebuild()
.venv/lib/python3.12/site-packages/_argon2_editable_loader.py:345: in _rebuild
    subprocess.run(self._build_cmd, cwd=self._build_path, env=env, stdout=subprocess.DEVNULL, check=True)
/usr/local/lib/python3.12/subprocess.py:548: in run
    with Popen(*popenargs, **kwargs) as process:
/usr/local/lib/python3.12/subprocess.py:1026: in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
/usr/local/lib/python3.12/subprocess.py:1955: in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
E   FileNotFoundError: [Errno 2] No such file or directory: '/root/.cache/uv/builds-v0/.tmpkZMobh/bin/ninja'
================================================== short test summary info ===================================================
ERROR tests/test_kats.py - FileNotFoundError: [Errno 2] No such file or directory: '/root/.cache/uv/builds-v0/.tmpkZMobh/bin/ninja'
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! Interrupted: 1 error during collection !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
====================================================== 1 error in 1.99s ======================================================
DEBUG Command exited with code: 2
```

meson-python allows for using non-temporary build folders by passing the `-Cbuild-dir` argument, but I had no luck even when using this option, as seen below.


```
root@85a6eec4d500:/app# uv run --verbose --no-cache -Cbuild-dir=./asdf pytest
DEBUG uv 0.5.1
DEBUG Found project root: `/app`
DEBUG No workspace root found, using project root
DEBUG Discovered project `argon2` at: /app
DEBUG Reading Python requests from version file at `/app/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found `cpython-3.12.7-linux-x86_64-gnu` at `/usr/local/bin/python` (search path)
Using CPython 3.12.7 interpreter at: /usr/local/bin/python
Creating virtual environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: argon2 @ file:///app
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 10 packages in 5ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: argon2 @ file:///app
DEBUG Identified uncached distribution: iniconfig==2.0.0
DEBUG Identified uncached distribution: nodeenv==1.9.1
DEBUG Identified uncached distribution: packaging==24.2
DEBUG Identified uncached distribution: pluggy==1.5.0
DEBUG Identified uncached distribution: pyright==1.1.388
DEBUG Identified uncached distribution: pytest==8.3.3
DEBUG Identified uncached distribution: ruff==0.7.2
DEBUG Identified uncached distribution: typing-extensions==4.12.2
DEBUG Acquired lock for `/tmp/.tmp6MYXQI/sdists-v6/editable/df64fc0b914aef95`
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a0/57/4642e57484d80d274750dcc872ea66655bbd7e66e986fede31e1865b463d/ruff-0.7.2-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6b/77/7440a06a8ead44c7757a64362dd22df5760f9b12dc5f11b6188cd2fc27a0/pytest-8.3.3-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d2/1d/1b658dbd2b9fa9c4c9f32accbfc0205d532c8c6194dc0f2a4c0428e7128a/nodeenv-1.9.1-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/03/57/7fb00363b7f267a398c5bdf4f55f3e64f7c2076b2e7d2901b3373d52b6ff/pyright-1.1.388-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl
DEBUG Building: argon2 @ file:///app
DEBUG No workspace root found, using project root
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: meson-python*
DEBUG No cache entry for: https://pypi.org/simple/meson-python/
DEBUG Searching for a compatible version of meson-python (*)
DEBUG Selecting: meson-python==0.17.1 [compatible] (meson_python-0.17.1-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7d/ec/40c0ddd29ef4daa6689a2b9c5ced47d5b58fa54ae149b19e9a97f4979c8c/meson_python-0.17.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for meson-python==0.17.1: meson{python_full_version >= '3.12'}>=1.2.3
DEBUG Adding transitive dependency for meson-python==0.17.1: packaging>=19.0
DEBUG Adding transitive dependency for meson-python==0.17.1: pyproject-metadata>=0.7.1
DEBUG No cache entry for: https://pypi.org/simple/meson/
DEBUG No cache entry for: https://pypi.org/simple/packaging/
DEBUG No cache entry for: https://pypi.org/simple/pyproject-metadata/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (>=1.2.3)
DEBUG Selecting: meson==1.6.0 [compatible] (meson-1.6.0-py3-none-any.whl)
DEBUG Adding transitive dependency for meson==1.6.0: meson==1.6.0
DEBUG Adding transitive dependency for meson==1.6.0: meson{python_full_version >= '3.12'}==1.6.0
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (==1.6.0)
DEBUG Selecting: meson==1.6.0 [compatible] (meson-1.6.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/76/73/3dc4edc855c9988ff05ea5590f5c7bda72b6e0d138b2ddc1fab92a1f242f/meson-1.6.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e8/61/9dd3e68d2b6aa40a5fc678662919be3c3a7bf22cba5a6b4437619b77e156/pyproject_metadata-0.9.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of meson (==1.6.0)
DEBUG Selecting: meson==1.6.0 [compatible] (meson-1.6.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=19.0)
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Searching for a compatible version of pyproject-metadata (>=0.7.1)
DEBUG Selecting: pyproject-metadata==0.9.0 [compatible] (pyproject_metadata-0.9.0-py3-none-any.whl)
DEBUG Adding transitive dependency for pyproject-metadata==0.9.0: packaging>=19.0
DEBUG Tried 4 versions: meson 1, meson-python 1, packaging 1, pyproject-metadata 1
DEBUG marker environment resolution took 1.173s
DEBUG Installing in meson==1.6.0, meson-python==0.17.1, packaging==24.2, pyproject-metadata==0.9.0 in /tmp/.tmp6MYXQI/builds-v0/.tmp3dEbLu
DEBUG Identified uncached distribution: meson==1.6.0
DEBUG Identified uncached distribution: meson-python==0.17.1
DEBUG Requirement already cached: packaging==24.2
DEBUG Identified uncached distribution: pyproject-metadata==0.9.0
DEBUG Downloading and building requirements for build: meson==1.6.0, meson-python==0.17.1, pyproject-metadata==0.9.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7d/ec/40c0ddd29ef4daa6689a2b9c5ced47d5b58fa54ae149b19e9a97f4979c8c/meson_python-0.17.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/76/73/3dc4edc855c9988ff05ea5590f5c7bda72b6e0d138b2ddc1fab92a1f242f/meson-1.6.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e8/61/9dd3e68d2b6aa40a5fc678662919be3c3a7bf22cba5a6b4437619b77e156/pyproject_metadata-0.9.0-py3-none-any.whl
DEBUG Installing build requirements: pyproject-metadata==0.9.0, meson-python==0.17.1, meson==1.6.0, packaging==24.2
DEBUG Creating PEP 517 build environment
DEBUG Calling `mesonpy.get_requires_for_build_editable()`
DEBUG No workspace root found, using project root
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: meson-python*
DEBUG Adding direct dependency: ninja>=1.8.2
DEBUG Adding direct dependency: patchelf>=0.11.0
DEBUG Searching for a compatible version of meson-python (*)
DEBUG Selecting: meson-python==0.17.1 [compatible] (meson_python-0.17.1-py3-none-any.whl)
DEBUG Adding transitive dependency for meson-python==0.17.1: meson{python_full_version >= '3.12'}>=1.2.3
DEBUG Adding transitive dependency for meson-python==0.17.1: packaging>=19.0
DEBUG Adding transitive dependency for meson-python==0.17.1: pyproject-metadata>=0.7.1
DEBUG No cache entry for: https://pypi.org/simple/ninja/
DEBUG No cache entry for: https://pypi.org/simple/patchelf/
DEBUG Searching for a compatible version of ninja (>=1.8.2)
DEBUG Selecting: ninja==1.11.1.1 [compatible] (ninja-1.11.1.1-py2.py3-none-manylinux1_x86_64.manylinux_2_5_x86_64.whl)        
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6d/92/8d7aebd4430ab5ff65df2bfee6d5745f95c004284db2d8ca76dcbfd9de47/ninja-1.11.1.1-py2.py3-none-manylinux1_x86_64.manylinux_2_5_x86_64.whl.metadata
DEBUG Searching for a compatible version of patchelf (>=0.11.0)
DEBUG Searching for a compatible version of patchelf (>=0.11.0, <0.18.0.0 | >0.18.0.0)
DEBUG Selecting: patchelf==0.17.2.1 [compatible] (patchelf-0.17.2.1-py2.py3-none-manylinux_2_5_x86_64.manylinux1_x86_64.musllinux_1_1_x86_64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c6/73/c3105c973dd2afcdc5d946ee211d5c4ecdf9d27bb54ae835b144e706e86d/patchelf-0.17.2.1-py2.py3-none-manylinux_2_5_x86_64.manylinux1_x86_64.musllinux_1_1_x86_64.whl.metadata
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (>=1.2.3)
DEBUG Selecting: meson==1.6.0 [compatible] (meson-1.6.0-py3-none-any.whl)
DEBUG Adding transitive dependency for meson==1.6.0: meson==1.6.0
DEBUG Adding transitive dependency for meson==1.6.0: meson{python_full_version >= '3.12'}==1.6.0
DEBUG Searching for a compatible version of meson{python_full_version >= '3.12'} (==1.6.0)
DEBUG Selecting: meson==1.6.0 [compatible] (meson-1.6.0-py3-none-any.whl)
DEBUG Searching for a compatible version of meson (==1.6.0)
DEBUG Selecting: meson==1.6.0 [compatible] (meson-1.6.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=19.0)
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Searching for a compatible version of pyproject-metadata (>=0.7.1)
DEBUG Selecting: pyproject-metadata==0.9.0 [compatible] (pyproject_metadata-0.9.0-py3-none-any.whl)
DEBUG Adding transitive dependency for pyproject-metadata==0.9.0: packaging>=19.0
DEBUG Tried 7 versions: patchelf 2, meson 1, meson-python 1, ninja 1, packaging 1, pyproject-metadata 1
DEBUG marker environment resolution took 0.445s
DEBUG Installing in meson==1.6.0, meson-python==0.17.1, ninja==1.11.1.1, packaging==24.2, patchelf==0.17.2.1, pyproject-metadata==0.9.0 in /tmp/.tmp6MYXQI/builds-v0/.tmp3dEbLu
DEBUG Requirement already installed: meson==1.6.0
DEBUG Requirement already installed: meson-python==0.17.1
DEBUG Identified uncached distribution: ninja==1.11.1.1
DEBUG Requirement already installed: packaging==24.2
DEBUG Identified uncached distribution: patchelf==0.17.2.1
DEBUG Requirement already installed: pyproject-metadata==0.9.0
DEBUG Downloading and building requirements for build: ninja==1.11.1.1, patchelf==0.17.2.1
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6d/92/8d7aebd4430ab5ff65df2bfee6d5745f95c004284db2d8ca76dcbfd9de47/ninja-1.11.1.1-py2.py3-none-manylinux1_x86_64.manylinux_2_5_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c6/73/c3105c973dd2afcdc5d946ee211d5c4ecdf9d27bb54ae835b144e706e86d/patchelf-0.17.2.1-py2.py3-none-manylinux_2_5_x86_64.manylinux1_x86_64.musllinux_1_1_x86_64.whl
DEBUG Installing build requirements: ninja==1.11.1.1, patchelf==0.17.2.1
DEBUG Calling `mesonpy.build_editable("/tmp/.tmp6MYXQI/builds-v0/.tmp1kVsr6", {"build-dir":"./asdf"}, None)`
DEBUG + meson setup /app /app/asdf -Dbuildtype=release -Db_ndebug=if-release -Db_vscrt=md --native-file=/app/asdf/meson-python-native-file.ini
DEBUG The Meson build system
DEBUG Version: 1.6.0
DEBUG Source dir: /app
DEBUG Build dir: /app/asdf
DEBUG Build type: native build
DEBUG Project name: argon2
DEBUG Project version: undefined
DEBUG C compiler for the host machine: cc (gcc 12.2.0 "cc (Debian 12.2.0-14) 12.2.0")
DEBUG C linker for the host machine: cc ld.bfd 2.40
DEBUG Host machine cpu family: x86_64
DEBUG Host machine cpu: x86_64
DEBUG Program python found: YES (/tmp/.tmp6MYXQI/builds-v0/.tmp3dEbLu/bin/python)
DEBUG Build targets in project: 1
DEBUG
DEBUG argon2 undefined
DEBUG
DEBUG   User defined options
DEBUG     Native files: /app/asdf/meson-python-native-file.ini
DEBUG     b_ndebug    : if-release
DEBUG     b_vscrt     : md
DEBUG     buildtype   : release
DEBUG
DEBUG Found ninja-1.11.1.git.kitware.jobserver-1 at /tmp/.tmp6MYXQI/builds-v0/.tmp3dEbLu/bin/ninja
DEBUG + /tmp/.tmp6MYXQI/builds-v0/.tmp3dEbLu/bin/ninja
DEBUG [1/7] Compiling C object libargon2.so.p/libargon2_src_thread.c.o
DEBUG [2/7] Compiling C object libargon2.so.p/libargon2_src_blake2_blake2b.c.o
DEBUG [3/7] Compiling C object libargon2.so.p/libargon2_src_argon2.c.o
DEBUG [4/7] Compiling C object libargon2.so.p/libargon2_src_encoding.c.o
DEBUG [5/7] Compiling C object libargon2.so.p/libargon2_src_core.c.o
DEBUG [6/7] Compiling C object libargon2.so.p/libargon2_src_ref.c.o
DEBUG [7/7] Linking target libargon2.so
DEBUG Finished building: argon2 @ file:///app
DEBUG Released lock at `/tmp/.tmp6MYXQI/sdists-v6/editable/df64fc0b914aef95/.lock`
Prepared 9 packages in 13.72s
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/nodeenv.py` to `/tmp/.tmp6MYXQI/archive-v0/AZzhbKbTEXMJhhJHNWvPq/nodeenv.py`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/_pytest/stepwise.py` to `/tmp/.tmp6MYXQI/archive-v0/RWzx4EvgVjd9z6jlHaA_0/_pytest/stepwise.py`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/packaging/_parser.py` to `/tmp/.tmp6MYXQI/archive-v0/zJt6ckvjuO0Ffbo2yt7lc/packaging/_parser.py`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/typing_extensions-4.12.2.dist-info/WHEEL` to `/tmp/.tmp6MYXQI/archive-v0/w3XBrPtSoa2WDsnZ-st4r/typing_extensions-4.12.2.dist-info/WHEEL`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/iniconfig/exceptions.py` to `/tmp/.tmp6MYXQI/archive-v0/wbP-baDwWImVLSOSGVwnG/iniconfig/exceptions.py`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/pyright-1.1.388.dist-info/WHEEL` to `/tmp/.tmp6MYXQI/archive-v0/MbdtoqZSaYKyb--tkxzgm/pyright-1.1.388.dist-info/WHEEL`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/pluggy-1.5.0.dist-info/WHEEL` to `/tmp/.tmp6MYXQI/archive-v0/gZpe5UTtpi-Q7jUCguyOy/pluggy-1.5.0.dist-info/WHEEL`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/argon2-0.1.0.dist-info/WHEEL` to `/tmp/.tmp6MYXQI/archive-v0/eHsc0zNV6LbaefsHNGiwn/argon2-0.1.0.dist-info/WHEEL`, attempting to copy files as a fallback
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
DEBUG Failed to hardlink `/app/.venv/lib/python3.12/site-packages/ruff/__init__.py` to `/tmp/.tmp6MYXQI/archive-v0/de2PDSf1w5ZeJJMj8CDT_/ruff/__init__.py`, attempting to copy files as a fallback
Installed 9 packages in 1.08s
 + argon2==0.1.0 (from file:///app)
 + iniconfig==2.0.0
 + nodeenv==1.9.1
 + packaging==24.2
 + pluggy==1.5.0
 + pyright==1.1.388
 + pytest==8.3.3
 + ruff==0.7.2
 + typing-extensions==4.12.2
DEBUG Using Python 3.12.7 interpreter at: /app/.venv/bin/python
DEBUG Running `pytest`
==================================================== test session starts =====================================================
platform linux -- Python 3.12.7, pytest-8.3.3, pluggy-1.5.0
rootdir: /app
configfile: pyproject.toml
collected 0 items / 1 error

=========================================================== ERRORS ===========================================================
____________________________________________ ERROR collecting tests/test_kats.py _____________________________________________
tests/test_kats.py:3: in <module>
    import argon2
<frozen importlib._bootstrap>:1360: in _find_and_load
    ???
<frozen importlib._bootstrap>:1322: in _find_and_load_unlocked
    ???
<frozen importlib._bootstrap>:1262: in _find_spec
    ???
.venv/lib/python3.12/site-packages/_argon2_editable_loader.py:311: in find_spec
    tree = self._rebuild()
.venv/lib/python3.12/site-packages/_argon2_editable_loader.py:345: in _rebuild
    subprocess.run(self._build_cmd, cwd=self._build_path, env=env, stdout=subprocess.DEVNULL, check=True)
/usr/local/lib/python3.12/subprocess.py:548: in run
    with Popen(*popenargs, **kwargs) as process:
/usr/local/lib/python3.12/subprocess.py:1026: in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
/usr/local/lib/python3.12/subprocess.py:1955: in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
E   FileNotFoundError: [Errno 2] No such file or directory: '/tmp/.tmp6MYXQI/builds-v0/.tmp3dEbLu/bin/ninja'
================================================== short test summary info ===================================================
ERROR tests/test_kats.py - FileNotFoundError: [Errno 2] No such file or directory: '/tmp/.tmp6MYXQI/builds-v0/.tmp3dEbLu/bin/ninja'
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! Interrupted: 1 error during collection !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
====================================================== 1 error in 1.91s ======================================================
DEBUG Command exited with code: 2
```

All of the commands above were run in a clean `python:3.12` Docker container. The python version used was `3.12.7`. The uv version used was `0.5.1`.

Following is my pyproject.toml file.

```toml
[project]
name = "argon2"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "hacksparr0w", email = "hacksparr0w@protonmail.com" }
]
requires-python = ">=3.12"
dependencies = []

[build-system]
build-backend = "mesonpy"
requires = ["meson-python"]

[dependency-groups]
dev = [
    "pyright>=1.1.388",
    "pytest>=8.3.3",
    "ruff>=0.7.2",
]
```

I can link the repository in question, if necessary.

---

_Comment by @charliermarsh on 2024-11-09 13:39_

Would you mind sharing a full reproduction? Like a repository (as mentioned at the end) and a sequence of commands?

---

_Label `needs-mre` added by @charliermarsh on 2024-11-09 13:39_

---

_Comment by @hacksparr0w on 2024-11-09 13:49_

Thank you for your quick reply.

The repository in question is available here: https://github.com/hacksparr0w/argon2

I'm using the following commands to get the environment up and running:

```sh
docker run --volume C:\Users\hacksparr0w\argon2:/app -it python:3.12 bash
```

Then once in the container:

```sh
cd /app
pip install uv
uv run pytest
```

---

_Comment by @samypr100 on 2024-11-09 17:15_

There's known incompatibilties with meson-python as it does not work well with build isolation.

Some of this was primarily discussed in https://github.com/astral-sh/uv/pull/7857


---

_Comment by @woutervh on 2024-11-09 17:36_

Maybe related:
The package **dbus-python** has mesonpy-build-backend.
I can not install that package with an uv-installed python.  (any known workarounds?)
Issue here:  https://gitlab.freedesktop.org/dbus/dbus-python/-/issues/53

---

_Comment by @samypr100 on 2024-11-09 23:50_

> Maybe related: The package **dbus-python** has mesonpy-build-backend. I can not install that package with an uv-installed python. (any known workarounds?) Issue here: https://gitlab.freedesktop.org/dbus/dbus-python/-/issues/53

@woutervh I think this is unrelated to this issue 😄

In your case, it seems that error is related to using an `uv` managed python version to build this. I think with this setup of meson.build you would still need to at least have `python3.9-dev` installed on the system so that meson can find the right development headers via pkgconfig/sysconfig.

For example, the pkgconfig bundled in the uv standalone python downloads will not contain the paths meson expects. So even if you set `export PKG_CONFIG_PATH=$HOME/.local/share/uv/python/cpython-3.9.20-linux-x86_64-gnu/lib/pkgconfig`  it would not work unless you patch it.

You can also instead leverage @bluss [sysconfigpatcher](https://github.com/bluss/sysconfigpatcher) to adjust the uv python downloads to make this simpler.

TL;DR

```bash
uv python install 3.9.20
uv venv -p 3.9
uv tool install 'git+https://github.com/bluss/sysconfigpatcher'
sysconfigpatcher $HOME/.local/share/uv/python/cpython-3.9.20-linux-x86_64-gnu
uv pip install dbus-python
```

---
