```yaml
number: 15415
title: uv fails to build package with cmake 4.1.0 but system pip succeeds
type: issue
state: closed
author: thomasbbrunner
labels:
  - bug
assignees: []
created_at: 2025-08-21T13:49:26Z
updated_at: 2025-08-27T15:28:03Z
url: https://github.com/astral-sh/uv/issues/15415
synced_at: 2026-01-10T03:23:54Z
```

# uv fails to build package with cmake 4.1.0 but system pip succeeds

---

_Issue opened by @thomasbbrunner on 2025-08-21 13:49_

### Summary

We have to build the ([multi-agent-ale-py](https://github.com/Farama-Foundation/Multi-Agent-ALE)) package as they don't publish Python 3.11 wheels.

Recently, the build started to fail and we **tracked it down to the new release of the Python `cmake` 4.1.0 package**. Reverting to the previous release (4.0.3) fixes the issue.

However, if we build the package using the system's pip and cmake 4.1.0, the build works. **Only when using uv does the build fail.**

## Reproduction

This should work (also works with `--use-pep517`):
```
git clone https://github.com/Farama-Foundation/Multi-Agent-ALE.git
cd Multi-Agent-ALE
python3.11 -m pip install .
```

<details>

<summary> Full log </summary>

```
~/Multi-Agent-ALE$ python3.11 -m pip install . -v
Using pip 22.0.2 from /usr/lib/python3/dist-packages/pip (python 3.11)
Defaulting to user installation because normal site-packages is not writeable
Processing /home/coder/Multi-Agent-ALE
  Running command pip subprocess to install build dependencies
  Collecting setuptools>=42
    Using cached setuptools-80.9.0-py3-none-any.whl (1.2 MB)
  Collecting wheel
    Using cached wheel-0.45.1-py3-none-any.whl (72 kB)
  Collecting cmake>=3.14
    Using cached cmake-4.1.0-py3-none-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (29.7 MB)
  Collecting ninja
    Using cached ninja-1.13.0-py3-none-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (180 kB)
  Installing collected packages: wheel, setuptools, ninja, cmake
  Successfully installed cmake-4.1.0 ninja-1.13.0 setuptools-80.9.0 wheel-0.45.1
  Installing build dependencies ... done
  Running command Getting requirements to build wheel
  running egg_info
  creating multi_agent_ale_py.egg-info
  writing manifest file 'multi_agent_ale_py.egg-info/SOURCES.txt'
  writing manifest file 'multi_agent_ale_py.egg-info/SOURCES.txt'
  Getting requirements to build wheel ... done
  Running command Preparing metadata (pyproject.toml)
  running dist_info
  creating /tmp/pip-modern-metadata-e2t407it/multi_agent_ale_py.egg-info
  writing manifest file '/tmp/pip-modern-metadata-e2t407it/multi_agent_ale_py.egg-info/SOURCES.txt'
  writing manifest file '/tmp/pip-modern-metadata-e2t407it/multi_agent_ale_py.egg-info/SOURCES.txt'
  Preparing metadata (pyproject.toml) ... done
Requirement already satisfied: numpy in /home/coder/.local/lib/python3.11/site-packages (from multi-agent-ale-py==0.1.11) (2.3.2)
Building wheels for collected packages: multi-agent-ale-py
  Running command Building wheel for multi-agent-ale-py (pyproject.toml)
  running bdist_wheel
  running build
  running build_py
  creating build
  creating build/lib.linux-x86_64-3.11
  creating build/lib.linux-x86_64-3.11/multi_agent_ale_py
  copying multi_agent_ale_py/ale_python_interface.py -> build/lib.linux-x86_64-3.11/multi_agent_ale_py
  copying multi_agent_ale_py/__init__.py -> build/lib.linux-x86_64-3.11/multi_agent_ale_py
  running egg_info
  writing manifest file 'multi_agent_ale_py.egg-info/SOURCES.txt'
  copying multi_agent_ale_py/ale_c_wrapper.cpp -> build/lib.linux-x86_64-3.11/multi_agent_ale_py
  copying multi_agent_ale_py/ale_c_wrapper.h -> build/lib.linux-x86_64-3.11/multi_agent_ale_py
  creating build/lib.linux-x86_64-3.11/multi_agent_ale_py/tests
  creating build/lib.linux-x86_64-3.11/multi_agent_ale_py/tests/fixtures
  copying multi_agent_ale_py/tests/fixtures/tetris.bin -> build/lib.linux-x86_64-3.11/multi_agent_ale_py/tests/fixtures
  running build_ext
  -- The CXX compiler identification is GNU 11.4.0
  -- Detecting CXX compiler ABI info
  -- Detecting CXX compiler ABI info - done
  -- Check for working CXX compiler: /usr/bin/c++ - skipped
  -- Detecting CXX compile features
  -- Detecting CXX compile features - done
  -- Found ZLIB: /usr/lib/x86_64-linux-gnu/libz.so (found version "1.2.11")
  -- Looking for C++ include pthread.h
  -- Looking for C++ include pthread.h - found
  -- Performing Test CMAKE_HAVE_LIBC_PTHREAD
  -- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
  -- Found Threads: TRUE
  -- Configuring done
  -- Generating done
  CMake Warning:
    Manually-specified variables were not used by the project:

      BUILD_CLI
      BUILD_EXAMPLES
      OUTPUT_NAME
      USE_RLGLUE


  -- Build files have been written to: /home/coder/Multi-Agent-ALE/build/temp.linux-x86_64-3.11
  [  0%] Building CXX object src/CMakeFiles/ale.dir/common/ColourPalette.cpp.o

   (removed)

  [100%] Built target ale-c-lib
  running install
  running install_lib
  creating build/bdist.linux-x86_64
  creating build/bdist.linux-x86_64/wheel
  creating build/bdist.linux-x86_64/wheel/multi_agent_ale_py
  creating build/bdist.linux-x86_64/wheel/multi_agent_ale_py/tests
  creating build/bdist.linux-x86_64/wheel/multi_agent_ale_py/tests/fixtures
  copying build/lib.linux-x86_64-3.11/multi_agent_ale_py/tests/fixtures/tetris.bin -> build/bdist.linux-x86_64/wheel/multi_agent_ale_py/tests/fixtures
  copying build/lib.linux-x86_64-3.11/multi_agent_ale_py/libale_c.so -> build/bdist.linux-x86_64/wheel/multi_agent_ale_py
  copying build/lib.linux-x86_64-3.11/multi_agent_ale_py/ale_python_interface.py -> build/bdist.linux-x86_64/wheel/multi_agent_ale_py
  copying build/lib.linux-x86_64-3.11/multi_agent_ale_py/ale_c_wrapper.h -> build/bdist.linux-x86_64/wheel/multi_agent_ale_py
  copying build/lib.linux-x86_64-3.11/multi_agent_ale_py/ale_c_wrapper.cpp -> build/bdist.linux-x86_64/wheel/multi_agent_ale_py
  copying build/lib.linux-x86_64-3.11/multi_agent_ale_py/__init__.py -> build/bdist.linux-x86_64/wheel/multi_agent_ale_py
  running install_egg_info
  Copying multi_agent_ale_py.egg-info to build/bdist.linux-x86_64/wheel/multi_agent_ale_py-0.1.11.egg-info
  running install_scripts
  Building wheel for multi-agent-ale-py (pyproject.toml) ... done
  Created wheel for multi-agent-ale-py: filename=multi_agent_ale_py-0.1.11-cp311-cp311-linux_x86_64.whl size=721928 sha256=6011779ed859c9329777faa6e89b73eb4cdb4386a3b56299880ee5de8f1b57d1
  Stored in directory: /home/coder/.cache/pip/wheels/4a/cf/25/59edafccf7394775245d4fefe2006aa7ef2dc740edfb972b33
Successfully built multi-agent-ale-py
Installing collected packages: multi-agent-ale-py
  Attempting uninstall: multi-agent-ale-py
    Found existing installation: multi-agent-ale-py 0.1.11
    Uninstalling multi-agent-ale-py-0.1.11:
      Removing file or directory /home/coder/.local/lib/python3.11/site-packages/multi_agent_ale_py-0.1.11.dist-info/
      Removing file or directory /home/coder/.local/lib/python3.11/site-packages/multi_agent_ale_py/
      Successfully uninstalled multi-agent-ale-py-0.1.11
Successfully installed multi-agent-ale-py-0.1.11
```

</details>

This fails:

```
git clone https://github.com/Farama-Foundation/Multi-Agent-ALE.git
cd Multi-Agent-ALE
uv python pin 3.11
uv venv
uv pip install .
```
<details>

<summary>Full error</summary>

```
~/Multi-Agent-ALE$ uv pip install . -v
DEBUG uv 0.8.12
DEBUG Marking explicit source tree for reinstall: `/home/coder/Multi-Agent-ALE`
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.11.0rc1-linux-x86_64-gnu` at `/home/coder/Multi-Agent-ALE/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.11.0rc1 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG Stripping pre-release from `python_full_version`: 3.11.0rc1
DEBUG Using request timeout of 30s
DEBUG No static `pyproject.toml` available for: file:///home/coder/Multi-Agent-ALE (FieldNotFound("project"))
DEBUG Acquired lock for `/home/coder/.cache/uv/sdists-v9/path/eae9f7bbc3959eea`
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1755771388, tv_nsec: 389066117 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1755771388, tv_nsec: 389066117 })))}
DEBUG Preparing metadata for: file:///home/coder/Multi-Agent-ALE
DEBUG Assessing Python executable as base candidate: /usr/bin/python3.11
DEBUG Using base executable for virtual environment: /usr/bin/python3.11
DEBUG Resolving build requirements
DEBUG Stripping pre-release from `python_full_version`: 3.11.0rc1
DEBUG Solving with installed Python version: 3.11rc1
DEBUG Solving with target Python version: >=3.11
DEBUG Adding direct dependency: setuptools>=42
DEBUG Adding direct dependency: wheel*
DEBUG Adding direct dependency: cmake>=3.14
DEBUG Adding direct dependency: ninja*
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Found fresh response for: https://pypi.org/simple/wheel/
DEBUG Found fresh response for: https://pypi.org/simple/ninja/
DEBUG Searching for a compatible version of setuptools (>=42)
DEBUG Selecting: setuptools==80.9.0 [compatible] (setuptools-80.9.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/cmake/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ed/de/0e6edf44d6a04dabd0318a519125ed0415ce437ad5a1ec9b9be03d9048cf/ninja-1.13.0-py3-none-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata
DEBUG Searching for a compatible version of wheel (*)
DEBUG Searching for a compatible version of wheel (<0.46.1 | >0.46.1)
DEBUG Searching for a compatible version of wheel (<0.46.0 | >0.46.0, <0.46.1 | >0.46.1)
DEBUG Selecting: wheel==0.45.1 [compatible] (wheel-0.45.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/34/da/0217073d5b3fb8655b3de8af4e9e797a25ae28a2932b1138452a0dc89e9f/cmake-4.1.0-py3-none-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0b/2c/87f3254fd8ffd29e4c02732eee68a83a1d3c346ae39bc6822dcbcb697f2b/wheel-0.45.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of cmake (>=3.14)
DEBUG Selecting: cmake==4.1.0 [compatible] (cmake-4.1.0-py3-none-manylinux2014_x86_64.manylinux_2_17_x86_64.whl)
DEBUG Searching for a compatible version of ninja (*)
DEBUG Selecting: ninja==1.13.0 [compatible] (ninja-1.13.0-py3-none-manylinux2014_x86_64.manylinux_2_17_x86_64.whl)
DEBUG Tried 4 versions: cmake 1, ninja 1, setuptools 1, wheel 1
DEBUG marker environment resolution took 0.003s
DEBUG Installing in ninja==1.13.0, cmake==4.1.0, wheel==0.45.1, setuptools==80.9.0 in /home/coder/.cache/uv/builds-v0/.tmpZWInxH
DEBUG Registry requirement already cached: ninja==1.13.0
DEBUG Registry requirement already cached: cmake==4.1.0
DEBUG Registry requirement already cached: wheel==0.45.1
DEBUG Registry requirement already cached: setuptools==80.9.0
DEBUG Installing build requirements: ninja==1.13.0, cmake==4.1.0, wheel==0.45.1, setuptools==80.9.0
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta.get_requires_for_build_wheel()`
DEBUG running egg_info
DEBUG creating multi_agent_ale_py.egg-info
DEBUG writing multi_agent_ale_py.egg-info/PKG-INFO
DEBUG writing dependency_links to multi_agent_ale_py.egg-info/dependency_links.txt
DEBUG writing requirements to multi_agent_ale_py.egg-info/requires.txt
DEBUG writing top-level names to multi_agent_ale_py.egg-info/top_level.txt
DEBUG writing manifest file 'multi_agent_ale_py.egg-info/SOURCES.txt'
DEBUG reading manifest file 'multi_agent_ale_py.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG adding license file 'LICENSE.md'
DEBUG writing manifest file 'multi_agent_ale_py.egg-info/SOURCES.txt'
DEBUG Locking the source tree for setuptools
DEBUG Acquired lock for `/home/coder/Multi-Agent-ALE`
DEBUG Calling `setuptools.build_meta.prepare_metadata_for_build_wheel()`
DEBUG running dist_info
DEBUG creating /home/coder/.cache/uv/builds-v0/.tmpZWInxH/metadata_directory/multi_agent_ale_py.egg-info
DEBUG writing /home/coder/.cache/uv/builds-v0/.tmpZWInxH/metadata_directory/multi_agent_ale_py.egg-info/PKG-INFO
DEBUG writing dependency_links to /home/coder/.cache/uv/builds-v0/.tmpZWInxH/metadata_directory/multi_agent_ale_py.egg-info/dependency_links.txt
DEBUG writing requirements to /home/coder/.cache/uv/builds-v0/.tmpZWInxH/metadata_directory/multi_agent_ale_py.egg-info/requires.txt
DEBUG writing top-level names to /home/coder/.cache/uv/builds-v0/.tmpZWInxH/metadata_directory/multi_agent_ale_py.egg-info/top_level.txt
DEBUG writing manifest file '/home/coder/.cache/uv/builds-v0/.tmpZWInxH/metadata_directory/multi_agent_ale_py.egg-info/SOURCES.txt'
DEBUG reading manifest file '/home/coder/.cache/uv/builds-v0/.tmpZWInxH/metadata_directory/multi_agent_ale_py.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG adding license file 'LICENSE.md'
DEBUG writing manifest file '/home/coder/.cache/uv/builds-v0/.tmpZWInxH/metadata_directory/multi_agent_ale_py.egg-info/SOURCES.txt'
DEBUG creating '/home/coder/.cache/uv/builds-v0/.tmpZWInxH/metadata_directory/multi_agent_ale_py-0.1.11.dist-info'
DEBUG Released lock at `/tmp/uv-setuptools-f57b8052b04729c3.lock`
DEBUG Prepared metadata for: file:///home/coder/Multi-Agent-ALE
DEBUG Released lock at `/home/coder/.cache/uv/sdists-v9/path/eae9f7bbc3959eea/.lock`
DEBUG Solving with installed Python version: 3.11rc1
DEBUG Solving with target Python version: >=3.11
DEBUG Adding direct dependency: multi-agent-ale-py*
DEBUG Searching for a compatible version of multi-agent-ale-py @ file:///home/coder/Multi-Agent-ALE (*)
DEBUG Adding transitive dependency for multi-agent-ale-py==0.1.11: numpy*
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
DEBUG Searching for a compatible version of numpy (*)
DEBUG Selecting: numpy==2.3.2 [compatible] (numpy-2.3.2-cp311-cp311-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c8/b0/fbeee3000a51ebf7222016e2939b5c5ecf8000a19555d04a18f1e02521b8/numpy-2.3.2-cp311-cp311-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl.metadata
DEBUG Tried 2 versions: multi-agent-ale-py 1, numpy 1
DEBUG marker environment resolution took 0.003s
Resolved 2 packages in 512ms
DEBUG Must revalidate requirement: multi-agent-ale-py
DEBUG Registry requirement already cached: numpy==2.3.2
DEBUG Acquired lock for `/home/coder/.cache/uv/sdists-v9/path/eae9f7bbc3959eea`
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1755771388, tv_nsec: 389066117 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1755771388, tv_nsec: 389066117 })))}
   Building multi-agent-ale-py @ file:///home/coder/Multi-Agent-ALE
