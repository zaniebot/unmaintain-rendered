---
number: 11822
title: uv does not use correct Python version, fails to find wheel, tries to build package, fails
type: issue
state: open
author: Ark-kun
labels:
  - bug
assignees: []
created_at: 2025-02-27T06:48:12Z
updated_at: 2025-06-01T18:36:38Z
url: https://github.com/astral-sh/uv/issues/11822
synced_at: 2026-01-10T01:25:11Z
---

# uv does not use correct Python version, fails to find wheel, tries to build package, fails

---

_Issue opened by @Ark-kun on 2025-02-27 06:48_

### Summary

We have an internal tool that installs Python 3.10, `virtualenv`, creates virtual environment, installs `uv` and then installs `skypilot` using `uv`.

This worked for many people before, but it stopped working now.

I traced the issue to the following:

* The active Python version is `3.10`.
* But `uv` finds `3.13` somewhere.
* uv tries to install `pendulum` dependency which does not have wheel for `3.13`.
* uv tries to build `pendulum`, but fails since `maturin` cannot find `cargo`

Details:

```
ark@Mac proj % which python
/Users/ark/.pyenv/virtualenvs/proj/3.10.14/bin/python
```

```
ark@Mac proj % uv python list
cpython-3.14.0a5+freethreaded-macos-aarch64-none    <download available>
cpython-3.14.0a5-macos-aarch64-none                 <download available>
cpython-3.13.2+freethreaded-macos-aarch64-none      <download available>
cpython-3.13.2-macos-aarch64-none                   /opt/homebrew/opt/python@3.13/bin/python3.13 -> ../Frameworks/Python.framework/Versions/3.13/bin/python3.13
cpython-3.13.2-macos-aarch64-none                   <download available>
cpython-3.12.9-macos-aarch64-none                   <download available>
cpython-3.11.11-macos-aarch64-none                  /opt/homebrew/opt/python@3.11/bin/python3.11 -> ../Frameworks/Python.framework/Versions/3.11/bin/python3.11
cpython-3.11.11-macos-aarch64-none                  <download available>
cpython-3.10.16-macos-aarch64-none                  /Users/ark/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10
cpython-3.9.21-macos-aarch64-none                   <download available>
cpython-3.9.6-macos-aarch64-none                    /Library/Developer/CommandLineTools/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
cpython-3.8.20-macos-aarch64-none                   <download available>
pypy-3.11.11-macos-aarch64-none                     <download available>
pypy-3.10.19-macos-aarch64-none                     <download available>
pypy-3.9.19-macos-aarch64-none                      <download available>
pypy-3.8.16-macos-aarch64-none                      <download available>
```

```
ark@Mac proj % uv tool install "skypilot-nightly[gcp]==1.0.0.dev20240810" 
Resolved 60 packages in 226ms
  × Failed to build `pendulum==3.0.0`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `maturin.build_wheel` failed (exit status: 1)
      [stdout]
      Running `maturin pep517 build-wheel -i
      /Users/ark/.cache/uv/builds-v0/.tmpvFWEoX/bin/python --compatibility
      off`
      [stderr]
      💥 maturin failed
        Caused by: Cargo metadata failed. Do you have cargo in your PATH?
        Caused by: No such file or directory (os error 2)
      Error: command ['maturin', 'pep517', 'build-wheel', '-i',
      '/Users/ark/.cache/uv/builds-v0/.tmpvFWEoX/bin/python',
      '--compatibility', 'off'] returned non-zero exit status 1
      hint: This usually indicates a problem with the package or the build
      environment.
  help: `pendulum` (v3.0.0) was included because `skypilot-nightly`
        (v1.0.0.dev20240810) depends on `pendulum`
```

```
ark@Mac proj % uv tool install "skypilot-nightly[gcp]==1.0.0.dev20240810" --verbose
...
DEBUG Building: pendulum==3.0.0
DEBUG Using base executable for virtual environment: /opt/homebrew/opt/python@3.13/bin/python3.13
...
```

