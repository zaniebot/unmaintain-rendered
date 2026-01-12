```yaml
number: 12173
title: "`venv` module does not work on when invoked via a symlink to a `uv`-installed Python"
type: issue
state: open
author: pganssle
labels:
  - bug
assignees: []
created_at: 2025-03-14T19:05:46Z
updated_at: 2025-07-28T23:10:35Z
url: https://github.com/astral-sh/uv/issues/12173
synced_at: 2026-01-12T16:00:57Z
```

# `venv` module does not work on when invoked via a symlink to a `uv`-installed Python

---

_@pganssle_

### Summary

If I do the following:

```
tmpdir=$(mktemp -d)
cd $tmpdir
uv python install 3.13.2  # Version doesn't matter here
ln -s ~/.local/share/uv/python/cpython-3.13.2-linux-x86_64-gnu/bin/python3.13 python
./python -m venv venv
```

The command fails with:


> Error: Command '['/tmp/tmp.ATu00koNaI/venv/bin/python', '-m', 'ensurepip', '--upgrade', '--default-pip']' returned non-zero exit status 1.

Invoking the python interpreter by its full path works just fine.

Doing the same thing with a `pyenv`-installed Python works as well, so this seems to be `uv`-specific.

Possbly releated is #8879. The title of [this issue](https://github.com/astral-sh/python-build-standalone/issues/380) seems similar, but the reproducer makes it seem like it's something else, and it breaks in a different way.

### Platform

Linux

### Version

0.5.29

### Python version

3.13.2

---

_Label `bug` added by @pganssle on 2025-03-14 19:05_

---

_Comment by @konstin on 2025-03-14 19:15_

I can confirm that this one of those bugs around PYTHONHOME:

```
$ cat venv/pyvenv.cfg 
home = /tmp/tmp.WlJ3dJqgNI
include-system-site-packages = false
version = 3.13.0
executable = /home/konsti/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/bin/python3.13
command = /tmp/tmp.WlJ3dJqgNI/python -m venv /tmp/tmp.WlJ3dJqgNI/venv
```

uv correctly resolves the Python's home, so I count this as another case of https://github.com/astral-sh/python-build-standalone/issues/380:

```
$ uv venv -p ./python uv-venv
$ cat uv-venv/pyvenv.cfg 
home = /home/konsti/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/bin
implementation = CPython
uv = 0.6.6
version_info = 3.13.0
include-system-site-packages = false
```

Note that the `home` key is also incorrect for apt-installed system Python, though the system Python works anyway (I think it because it knows its PYTHONHOME from being compiled in):

```
$ ln -s /usr/bin/python usr-python
$ ./usr-python -m venv usr-venv
$ cat usr-venv/pyvenv.cfg
home = /tmp/tmp.WlJ3dJqgNI
include-system-site-packages = false
version = 3.12.3
executable = /usr/bin/python3.12
command = /tmp/tmp.WlJ3dJqgNI/usr-python -m venv /tmp/tmp.WlJ3dJqgNI/usr-venv
```

---

_Comment by @henryiii on 2025-03-21 18:59_

Here's a reproducer for `uvx cibuildwheel`:

