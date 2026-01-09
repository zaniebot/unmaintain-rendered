---
number: 9559
title: uv rebuilds cached packages on Termux
type: issue
state: open
author: milisp
labels:
  - cache
assignees: []
created_at: 2024-12-01T23:43:34Z
updated_at: 2025-12-16T06:15:57Z
url: https://github.com/astral-sh/uv/issues/9559
synced_at: 2026-01-07T13:12:18-06:00
---

# uv rebuilds cached packages on Termux

---

_Issue opened by @milisp on 2024-12-01 23:43_

```sh
uv add pyzmq==25.1.1
uv add jupyter # this will build pyzmq again 
```

---

_Comment by @charliermarsh on 2024-12-01 23:44_

I don't believe that builds `pyzmq` again. We cache the build.

---

_Comment by @milisp on 2024-12-01 23:58_

Maybe it's just an issue in android phone ![image](https://github.com/user-attachments/assets/8b427752-d895-4f1c-be35-3d0258211718)

System info: Termux proot-distro debian

---

_Comment by @milisp on 2024-12-02 00:04_

If The first time build success. It will not again. But if it fails it will build again 

---

_Comment by @milisp on 2024-12-02 00:33_

This can cache results in the current project. If you start a new project. It will be built again. I think it should be cached cross-project. 

---

_Comment by @samypr100 on 2024-12-02 00:35_

> If The first time build success. It will not again. But if it fails it will build again

That sounds correct.

> This can cache results in the current project. If you start a new project. It will be built again. I think it should be cached cross-project.

As this is on Android via Termux, what's the output of `uv cache dir`?


---

_Comment by @milisp on 2024-12-02 00:40_

### System info: Termux proot-distro debian
> uv cache dir 

```sh
/root/.cache/uv
```

---

_Comment by @samypr100 on 2024-12-02 00:54_