Expected result:

uv uses the active python (`/Users/ark/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10`). If uv does not like the active Python it should complain instead of stealthily trying to do something with random Python it can get somewhere. I've spent whole day debugging these issues and I think this could have been handled better (still no progress).

The `/Users/ark/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10` seems to have been installed by `uv`. `uv` should not ignore the Pythons it just installed.

### Platform

macos 14 arm64

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

### Python version

Python 3.10

---

_Label `bug` added by @Ark-kun on 2025-02-27 06:48_

---

_Comment by @wheynelau on 2025-02-27 14:20_

Just wondering, is it not possible to use `uv` to do the job of `virtualenv`? That is to use `uv venv --python=3.10` or `uv init --python=3.10`?  Although I do agree that maybe it should complain

---

_Comment by @zanieb on 2025-02-27 14:42_

What does `uv python find -v` give?

---

_Comment by @Ark-kun on 2025-03-03 17:47_

>What does uv python find -v give?

I apologize for taking your time. After many tries with the internal tool I somehow fixed my system environment. And now I can no longer reproduce the issue (I tried to remove the virtual env and start again, but things no longer break).

Now the `uv python find -v` command returns this:
```
ark@Mac proj % uv python find -v                                                             
DEBUG uv 0.6.0 (Homebrew 2025-02-14)
DEBUG Found project root: `/Users/ark/src/github.com/org/proj`
DEBUG No workspace root found, using project root
DEBUG Using Python request `==3.10.*` from `requires-python` metadata
DEBUG Searching for Python ==3.10.* in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.10.14-macos-aarch64-none` at `/Users/ark/.pyenv/virtualenvs/proj/3.10.14/bin/python3` (active virtual environment)
/Users/ark/.pyenv/virtualenvs/proj/3.10.14/bin/python3
```


When the failure was still reproducible, the verbose log contained this explanation:

```
% uv tool install "skypilot-nightly[gcp]==1.0.0.dev20240810" --verbose
DEBUG uv 0.6.3 (a0b9f22a2 2025-02-24)
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/Users/ark/.local/share/uv/python`
DEBUG Found `cpython-3.10.14-macos-aarch64-none` at `/Users/ark/.pyenv/virtualenvs/proj/3.10.14/bin/python` (first executable in the search path)
DEBUG Ignoring Python interpreter at `/Users/ark/.pyenv/virtualenvs/proj/3.10.14/bin/python`: system interpreter required
DEBUG Found `cpython-3.10.14-macos-aarch64-none` at `/Users/ark/.pyenv/virtualenvs/proj/3.10.14/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/Users/ark/.pyenv/virtualenvs/proj/3.10.14/bin/python3`: system interpreter required
DEBUG Found `cpython-3.10.14-macos-aarch64-none` at `/Users/ark/.pyenv/virtualenvs/proj/3.10.14/bin/python3.10` (search path)
DEBUG Ignoring Python interpreter at `/Users/ark/.pyenv/virtualenvs/proj/3.10.14/bin/python3.10`: system interpreter required
DEBUG Found `cpython-3.10.14-macos-aarch64-none` at `/Users/ark/.pyenv/shims/python` (search path)
DEBUG Ignoring Python interpreter at `/Users/ark/.pyenv/virtualenvs/proj/3.10.14/bin/python`: system interpreter required
DEBUG Found `cpython-3.10.14-macos-aarch64-none` at `/Users/ark/.pyenv/shims/python3` (search path)
DEBUG Ignoring Python interpreter at `/Users/ark/.pyenv/virtualenvs/proj/3.10.14/bin/python3`: system interpreter required
DEBUG Found `cpython-3.10.14-macos-aarch64-none` at `/Users/ark/.pyenv/shims/python3.10` (search path)
DEBUG Ignoring Python interpreter at `/Users/ark/.pyenv/virtualenvs/proj/3.10.14/bin/python3.10`: system interpreter required
DEBUG Found `cpython-3.13.2-macos-aarch64-none` at `/opt/homebrew/bin/python3` (search path)
DEBUG Acquired lock for `.dev/uv/tools/venvs`
DEBUG Checking for Python environment at `.dev/uv/tools/venvs/skypilot-nightly`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.2
```