DEBUG Building: multi-agent-ale-py @ file:///home/coder/Multi-Agent-ALE
DEBUG Not using uv build backend direct build of multi-agent-ale-py @ file:///home/coder/Multi-Agent-ALE, no pyproject.toml: TOML parse error at line 1, column 1
  |
1 | [build-system]
  | ^
missing field `project`

DEBUG Assessing Python executable as base candidate: /usr/bin/python3.11
DEBUG Creating build environment for: multi-agent-ale-py @ file:///home/coder/Multi-Agent-ALE
DEBUG Locking the source tree for setuptools
DEBUG Acquired lock for `/home/coder/Multi-Agent-ALE`
DEBUG Calling `setuptools.build_meta.build_wheel("/home/coder/.cache/uv/builds-v0/.tmpJmXnU3", {}, "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/metadata_directory/multi_agent_ale_py-0.1.11.dist-info")`
DEBUG running bdist_wheel
DEBUG running build
DEBUG running build_py
DEBUG creating build/lib.linux-x86_64-cpython-311/multi_agent_ale_py
DEBUG copying multi_agent_ale_py/ale_python_interface.py -> build/lib.linux-x86_64-cpython-311/multi_agent_ale_py
DEBUG copying multi_agent_ale_py/__init__.py -> build/lib.linux-x86_64-cpython-311/multi_agent_ale_py
DEBUG running egg_info
DEBUG writing multi_agent_ale_py.egg-info/PKG-INFO
DEBUG writing dependency_links to multi_agent_ale_py.egg-info/dependency_links.txt
DEBUG writing requirements to multi_agent_ale_py.egg-info/requires.txt
DEBUG writing top-level names to multi_agent_ale_py.egg-info/top_level.txt
DEBUG reading manifest file 'multi_agent_ale_py.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG adding license file 'LICENSE.md'
DEBUG writing manifest file 'multi_agent_ale_py.egg-info/SOURCES.txt'
DEBUG /home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/command/build_py.py:212: _Warning: Package 'multi_agent_ale_py.tests.fixtures' is absent from the `packages` configuration.
DEBUG !!
DEBUG
DEBUG         ********************************************************************************
DEBUG         ############################
DEBUG         # Package would be ignored #
DEBUG         ############################
DEBUG         Python recognizes 'multi_agent_ale_py.tests.fixtures' as an importable package[^1],
DEBUG         but it is absent from setuptools' `packages` configuration.
DEBUG
DEBUG         This leads to an ambiguous overall configuration. If you want to distribute this
DEBUG         package, please make sure that 'multi_agent_ale_py.tests.fixtures' is explicitly added
DEBUG         to the `packages` configuration field.
DEBUG
DEBUG         Alternatively, you can also rely on setuptools' discovery methods
DEBUG         (for example by using `find_namespace_packages(...)`/`find_namespace:`
DEBUG         instead of `find_packages(...)`/`find:`).
DEBUG
DEBUG         You can read more about "package discovery" on setuptools documentation page:
DEBUG
DEBUG         - https://setuptools.pypa.io/en/latest/userguide/package_discovery.html
DEBUG
DEBUG         If you don't want 'multi_agent_ale_py.tests.fixtures' to be distributed and are
DEBUG         already explicitly excluding 'multi_agent_ale_py.tests.fixtures' via
DEBUG         `find_namespace_packages(...)/find_namespace` or `find_packages(...)/find`,
DEBUG         you can try to use `exclude_package_data`, or `include-package-data=False` in
DEBUG         combination with a more fine grained `package-data` configuration.
DEBUG
DEBUG         You can read more about "package data files" on setuptools documentation page:
DEBUG
DEBUG         - https://setuptools.pypa.io/en/latest/userguide/datafiles.html
DEBUG
DEBUG
DEBUG         [^1]: For Python, any directory (with suitable naming) can be imported,
DEBUG               even if it does not contain any `.py` files.
DEBUG               On the other hand, currently there is no concept of package data
DEBUG               directory, all directories are treated like packages.
DEBUG         ********************************************************************************
DEBUG
DEBUG !!
DEBUG   check.warn(importable)
DEBUG copying multi_agent_ale_py/ale_c_wrapper.cpp -> build/lib.linux-x86_64-cpython-311/multi_agent_ale_py
DEBUG copying multi_agent_ale_py/ale_c_wrapper.h -> build/lib.linux-x86_64-cpython-311/multi_agent_ale_py
DEBUG creating build/lib.linux-x86_64-cpython-311/multi_agent_ale_py/tests/fixtures
DEBUG copying multi_agent_ale_py/tests/fixtures/tetris.bin -> build/lib.linux-x86_64-cpython-311/multi_agent_ale_py/tests/fixtures
DEBUG running build_ext
DEBUG -- The CXX compiler identification is GNU 11.4.0
DEBUG -- Detecting CXX compiler ABI info
DEBUG -- Detecting CXX compiler ABI info - done
DEBUG -- Check for working CXX compiler: /usr/bin/c++ - skipped
DEBUG -- Detecting CXX compile features
DEBUG -- Detecting CXX compile features - done
DEBUG CMake Error at /home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/cmake/data/share/cmake-4.1/Modules/FindPackageHandleStandardArgs.cmake:227 (message):
DEBUG   Could NOT find ZLIB (missing: ZLIB_LIBRARY) (found version "1.2.11")
DEBUG Call Stack (most recent call first):
DEBUG   /home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/cmake/data/share/cmake-4.1/Modules/FindPackageHandleStandardArgs.cmake:591 (_FPHSA_FAILURE_MESSAGE)
DEBUG   /home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/cmake/data/share/cmake-4.1/Modules/FindZLIB.cmake:229 (find_package_handle_standard_args)
DEBUG   src/CMakeLists.txt:24 (find_package)
DEBUG
DEBUG
DEBUG -- Configuring incomplete, errors occurred!
DEBUG Traceback (most recent call last):
DEBUG   File "<string>", line 11, in <module>
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/build_meta.py", line 435, in build_wheel
DEBUG     return _build(['bdist_wheel', '--dist-info-dir', str(metadata_directory)])
DEBUG            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/build_meta.py", line 423, in _build
DEBUG     return self._build_with_temp_dir(
DEBUG            ^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/build_meta.py", line 404, in _build_with_temp_dir
DEBUG     self.run_setup()
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/build_meta.py", line 317, in run_setup
DEBUG     exec(code, locals())
DEBUG   File "<string>", line 136, in <module>
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/__init__.py", line 115, in setup
DEBUG     return distutils.core.setup(**attrs)
DEBUG            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 186, in setup
DEBUG     return run_commands(dist)
DEBUG            ^^^^^^^^^^^^^^^^^^
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 202, in run_commands
DEBUG     dist.run_commands()
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 1002, in run_commands
DEBUG     self.run_command(cmd)
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/dist.py", line 1102, in run_command
DEBUG     super().run_command(command)
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 1021, in run_command
DEBUG     cmd_obj.run()
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/command/bdist_wheel.py", line 370, in run
DEBUG     self.run_command("build")
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/cmd.py", line 357, in run_command
DEBUG     self.distribution.run_command(command)
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/dist.py", line 1102, in run_command
DEBUG     super().run_command(command)
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 1021, in run_command
DEBUG     cmd_obj.run()
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/command/build.py", line 135, in run
DEBUG     self.run_command(cmd_name)
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/cmd.py", line 357, in run_command
DEBUG     self.distribution.run_command(command)
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/dist.py", line 1102, in run_command
DEBUG     super().run_command(command)
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 1021, in run_command
DEBUG     cmd_obj.run()
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/command/build_ext.py", line 96, in run
DEBUG     _build_ext.run(self)
DEBUG   File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/command/build_ext.py", line 368, in run
DEBUG     self.build_extensions()
DEBUG   File "<string>", line 68, in build_extensions
DEBUG   File "/usr/lib/python3.11/subprocess.py", line 413, in check_call
DEBUG     raise CalledProcessError(retcode, cmd)
DEBUG subprocess.CalledProcessError: Command '['cmake', '/home/coder/Multi-Agent-ALE', '-DOUTPUT_NAME=multi_agent_ale_py.libale_c', '-DCMAKE_BUILD_TYPE=Release', '-DCMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE=/home/coder/Multi-Agent-ALE/build/lib.linux-x86_64-cpython-311/multi_agent_ale_py', '-DCMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE=/home/coder/Multi-Agent-ALE/build/lib.linux-x86_64-cpython-311/multi_agent_ale_py', '-DCMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE=build/temp.linux-x86_64-cpython-311', '-DPYTHON_MODULE_EXTENSION=.so', '-DUSE_SDL=OFF', '-DUSE_RLGLUE=OFF', '-DBUILD_EXAMPLES=OFF', '-DBUILD_CPP_LIB=OFF', '-DBUILD_CLI=OFF', '-DBUILD_C_LIB=ON']' returned non-zero exit status 1.
DEBUG Released lock at `/tmp/uv-setuptools-f57b8052b04729c3.lock`
DEBUG Released lock at `/home/coder/.cache/uv/sdists-v9/path/eae9f7bbc3959eea/.lock`
  × Failed to build `multi-agent-ale-py @ file:///home/coder/Multi-Agent-ALE`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_wheel` failed (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      creating build/lib.linux-x86_64-cpython-311/multi_agent_ale_py
      copying multi_agent_ale_py/ale_python_interface.py -> build/lib.linux-x86_64-cpython-311/multi_agent_ale_py
      copying multi_agent_ale_py/__init__.py -> build/lib.linux-x86_64-cpython-311/multi_agent_ale_py
      running egg_info
      writing multi_agent_ale_py.egg-info/PKG-INFO
      writing dependency_links to multi_agent_ale_py.egg-info/dependency_links.txt
      writing requirements to multi_agent_ale_py.egg-info/requires.txt
      writing top-level names to multi_agent_ale_py.egg-info/top_level.txt
      reading manifest file 'multi_agent_ale_py.egg-info/SOURCES.txt'
      reading manifest template 'MANIFEST.in'
      adding license file 'LICENSE.md'
      writing manifest file 'multi_agent_ale_py.egg-info/SOURCES.txt'
      copying multi_agent_ale_py/ale_c_wrapper.cpp -> build/lib.linux-x86_64-cpython-311/multi_agent_ale_py
      copying multi_agent_ale_py/ale_c_wrapper.h -> build/lib.linux-x86_64-cpython-311/multi_agent_ale_py
      creating build/lib.linux-x86_64-cpython-311/multi_agent_ale_py/tests/fixtures
      copying multi_agent_ale_py/tests/fixtures/tetris.bin -> build/lib.linux-x86_64-cpython-311/multi_agent_ale_py/tests/fixtures
      running build_ext
      -- The CXX compiler identification is GNU 11.4.0
      -- Detecting CXX compiler ABI info
      -- Detecting CXX compiler ABI info - done
      -- Check for working CXX compiler: /usr/bin/c++ - skipped
      -- Detecting CXX compile features
      -- Detecting CXX compile features - done
      -- Configuring incomplete, errors occurred!

      [stderr]
      /home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/command/build_py.py:212: _Warning: Package
      'multi_agent_ale_py.tests.fixtures' is absent from the `packages` configuration.
      !!

              ********************************************************************************
              ############################
              # Package would be ignored #
              ############################
              Python recognizes 'multi_agent_ale_py.tests.fixtures' as an importable package[^1],
              but it is absent from setuptools' `packages` configuration.

              This leads to an ambiguous overall configuration. If you want to distribute this
              package, please make sure that 'multi_agent_ale_py.tests.fixtures' is explicitly added
              to the `packages` configuration field.

              Alternatively, you can also rely on setuptools' discovery methods
              (for example by using `find_namespace_packages(...)`/`find_namespace:`
              instead of `find_packages(...)`/`find:`).

              You can read more about "package discovery" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/package_discovery.html

              If you don't want 'multi_agent_ale_py.tests.fixtures' to be distributed and are
              already explicitly excluding 'multi_agent_ale_py.tests.fixtures' via
              `find_namespace_packages(...)/find_namespace` or `find_packages(...)/find`,
              you can try to use `exclude_package_data`, or `include-package-data=False` in
              combination with a more fine grained `package-data` configuration.

              You can read more about "package data files" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/datafiles.html


              [^1]: For Python, any directory (with suitable naming) can be imported,
                    even if it does not contain any `.py` files.
                    On the other hand, currently there is no concept of package data
                    directory, all directories are treated like packages.
              ********************************************************************************

      !!
        check.warn(importable)
      CMake Error at /home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/cmake/data/share/cmake-4.1/Modules/FindPackageHandleStandardArgs.cmake:227
      (message):
        Could NOT find ZLIB (missing: ZLIB_LIBRARY) (found version "1.2.11")
      Call Stack (most recent call first):
        /home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/cmake/data/share/cmake-4.1/Modules/FindPackageHandleStandardArgs.cmake:591
      (_FPHSA_FAILURE_MESSAGE)
        /home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/cmake/data/share/cmake-4.1/Modules/FindZLIB.cmake:229
      (find_package_handle_standard_args)
        src/CMakeLists.txt:24 (find_package)


      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/build_meta.py", line 435, in build_wheel
          return _build(['bdist_wheel', '--dist-info-dir', str(metadata_directory)])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/build_meta.py", line 423, in _build
          return self._build_with_temp_dir(
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/build_meta.py", line 404, in _build_with_temp_dir
          self.run_setup()
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/build_meta.py", line 317, in run_setup
          exec(code, locals())
        File "<string>", line 136, in <module>
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/__init__.py", line 115, in setup
          return distutils.core.setup(**attrs)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 186, in setup
          return run_commands(dist)
                 ^^^^^^^^^^^^^^^^^^
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 202, in run_commands
          dist.run_commands()
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 1002, in run_commands
          self.run_command(cmd)
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/dist.py", line 1102, in run_command
          super().run_command(command)
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 1021, in run_command
          cmd_obj.run()
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/command/bdist_wheel.py", line 370, in run
          self.run_command("build")
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/cmd.py", line 357, in run_command
          self.distribution.run_command(command)
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/dist.py", line 1102, in run_command
          super().run_command(command)
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 1021, in run_command
          cmd_obj.run()
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/command/build.py", line 135, in run
          self.run_command(cmd_name)
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/cmd.py", line 357, in run_command
          self.distribution.run_command(command)
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/dist.py", line 1102, in run_command
          super().run_command(command)
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 1021, in run_command
          cmd_obj.run()
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/command/build_ext.py", line 96, in run
          _build_ext.run(self)
        File "/home/coder/.cache/uv/builds-v0/.tmpZWInxH/lib/python3.11/site-packages/setuptools/_distutils/command/build_ext.py", line 368, in run
          self.build_extensions()
        File "<string>", line 68, in build_extensions
        File "/usr/lib/python3.11/subprocess.py", line 413, in check_call
          raise CalledProcessError(retcode, cmd)
      subprocess.CalledProcessError: Command '['cmake', '/home/coder/Multi-Agent-ALE', '-DOUTPUT_NAME=multi_agent_ale_py.libale_c',
      '-DCMAKE_BUILD_TYPE=Release', '-DCMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE=/home/coder/Multi-Agent-ALE/build/lib.linux-x86_64-cpython-311/multi_agent_ale_py',
      '-DCMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE=/home/coder/Multi-Agent-ALE/build/lib.linux-x86_64-cpython-311/multi_agent_ale_py',
      '-DCMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE=build/temp.linux-x86_64-cpython-311', '-DPYTHON_MODULE_EXTENSION=.so', '-DUSE_SDL=OFF', '-DUSE_RLGLUE=OFF',
      '-DBUILD_EXAMPLES=OFF', '-DBUILD_CPP_LIB=OFF', '-DBUILD_CLI=OFF', '-DBUILD_C_LIB=ON']' returned non-zero exit status 1.

      hint: This usually indicates a problem with the package or the build environment.
DEBUG Released lock at `/home/coder/Multi-Agent-ALE/.venv/.lock`

```

</details>

In both cases, cmake 4.1.0 is being used (see the detailed logs). The build error is a CMake error, in which one dependency cannot be found. However, the dependency is installed in the system (and the build using pip does find it).

```
Could NOT find ZLIB (missing: ZLIB_LIBRARY) (found version "1.2.11")
```

The same error also happens when adding `multi-agent-ale-py` as a dependency to another package.

This is reproducible on a clean AWS EC2 instance with Python 3.11. The build requires having `build-essential`, `cmake` and `zlib1g-dev` system dependencies.

## Questions

I'm unsure if this is an issue with the cmake package or uv. But given that the system pip succeeds, I'm betting on the later.

Can you reproduce this behavior on your side? What could be causing this?

### Platform

Linux 5.10.239-236.958.amzn2.x86_64 x86_64 GNU/Linux

### Version

uv 0.8.12

### Python version

Python 3.11

---

_Label `bug` added by @thomasbbrunner on 2025-08-21 13:49_

---

_Comment by @thomasbbrunner on 2025-08-21 13:54_

Potentially linked to https://github.com/astral-sh/uv/issues/14382?

---

_Comment by @thomasbbrunner on 2025-08-21 14:36_

Digging a bit further, the exact commit in the cmake repo that breaks the build is https://github.com/scikit-build/cmake-python-distributions/commit/e057480e19a26f5c08cbda97ce2ee49de83fb773, which is the update to the 4.1.0 CMake version.

---

_Comment by @zsol on 2025-08-22 15:49_

FWIW regular pip also fails for me:
```
python3.11 -m venv venv
venv/bin/python -m pip install .
```

I tried with both the python3.11 built into uv (through PBS) as well as python3.11 from the ubuntu deadsnakes ppa.

The output is the same (https://gist.github.com/zsol/683be5146ff61ba14ea763d9b88097fd - i modified `setup.py` to dump all env vars before invoking cmake)

---

_Comment by @geofft on 2025-08-22 16:12_

As a quick data point, it _does_ build with pip on my machine (Ubuntu 24.04, system package zlib1g-dev is installed, system package cmake is _not_ installed), and fails with uv. I'm working on minimizing this and figuring out what's going on, I believe I can reproduce this with a relatively small setup.py and CMakeLists.txt. 

---

_Comment by @thomasbbrunner on 2025-08-22 16:52_

One difference I notice between @zsol's repro and mine is the use of a venv with pip. In my original reproduction I did not build the package in a venv.

Maybe the venv is causing this to break? If so, that'd potentially point to cmake being the issue.

---

_Comment by @geofft on 2025-08-22 17:30_

Oh no, I found the problem. uv sets `CLICOLOR_FORCE` in the build frontend, and that's causing CMakeParseImplicitIncludeInfo.cmake to have a bad time parsing gcc's output, which is making it not find the Debian multiarch paths.

You can work around this by adding
```python
os.environ.pop("CLICOLOR_FORCE", None)
```
to the top of your setup.py somewhere. (Conversely, you can reproduce the problem with pip with `export CLICOLOR_FORCE=1`.)

If you want to see some details of what's going on, diff the file CMakeFiles/CMakeConfigureLog.yaml in a successful and unsuccessful run:
```diff
-        gmake[1]: Entering directory '/tmp/m/CMakeFiles/CMakeScratch/TryCompile-vmRC5a'
-        \x1b[0mBuilding CXX object CMakeFiles/cmTC_2c029.dir/CMakeCXXCompilerABI.cpp.o\x1b[0m
+        gmake[1]: Entering directory '/tmp/m/CMakeFiles/CMakeScratch/TryCompile-TBxxzs'
+        Building CXX object CMakeFiles/cmTC_05939.dir/CMakeCXXCompilerABI.cpp.o
 [...]
     message: |
-      Parsed CXX implicit include dir info: rv=loading
+      Parsed CXX implicit include dir info: rv=done
         found start of include info
-        warn: unable to parse implicit include dirs!
+        found start of implicit include info
+          add: [/usr/include/c++/13]
+          add: [/usr/include/x86_64-linux-gnu/c++/13]
+          add: [/usr/include/c++/13/backward]
+          add: [/usr/lib/gcc/x86_64-linux-gnu/13/include]
+          add: [/usr/local/include]
+          add: [/usr/include/x86_64-linux-gnu]
+          add: [/usr/include]
+        end of search list found
```

I am inclined to call this a CMake bug. I'm not sure exactly where the parsing failure is, but I'm slightly suspicious of this regex preprocessing because the relevant output lines seem to be the same in both cases:
```cmake
  # go through each line of output...
  string(REGEX REPLACE "\r*\n" ";" output_lines "${text}")
```
But in the meantime it might be worth uv having an option to stop passing `CLICOLOR_FORCE=1` to the build backend (currently it's unconditional in the build frontend, as far as I can tell).

---

_Comment by @zanieb on 2025-08-22 19:13_

cc @konstin I think this has come up before.

---

_Comment by @zanieb on 2025-08-22 19:13_

See also

- https://github.com/astral-sh/uv/issues/12564

---

_Closed by @zsol on 2025-08-27 15:28_

---