If the cache dir is the same for both projects `uv` should use the previous build (assuming dependency constraints haven't change). I've not used proot-distro with termux, so I'm not sure if this affects behavior. I did some tests via stock termux and could not reproduce, is it only when building extensions?

Can you share step by step commands and verbose logs (via --verbose) along the way?

---

_Label `needs-mre` added by @samypr100 on 2024-12-02 00:55_

---

_Comment by @milisp on 2024-12-02 02:06_

### I try in macos it can cache cross project, but not in android

maybe i should not use it in android too much

## android nushell

### prepare

```sh
pkg install python libzmq uv proot-distro
proot-distro install debian
proot-distro login debian
nu ## nushell
## cd ~/projects
```

### project one

```sh
anonymous in ~/projects
❯ uv init one --verbose
DEBUG uv 0.5.5
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found `cpython-3.12.7-linux-aarch64-none` at `/data/data/com.termux/files/usr/bin/python` (search path)
DEBUG Writing Python versions to `/root/projects/one/.python-version`
Initialized project `one` at `/root/projects/one`

anonymous in ~/projects
❯ cd one/

anonymous in one on  master [?] is 📦 v0.1.0 via 🐍 v3.12.7
❯ uv add pyzmq==25.1.1 --verbose
DEBUG uv 0.5.5
DEBUG Found project root: `/root/projects/one`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/root/projects/one/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found `cpython-3.12.7-linux-aarch64-none` at `/data/data/com.termux/files/usr/bin/python3.12` (search path)
Using CPython 3.12.7 interpreter at: /data/data/com.termux/files/usr/bin/python3.12
Creating virtual environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: one @ file:///root/projects/one
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: one*
DEBUG Searching for a compatible version of one @ file:///root/projects/one (*)
DEBUG Adding transitive dependency for one==0.1.0: pyzmq>=25.1.1, <25.1.1+
DEBUG Found fresh response for: https://pypi.org/simple/pyzmq/
DEBUG Searching for a compatible version of pyzmq (>=25.1.1, <25.1.1+)
DEBUG Selecting: pyzmq==25.1.1 [compatible] (pyzmq-25.1.1-cp312-cp312-macosx_10_15_universal2.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d6/2a/812e25ea9fe6eeb18aed211e1dcd2fb6836fd79a848f4ad8cee8e11f839c/pyzmq-25.1.1-cp312-cp312-macosx_10_15_universal2.whl.metadata
DEBUG Adding transitive dependency for pyzmq==25.1.1: cffi{implementation_name == 'pypy'}*
DEBUG Found fresh response for: https://pypi.org/simple/cffi/
DEBUG Searching for a compatible version of cffi{implementation_name == 'pypy'} (*)
DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for cffi==1.17.1: cffi==1.17.1
DEBUG Adding transitive dependency for cffi==1.17.1: cffi{implementation_name == 'pypy'}==1.17.1
DEBUG Searching for a compatible version of cffi{implementation_name == 'pypy'} (==1.17.1)
DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5a/84/e94227139ee5fb4d600a7a4927f322e1d4aea6fdc50bd3fca8493caba23f/cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl.metadata
DEBUG Adding transitive dependency for cffi==1.17.1: pycparser*
DEBUG Searching for a compatible version of cffi (==1.17.1)
DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for cffi==1.17.1: pycparser*
DEBUG Found fresh response for: https://pypi.org/simple/pycparser/
DEBUG Searching for a compatible version of pycparser (*)
DEBUG Selecting: pycparser==2.22 [compatible] (pycparser-2.22-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl.metadata
DEBUG Tried 4 versions: cffi 1, one 1, pycparser 1, pyzmq 1
DEBUG all marker environments resolution took 0.068s
Resolved 4 packages in 84ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: pyzmq==25.1.1
DEBUG Acquired lock for `/root/.cache/uv/sdists-v6/pypi/pyzmq/25.1.1`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3f/7c/69d31a75a3fe9bbab349de7935badac61396f22baf4ab53179a8d940d58e/pyzmq-25.1.1.tar.gz
DEBUG Building: pyzmq==25.1.1
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: setuptools*
DEBUG Adding direct dependency: setuptools-scm*
DEBUG Adding direct dependency: setuptools-scm[toml]*
DEBUG Adding direct dependency: wheel*
DEBUG Adding direct dependency: packaging*
DEBUG Adding direct dependency: cython{implementation_name == 'cpython'}>=0.29
DEBUG Adding direct dependency: cython{python_full_version >= '3.12' and implementation_name == 'cpython'}>=0.29.35
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==75.6.0 [compatible] (setuptools-75.6.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Found fresh response for: https://pypi.org/simple/setuptools-scm/
DEBUG Found fresh response for: https://pypi.org/simple/wheel/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/55/21/47d163f615df1d30c094f6c8bbb353619274edccf0327b185cc2493c2c33/setuptools-75.6.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a0/b9/1906bfeb30f2fc13bb39bf7ddb8749784c05faadbd18a21cf141ba37bff2/setuptools_scm-8.1.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of setuptools-scm (*)
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0b/2c/87f3254fd8ffd29e4c02732eee68a83a1d3c346ae39bc6822dcbcb697f2b/wheel-0.45.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools-scm[toml] (*)
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/cython/
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm==8.1.0
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm[toml]==8.1.0
DEBUG Searching for a compatible version of setuptools-scm[toml] (==8.1.0)
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Searching for a compatible version of wheel (*)
DEBUG Selecting: wheel==0.45.1 [compatible] (wheel-0.45.1-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20)
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Searching for a compatible version of cython{python_full_version >= '3.12' and implementation_name == 'cpython'} (>=0.29.35)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for cython==3.0.11: cython==3.0.11
DEBUG Adding transitive dependency for cython==3.0.11: cython{python_full_version >= '3.12' and implementation_name == 'cpython'}==3.0.11
DEBUG Searching for a compatible version of cython{python_full_version >= '3.12' and implementation_name == 'cpython'} (==3.0.11)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/43/39/bdbec9142bc46605b54d674bf158a78b191c2b75be527c6dcf3e6dfe90b8/Cython-3.0.11-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of cython (==3.0.11)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of cython{implementation_name == 'cpython'} (>=0.29)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for cython==3.0.11: cython==3.0.11
DEBUG Adding transitive dependency for cython==3.0.11: cython{implementation_name == 'cpython'}==3.0.11
DEBUG Searching for a compatible version of cython{implementation_name == 'cpython'} (==3.0.11)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Tried 5 versions: cython 1, packaging 1, setuptools 1, setuptools-scm 1, wheel 1
DEBUG marker environment resolution took 0.056s
DEBUG Installing in packaging==24.2, wheel==0.45.1, setuptools-scm==8.1.0, setuptools==75.6.0, cython==3.0.11 in /root/.cache/uv/builds-v0/.tmpif8MUm
DEBUG Requirement already cached: packaging==24.2
DEBUG Requirement already cached: wheel==0.45.1
DEBUG Requirement already cached: setuptools-scm==8.1.0
DEBUG Requirement already cached: setuptools==75.6.0
DEBUG Requirement already cached: cython==3.0.11
DEBUG Installing build requirements: packaging==24.2, wheel==0.45.1, setuptools-scm==8.1.0, setuptools==75.6.0, cython==3.0.11
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta.get_requires_for_build_wheel()`
DEBUG /root/.cache/uv/builds-v0/.tmpif8MUm/lib/python3.12/site-packages/setuptools/_distutils/dist.py:261: UserWarning: Unknown distribution option: 'cffi_modules'
DEBUG   warnings.warn(msg)
DEBUG running egg_info
DEBUG writing pyzmq.egg-info/PKG-INFO
DEBUG writing dependency_links to pyzmq.egg-info/dependency_links.txt
DEBUG writing requirements to pyzmq.egg-info/requires.txt
DEBUG writing top-level names to pyzmq.egg-info/top_level.txt
DEBUG running configure
DEBUG Settings obtained from pkg-config: {'library_dirs': ['/data/data/com.termux/files/usr/lib'], 'include_dirs': ['/data/data/com.termux/files/usr/include'], 'libraries': ['zmq']}
DEBUG {'libraries': ['zmq'], 'include_dirs': ['/data/data/com.termux/files/usr/include'], 'library_dirs': ['/data/data/com.termux/files/usr/lib'], 'runtime_library_dirs': ['/data/data/com.termux/files/usr/lib'], 'extra_link_args': []}
DEBUG Configure: Autodetecting ZMQ settings...
DEBUG     Custom ZMQ dir:
DEBUG Checking for timer_create
DEBUG ** Errors about missing timer_create are a normal part of this process **
DEBUG aarch64-linux-android-clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fstack-protector-strong -O3 -fstack-protector-strong -O3 -fPIC -I/root/.cache/uv/builds-v0/.tmpif8MUm/include -I/data/data/com.termux/files/usr/include/python3.12 -c /tmp/timer_createq7cbtd82.c -o tmp/timer_createq7cbtd82.o
DEBUG aarch64-linux-android-clang tmp/timer_createq7cbtd82.o -L/data/data/com.termux/files/usr/lib -o a.out
DEBUG aarch64-linux-android-clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fstack-protector-strong -O3 -fstack-protector-strong -O3 -fPIC -I/data/data/com.termux/files/usr/include -Izmq/utils -c build/temp.linux-aarch64-cpython-312/scratch/vers.c -o build/temp.linux-aarch64-cpython-312/scratch/vers.o
DEBUG aarch64-linux-android-clang build/temp.linux-aarch64-cpython-312/scratch/vers.o -L/data/data/com.termux/files/usr/lib -Wl,--enable-new-dtags,-rpath,/data/data/com.termux/files/usr/lib -lzmq -o build/temp.linux-aarch64-cpython-312/scratch/vers
DEBUG     ZMQ version detected: 4.3.5
DEBUG ERROR setuptools_scm._file_finders.git listing git files failed - pretending there aren't any
DEBUG reading manifest file 'pyzmq.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG warning: no previously-included files found matching 'bundled/zeromq/src/Makefile*'
DEBUG warning: no previously-included files found matching 'bundled/zeromq/src/platform.hpp'
DEBUG adding license file 'LICENSE.BSD'
DEBUG adding license file 'LICENSE.LESSER'
DEBUG adding license file 'AUTHORS.md'
DEBUG writing manifest file 'pyzmq.egg-info/SOURCES.txt'
DEBUG ************************************************
DEBUG ************************************************
DEBUG Calling `setuptools.build_meta.build_wheel("/root/.cache/uv/builds-v0/.tmpnBLoNi", {}, None)`
DEBUG /root/.cache/uv/builds-v0/.tmpif8MUm/lib/python3.12/site-packages/setuptools/_distutils/dist.py:261: UserWarning: Unknown distribution option: 'cffi_modules'
DEBUG   warnings.warn(msg)
DEBUG running bdist_wheel
DEBUG running build
DEBUG running build_py
DEBUG copying zmq/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/_future.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/_typing.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/asyncio.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/constants.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/decorators.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/error.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/auth/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/auth
DEBUG copying zmq/auth/asyncio.py -> build/lib.linux-aarch64-cpython-312/zmq/auth
DEBUG copying zmq/auth/base.py -> build/lib.linux-aarch64-cpython-312/zmq/auth
DEBUG copying zmq/auth/certs.py -> build/lib.linux-aarch64-cpython-312/zmq/auth
DEBUG copying zmq/auth/ioloop.py -> build/lib.linux-aarch64-cpython-312/zmq/auth
DEBUG copying zmq/auth/thread.py -> build/lib.linux-aarch64-cpython-312/zmq/auth
DEBUG copying zmq/backend/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/backend
DEBUG copying zmq/backend/select.py -> build/lib.linux-aarch64-cpython-312/zmq/backend
DEBUG copying zmq/backend/cffi/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/_poll.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/context.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/devices.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/error.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/message.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/socket.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/utils.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cython/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/devices/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/devices/basedevice.py -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/devices/monitoredqueue.py -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/devices/monitoredqueuedevice.py -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/devices/proxydevice.py -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/devices/proxysteerabledevice.py -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/eventloop/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/eventloop
DEBUG copying zmq/eventloop/_deprecated.py -> build/lib.linux-aarch64-cpython-312/zmq/eventloop
DEBUG copying zmq/eventloop/future.py -> build/lib.linux-aarch64-cpython-312/zmq/eventloop
DEBUG copying zmq/eventloop/ioloop.py -> build/lib.linux-aarch64-cpython-312/zmq/eventloop
DEBUG copying zmq/eventloop/zmqstream.py -> build/lib.linux-aarch64-cpython-312/zmq/eventloop
DEBUG copying zmq/green/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/green
DEBUG copying zmq/green/core.py -> build/lib.linux-aarch64-cpython-312/zmq/green
DEBUG copying zmq/green/device.py -> build/lib.linux-aarch64-cpython-312/zmq/green
DEBUG copying zmq/green/poll.py -> build/lib.linux-aarch64-cpython-312/zmq/green
DEBUG copying zmq/green/eventloop/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/green/eventloop
DEBUG copying zmq/green/eventloop/ioloop.py -> build/lib.linux-aarch64-cpython-312/zmq/green/eventloop
DEBUG copying zmq/green/eventloop/zmqstream.py -> build/lib.linux-aarch64-cpython-312/zmq/green/eventloop
DEBUG copying zmq/log/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/log
DEBUG copying zmq/log/__main__.py -> build/lib.linux-aarch64-cpython-312/zmq/log
DEBUG copying zmq/log/handlers.py -> build/lib.linux-aarch64-cpython-312/zmq/log
DEBUG copying zmq/ssh/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/ssh
DEBUG copying zmq/ssh/forward.py -> build/lib.linux-aarch64-cpython-312/zmq/ssh
DEBUG copying zmq/ssh/tunnel.py -> build/lib.linux-aarch64-cpython-312/zmq/ssh
DEBUG copying zmq/sugar/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/attrsettr.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/context.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/frame.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/poll.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/socket.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/stopwatch.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/tracker.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/version.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/tests/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/conftest.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_asyncio.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_auth.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_cffi_backend.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_constants.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_context.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_cython.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_decorators.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_device.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_draft.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_error.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_etc.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_ext.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_future.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_imports.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_includes.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_ioloop.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_log.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_message.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_monitor.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_monqueue.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_multipart.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_mypy.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_pair.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_poll.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_proxy_steerable.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_pubsub.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_reqrep.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_retry_eintr.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_security.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_socket.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_ssh.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_version.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_win32_shim.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_z85.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_zmqstream.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/utils/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/garbage.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/interop.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/jsonapi.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/monitor.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/strtypes.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/win32.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/z85.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/__init__.pyi -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/py.typed -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/__init__.pxd -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/backend/__init__.pyi -> build/lib.linux-aarch64-cpython-312/zmq/backend
DEBUG copying zmq/backend/cffi/_cdefs.h -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cython/__init__.pxd -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/backend/cython/checkrc.pxd -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/backend/cython/context.pxd -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/backend/cython/libzmq.pxd -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/backend/cython/message.pxd -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/backend/cython/socket.pxd -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/backend/cython/constant_enums.pxi -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/devices/monitoredqueue.pxd -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/sugar/__init__.pyi -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/tests/cython_ext.pyx -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/utils/buffers.pxd -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/getpid_compat.h -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/ipcmaxlen.h -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/mutex.h -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/pyversion_compat.h -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/zmq_compat.h -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG running build_ext
DEBUG running configure
DEBUG Settings obtained from pkg-config: {'library_dirs': ['/data/data/com.termux/files/usr/lib'], 'include_dirs': ['/data/data/com.termux/files/usr/include'], 'libraries': ['zmq']}
DEBUG {'libraries': ['zmq'], 'include_dirs': ['/data/data/com.termux/files/usr/include'], 'library_dirs': ['/data/data/com.termux/files/usr/lib'], 'runtime_library_dirs': ['/data/data/com.termux/files/usr/lib'], 'extra_link_args': []}
DEBUG Configure: Autodetecting ZMQ settings...
DEBUG     Custom ZMQ dir:
DEBUG Checking for timer_create
DEBUG ** Errors about missing timer_create are a normal part of this process **
DEBUG aarch64-linux-android-clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fstack-protector-strong -O3 -fstack-protector-strong -O3 -fPIC -I/root/.cache/uv/builds-v0/.tmpif8MUm/include -I/data/data/com.termux/files/usr/include/python3.12 -c /tmp/timer_createmru0hso_.c -o tmp/timer_createmru0hso_.o
DEBUG aarch64-linux-android-clang tmp/timer_createmru0hso_.o -L/data/data/com.termux/files/usr/lib -o a.out
DEBUG aarch64-linux-android-clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fstack-protector-strong -O3 -fstack-protector-strong -O3 -fPIC -I/data/data/com.termux/files/usr/include -Izmq/utils -c build/temp.linux-aarch64-cpython-312/scratch/vers.c -o build/temp.linux-aarch64-cpython-312/scratch/vers.o
DEBUG aarch64-linux-android-clang build/temp.linux-aarch64-cpython-312/scratch/vers.o -L/data/data/com.termux/files/usr/lib -Wl,--enable-new-dtags,-rpath,/data/data/com.termux/files/usr/lib -lzmq -o build/temp.linux-aarch64-cpython-312/scratch/vers
DEBUG     ZMQ version detected: 4.3.5
DEBUG ************************************************
DEBUG ************************************************
DEBUG installing to build/bdist.linux-aarch64/wheel
DEBUG running install
DEBUG running install_lib
DEBUG creating build/bdist.linux-aarch64/wheel
DEBUG creating build/bdist.linux-aarch64/wheel/zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/_future.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/_typing.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/asyncio.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/constants.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/decorators.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/error.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/auth
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/auth/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/auth
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/auth/asyncio.py -> build/bdist.linux-aarch64/wheel/./zmq/auth
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/auth/base.py -> build/bdist.linux-aarch64/wheel/./zmq/auth
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/auth/certs.py -> build/bdist.linux-aarch64/wheel/./zmq/auth
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/auth/ioloop.py -> build/bdist.linux-aarch64/wheel/./zmq/auth
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/auth/thread.py -> build/bdist.linux-aarch64/wheel/./zmq/auth
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/backend
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/backend
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/select.py -> build/bdist.linux-aarch64/wheel/./zmq/backend
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/_poll.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/context.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/devices.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/error.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/message.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/socket.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/utils.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/_cdefs.h -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/__init__.pxd -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/checkrc.pxd -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/context.pxd -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/libzmq.pxd -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/message.pxd -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/socket.pxd -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/constant_enums.pxi -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/_proxy_steerable.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/_device.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/_poll.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/_version.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/context.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/error.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/message.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/socket.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/utils.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/__init__.pyi -> build/bdist.linux-aarch64/wheel/./zmq/backend
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/basedevice.py -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/monitoredqueue.py -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/monitoredqueuedevice.py -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/proxydevice.py -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/proxysteerabledevice.py -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/monitoredqueue.pxd -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/monitoredqueue.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/eventloop/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/eventloop/_deprecated.py -> build/bdist.linux-aarch64/wheel/./zmq/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/eventloop/future.py -> build/bdist.linux-aarch64/wheel/./zmq/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/eventloop/ioloop.py -> build/bdist.linux-aarch64/wheel/./zmq/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/eventloop/zmqstream.py -> build/bdist.linux-aarch64/wheel/./zmq/eventloop
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/green
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/green
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/core.py -> build/bdist.linux-aarch64/wheel/./zmq/green
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/device.py -> build/bdist.linux-aarch64/wheel/./zmq/green
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/poll.py -> build/bdist.linux-aarch64/wheel/./zmq/green
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/green/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/eventloop/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/green/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/eventloop/ioloop.py -> build/bdist.linux-aarch64/wheel/./zmq/green/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/eventloop/zmqstream.py -> build/bdist.linux-aarch64/wheel/./zmq/green/eventloop
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/log
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/log/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/log
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/log/__main__.py -> build/bdist.linux-aarch64/wheel/./zmq/log
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/log/handlers.py -> build/bdist.linux-aarch64/wheel/./zmq/log
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/ssh
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/ssh/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/ssh
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/ssh/forward.py -> build/bdist.linux-aarch64/wheel/./zmq/ssh
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/ssh/tunnel.py -> build/bdist.linux-aarch64/wheel/./zmq/ssh
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/attrsettr.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/context.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/frame.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/poll.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/socket.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/stopwatch.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/tracker.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/version.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/__init__.pyi -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/conftest.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_asyncio.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_auth.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_cffi_backend.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_constants.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_context.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_cython.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_decorators.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_device.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_draft.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_error.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_etc.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_ext.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_future.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_imports.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_includes.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_ioloop.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_log.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_message.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_monitor.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_monqueue.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_multipart.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_mypy.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_pair.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_poll.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_proxy_steerable.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_pubsub.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_reqrep.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_retry_eintr.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_security.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_socket.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_ssh.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_version.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_win32_shim.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_z85.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_zmqstream.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/cython_ext.pyx -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/garbage.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/interop.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/jsonapi.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/monitor.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/strtypes.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/win32.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/z85.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/buffers.pxd -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/getpid_compat.h -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/ipcmaxlen.h -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/mutex.h -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/pyversion_compat.h -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/zmq_compat.h -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/compiler.json -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/config.json -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/__init__.pyi -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/py.typed -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/__init__.pxd -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG running install_egg_info
DEBUG running egg_info
DEBUG writing pyzmq.egg-info/PKG-INFO
DEBUG writing dependency_links to pyzmq.egg-info/dependency_links.txt
DEBUG writing requirements to pyzmq.egg-info/requires.txt
DEBUG writing top-level names to pyzmq.egg-info/top_level.txt
DEBUG ERROR setuptools_scm._file_finders.git listing git files failed - pretending there aren't any
DEBUG reading manifest file 'pyzmq.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG warning: no previously-included files found matching 'bundled/zeromq/src/Makefile*'
DEBUG warning: no previously-included files found matching 'bundled/zeromq/src/platform.hpp'
DEBUG adding license file 'LICENSE.BSD'
DEBUG adding license file 'LICENSE.LESSER'
DEBUG adding license file 'AUTHORS.md'
DEBUG writing manifest file 'pyzmq.egg-info/SOURCES.txt'
DEBUG Copying pyzmq.egg-info to build/bdist.linux-aarch64/wheel/./pyzmq-25.1.1-py3.12.egg-info
DEBUG running install_scripts
DEBUG creating build/bdist.linux-aarch64/wheel/pyzmq-25.1.1.dist-info/WHEEL
DEBUG creating '/root/.cache/uv/builds-v0/.tmpnBLoNi/.tmp-06mjfziv/pyzmq-25.1.1-cp312-cp312-linux_aarch64.whl' and adding 'build/bdist.linux-aarch64/wheel' to it
DEBUG adding 'zmq/__init__.pxd'
DEBUG adding 'zmq/__init__.py'
DEBUG adding 'zmq/__init__.pyi'
DEBUG adding 'zmq/_future.py'
DEBUG adding 'zmq/_typing.py'
DEBUG adding 'zmq/asyncio.py'
DEBUG adding 'zmq/constants.py'
DEBUG adding 'zmq/decorators.py'
DEBUG adding 'zmq/error.py'
DEBUG adding 'zmq/py.typed'
DEBUG adding 'zmq/auth/__init__.py'
DEBUG adding 'zmq/auth/asyncio.py'
DEBUG adding 'zmq/auth/base.py'
DEBUG adding 'zmq/auth/certs.py'
DEBUG adding 'zmq/auth/ioloop.py'
DEBUG adding 'zmq/auth/thread.py'
DEBUG adding 'zmq/backend/__init__.py'
DEBUG adding 'zmq/backend/__init__.pyi'
DEBUG adding 'zmq/backend/select.py'
DEBUG adding 'zmq/backend/cffi/__init__.py'
DEBUG adding 'zmq/backend/cffi/_cdefs.h'
DEBUG adding 'zmq/backend/cffi/_poll.py'
DEBUG adding 'zmq/backend/cffi/context.py'
DEBUG adding 'zmq/backend/cffi/devices.py'
DEBUG adding 'zmq/backend/cffi/error.py'
DEBUG adding 'zmq/backend/cffi/message.py'
DEBUG adding 'zmq/backend/cffi/socket.py'
DEBUG adding 'zmq/backend/cffi/utils.py'
DEBUG adding 'zmq/backend/cython/__init__.pxd'
DEBUG adding 'zmq/backend/cython/__init__.py'
DEBUG adding 'zmq/backend/cython/_device.cpython-312.so'
DEBUG adding 'zmq/backend/cython/_poll.cpython-312.so'
DEBUG adding 'zmq/backend/cython/_proxy_steerable.cpython-312.so'
DEBUG adding 'zmq/backend/cython/_version.cpython-312.so'
DEBUG adding 'zmq/backend/cython/checkrc.pxd'
DEBUG adding 'zmq/backend/cython/constant_enums.pxi'
DEBUG adding 'zmq/backend/cython/context.cpython-312.so'
DEBUG adding 'zmq/backend/cython/context.pxd'
DEBUG adding 'zmq/backend/cython/error.cpython-312.so'
DEBUG adding 'zmq/backend/cython/libzmq.pxd'
DEBUG adding 'zmq/backend/cython/message.cpython-312.so'
DEBUG adding 'zmq/backend/cython/message.pxd'
DEBUG adding 'zmq/backend/cython/socket.cpython-312.so'
DEBUG adding 'zmq/backend/cython/socket.pxd'
DEBUG adding 'zmq/backend/cython/utils.cpython-312.so'
DEBUG adding 'zmq/devices/__init__.py'
DEBUG adding 'zmq/devices/basedevice.py'
DEBUG adding 'zmq/devices/monitoredqueue.cpython-312.so'
DEBUG adding 'zmq/devices/monitoredqueue.pxd'
DEBUG adding 'zmq/devices/monitoredqueue.py'
DEBUG adding 'zmq/devices/monitoredqueuedevice.py'
DEBUG adding 'zmq/devices/proxydevice.py'
DEBUG adding 'zmq/devices/proxysteerabledevice.py'
DEBUG adding 'zmq/eventloop/__init__.py'
DEBUG adding 'zmq/eventloop/_deprecated.py'
DEBUG adding 'zmq/eventloop/future.py'
DEBUG adding 'zmq/eventloop/ioloop.py'
DEBUG adding 'zmq/eventloop/zmqstream.py'
DEBUG adding 'zmq/green/__init__.py'
DEBUG adding 'zmq/green/core.py'
DEBUG adding 'zmq/green/device.py'
DEBUG adding 'zmq/green/poll.py'
DEBUG adding 'zmq/green/eventloop/__init__.py'
DEBUG adding 'zmq/green/eventloop/ioloop.py'
DEBUG adding 'zmq/green/eventloop/zmqstream.py'
DEBUG adding 'zmq/log/__init__.py'
DEBUG adding 'zmq/log/__main__.py'
DEBUG adding 'zmq/log/handlers.py'
DEBUG adding 'zmq/ssh/__init__.py'
DEBUG adding 'zmq/ssh/forward.py'
DEBUG adding 'zmq/ssh/tunnel.py'
DEBUG adding 'zmq/sugar/__init__.py'
DEBUG adding 'zmq/sugar/__init__.pyi'
DEBUG adding 'zmq/sugar/attrsettr.py'
DEBUG adding 'zmq/sugar/context.py'
DEBUG adding 'zmq/sugar/frame.py'
DEBUG adding 'zmq/sugar/poll.py'
DEBUG adding 'zmq/sugar/socket.py'
DEBUG adding 'zmq/sugar/stopwatch.py'
DEBUG adding 'zmq/sugar/tracker.py'
DEBUG adding 'zmq/sugar/version.py'
DEBUG adding 'zmq/tests/__init__.py'
DEBUG adding 'zmq/tests/conftest.py'
DEBUG adding 'zmq/tests/cython_ext.pyx'
DEBUG adding 'zmq/tests/test_asyncio.py'
DEBUG adding 'zmq/tests/test_auth.py'
DEBUG adding 'zmq/tests/test_cffi_backend.py'
DEBUG adding 'zmq/tests/test_constants.py'
DEBUG adding 'zmq/tests/test_context.py'
DEBUG adding 'zmq/tests/test_cython.py'
DEBUG adding 'zmq/tests/test_decorators.py'
DEBUG adding 'zmq/tests/test_device.py'
DEBUG adding 'zmq/tests/test_draft.py'
DEBUG adding 'zmq/tests/test_error.py'
DEBUG adding 'zmq/tests/test_etc.py'
DEBUG adding 'zmq/tests/test_ext.py'
DEBUG adding 'zmq/tests/test_future.py'
DEBUG adding 'zmq/tests/test_imports.py'
DEBUG adding 'zmq/tests/test_includes.py'
DEBUG adding 'zmq/tests/test_ioloop.py'
DEBUG adding 'zmq/tests/test_log.py'
DEBUG adding 'zmq/tests/test_message.py'
DEBUG adding 'zmq/tests/test_monitor.py'
DEBUG adding 'zmq/tests/test_monqueue.py'
DEBUG adding 'zmq/tests/test_multipart.py'
DEBUG adding 'zmq/tests/test_mypy.py'
DEBUG adding 'zmq/tests/test_pair.py'
DEBUG adding 'zmq/tests/test_poll.py'
DEBUG adding 'zmq/tests/test_proxy_steerable.py'
DEBUG adding 'zmq/tests/test_pubsub.py'
DEBUG adding 'zmq/tests/test_reqrep.py'
DEBUG adding 'zmq/tests/test_retry_eintr.py'
DEBUG adding 'zmq/tests/test_security.py'
DEBUG adding 'zmq/tests/test_socket.py'
DEBUG adding 'zmq/tests/test_ssh.py'
DEBUG adding 'zmq/tests/test_version.py'
DEBUG adding 'zmq/tests/test_win32_shim.py'
DEBUG adding 'zmq/tests/test_z85.py'
DEBUG adding 'zmq/tests/test_zmqstream.py'
DEBUG adding 'zmq/utils/__init__.py'
DEBUG adding 'zmq/utils/buffers.pxd'
DEBUG adding 'zmq/utils/compiler.json'
DEBUG adding 'zmq/utils/config.json'
DEBUG adding 'zmq/utils/garbage.py'
DEBUG adding 'zmq/utils/getpid_compat.h'
DEBUG adding 'zmq/utils/interop.py'
DEBUG adding 'zmq/utils/ipcmaxlen.h'
DEBUG adding 'zmq/utils/jsonapi.py'
DEBUG adding 'zmq/utils/monitor.py'
DEBUG adding 'zmq/utils/mutex.h'
DEBUG adding 'zmq/utils/pyversion_compat.h'
DEBUG adding 'zmq/utils/strtypes.py'
DEBUG adding 'zmq/utils/win32.py'
DEBUG adding 'zmq/utils/z85.py'
DEBUG adding 'zmq/utils/zmq_compat.h'
DEBUG adding 'pyzmq-25.1.1.dist-info/AUTHORS.md'
DEBUG adding 'pyzmq-25.1.1.dist-info/LICENSE.BSD'
DEBUG adding 'pyzmq-25.1.1.dist-info/LICENSE.LESSER'
DEBUG adding 'pyzmq-25.1.1.dist-info/METADATA'
DEBUG adding 'pyzmq-25.1.1.dist-info/WHEEL'
DEBUG adding 'pyzmq-25.1.1.dist-info/top_level.txt'
DEBUG adding 'pyzmq-25.1.1.dist-info/RECORD'
DEBUG removing build/bdist.linux-aarch64/wheel
DEBUG Finished building: pyzmq==25.1.1
DEBUG Released lock at `/root/.cache/uv/sdists-v6/pypi/pyzmq/25.1.1/.lock`
Prepared 1 package in 1m 31s
Installed 1 package in 362ms
 + pyzmq==25.1.1
```

### project two

```sh
cd ~/projects
uv init two
cd two
```

```sh
anonymous in two on  master [?] is 📦 v0.1.0 via 🐍 v3.12.7
❯ uv add pyzmq==25.1.1 --verbose
DEBUG uv 0.5.5
DEBUG Found project root: `/root/projects/two`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/root/projects/two/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found `cpython-3.12.7-linux-aarch64-none` at `/data/data/com.termux/files/usr/bin/python3.12` (search path)
Using CPython 3.12.7 interpreter at: /data/data/com.termux/files/usr/bin/python3.12
Creating virtual environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: two @ file:///root/projects/two
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: two*
DEBUG Searching for a compatible version of two @ file:///root/projects/two (*)
DEBUG Adding transitive dependency for two==0.1.0: pyzmq>=25.1.1, <25.1.1+
DEBUG Found stale response for: https://pypi.org/simple/pyzmq/
DEBUG Sending revalidation request for: https://pypi.org/simple/pyzmq/
DEBUG Found not-modified response for: https://pypi.org/simple/pyzmq/
DEBUG Searching for a compatible version of pyzmq (>=25.1.1, <25.1.1+)
DEBUG Selecting: pyzmq==25.1.1 [compatible] (pyzmq-25.1.1-cp312-cp312-macosx_10_15_universal2.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d6/2a/812e25ea9fe6eeb18aed211e1dcd2fb6836fd79a848f4ad8cee8e11f839c/pyzmq-25.1.1-cp312-cp312-macosx_10_15_universal2.whl.metadata
DEBUG Adding transitive dependency for pyzmq==25.1.1: cffi{implementation_name == 'pypy'}*
DEBUG Found stale response for: https://pypi.org/simple/cffi/
DEBUG Sending revalidation request for: https://pypi.org/simple/cffi/
DEBUG Found not-modified response for: https://pypi.org/simple/cffi/
DEBUG Searching for a compatible version of cffi{implementation_name == 'pypy'} (*)
DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for cffi==1.17.1: cffi==1.17.1
DEBUG Adding transitive dependency for cffi==1.17.1: cffi{implementation_name == 'pypy'}==1.17.1
DEBUG Searching for a compatible version of cffi{implementation_name == 'pypy'} (==1.17.1)
DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5a/84/e94227139ee5fb4d600a7a4927f322e1d4aea6fdc50bd3fca8493caba23f/cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl.metadata
DEBUG Adding transitive dependency for cffi==1.17.1: pycparser*
DEBUG Searching for a compatible version of cffi (==1.17.1)
DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for cffi==1.17.1: pycparser*
DEBUG Found stale response for: https://pypi.org/simple/pycparser/
DEBUG Sending revalidation request for: https://pypi.org/simple/pycparser/
DEBUG Found not-modified response for: https://pypi.org/simple/pycparser/
DEBUG Searching for a compatible version of pycparser (*)
DEBUG Selecting: pycparser==2.22 [compatible] (pycparser-2.22-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl.metadata
DEBUG Tried 4 versions: cffi 1, pycparser 1, pyzmq 1, two 1
DEBUG all marker environments resolution took 0.998s
Resolved 4 packages in 1.02s
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: pyzmq==25.1.1
DEBUG Acquired lock for `/root/.cache/uv/sdists-v6/pypi/pyzmq/25.1.1`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3f/7c/69d31a75a3fe9bbab349de7935badac61396f22baf4ab53179a8d940d58e/pyzmq-25.1.1.tar.gz
DEBUG Building: pyzmq==25.1.1
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: setuptools*
DEBUG Adding direct dependency: setuptools-scm*
DEBUG Adding direct dependency: setuptools-scm[toml]*
DEBUG Adding direct dependency: wheel*
DEBUG Adding direct dependency: packaging*
DEBUG Adding direct dependency: cython{implementation_name == 'cpython'}>=0.29
DEBUG Adding direct dependency: cython{python_full_version >= '3.12' and implementation_name == 'cpython'}>=0.29.35
DEBUG Found stale response for: https://pypi.org/simple/setuptools-scm/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools-scm/
DEBUG Found stale response for: https://pypi.org/simple/wheel/
DEBUG Sending revalidation request for: https://pypi.org/simple/wheel/
DEBUG Found stale response for: https://pypi.org/simple/packaging/
DEBUG Sending revalidation request for: https://pypi.org/simple/packaging/
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG Found stale response for: https://pypi.org/simple/cython/
DEBUG Sending revalidation request for: https://pypi.org/simple/cython/
DEBUG Found not-modified response for: https://pypi.org/simple/cython/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/wheel/
DEBUG Found not-modified response for: https://pypi.org/simple/packaging/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools-scm/
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==75.6.0 [compatible] (setuptools-75.6.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0b/2c/87f3254fd8ffd29e4c02732eee68a83a1d3c346ae39bc6822dcbcb697f2b/wheel-0.45.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/55/21/47d163f615df1d30c094f6c8bbb353619274edccf0327b185cc2493c2c33/setuptools-75.6.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a0/b9/1906bfeb30f2fc13bb39bf7ddb8749784c05faadbd18a21cf141ba37bff2/setuptools_scm-8.1.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of setuptools-scm (*)
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools-scm[toml] (*)
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm==8.1.0
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm[toml]==8.1.0
DEBUG Searching for a compatible version of setuptools-scm[toml] (==8.1.0)
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Searching for a compatible version of wheel (*)
DEBUG Selecting: wheel==0.45.1 [compatible] (wheel-0.45.1-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20)
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Searching for a compatible version of cython{python_full_version >= '3.12' and implementation_name == 'cpython'} (>=0.29.35)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for cython==3.0.11: cython==3.0.11
DEBUG Adding transitive dependency for cython==3.0.11: cython{python_full_version >= '3.12' and implementation_name == 'cpython'}==3.0.11
DEBUG Searching for a compatible version of cython{python_full_version >= '3.12' and implementation_name == 'cpython'} (==3.0.11)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/43/39/bdbec9142bc46605b54d674bf158a78b191c2b75be527c6dcf3e6dfe90b8/Cython-3.0.11-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of cython (==3.0.11)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of cython{implementation_name == 'cpython'} (>=0.29)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for cython==3.0.11: cython==3.0.11
DEBUG Adding transitive dependency for cython==3.0.11: cython{implementation_name == 'cpython'}==3.0.11
DEBUG Searching for a compatible version of cython{implementation_name == 'cpython'} (==3.0.11)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Tried 5 versions: cython 1, packaging 1, setuptools 1, setuptools-scm 1, wheel 1
DEBUG marker environment resolution took 0.611s
DEBUG Installing in packaging==24.2, wheel==0.45.1, setuptools-scm==8.1.0, setuptools==75.6.0, cython==3.0.11 in /root/.cache/uv/builds-v0/.tmpn8Z6Cu
DEBUG Requirement already cached: packaging==24.2
DEBUG Requirement already cached: wheel==0.45.1
DEBUG Requirement already cached: setuptools-scm==8.1.0
DEBUG Requirement already cached: setuptools==75.6.0
DEBUG Requirement already cached: cython==3.0.11
DEBUG Installing build requirements: packaging==24.2, wheel==0.45.1, setuptools-scm==8.1.0, setuptools==75.6.0, cython==3.0.11
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta.get_requires_for_build_wheel()`
DEBUG /root/.cache/uv/builds-v0/.tmpn8Z6Cu/lib/python3.12/site-packages/setuptools/_distutils/dist.py:261: UserWarning: Unknown distribution option: 'cffi_modules'
DEBUG   warnings.warn(msg)
DEBUG running egg_info
DEBUG writing pyzmq.egg-info/PKG-INFO
DEBUG writing dependency_links to pyzmq.egg-info/dependency_links.txt
DEBUG writing requirements to pyzmq.egg-info/requires.txt
DEBUG writing top-level names to pyzmq.egg-info/top_level.txt
DEBUG running configure
DEBUG Settings obtained from pkg-config: {'library_dirs': ['/data/data/com.termux/files/usr/lib'], 'include_dirs': ['/data/data/com.termux/files/usr/include'], 'libraries': ['zmq']}
DEBUG {'libraries': ['zmq'], 'include_dirs': ['/data/data/com.termux/files/usr/include'], 'library_dirs': ['/data/data/com.termux/files/usr/lib'], 'runtime_library_dirs': ['/data/data/com.termux/files/usr/lib'], 'extra_link_args': []}
DEBUG Configure: Autodetecting ZMQ settings...
DEBUG     Custom ZMQ dir:
DEBUG Checking for timer_create
DEBUG ** Errors about missing timer_create are a normal part of this process **
DEBUG aarch64-linux-android-clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fstack-protector-strong -O3 -fstack-protector-strong -O3 -fPIC -I/root/.cache/uv/builds-v0/.tmpn8Z6Cu/include -I/data/data/com.termux/files/usr/include/python3.12 -c /tmp/timer_createmzhh8ugk.c -o tmp/timer_createmzhh8ugk.o
DEBUG aarch64-linux-android-clang tmp/timer_createmzhh8ugk.o -L/data/data/com.termux/files/usr/lib -o a.out
DEBUG aarch64-linux-android-clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fstack-protector-strong -O3 -fstack-protector-strong -O3 -fPIC -I/data/data/com.termux/files/usr/include -Izmq/utils -c build/temp.linux-aarch64-cpython-312/scratch/vers.c -o build/temp.linux-aarch64-cpython-312/scratch/vers.o
DEBUG aarch64-linux-android-clang build/temp.linux-aarch64-cpython-312/scratch/vers.o -L/data/data/com.termux/files/usr/lib -Wl,--enable-new-dtags,-rpath,/data/data/com.termux/files/usr/lib -lzmq -o build/temp.linux-aarch64-cpython-312/scratch/vers
DEBUG     ZMQ version detected: 4.3.5
DEBUG ERROR setuptools_scm._file_finders.git listing git files failed - pretending there aren't any
DEBUG reading manifest file 'pyzmq.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG warning: no previously-included files found matching 'bundled/zeromq/src/Makefile*'
DEBUG warning: no previously-included files found matching 'bundled/zeromq/src/platform.hpp'
DEBUG adding license file 'LICENSE.BSD'
DEBUG adding license file 'LICENSE.LESSER'
DEBUG adding license file 'AUTHORS.md'
DEBUG writing manifest file 'pyzmq.egg-info/SOURCES.txt'
DEBUG ************************************************
DEBUG ************************************************
DEBUG Calling `setuptools.build_meta.build_wheel("/root/.cache/uv/builds-v0/.tmpuxfdJH", {}, None)`
DEBUG /root/.cache/uv/builds-v0/.tmpn8Z6Cu/lib/python3.12/site-packages/setuptools/_distutils/dist.py:261: UserWarning: Unknown distribution option: 'cffi_modules'
DEBUG   warnings.warn(msg)
DEBUG running bdist_wheel
DEBUG running build
DEBUG running build_py
DEBUG copying zmq/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/_future.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/_typing.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/asyncio.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/constants.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/decorators.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/error.py -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/auth/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/auth
DEBUG copying zmq/auth/asyncio.py -> build/lib.linux-aarch64-cpython-312/zmq/auth
DEBUG copying zmq/auth/base.py -> build/lib.linux-aarch64-cpython-312/zmq/auth
DEBUG copying zmq/auth/certs.py -> build/lib.linux-aarch64-cpython-312/zmq/auth
DEBUG copying zmq/auth/ioloop.py -> build/lib.linux-aarch64-cpython-312/zmq/auth
DEBUG copying zmq/auth/thread.py -> build/lib.linux-aarch64-cpython-312/zmq/auth
DEBUG copying zmq/backend/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/backend
DEBUG copying zmq/backend/select.py -> build/lib.linux-aarch64-cpython-312/zmq/backend
DEBUG copying zmq/backend/cffi/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/_poll.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/context.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/devices.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/error.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/message.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/socket.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cffi/utils.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cython/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/devices/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/devices/basedevice.py -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/devices/monitoredqueue.py -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/devices/monitoredqueuedevice.py -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/devices/proxydevice.py -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/devices/proxysteerabledevice.py -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/eventloop/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/eventloop
DEBUG copying zmq/eventloop/_deprecated.py -> build/lib.linux-aarch64-cpython-312/zmq/eventloop
DEBUG copying zmq/eventloop/future.py -> build/lib.linux-aarch64-cpython-312/zmq/eventloop
DEBUG copying zmq/eventloop/ioloop.py -> build/lib.linux-aarch64-cpython-312/zmq/eventloop
DEBUG copying zmq/eventloop/zmqstream.py -> build/lib.linux-aarch64-cpython-312/zmq/eventloop
DEBUG copying zmq/green/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/green
DEBUG copying zmq/green/core.py -> build/lib.linux-aarch64-cpython-312/zmq/green
DEBUG copying zmq/green/device.py -> build/lib.linux-aarch64-cpython-312/zmq/green
DEBUG copying zmq/green/poll.py -> build/lib.linux-aarch64-cpython-312/zmq/green
DEBUG copying zmq/green/eventloop/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/green/eventloop
DEBUG copying zmq/green/eventloop/ioloop.py -> build/lib.linux-aarch64-cpython-312/zmq/green/eventloop
DEBUG copying zmq/green/eventloop/zmqstream.py -> build/lib.linux-aarch64-cpython-312/zmq/green/eventloop
DEBUG copying zmq/log/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/log
DEBUG copying zmq/log/__main__.py -> build/lib.linux-aarch64-cpython-312/zmq/log
DEBUG copying zmq/log/handlers.py -> build/lib.linux-aarch64-cpython-312/zmq/log
DEBUG copying zmq/ssh/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/ssh
DEBUG copying zmq/ssh/forward.py -> build/lib.linux-aarch64-cpython-312/zmq/ssh
DEBUG copying zmq/ssh/tunnel.py -> build/lib.linux-aarch64-cpython-312/zmq/ssh
DEBUG copying zmq/sugar/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/attrsettr.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/context.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/frame.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/poll.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/socket.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/stopwatch.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/tracker.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/sugar/version.py -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/tests/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/conftest.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_asyncio.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_auth.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_cffi_backend.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_constants.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_context.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_cython.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_decorators.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_device.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_draft.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_error.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_etc.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_ext.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_future.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_imports.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_includes.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_ioloop.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_log.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_message.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_monitor.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_monqueue.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_multipart.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_mypy.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_pair.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_poll.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_proxy_steerable.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_pubsub.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_reqrep.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_retry_eintr.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_security.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_socket.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_ssh.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_version.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_win32_shim.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_z85.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/tests/test_zmqstream.py -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/utils/__init__.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/garbage.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/interop.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/jsonapi.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/monitor.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/strtypes.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/win32.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/z85.py -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/__init__.pyi -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/py.typed -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/__init__.pxd -> build/lib.linux-aarch64-cpython-312/zmq
DEBUG copying zmq/backend/__init__.pyi -> build/lib.linux-aarch64-cpython-312/zmq/backend
DEBUG copying zmq/backend/cffi/_cdefs.h -> build/lib.linux-aarch64-cpython-312/zmq/backend/cffi
DEBUG copying zmq/backend/cython/__init__.pxd -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/backend/cython/checkrc.pxd -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/backend/cython/context.pxd -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/backend/cython/libzmq.pxd -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/backend/cython/message.pxd -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/backend/cython/socket.pxd -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/backend/cython/constant_enums.pxi -> build/lib.linux-aarch64-cpython-312/zmq/backend/cython
DEBUG copying zmq/devices/monitoredqueue.pxd -> build/lib.linux-aarch64-cpython-312/zmq/devices
DEBUG copying zmq/sugar/__init__.pyi -> build/lib.linux-aarch64-cpython-312/zmq/sugar
DEBUG copying zmq/tests/cython_ext.pyx -> build/lib.linux-aarch64-cpython-312/zmq/tests
DEBUG copying zmq/utils/buffers.pxd -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/getpid_compat.h -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/ipcmaxlen.h -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/mutex.h -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/pyversion_compat.h -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG copying zmq/utils/zmq_compat.h -> build/lib.linux-aarch64-cpython-312/zmq/utils
DEBUG running build_ext
DEBUG running configure
DEBUG Settings obtained from pkg-config: {'library_dirs': ['/data/data/com.termux/files/usr/lib'], 'include_dirs': ['/data/data/com.termux/files/usr/include'], 'libraries': ['zmq']}
DEBUG {'libraries': ['zmq'], 'include_dirs': ['/data/data/com.termux/files/usr/include'], 'library_dirs': ['/data/data/com.termux/files/usr/lib'], 'runtime_library_dirs': ['/data/data/com.termux/files/usr/lib'], 'extra_link_args': []}
DEBUG Configure: Autodetecting ZMQ settings...
DEBUG     Custom ZMQ dir:
DEBUG Checking for timer_create
DEBUG ** Errors about missing timer_create are a normal part of this process **
DEBUG aarch64-linux-android-clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fstack-protector-strong -O3 -fstack-protector-strong -O3 -fPIC -I/root/.cache/uv/builds-v0/.tmpn8Z6Cu/include -I/data/data/com.termux/files/usr/include/python3.12 -c /tmp/timer_create80b1gf0g.c -o tmp/timer_create80b1gf0g.o
DEBUG aarch64-linux-android-clang tmp/timer_create80b1gf0g.o -L/data/data/com.termux/files/usr/lib -o a.out
DEBUG aarch64-linux-android-clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fstack-protector-strong -O3 -fstack-protector-strong -O3 -fPIC -I/data/data/com.termux/files/usr/include -Izmq/utils -c build/temp.linux-aarch64-cpython-312/scratch/vers.c -o build/temp.linux-aarch64-cpython-312/scratch/vers.o
DEBUG aarch64-linux-android-clang build/temp.linux-aarch64-cpython-312/scratch/vers.o -L/data/data/com.termux/files/usr/lib -Wl,--enable-new-dtags,-rpath,/data/data/com.termux/files/usr/lib -lzmq -o build/temp.linux-aarch64-cpython-312/scratch/vers
DEBUG     ZMQ version detected: 4.3.5
DEBUG ************************************************
DEBUG ************************************************
DEBUG installing to build/bdist.linux-aarch64/wheel
DEBUG running install
DEBUG running install_lib
DEBUG creating build/bdist.linux-aarch64/wheel
DEBUG creating build/bdist.linux-aarch64/wheel/zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/_future.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/_typing.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/asyncio.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/constants.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/decorators.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/error.py -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/auth
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/auth/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/auth
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/auth/asyncio.py -> build/bdist.linux-aarch64/wheel/./zmq/auth
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/auth/base.py -> build/bdist.linux-aarch64/wheel/./zmq/auth
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/auth/certs.py -> build/bdist.linux-aarch64/wheel/./zmq/auth
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/auth/ioloop.py -> build/bdist.linux-aarch64/wheel/./zmq/auth
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/auth/thread.py -> build/bdist.linux-aarch64/wheel/./zmq/auth
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/backend
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/backend
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/select.py -> build/bdist.linux-aarch64/wheel/./zmq/backend
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/_poll.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/context.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/devices.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/error.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/message.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/socket.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/utils.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cffi/_cdefs.h -> build/bdist.linux-aarch64/wheel/./zmq/backend/cffi
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/__init__.pxd -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/checkrc.pxd -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/context.pxd -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/libzmq.pxd -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/message.pxd -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/socket.pxd -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/constant_enums.pxi -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/_proxy_steerable.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/_device.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/_poll.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/_version.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/context.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/error.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/message.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/socket.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/cython/utils.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/backend/cython
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/backend/__init__.pyi -> build/bdist.linux-aarch64/wheel/./zmq/backend
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/basedevice.py -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/monitoredqueue.py -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/monitoredqueuedevice.py -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/proxydevice.py -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/proxysteerabledevice.py -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/monitoredqueue.pxd -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/devices/monitoredqueue.cpython-312.so -> build/bdist.linux-aarch64/wheel/./zmq/devices
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/eventloop/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/eventloop/_deprecated.py -> build/bdist.linux-aarch64/wheel/./zmq/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/eventloop/future.py -> build/bdist.linux-aarch64/wheel/./zmq/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/eventloop/ioloop.py -> build/bdist.linux-aarch64/wheel/./zmq/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/eventloop/zmqstream.py -> build/bdist.linux-aarch64/wheel/./zmq/eventloop
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/green
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/green
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/core.py -> build/bdist.linux-aarch64/wheel/./zmq/green
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/device.py -> build/bdist.linux-aarch64/wheel/./zmq/green
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/poll.py -> build/bdist.linux-aarch64/wheel/./zmq/green
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/green/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/eventloop/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/green/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/eventloop/ioloop.py -> build/bdist.linux-aarch64/wheel/./zmq/green/eventloop
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/green/eventloop/zmqstream.py -> build/bdist.linux-aarch64/wheel/./zmq/green/eventloop
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/log
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/log/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/log
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/log/__main__.py -> build/bdist.linux-aarch64/wheel/./zmq/log
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/log/handlers.py -> build/bdist.linux-aarch64/wheel/./zmq/log
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/ssh
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/ssh/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/ssh
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/ssh/forward.py -> build/bdist.linux-aarch64/wheel/./zmq/ssh
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/ssh/tunnel.py -> build/bdist.linux-aarch64/wheel/./zmq/ssh
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/attrsettr.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/context.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/frame.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/poll.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/socket.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/stopwatch.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/tracker.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/version.py -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/sugar/__init__.pyi -> build/bdist.linux-aarch64/wheel/./zmq/sugar
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/conftest.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_asyncio.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_auth.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_cffi_backend.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_constants.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_context.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_cython.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_decorators.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_device.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_draft.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_error.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_etc.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_ext.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_future.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_imports.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_includes.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_ioloop.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_log.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_message.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_monitor.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_monqueue.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_multipart.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_mypy.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_pair.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_poll.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_proxy_steerable.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_pubsub.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_reqrep.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_retry_eintr.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_security.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_socket.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_ssh.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_version.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_win32_shim.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_z85.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/test_zmqstream.py -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/tests/cython_ext.pyx -> build/bdist.linux-aarch64/wheel/./zmq/tests
DEBUG creating build/bdist.linux-aarch64/wheel/zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/__init__.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/garbage.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/interop.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/jsonapi.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/monitor.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/strtypes.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/win32.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/z85.py -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/buffers.pxd -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/getpid_compat.h -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/ipcmaxlen.h -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/mutex.h -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/pyversion_compat.h -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/zmq_compat.h -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/compiler.json -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/utils/config.json -> build/bdist.linux-aarch64/wheel/./zmq/utils
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/__init__.pyi -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/py.typed -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG copying build/lib.linux-aarch64-cpython-312/zmq/__init__.pxd -> build/bdist.linux-aarch64/wheel/./zmq
DEBUG running install_egg_info
DEBUG running egg_info
DEBUG writing pyzmq.egg-info/PKG-INFO
DEBUG writing dependency_links to pyzmq.egg-info/dependency_links.txt
DEBUG writing requirements to pyzmq.egg-info/requires.txt
DEBUG writing top-level names to pyzmq.egg-info/top_level.txt
DEBUG ERROR setuptools_scm._file_finders.git listing git files failed - pretending there aren't any
DEBUG reading manifest file 'pyzmq.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG warning: no previously-included files found matching 'bundled/zeromq/src/Makefile*'
DEBUG warning: no previously-included files found matching 'bundled/zeromq/src/platform.hpp'
DEBUG adding license file 'LICENSE.BSD'
DEBUG adding license file 'LICENSE.LESSER'
DEBUG adding license file 'AUTHORS.md'
DEBUG writing manifest file 'pyzmq.egg-info/SOURCES.txt'
DEBUG Copying pyzmq.egg-info to build/bdist.linux-aarch64/wheel/./pyzmq-25.1.1-py3.12.egg-info
DEBUG running install_scripts
DEBUG creating build/bdist.linux-aarch64/wheel/pyzmq-25.1.1.dist-info/WHEEL
DEBUG creating '/root/.cache/uv/builds-v0/.tmpuxfdJH/.tmp-o72l38nr/pyzmq-25.1.1-cp312-cp312-linux_aarch64.whl' and adding 'build/bdist.linux-aarch64/wheel' to it
DEBUG adding 'zmq/__init__.pxd'
DEBUG adding 'zmq/__init__.py'
DEBUG adding 'zmq/__init__.pyi'
DEBUG adding 'zmq/_future.py'
DEBUG adding 'zmq/_typing.py'
DEBUG adding 'zmq/asyncio.py'
DEBUG adding 'zmq/constants.py'
DEBUG adding 'zmq/decorators.py'
DEBUG adding 'zmq/error.py'
DEBUG adding 'zmq/py.typed'
DEBUG adding 'zmq/auth/__init__.py'
DEBUG adding 'zmq/auth/asyncio.py'
DEBUG adding 'zmq/auth/base.py'
DEBUG adding 'zmq/auth/certs.py'
DEBUG adding 'zmq/auth/ioloop.py'
DEBUG adding 'zmq/auth/thread.py'
DEBUG adding 'zmq/backend/__init__.py'
DEBUG adding 'zmq/backend/__init__.pyi'
DEBUG adding 'zmq/backend/select.py'
DEBUG adding 'zmq/backend/cffi/__init__.py'
DEBUG adding 'zmq/backend/cffi/_cdefs.h'
DEBUG adding 'zmq/backend/cffi/_poll.py'
DEBUG adding 'zmq/backend/cffi/context.py'
DEBUG adding 'zmq/backend/cffi/devices.py'
DEBUG adding 'zmq/backend/cffi/error.py'
DEBUG adding 'zmq/backend/cffi/message.py'
DEBUG adding 'zmq/backend/cffi/socket.py'
DEBUG adding 'zmq/backend/cffi/utils.py'
DEBUG adding 'zmq/backend/cython/__init__.pxd'
DEBUG adding 'zmq/backend/cython/__init__.py'
DEBUG adding 'zmq/backend/cython/_device.cpython-312.so'
DEBUG adding 'zmq/backend/cython/_poll.cpython-312.so'
DEBUG adding 'zmq/backend/cython/_proxy_steerable.cpython-312.so'
DEBUG adding 'zmq/backend/cython/_version.cpython-312.so'
DEBUG adding 'zmq/backend/cython/checkrc.pxd'
DEBUG adding 'zmq/backend/cython/constant_enums.pxi'
DEBUG adding 'zmq/backend/cython/context.cpython-312.so'
DEBUG adding 'zmq/backend/cython/context.pxd'
DEBUG adding 'zmq/backend/cython/error.cpython-312.so'
DEBUG adding 'zmq/backend/cython/libzmq.pxd'
DEBUG adding 'zmq/backend/cython/message.cpython-312.so'
DEBUG adding 'zmq/backend/cython/message.pxd'
DEBUG adding 'zmq/backend/cython/socket.cpython-312.so'
DEBUG adding 'zmq/backend/cython/socket.pxd'
DEBUG adding 'zmq/backend/cython/utils.cpython-312.so'
DEBUG adding 'zmq/devices/__init__.py'
DEBUG adding 'zmq/devices/basedevice.py'
DEBUG adding 'zmq/devices/monitoredqueue.cpython-312.so'
DEBUG adding 'zmq/devices/monitoredqueue.pxd'
DEBUG adding 'zmq/devices/monitoredqueue.py'
DEBUG adding 'zmq/devices/monitoredqueuedevice.py'
DEBUG adding 'zmq/devices/proxydevice.py'
DEBUG adding 'zmq/devices/proxysteerabledevice.py'
DEBUG adding 'zmq/eventloop/__init__.py'
DEBUG adding 'zmq/eventloop/_deprecated.py'
DEBUG adding 'zmq/eventloop/future.py'
DEBUG adding 'zmq/eventloop/ioloop.py'
DEBUG adding 'zmq/eventloop/zmqstream.py'
DEBUG adding 'zmq/green/__init__.py'
DEBUG adding 'zmq/green/core.py'
DEBUG adding 'zmq/green/device.py'
DEBUG adding 'zmq/green/poll.py'
DEBUG adding 'zmq/green/eventloop/__init__.py'
DEBUG adding 'zmq/green/eventloop/ioloop.py'
DEBUG adding 'zmq/green/eventloop/zmqstream.py'
DEBUG adding 'zmq/log/__init__.py'
DEBUG adding 'zmq/log/__main__.py'
DEBUG adding 'zmq/log/handlers.py'
DEBUG adding 'zmq/ssh/__init__.py'
DEBUG adding 'zmq/ssh/forward.py'
DEBUG adding 'zmq/ssh/tunnel.py'
DEBUG adding 'zmq/sugar/__init__.py'
DEBUG adding 'zmq/sugar/__init__.pyi'
DEBUG adding 'zmq/sugar/attrsettr.py'
DEBUG adding 'zmq/sugar/context.py'
DEBUG adding 'zmq/sugar/frame.py'
DEBUG adding 'zmq/sugar/poll.py'
DEBUG adding 'zmq/sugar/socket.py'
DEBUG adding 'zmq/sugar/stopwatch.py'
DEBUG adding 'zmq/sugar/tracker.py'
DEBUG adding 'zmq/sugar/version.py'
DEBUG adding 'zmq/tests/__init__.py'
DEBUG adding 'zmq/tests/conftest.py'
DEBUG adding 'zmq/tests/cython_ext.pyx'
DEBUG adding 'zmq/tests/test_asyncio.py'
DEBUG adding 'zmq/tests/test_auth.py'
DEBUG adding 'zmq/tests/test_cffi_backend.py'
DEBUG adding 'zmq/tests/test_constants.py'
DEBUG adding 'zmq/tests/test_context.py'
DEBUG adding 'zmq/tests/test_cython.py'
DEBUG adding 'zmq/tests/test_decorators.py'
DEBUG adding 'zmq/tests/test_device.py'
DEBUG adding 'zmq/tests/test_draft.py'
DEBUG adding 'zmq/tests/test_error.py'
DEBUG adding 'zmq/tests/test_etc.py'
DEBUG adding 'zmq/tests/test_ext.py'
DEBUG adding 'zmq/tests/test_future.py'
DEBUG adding 'zmq/tests/test_imports.py'
DEBUG adding 'zmq/tests/test_includes.py'
DEBUG adding 'zmq/tests/test_ioloop.py'
DEBUG adding 'zmq/tests/test_log.py'
DEBUG adding 'zmq/tests/test_message.py'
DEBUG adding 'zmq/tests/test_monitor.py'
DEBUG adding 'zmq/tests/test_monqueue.py'
DEBUG adding 'zmq/tests/test_multipart.py'
DEBUG adding 'zmq/tests/test_mypy.py'
DEBUG adding 'zmq/tests/test_pair.py'
DEBUG adding 'zmq/tests/test_poll.py'
DEBUG adding 'zmq/tests/test_proxy_steerable.py'
DEBUG adding 'zmq/tests/test_pubsub.py'
DEBUG adding 'zmq/tests/test_reqrep.py'
DEBUG adding 'zmq/tests/test_retry_eintr.py'
DEBUG adding 'zmq/tests/test_security.py'
DEBUG adding 'zmq/tests/test_socket.py'
DEBUG adding 'zmq/tests/test_ssh.py'
DEBUG adding 'zmq/tests/test_version.py'
DEBUG adding 'zmq/tests/test_win32_shim.py'
DEBUG adding 'zmq/tests/test_z85.py'
DEBUG adding 'zmq/tests/test_zmqstream.py'
DEBUG adding 'zmq/utils/__init__.py'
DEBUG adding 'zmq/utils/buffers.pxd'
DEBUG adding 'zmq/utils/compiler.json'
DEBUG adding 'zmq/utils/config.json'
DEBUG adding 'zmq/utils/garbage.py'
DEBUG adding 'zmq/utils/getpid_compat.h'
DEBUG adding 'zmq/utils/interop.py'
DEBUG adding 'zmq/utils/ipcmaxlen.h'
DEBUG adding 'zmq/utils/jsonapi.py'
DEBUG adding 'zmq/utils/monitor.py'
DEBUG adding 'zmq/utils/mutex.h'
DEBUG adding 'zmq/utils/pyversion_compat.h'
DEBUG adding 'zmq/utils/strtypes.py'
DEBUG adding 'zmq/utils/win32.py'
DEBUG adding 'zmq/utils/z85.py'
DEBUG adding 'zmq/utils/zmq_compat.h'
DEBUG adding 'pyzmq-25.1.1.dist-info/AUTHORS.md'
DEBUG adding 'pyzmq-25.1.1.dist-info/LICENSE.BSD'
DEBUG adding 'pyzmq-25.1.1.dist-info/LICENSE.LESSER'
DEBUG adding 'pyzmq-25.1.1.dist-info/METADATA'
DEBUG adding 'pyzmq-25.1.1.dist-info/WHEEL'
DEBUG adding 'pyzmq-25.1.1.dist-info/top_level.txt'
DEBUG adding 'pyzmq-25.1.1.dist-info/RECORD'
DEBUG removing build/bdist.linux-aarch64/wheel
DEBUG Finished building: pyzmq==25.1.1
DEBUG Released lock at `/root/.cache/uv/sdists-v6/pypi/pyzmq/25.1.1/.lock`
Prepared 1 package in 1m 34s
Installed 1 package in 376ms
 + pyzmq==25.1.1
```

---

_Comment by @samypr100 on 2024-12-02 04:13_

Thanks for the logs, it is definitely rebuilding despite the cache. To confirm, are you using `UV_LINK_MODE=copy`?

---

_Label `needs-mre` removed by @samypr100 on 2024-12-02 04:13_

---

_Label `cache` added by @samypr100 on 2024-12-02 04:13_

---

_Comment by @milisp on 2024-12-02 08:19_

> are you using `UV_LINK_MODE=copy`?

No, but I tried other commands before. Because I want to install jupyterlab

```sh
# in ~/projects/base dir
uvx jupyter-lab # this download python3.13.0
uvx --from jupyterlab jupyter-lab --allow-root  # this work once but after I type the commands below it can't work anymore 
uv pip install jupyter
uv pip install notebook --system
uv add --dev ipykernel

uv run ipython kernel install --user --name=base
uv run --with jupyter jupyter lab
```

---

_Comment by @charliermarsh on 2024-12-02 14:02_

It looks like a real problem based on this logs. I assume it's specific to Termux? I have no idea how to set that up and test it though hah.

---

_Comment by @milisp on 2024-12-02 14:25_

I think the Termux team will fix this someday 😄 

---

_Comment by @samypr100 on 2024-12-02 14:48_

Based on my basic testing in termux it seems to happen only with sdists that end up building c/rust extensions. I tested with pure pre-built wheels and sdists and didn't seem to have issues with cache at a glance.
I don't have the tooling to build uv on termux right now to go further, will attempt to revisit later. If anyone wants to help troubleshoot on their termux setup, it would be very helpful. Maybe an issue with `uv-distribution` on android and tag compatibility?

---

_Renamed from "Can uv cache the build results. Don't build again." to "uv re-builds cached packages on Termux" by @charliermarsh on 2024-12-27 13:56_

---

_Renamed from "uv re-builds cached packages on Termux" to "uv rebuilds cached packages on Termux" by @charliermarsh on 2024-12-27 13:56_

---

_Comment by @gphg on 2025-04-14 02:30_

The information is misleading. It is under Termux AND a [Termux's proot](https://github.com/termux/proot-distro): Debian.

Because it is under proot, it runs as root.
https://github.com/astral-sh/uv/issues/9559#issuecomment-2510411783

Unless you run the proot with isolated flag, I can't really tell what is wrong. Please tell us the path of `uv` binary, its version, and how it get installed.

---

_Comment by @milisp on 2025-04-14 03:48_

[#9559](https://github.com/astral-sh/uv/issues/9559#issuecomment-2510411783)


At that time uv version was 0.5.5, couldn't use it in termux, so I ran it under [Termux's proot](https://github.com/termux/proot-distro): Debian

## install and prepare

```sh
pkg install python uv libzmq proot-distro
proot-distro install debian
proot-distro login debian
```

## the path of uv binary

```sh
/data/data/com.termux/files/usr/bin/uv
```

## issue

```sh
uv init one
cd one
uv add pyzmq==25.1.1 -v

cd ..

uv init two
cd two
uv add pyzmq==25.1.1 -v # this will build pyzmq again 
```

Now uv version is 0.6.9, use in Termux. The same issue

```sh
pkg install python uv libzmq  # remove proot-distro step
```


---

_Comment by @gphg on 2025-04-14 06:37_

I assumed this only happens under proot.

Try `proot-distro login` with either `--termux-home` (warning: this populates the proot with your home directory!).

or `--isolated` (may requires to install `uv` on distro package manager).

`pyzmq` has a caveat, it is mentioned on the wiki: https://wiki.termux.com/wiki/Python

---

_Comment by @AdrianKeys on 2025-09-03 12:16_

> The information is misleading. It is under Termux AND a [Termux's proot](https://github.com/termux/proot-distro): Debian.
> 
> Because it is under proot, it runs as root. [#9559 (comment)](https://github.com/astral-sh/uv/issues/9559#issuecomment-2510411783)
> 
> Unless you run the proot with isolated flag, I can't really tell what is wrong. Please tell us the path of `uv` binary, its version, and how it get installed.

This does not only happen under proot. I encountered the same problem in termux without activating proot. 

Here are steps and details FYI.

# Env

Termux version 0.118.3, freshly obtained from F-drioid without setting any system env variables(like UV_LINK_MODE). 
All default.

# Full procedure:

In termux:

```bash
pkg install python uv
uv init test1
cd test1
```

pyproject.toml :

```toml
[project]
name = "test1"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
]

[[tool.uv.index]]
url = "https://pypi.tuna.tsinghua.edu.cn/simple"
default = true
```

```bash
uv add lxml
```

uv will download lxml source code and build lxml since lxml doesn't have a wheel for termux. 

(The building process may fail a few times saying missing some termux packages like clang/libXXXX.  pkg install these)

If uv caches the build correctly, the command below will not trigger another build. However uv rebuilds lxml.

```bash
uv sync
```


Here's obtained log via:
```bash
uv -vvv sync 2>&1 | tee uv-log.log
```

[uvlog.log](https://github.com/user-attachments/files/22117977/uvlog.log)


Line 22 ~ line 27 in the log:

```log
TRACE Comparing installed with source: InstalledDist { kind: Registry(InstalledRegistryDist { name: PackageName("lxml"), version: "6.0.1", path: "/data/data/com.termux/files/home/project/test1/.venv/lib/python3.12/site-packages/lxml-6.0.1.dist-info", cache_info: None, build_info: Some(BuildInfo { config_settings: ConfigSettings({}), extra_build_requires: [], extra_build_variables: {} }) }), metadata_cache: OnceLock(<uninit>), tags_cache: OnceLock(<uninit>) } Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "6.0.1" }]), index: Some(IndexMetadata { url: Url(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.tuna.tsinghua.edu.cn")), port: None, path: "/simple", query: None, fragment: None }, given: None }), format: Simple }), conflict: None }

DEBUG Platform tags mismatch for lxml: lxml==6.0.1

DEBUG Requirement installed, but mismatched:

  Installed: InstalledDist { kind: Registry(InstalledRegistryDist { name: PackageName("lxml"), version: "6.0.1", path: "/data/data/com.termux/files/home/project/test1/.venv/lib/python3.12/site-packages/lxml-6.0.1.dist-info", cache_info: None, build_info: Some(BuildInfo { config_settings: ConfigSettings({}), extra_build_requires: [], extra_build_variables: {} }) }), metadata_cache: OnceLock(<uninit>), tags_cache: OnceLock(Some(ExpandedTags([Small { small: WheelTagSmall { python_tag: CPython { python_version: (3, 12) }, abi_tag: CPython { gil_disabled: false, python_version: (3, 12) }, platform_tag: Linux { arch: Aarch64 } } }]))) }

  Requested: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "6.0.1" }]), index: Some(IndexMetadata { url: Url(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.tuna.tsinghua.edu.cn")), port: None, path: "/simple", query: None, fragment: None }, given: None }), format: Simple }), conflict: None }

DEBUG Identified uncached distribution: lxml==6.0.1
```

The package uv just built mismatches for the package that uv is gonna build and once uv finish building the package, it will mismatch again hence uv will never use cache. Maybe something happened in UV's matching logic for package that doesn't provide a wheel and needs building locally

---

_Referenced in [astral-sh/uv#15663](../../astral-sh/uv/pulls/15663.md) on 2025-09-03 13:47_

---

_Comment by @zanieb on 2025-09-03 14:14_

Hm, it sounds like the tags of the installed wheel do not match the tags of the interpreter. I'll add better logging in [#15663](https://github.com/astral-sh/uv/pull/15663)

---

_Comment by @AdrianKeys on 2025-09-10 15:05_

I retried uv -vvv sync after upgrading uv to 0.8.6 with further logging:

```log
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: testuv @ file:///data/data/com.termux/files/home/project/testuv
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 2 packages in 119ms
DEBUG Using request timeout of 30s
TRACE Comparing installed with source: InstalledDist { kind: Registry(InstalledRegistryDist { name: PackageName("lxml"), version: "6.0.1", path: "/data/data/com.termux/files/home/project/testuv/.venv/lib/python3.12/site-packages/lxml-6.0.1.dist-info", cache_info: None, build_info: Some(BuildInfo { config_settings: ConfigSettings({}), extra_build_requires: [], extra_build_variables: {} }) }), metadata_cache: OnceLock(<uninit>), tags_cache: OnceLock(<uninit>) } Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "6.0.1" }]), index: Some(IndexMetadata { url: Url(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.tuna.tsinghua.edu.cn")), port: None, path: "/simple", query: None, fragment: None }, given: None }), format: Simple }), conflict: None }
DEBUG Platform tags mismatch for lxml==6.0.1: The distribution is compatible with \x1b[36mLinux\x1b[39m (`\x1b[36mlinux_aarch64\x1b[39m`), but you're on \x1b[36mAndroid\x1b[39m (`\x1b[36mandroid_24_arm64_v8a\x1b[39m`)
DEBUG Requirement installed, but mismatched:
  Installed: InstalledDist { kind: Registry(InstalledRegistryDist { name: PackageName("lxml"), version: "6.0.1", path: "/data/data/com.termux/files/home/project/testuv/.venv/lib/python3.12/site-packages/lxml-6.0.1.dist-info", cache_info: None, build_info: Some(BuildInfo { config_settings: ConfigSettings({}), extra_build_requires: [], extra_build_variables: {} }) }), metadata_cache: OnceLock(<uninit>), tags_cache: OnceLock(Some(ExpandedTags([Small { small: WheelTagSmall { python_tag: CPython { python_version: (3, 12) }, abi_tag: CPython { gil_disabled: false, python_version: (3, 12) }, platform_tag: Linux { arch: Aarch64 } } }]))) }
  Requested: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "6.0.1" }]), index: Some(IndexMetadata { url: Url(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.tuna.tsinghua.edu.cn")), port: None, path: "/simple", query: None, fragment: None }, given: None }), format: Simple }), conflict: None }
DEBUG Identified uncached distribution: lxml==6.0.1
```

Maybe this is the cause?

```log
DEBUG Platform tags mismatch for lxml==6.0.1: The distribution is compatible with \x1b[36mLinux\x1b[39m (`\x1b[36mlinux_aarch64\x1b[39m`), but you're on \x1b[36mAndroid\x1b[39m (`\x1b[36mandroid_24_arm64_v8a\x1b[39m`)
DEBUG Requirement installed, but mismatched:
```

It seems that the ```wheel_tags``` uv obtained from the built package(linux_aarch64\) is different from the ```python_tag``` uv obtained (android_24_arm64_v8a). Maybe there's an inconsistency in the metadata and the how uv obtains platform info in termux.

---

_Comment by @zanieb on 2025-09-10 15:14_

It sounds like their build backend (setuptools) is not properly detecting the platform?

You might need to use config-settings to configure a plat-name or something? (It's really hard to understand how setuptools makes some of these decisions, so I can't give concrete guidance without going deep on it)

---

_Comment by @AdrianKeys on 2025-09-10 17:29_




> It sounds like their build backend (setuptools) is not properly detecting the platform?
> 
> You might need to use config-settings to configure a plat-name or something? (It's really hard to understand how setuptools makes some of these decisions, so I can't give concrete guidance without going deep on it)


This issue was introduced in Commit a513301 during a modification for
[‎crates/uv-python/python/get_interpreter_info.py](https://github.com/astral-sh/uv/commit/a513301b7a0a5d1dd840868ccfb82f49cbdbcd48#diff-6365d8de5d0242acb2f8a6a0a49e2405a837c40a74a973f58a5ccaf34e97f081).


In get_interpreter_info.py line 465, 473-488. 

```python
        musl_version = _get_musl_version(sys.executable)
        glibc_version = _get_glibc_version()

        if musl_version:
            operating_system = {
                "name": "musllinux",
                "major": musl_version[0],
                "minor": musl_version[1],
            }
        elif glibc_version != (-1, -1):
            operating_system = {
                "name": "manylinux",
                "major": glibc_version[0],
                "minor": glibc_version[1],
            }
        elif hasattr(sys, "getandroidapilevel"):
            operating_system = {
                "name": "android",
                "api_level": sys.getandroidapilevel(),
            }

```

Before this commit, on termux, func get_operating_system_and_architecture returns

```json
{"os": {"name": "manylinux", "major": -1, "minor": -1}, "arch": "aarch64"}, "manylinux_compatible": true, "gil_disabled": false, "pointer_size": "64"} 
```
This is compatible with metadata of wheels so rebuilds won't be triggered

After the commit, it became 

```json
{"os": {"name": "android", "api_level": 24}, "arch": "aarch64"}, "manylinux_compatible": false, "gil_disabled": false, "pointer_size": "64"}
```

Hence results in the difference between the wheel_tag and platform.





---

_Comment by @pradyunsg on 2025-09-14 16:14_

FWIW, here's the relevant function in setuptools for determining the wheel's tag:

https://github.com/pypa/setuptools/blob/v80.9.0/setuptools/_vendor/wheel/_bdist_wheel.py#L320

I think the issue here is the mismatch between what platform that the installer vs backend detect as currently active.


---

_Comment by @zanieb on 2025-09-14 18:08_

I guess the ultimate question is whether android wheels are manylinux compatible and whether they should be building a wheel for android specifically. Unfortunately I'm not an expert on that.

cc @dead10ck and @jooon who might have some context.

---

_Comment by @dead10ck on 2025-09-18 02:58_

> This issue was introduced in Commit [a513301](https://github.com/astral-sh/uv/commit/a513301b7a0a5d1dd840868ccfb82f49cbdbcd48) during a modification for [‎crates/uv-python/python/get_interpreter_info.py](https://github.com/astral-sh/uv/commit/a513301b7a0a5d1dd840868ccfb82f49cbdbcd48#diff-6365d8de5d0242acb2f8a6a0a49e2405a837c40a74a973f58a5ccaf34e97f081).
> 
> In get_interpreter_info.py line 465, 473-488.
> 
>         musl_version = _get_musl_version(sys.executable)
>         glibc_version = _get_glibc_version()
> 
>         if musl_version:
>             operating_system = {
>                 "name": "musllinux",
>                 "major": musl_version[0],
>                 "minor": musl_version[1],
>             }
>         elif glibc_version != (-1, -1):
>             operating_system = {
>                 "name": "manylinux",
>                 "major": glibc_version[0],
>                 "minor": glibc_version[1],
>             }
>         elif hasattr(sys, "getandroidapilevel"):
>             operating_system = {
>                 "name": "android",
>                 "api_level": sys.getandroidapilevel(),
>             }
> 
> Before this commit, on termux, func get_operating_system_and_architecture returns
> 
> {"os": {"name": "manylinux", "major": -1, "minor": -1}, "arch": "aarch64"}, "manylinux_compatible": true, "gil_disabled": false, "pointer_size": "64"} 
> 
> This is compatible with metadata of wheels so rebuilds won't be triggered
> 
> After the commit, it became
> 
> {"os": {"name": "android", "api_level": 24}, "arch": "aarch64"}, "manylinux_compatible": false, "gil_disabled": false, "pointer_size": "64"}
> 
> Hence results in the difference between the wheel_tag and platform.

Ah, I noticed this regression recently too; after this change, uv has become almost useless on Termux because I can't even `uv run` a single command without the venv getting completely rebuilt. 

I am not well researched in this at all, so take this for what it's worth, but just from taking a read through [PEP-738](https://peps.python.org/pep-0738/), which appears to be what defines the android platform tag, it looks like this is not supposed to be a thing until Python 3.13.

I don't know if these platform tag changes are typically backported, or how much of the python packaging ecosystem does anything with the android platform tags yet. But clearly setuptools does not yet detect it, which evidently renders it harmful in practice. uv will now always compute a different platform tag than setuptools and any (most?) other build backends that still end up with "Linux".

It looks like the PEP specifies some changes that seem very unlikely to be backported:

> Most of the values returned by the `platform` module will match those returned by `os.uname()`, with the exception of:
>
> * `platform.system()` - `"Android"`, instead of the default `"Linux"`
> * `platform.release()` - Android version number, as a string (e.g. `"14"`), instead of the Linux kernel version

So if supporting Android is the goal, then as ugly as this always is, it would probably make sense to only return the android tag if the Python interpreter is >= 3.13.

Even then, though, there's the practical issue of support in the ecosystem. If half or more of the common build backends don't support this tag anyway, then this is all kind of moot.

I think some more context would help; #9319 didn't really explain any reasoning for the change back to the old behavior. @charliermarsh mentions that part of the reason is because it's vendored. I don't know what it's vendored from, or how important it is to keep it the same as upstream, but at the end of the day, if the script is broken, it needs fixing.

---

_Comment by @dead10ck on 2025-09-18 03:28_

Come to think of it, this particular regression started happening much more recently than 0.5. I think https://github.com/astral-sh/uv/pull/15646 is playing a role here too -- ironically, adding support for Android tags broke Termux. But it could be it was the combo of emitting the tag and then supporting it in the uv binary.

---

_Comment by @dead10ck on 2025-09-18 15:03_

Ok, I've bisected the regression to this commit: https://github.com/astral-sh/uv/commit/be4d5b72

So if I'm not mistaken, the issue here is caused by two things:

1. #9319 changed the detected tag from `Linux` to `Android`. This caused the uv to detect the Android tag instead of the Linux tag, but didn't immediately cause any problems, until
2. #15484 started rejecting any installed packages that don't match the tags that uv detects.

The Android tag issue is definitely an issue in its own right without a clear answer, though it should be addressed somehow, as this will only become a bigger problem as Python 3.13+ is adopted over time.

On the other hand, I definitely see the logic behind #15484, but I also wonder if it wouldn't make sense to provide an escape hatch, like a config option or something that can be used to accept installed packages that don't match what uv detects. There are changes to tag/platform detections over time which can cause mismatches, but those mismatches don't necessarily mean the installed package is unusable. uv can't control what the various build backends do or don't do to detect the correct tags on a given system, nor can it control what version of those build backends are used in any given project. Mismatches are going to be inescapable.

---

_Comment by @zanieb on 2025-09-18 15:14_

cc @freakboy3742 @mhsmith @timrid

---

_Comment by @freakboy3742 on 2025-09-18 15:54_

If I'm understanding the question correctly: This is a case where the behavior changed in Python 3.13, as a result of PEP 783. Python 3.12 (and earlier) included nominal support for Android (mostly in the form of `sys.getandroidapilevel()`), and `sys.platform` would return `linux`, but there was no *official* support for Android as a platform.

As part of Python 3.13, PEP 783 was formalised, and `sys.platform` was defined as `android` on Android devices, with other `sys`, `platform` and `sysconfig` tags following suit. I've seen no evidence that setuptools or pip is returning the "wrong" answer (where "right" is what the PEP defines) on Python versions that officially support Android. These changes won't be backported, because they would change runtime behavior of a Python release that predates the PEP.

Strictly by the standards: there are no Python 3.12 (and earlier) Android wheels, because the tag standard wasn't defined. Any wheel support that exists for those releases would be based on background Linux tagging approaches, or patched versions of pip that include support for unofficial tag schemes.

I can't speak to what the appropriate fix would be for uv; but if there's any intention for uv to support Python 3.12 and earlier on Android (where "on" is either termux-style "on device" operation, or targeting Android for app development using Chaquopy, Briefcase, Kivy, or the like), there is a discontinuity in behavior that needs to be accommodated.

---

_Comment by @zanieb on 2025-09-18 15:59_

Thanks for the details Russell!

Is `sys.getandroidapilevel` available on Python <3.13? It seems like some of these failing examples are on Python 3.12?

---

_Comment by @freakboy3742 on 2025-09-18 16:32_

@zanieb `sys.getandroidapilevel()` was added in Python 3.7.

---

_Comment by @zanieb on 2025-09-18 18:44_

Great thanks! So... we should do one of:

1. Return `linux` instead of `android` in our interpreter query script on Python <3.13
2. Consider `linux` tags compatible with `android` on Python <3.13
3. Convert `android` tags to `linux` tags on Python <3.13

I think (2) seems more proper than (1). (3) seems vaguely more complicated. (1) is probably the easiest.

---

_Comment by @samypr100 on 2025-09-19 20:21_

@zanieb I think (1) was the closest to the previous known "working" behavior. Would you want to start with that to see if it resolves the issue before investing more time in (2)?

---

_Comment by @thegreekgeek on 2025-12-16 06:15_

Any movement on this?

---