And the environment state:
```
ark@Mac proj % python -c "import sys; print(sys.prefix, sys.base_prefix)"
/Users/ark/.pyenv/virtualenvs/proj/3.10.14 /Users/ark/.pyenv/versions/3.10.14
ark@Mac proj % echo $VIRTUAL_ENV                                         
/Users/ark/.pyenv/virtualenvs/proj/3.10.14
```

But now the log is:
```
ark@Mac proj % uv tool install "skypilot-nightly[gcp]==1.0.0.dev20240810" --verbose
DEBUG uv 0.6.3 (a0b9f22a2 2025-02-24)
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/Users/ark/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.10.16-macos-aarch64-none`
DEBUG Found `cpython-3.10.16-macos-aarch64-none` at `/Users/ark/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10` (managed installations)
DEBUG Acquired lock for `.dev/uv/tools/venvs`
```

I see that there is now a new `/Users/ark/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10` and uv defaults to it. If I remove it it'll probably starts failing again.


---

_Comment by @Ark-kun on 2025-03-03 17:50_

As a user I'd have liked to be notified (in non-verbose log) when the following occurs:

* uv is installed in a Python virtual environment (e.g. with python=3.10.14)
* uv decides to use Python version that's different from that environment (e.g. python=3.13)

---

_Comment by @futurewasfree on 2025-04-27 09:58_

Same thing happened to me today.
It uses different version when I run `uv build`:

```
Building source distribution...
*** scikit-build-core 0.11.1 (sdist)
Building wheel from source distribution...
*** scikit-build-core 0.11.1 using CMake 4.0.1 (wheel)
*** Configuring CMake...
loading initial cache file /tmp/tmp6086118p/build/CMakeInit.txt
-- The CXX compiler identification is GNU 11.4.0
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/g++-11 - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found Python: /home/sergei/.cache/uv/builds-v0/.tmp8eKhQi/bin/python (found suitable version "3.13.3", minimum required is "3.6") found components: Interpreter Development Development.Module Development.Embed
...
```

pyproject.toml (redacted):
```
[project]
requires-python = ">= 3.12, <3.13"

...

