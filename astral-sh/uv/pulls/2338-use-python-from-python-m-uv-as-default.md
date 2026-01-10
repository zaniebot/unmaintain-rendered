```yaml
number: 2338
title: " Use python from `python -m uv` as default "
type: pull_request
state: closed
author: konstin
labels:
  - compatibility
assignees: []
base: main
head: konsti/default-python
created_at: 2024-03-10T13:37:55Z
updated_at: 2024-05-07T13:11:52Z
url: https://github.com/astral-sh/uv/pull/2338
synced_at: 2026-01-10T14:37:54Z
```

#  Use python from `python -m uv` as default 

---

_Pull request opened by @konstin on 2024-03-10 13:37_

Python tools run with `python -m <tool>` will use this `python` as their default python, including pip, virtualenv and the built-in venv. Calling Python tools this way is common, the [pip docs](https://pip.pypa.io/en/stable/user_guide/) use `python -m pip` exclusively, the built-in venv can only be called this way and certain project layouts require `python -m pytest` over `pytest`. This python interpreter takes precedence over the currently active (v)env.

These tools are all written in python and read `sys.executable`. To emulate this, we're setting `UV_DEFAULT_PYTHON` in the python module-launcher shim and read it in the default python discovery, prioritizing it over the active venv. User can also set `UV_DEFAULT_PYTHON` for their own purposes.

The test covers only half of the feature since we don't build the python package before running the tests.

Fixes https://github.com/astral-sh/uv/issues/2058
Fixes https://github.com/astral-sh/uv/issues/2222

---

_Label `compatibility` added by @konstin on 2024-03-10 13:37_

---

_Comment by @charliermarsh on 2024-03-10 13:48_

Rebase please when you can.

---

_Comment by @konstin on 2024-03-10 14:13_

Done

---

_@charliermarsh reviewed on 2024-03-10 14:38_

I'm okay with this in theory but it's a bit weird that `.venv/bin/python -m uv --system` now returns the virtualenv Python. I'm not sure that this should apply when you use `--system` or `--python`. What do you think?

(I'd also like @zanieb opinion before we merge anything here.)


---

_Review requested from @zanieb by @zanieb on 2024-03-10 15:02_

---

_Comment by @sbidoul on 2024-03-10 15:33_

> I'm not sure that this should apply when you use --system or --python.

This would certainly be surprising if `--python` was not honored.

---

_Comment by @konstin on 2024-03-10 15:57_

`--system` and `--python` should still have precedence, this should only affect the default when performing interpreter discovery with no specification or a version number.

Setup:

```
pipx run maturin build && venv310/bin/pip install --force-reinstall uv --find-links target/wheels
```

Black has
```
Requires-Dist: tomli>=1.1.0; python_version < '3.11'
Requires-Dist: typing-extensions>=4.0.1; python_version < '3.11'
```
which we can use to see what python was used.

```
$ venv310/bin/uv pip install --python venv311/bin/python black
Resolved 6 packages in 143ms
Downloaded 2 packages in 724ms
Installed 6 packages in 9ms
 + black==24.2.0
 + click==8.1.7
 + mypy-extensions==1.0.0
 + packaging==24.0
 + pathspec==0.12.1
 + platformdirs==4.2.0
$ venv310/bin/uv pip install --python venv310/bin/python black
Resolved 8 packages in 96ms
Downloaded 3 packages in 258ms
Installed 8 packages in 7ms
 + black==24.2.0
 + click==8.1.7
 + mypy-extensions==1.0.0
 + packaging==24.0
 + pathspec==0.12.1
 + platformdirs==4.2.0
 + tomli==2.0.1
 + typing-extensions==4.10.0
```

The `--system` behavior is like before on my machine:

In a venv:

```
$ pipx run uv pip install -v --system black
⚠️  uv is already on your PATH and installed at /home/konsti/.cargo/bin/uv. Downloading and running anyway.
    0.000085s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv::requirements::from_source source=black
 uv_interpreter::python_query::find_default_python 
      0.001812s   0ms DEBUG uv_interpreter::python_query Starting interpreter discovery for default Python
      0.010261s   8ms  WARN uv_interpreter::interpreter Broken cache entry at /home/konsti/.cache/uv/interpreter-v0/b35dc727399b4008.msgpack, removing: array had incorrect length, expected 5
      0.010289s   8ms DEBUG uv_interpreter::interpreter Probing interpreter info for: /home/konsti/.local/bin/python
      0.021672s  19ms DEBUG uv_interpreter::interpreter Found Python 3.11.6 for: /home/konsti/.local/bin/python
    0.021736s DEBUG uv::commands::pip_install Using Python 3.11.6 environment at /home/konsti/.local/bin/python
$ venv310/bin/uv pip install -v --system black
    0.000200s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv::requirements::from_source source=black
 uv_interpreter::find_python::find_default_python 
      0.006043s   0ms DEBUG uv_interpreter::find_python Starting interpreter discovery for default Python
      0.006148s   0ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.1, skipping probing: /home/konsti/projects/uv/.venv/bin/python3
    0.006181s DEBUG uv::commands::pip_install Using Python 3.12.1 environment at /home/konsti/projects/uv/.venv/bin/python3
Audited 1 package in 6ms
```

Outside of a venv:

```
$ pipx run uv pip install -v --system black
⚠️  uv is already on your PATH and installed at /home/konsti/.cargo/bin/uv. Downloading and running anyway.
    0.000085s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv::requirements::from_source source=black
 uv_interpreter::python_query::find_default_python 
      0.001812s   0ms DEBUG uv_interpreter::python_query Starting interpreter discovery for default Python
      0.010261s   8ms  WARN uv_interpreter::interpreter Broken cache entry at /home/konsti/.cache/uv/interpreter-v0/b35dc727399b4008.msgpack, removing: array had incorrect length, expected 5
      0.010289s   8ms DEBUG uv_interpreter::interpreter Probing interpreter info for: /home/konsti/.local/bin/python
      0.021672s  19ms DEBUG uv_interpreter::interpreter Found Python 3.11.6 for: /home/konsti/.local/bin/python
    0.021736s DEBUG uv::commands::pip_install Using Python 3.11.6 environment at /home/konsti/.local/bin/python
error: The interpreter at /usr is externally managed, and indicates the following:

  To install Python packages system-wide, try apt install
  python3-xyz, where xyz is the package you are trying to
  install.
  If you wish to install a non-Debian-packaged Python package,
  create a virtual environment using python3 -m venv path/to/venv.
  Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
  sure you have python3-full installed.
  If you wish to install a non-Debian packaged Python application,
  it may be easiest to use pipx install xyz, which will manage a
  virtual environment for you. Make sure you have pipx installed.
  See /usr/share/doc/python3.11/README.venv for more information.

Consider creating a virtual environment with `uv venv`.
$ venv310/bin/uv pip install -v --system black
    0.000057s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv::requirements::from_source source=black
 uv_interpreter::python_query::find_default_python 
      0.001581s   0ms DEBUG uv_interpreter::python_query Starting interpreter discovery for default Python
      0.001619s   0ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.11.6, skipping probing: /home/konsti/.local/bin/python
    0.001625s DEBUG uv::commands::pip_install Using Python 3.11.6 environment at /home/konsti/.local/bin/python
error: The interpreter at /usr is externally managed, and indicates the following:

  To install Python packages system-wide, try apt install
  python3-xyz, where xyz is the package you are trying to
  install.
  If you wish to install a non-Debian-packaged Python package,
  create a virtual environment using python3 -m venv path/to/venv.
  Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
  sure you have python3-full installed.
  If you wish to install a non-Debian packaged Python application,
  it may be easiest to use pipx install xyz, which will manage a
  virtual environment for you. Make sure you have pipx installed.
  See /usr/share/doc/python3.11/README.venv for more information.

Consider creating a virtual environment with `uv venv`.
```

---

_Comment by @charliermarsh on 2024-03-10 16:02_

How is that? If you pass `--system`, it calls `PythonEnvironment::from_default_python` which calls `find_default_python`  which calls `try_find_default_python` which calls `find_python`, which is changed here to start by looking at `UV_DEFAULT_PYTHON`.

---

_Comment by @charliermarsh on 2024-03-10 16:03_

What does `venv310/bin/uv pip install black --system --verbose` print? It resolves to the system Python?

---

_Comment by @konstin on 2024-03-10 16:29_

Sorry, i got fooled by index precedence, `pipx run maturin build && venv310/bin/pip install --force-reinstall uv --find-links target/wheels --no-index` is the right test command.

The `--system` was indeed broken, i've added a system flag to differentiate system/non-system cases:

```
$ venv310/bin/python -m uv pip install black --system --verbose
    0.000220s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv::requirements::from_source source=black
 uv_interpreter::find_python::find_default_python 
      0.006423s   0ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.11.6, skipping probing: /home/konsti/.local/bin/python
    0.006465s DEBUG uv::commands::pip_install Using Python 3.11.6 environment at /home/konsti/.local/bin/python
error: The interpreter at /usr is externally managed, and indicates the following:

  To install Python packages system-wide, try apt install
  python3-xyz, where xyz is the package you are trying to
  install.
  If you wish to install a non-Debian-packaged Python package,
  create a virtual environment using python3 -m venv path/to/venv.
  Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
  sure you have python3-full installed.
  If you wish to install a non-Debian packaged Python application,
  it may be easiest to use pipx install xyz, which will manage a
  virtual environment for you. Make sure you have pipx installed.
  See /usr/share/doc/python3.11/README.venv for more information.

Consider creating a virtual environment with `uv venv`.
$ venv310/bin/python -m uv pip install black --verbose
    0.000215s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv::requirements::from_source source=black
    0.006121s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /home/konsti/projects/uv/venv310
    0.006220s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.10.13, skipping probing: /home/konsti/projects/uv/venv310/bin/python
    0.006243s DEBUG uv::commands::pip_install Using Python 3.10.13 environment at /home/konsti/projects/uv/venv310/bin/python
Audited 1 package in 6ms
$ venv310/bin/python -m uv pip install black --python venv311/bin/python --verbose
    0.000224s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv::requirements::from_source source=black
 uv_interpreter::find_python::find_requested_python request=venv311/bin/python
      0.006037s   0ms DEBUG uv_interpreter::find_python Starting interpreter discovery for Python @ `venv311/bin/python`
      0.006143s   0ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.11.7, skipping probing: venv311/bin/python
    0.006185s DEBUG uv::commands::pip_install Using Python 3.11.7 environment at /home/konsti/projects/uv/venv311/bin/python
Audited 1 package in 6ms
```

---

_Review comment by @konstin on `crates/uv-interpreter/src/find_python.rs`:74 on 2024-03-10 16:29_

Not sure if this should be a flag or two functions

---

_@konstin reviewed on 2024-03-10 16:29_

---

_Comment by @charliermarsh on 2024-03-10 19:24_

What about `.venv/bin/python -m uv pip install -p 3.7`, in a virtualenv that _is_ Python 3.7? Should that return `.venv/bin/python` or not? (I believe it currently does in this PR, is my read.)

---

_Comment by @konstin on 2024-03-10 19:49_

Currently, it does:
```console
$ a/bin/python -m uv pip install -p 3.10 -v black 
    0.000161s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv::requirements::from_source source=black
 uv_interpreter::find_python::find_requested_python request=3.10
      0.005736s   0ms DEBUG uv_interpreter::find_python Starting interpreter discovery for Python @ `3.10`
      0.005754s   0ms DEBUG uv_interpreter::find_python Trying UV_DEFAULT_PYTHON at /home/konsti/projects/uv/a/bin/python
      0.005825s   0ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.10.13, skipping probing: /home/konsti/projects/uv/a/bin/python
    0.005856s DEBUG uv::commands::pip_install Using Python 3.10.13 environment at /home/konsti/projects/uv/a/bin/python
Audited 1 package in 6ms
$ a/bin/python3.10 -m uv pip install -p 3.10 -v black 
    0.000168s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv::requirements::from_source source=black
 uv_interpreter::find_python::find_requested_python request=3.10
      0.006305s   0ms DEBUG uv_interpreter::find_python Starting interpreter discovery for Python @ `3.10`
      0.006342s   0ms DEBUG uv_interpreter::find_python Trying UV_DEFAULT_PYTHON at /home/konsti/projects/uv/a/bin/python3.10
      0.006412s   0ms DEBUG uv_interpreter::interpreter Probing interpreter info for: /home/konsti/projects/uv/a/bin/python3.10
      0.026986s  20ms DEBUG uv_interpreter::interpreter Found Python 3.10.13 for: /home/konsti/projects/uv/a/bin/python3.10
    0.027167s DEBUG uv::commands::pip_install Using Python 3.10.13 environment at /home/konsti/projects/uv/a/bin/python3.10
Audited 1 package in 27ms
```

A binary name works differently though:

```console
$ a/bin/python3.10 -m uv pip install -p python3.10 -v black 
    0.000172s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv::requirements::from_source source=black
 uv_interpreter::find_python::find_requested_python request=python3.10
      0.005800s   0ms DEBUG uv_interpreter::find_python Starting interpreter discovery for Python @ `python3.10`
      0.005906s   0ms DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.10.13, skipping probing: /home/konsti/.local/bin/python3.10
    0.005939s DEBUG uv::commands::pip_install Using Python 3.10.13 environment at /home/konsti/.local/bin/python3.10
Audited 1 package in 6ms
```

I don't have an opinion on how `pip install -p <non-path>` should work, i don't use that combination.

---

_@charliermarsh reviewed on 2024-03-10 19:52_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/main.rs`:45 on 2024-03-10 19:52_

(Should we just remove this binary entrypoint?)

---

_@charliermarsh reviewed on 2024-03-10 19:53_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_freeze.rs`:31 on 2024-03-10 19:53_

You could consider passing `system` here and on line 36. It might actually be clearer IMO, since we don't have the naked bool.

---

_Comment by @charliermarsh on 2024-03-10 19:53_

I don't have a strong opinion on it either.

---

_Comment by @zanieb on 2024-03-12 05:47_

I think different behavior for `-p 3.10` vs `-p python3.10` would be pretty confusing, I'd rather not introduce that.

---

_Comment by @konstin on 2024-03-12 09:33_

What do you want the sematics of
```shell
python -m uv pip compile -p 3.10
python -m uv pip compile -p python3.10
python3.10 -m uv pip compile -p 3.10
python3.10 -m uv pip compile -p python3.10
```
to be?

---

_Comment by @charliermarsh on 2024-03-12 14:31_

What if we only respect the `python -m` if no selectors are provided? I.e., there's no `-p` or `--system`.

---

_Comment by @zanieb on 2024-03-12 14:37_

`python3.10` and `3.10` should be equivalent.

`python -m uv pip compile -p 3.10` should use the active Python if it's 3.10 otherwise find the first Python 3.10 on your path, if not found it should use the active Python. Right? Compilation doesn't install anything so idk if it's a good example?

I'd be confused if we didn't use the spawning Python if `-p 3.10` was provided and was satisfied.

---

_Comment by @zanieb on 2024-03-12 14:41_

`python -m uv pip install -p 3.10 ...` feels more complicated since we don't want to install into the system without opt-in, maybe we should just error and say `--system` or `--python` is needed in that case?

---

_Comment by @charliermarsh on 2024-03-12 14:43_

> python -m uv pip install -p 3.10 ... feels more complicated since we don't want to install into the system without opt-in, maybe we should just error and say --system or --python is needed in that case?

But we already do this, right? `uv pip install -p 3.10` will install into system, and in that example, they _did_ provide `--python`.

---

_Comment by @zanieb on 2024-03-12 14:48_

@charliermarsh can you link to what you're referring to? What do you mean by "did provide --python"?

---

_Comment by @charliermarsh on 2024-03-12 14:51_

The example you gave was `python -m uv pip install -p 3.10`. The `-p` is `--python`, right?

---

_Comment by @zanieb on 2024-03-12 14:59_

Oh sorry 🤦‍♀️

I meant that to install into the system we should require a _path_ to a Python interpreter or the `--system` flag, that's kind of a separate from this pull request though.

---

_Comment by @konstin on 2024-03-13 11:43_

Given that #2386 is a far further ranging discussion with bigger changes attached, what do we need to move forward with this PR?

---

_Comment by @zanieb on 2024-03-13 14:33_

While this is nice to have I don't see a strong motivation to rush it, I'd rather come to a consensus in #2386 then assess if this meets the behavior there.

---

_Comment by @charliermarsh on 2024-03-25 14:49_

Based on https://github.com/astral-sh/uv/issues/2386, is there a way for us to slot this in in a forwards-compatible without doing the entirety of the changes in https://github.com/astral-sh/uv/issues/2386?

---

_Closed by @konstin on 2024-05-07 12:13_

---

_Comment by @zanieb on 2024-05-07 13:11_

I need to add an environment variable for this in #3266 — figure it will be a separate PR.

---