```console
$ docker run --rm -it ubuntu:24.10
# apt update && apt install -y curl git
# curl -LsSf https://astral.sh/uv/install.sh | sh
# source $HOME/.local/bin/env
# uv python install 3.12
# git clone https://github.com/scikit-hep/boost-histogram
# cd boost-histogram
# uvx cibuildwheel --only cp312-pyodide_wasm32

     _ _       _ _   _       _           _
 ___|_| |_ _ _|_| |_| |_ _ _| |_ ___ ___| |
|  _| | . | | | | | . | | | |   | -_| -_| |
|___|_|___|___|_|_|___|_____|_|_|___|___|_|

cibuildwheel version 2.23.1

Build options:
  platform: pyodide
  allow_empty: False
  architectures: wasm32
  build_selector:
    build_config: cp312-pyodide_wasm32
    skip_config:
    requires_python: >=3.8
    enable: ['cpython-freethreading', 'cpython-prerelease', 'pypy']
  output_dir: /boost-histogram/wheelhouse
  package_dir: /boost-histogram
  test_selector:
    skip_config: cp*-musllinux_* cp313t-*win* pp311-*
  before_all:
  before_build:
  before_test:
  build_frontend:
    *: build[uv]
    cp312-pyodide_wasm32:
      name: build
      args: ['--exports', 'whole_archive']
  build_verbosity: 0
  config_settings:
  container_engine: docker
  dependency_constraints: pinned
  environment:
    PIP_ONLY_BINARY="numpy"
    PIP_PREFER_BINARY="1"
  manylinux_images: None
  musllinux_images: None
  repair_command:
  test_command:
    *: pytest -n auto --benchmark-disable {project}/tests
    cp312-pyodide_wasm32: pytest --benchmark-disable {project}/tests
  test_extras:
  test_groups:
    test
  test_requires:
    cloudpickle
    hypothesis>=6.0
    pytest-benchmark
    pytest>=6.0
    pytest-xdist

Cache folder: /root/.cache/cibuildwheel

Here we go!


Building cp312-pyodide_wasm32 wheel
CPython 3.12 Pyodide

Setting up build environment...

+ Download https://github.com/pypa/get-virtualenv/blob/20.29.3/public/virtualenv.pyz?raw=true to /root/.cache/cibuildwheel/virtualenv-20.29.3.pyz
+ /root/.cache/uv/archive-v0/m8zF1t0bWCOV1lDOploP5/bin/python -sS /root/.cache/cibuildwheel/virtualenv-20.29.3.pyz --activators= --no-periodic-update --pip=embed --no-setuptools --no-wheel --python /root/.cache/uv/archive-v0/m8zF1t0bWCOV1lDOploP5/bin/python /tmp/cibw-run-zmv0cabc/cp312-pyodide_wasm32/build/venv
created virtual environment CPython3.12.9.final.0-64 in 259ms
  creator CPython3Posix(dest=/tmp/cibw-run-zmv0cabc/cp312-pyodide_wasm32/build/venv, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=embed, via=copy, app_data_dir=/root/.local/share/virtualenv)
    added seed packages: pip==25.0.1
+ python -m pip install --upgrade pip -c /root/.cache/uv/archive-v0/m8zF1t0bWCOV1lDOploP5/lib/python3.12/site-packages/cibuildwheel/resources/constraints-pyodide312.txt
Could not find platform independent libraries <prefix>
Could not find platform dependent libraries <exec_prefix>
Python path configuration:
  PYTHONHOME = (not set)
  PYTHONPATH = (not set)
  program name = '/tmp/cibw-run-zmv0cabc/cp312-pyodide_wasm32/build/venv/bin/python'
  isolated = 0
  environment = 1
  user site = 1
  safe_path = 0
  import site = 1
  is in build tree = 0
  stdlib dir = '/install/lib/python3.12'
  sys._base_executable = '/root/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12'
  sys.base_prefix = '/install'
  sys.base_exec_prefix = '/install'
  sys.platlibdir = 'lib'
  sys.executable = '/tmp/cibw-run-zmv0cabc/cp312-pyodide_wasm32/build/venv/bin/python'
  sys.prefix = '/install'
  sys.exec_prefix = '/install'
  sys.path = [
    '/install/lib/python312.zip',
    '/install/lib/python3.12',
    '/install/lib/python3.12/lib-dynload',
  ]
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'

Current thread 0x00007f8c21d3c740 (most recent call first):
  <no Python frame>
Traceback (most recent call last):
  File "/root/.cache/uv/archive-v0/m8zF1t0bWCOV1lDOploP5/bin/cibuildwheel", line 12, in <module>
    sys.exit(main())
             ^^^^^^
  File "/root/.cache/uv/archive-v0/m8zF1t0bWCOV1lDOploP5/lib/python3.12/site-packages/cibuildwheel/__main__.py", line 49, in main
    main_inner(global_options)
  File "/root/.cache/uv/archive-v0/m8zF1t0bWCOV1lDOploP5/lib/python3.12/site-packages/cibuildwheel/__main__.py", line 184, in main_inner
    build_in_directory(args)
  File "/root/.cache/uv/archive-v0/m8zF1t0bWCOV1lDOploP5/lib/python3.12/site-packages/cibuildwheel/__main__.py", line 351, in build_in_directory
    platform_module.build(options, tmp_path)
  File "/root/.cache/uv/archive-v0/m8zF1t0bWCOV1lDOploP5/lib/python3.12/site-packages/cibuildwheel/pyodide.py", line 244, in build
    env = setup_python(
          ^^^^^^^^^^^^^
  File "/root/.cache/uv/archive-v0/m8zF1t0bWCOV1lDOploP5/lib/python3.12/site-packages/cibuildwheel/pyodide.py", line 131, in setup_python
    call(
  File "/root/.cache/uv/archive-v0/m8zF1t0bWCOV1lDOploP5/lib/python3.12/site-packages/cibuildwheel/util.py", line 154, in call
    result = subprocess.run(
             ^^^^^^^^^^^^^^^
  File "/root/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/lib/python3.12/subprocess.py", line 573, in run
    raise CalledProcessError(retcode, process.args,
subprocess.CalledProcessError: Command '['/tmp/cibw-run-zmv0cabc/cp312-pyodide_wasm32/build/venv/bin/python', '-m', 'pip', 'install', '--upgrade', 'pip', '-c', '/root/.cache/uv/archive-v0/m8zF1t0bWCOV1lDOploP5/lib/python3.12/site-packages/cibuildwheel/resources/constraints-pyodide312.txt']' returned non-zero exit status 1.
```

https://github.com/pypa/cibuildwheel/issues/2327

---

_Assigned to @geofft by @geofft on 2025-07-28 23:10_

---