[build-system]
requires = ["scikit-build-core"]
build-backend = "scikit_build_core.build"
```

`uv install` correctly picks 3.12 and builds .so for python 3.12.

3.13 is coming from homebrew install:
```
cpython-3.14.0a6-linux-x86_64-gnu                 <download available>
cpython-3.14.0a6+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.13.3-linux-x86_64-gnu                   /home/linuxbrew/.linuxbrew/bin/python3.13 -> ../Cellar/python@3.13/3.13.3/bin/python3.13
cpython-3.13.3-linux-x86_64-gnu                   /home/linuxbrew/.linuxbrew/bin/python3 -> ../Cellar/python@3.13/3.13.3/bin/python3
cpython-3.13.3-linux-x86_64-gnu                   <download available>
cpython-3.13.3+freethreaded-linux-x86_64-gnu      <download available>
cpython-3.12.10-linux-x86_64-gnu                  <download available>
cpython-3.12.3-linux-x86_64-gnu                   /usr/bin/python3.12
cpython-3.12.3-linux-x86_64-gnu                   /usr/bin/python3 -> /etc/alternatives/python3
cpython-3.11.12-linux-x86_64-gnu                  /usr/bin/python3.11
cpython-3.11.12-linux-x86_64-gnu                  <download available>
cpython-3.10.17-linux-x86_64-gnu                  /usr/bin/python3.10
cpython-3.10.17-linux-x86_64-gnu                  <download available>
cpython-3.9.22-linux-x86_64-gnu                   <download available>
cpython-3.8.20-linux-x86_64-gnu                   <download available>
cpython-3.7.9-linux-x86_64-gnu                    <download available>
```

Please let me know if I could provide more debugging insights.



---

_Comment by @konstin on 2025-04-28 11:36_

Could you share `uv build -v`, especially the part of the uv logs in the beginning when uv is determining the Python interpreter?

---

_Comment by @futurewasfree on 2025-04-28 14:04_

And I've found a problem by looking at these logs, I've had `.python-version` file with `3.13` specified inside of it. 
Not sure where it came from:

```
DEBUG uv 0.6.14
DEBUG Found workspace root: `/home/sergei/Dev/libs/poisson-recon`
DEBUG Adding root workspace member: `/home/sergei/Dev/libs/poisson-recon`
DEBUG Reading Python requests from version file at `/home/sergei/Dev/.python-version`
DEBUG Searching for Python 3.13 in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/home/sergei/Dev/libs/poisson-recon/.venv/bin/python3` (virtual environment)
DEBUG Skipping interpreter at `.venv/bin/python3` from virtual environment: does not satisfy request `3.13`
DEBUG Searching for managed installations at `/home/sergei/.local/share/uv/python`
DEBUG Found `cpython-3.13.3-linux-x86_64-gnu` at `/home/linuxbrew/.linuxbrew/bin/python3.13` (first executable in the search path)
DEBUG Using request timeout of 30s
Building source distribution...
DEBUG No workspace root found, using project root
DEBUG Using base executable for virtual environment: /home/linuxbrew/.linuxbrew/opt/python@3.13/bin/python3.13
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13.3
DEBUG Adding direct dependency: scikit-build-core*
DEBUG Found stale response for: https://pypi.org/simple/scikit-build-core/
DEBUG Sending revalidation request for: https://pypi.org/simple/scikit-build-core/
DEBUG Found not-modified response for: https://pypi.org/simple/scikit-build-core/
DEBUG Searching for a compatible version of scikit-build-core (*)
DEBUG Selecting: scikit-build-core==0.11.1 [compatible] (scikit_build_core-0.11.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/41/54/79ec4bd01dc90d222c67d175c0324afb1c4998e523e96b44d67be0aafd28/scikit_build_core-0.11.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for scikit-build-core==0.11.1: packaging>=23.2
DEBUG Adding transitive dependency for scikit-build-core==0.11.1: pathspec>=0.10.1
DEBUG Found stale response for: https://pypi.org/simple/packaging/
DEBUG Sending revalidation request for: https://pypi.org/simple/packaging/
DEBUG Found stale response for: https://pypi.org/simple/pathspec/
DEBUG Sending revalidation request for: https://pypi.org/simple/pathspec/
DEBUG Found not-modified response for: https://pypi.org/simple/packaging/
DEBUG Found not-modified response for: https://pypi.org/simple/pathspec/
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Tried 3 versions: packaging 1, pathspec 1, scikit-build-core 1
DEBUG marker environment resolution took 0.076s
DEBUG Installing in pathspec==0.12.1, scikit-build-core==0.11.1, packaging==25.0 in /home/sergei/.cache/uv/builds-v0/.tmpGKIs3k
DEBUG Registry requirement already cached: pathspec==0.12.1
DEBUG Registry requirement already cached: scikit-build-core==0.11.1
DEBUG Registry requirement already cached: packaging==25.0
DEBUG Installing build requirements: pathspec==0.12.1, scikit-build-core==0.11.1, packaging==25.0
DEBUG Creating PEP 517 build environment
DEBUG Calling `scikit_build_core.build.get_requires_for_build_sdist()`
DEBUG No workspace root found, using project root
DEBUG Calling `scikit_build_core.build.build_sdist("/home/sergei/Dev/libs/poisson-recon/dist", {})`
*** scikit-build-core 0.11.1 (sdist)
Building wheel from source distribution...
DEBUG No workspace root found, using project root
DEBUG Using base executable for virtual environment: /home/linuxbrew/.linuxbrew/opt/python@3.13/bin/python3.13
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13.3
DEBUG Adding direct dependency: scikit-build-core*
DEBUG Searching for a compatible version of scikit-build-core (*)
DEBUG Selecting: scikit-build-core==0.11.1 [compatible] (scikit_build_core-0.11.1-py3-none-any.whl)
DEBUG Adding transitive dependency for scikit-build-core==0.11.1: packaging>=23.2
DEBUG Adding transitive dependency for scikit-build-core==0.11.1: pathspec>=0.10.1
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Tried 3 versions: packaging 1, pathspec 1, scikit-build-core 1
DEBUG marker environment resolution took 0.000s
DEBUG Installing in pathspec==0.12.1, scikit-build-core==0.11.1, packaging==25.0 in /home/sergei/.cache/uv/builds-v0/.tmpxrGHwG
DEBUG Registry requirement already cached: pathspec==0.12.1
DEBUG Registry requirement already cached: scikit-build-core==0.11.1
DEBUG Registry requirement already cached: packaging==25.0
DEBUG Installing build requirements: pathspec==0.12.1, scikit-build-core==0.11.1, packaging==25.0
DEBUG Creating PEP 517 build environment
DEBUG Calling `scikit_build_core.build.get_requires_for_build_wheel()`
DEBUG No workspace root found, using project root
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13.3
DEBUG Adding direct dependency: scikit-build-core*
DEBUG Adding direct dependency: ninja>=1.5
DEBUG Searching for a compatible version of scikit-build-core (*)
DEBUG Selecting: scikit-build-core==0.11.1 [compatible] (scikit_build_core-0.11.1-py3-none-any.whl)
DEBUG Adding transitive dependency for scikit-build-core==0.11.1: packaging>=23.2
DEBUG Adding transitive dependency for scikit-build-core==0.11.1: pathspec>=0.10.1
DEBUG Found stale response for: https://pypi.org/simple/ninja/
DEBUG Sending revalidation request for: https://pypi.org/simple/ninja/
DEBUG Found not-modified response for: https://pypi.org/simple/ninja/
DEBUG Searching for a compatible version of ninja (>=1.5)
DEBUG Selecting: ninja==1.11.1.4 [compatible] (ninja-1.11.1.4-py3-none-manylinux_2_12_x86_64.manylinux2010_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/eb/7a/455d2877fe6cf99886849c7f9755d897df32eaf3a0fba47b56e615f880f7/ninja-1.11.1.4-py3-none-manylinux_2_12_x86_64.manylinux2010_x86_64.whl.metadata
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Tried 4 versions: ninja 1, packaging 1, pathspec 1, scikit-build-core 1
DEBUG marker environment resolution took 0.017s
DEBUG Installing in scikit-build-core==0.11.1, packaging==25.0, ninja==1.11.1.4, pathspec==0.12.1 in /home/sergei/.cache/uv/builds-v0/.tmpxrGHwG
DEBUG Requirement already installed: scikit-build-core==0.11.1
DEBUG Requirement already installed: packaging==25.0
DEBUG Registry requirement already cached: ninja==1.11.1.4
DEBUG Requirement already installed: pathspec==0.12.1
DEBUG Installing build requirement: ninja==1.11.1.4
DEBUG Calling `scikit_build_core.build.build_wheel("/home/sergei/Dev/libs/poisson-recon/dist", {}, None)`
*** scikit-build-core 0.11.1 using CMake 4.0.1 (wheel)
*** Configuring CMake...
loading initial cache file /tmp/tmp2txvxoyg/build/CMakeInit.txt
-- The CXX compiler identification is GNU 11.4.0
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/g++-11 - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found Python: /home/sergei/.cache/uv/builds-v0/.tmpxrGHwG/bin/python (found version "3.13.3") found components: Interpreter Development Development.Module Development.Embed
```

After removal of it, it picks the right version (3.12) as in `pyproject.toml`

---

_Comment by @Kejooorek on 2025-06-01 18:36_

i've had simmilar issue and i've just deleted .python.version file and it works :)
thx!

---
