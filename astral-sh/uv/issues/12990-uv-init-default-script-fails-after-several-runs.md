```yaml
number: 12990
title: "`uv init` default script fails after several runs before `ModuleNotFoundError`"
type: issue
state: closed
author: grahamcracker1234
labels:
  - bug
assignees: []
created_at: 2025-04-20T16:17:50Z
updated_at: 2025-10-20T00:19:22Z
url: https://github.com/astral-sh/uv/issues/12990
synced_at: 2026-01-12T16:01:17Z
```

# `uv init` default script fails after several runs before `ModuleNotFoundError`

---

_@grahamcracker1234_

### Summary

After creating a brand new project, and running the default project script a few times, it suddenly breaks and stops working giving a `ModuleNotFoundError`. Sometimes it takes 1 run before failing, sometimes it takes 2. Each time you must delete the `.venv` and try again for it to work another time or two.


```sh
$ uv init --app --package foo  # Init new project
Initialized project `foo` at `<some-directory>/foo`
$ cd foo                       # Change to new project directory
$ uv run foo                   # Run the new project's default script
Using CPython 3.13.0 interpreter at: /Users/<user>/.pyenv/versions/3.13.3/bin/python3.13
Creating virtual environment at: .venv
      Built foo @ file://<some-directory>/foo
Installed 1 package in 1ms
Hello from foo!
$ uv run foo
      Built foo @ file://<some-directory>/foo
Uninstalled 1 package in 0.58ms
Installed 1 package in 1ms
Hello from foo!
$ uv run foo
Traceback (most recent call last):
  File "<some-directory>/foo/.venv/bin/foo", line 4, in <module>
    from foo import main
ModuleNotFoundError: No module named 'foo'
```



### Platform

Darwin 24.4.0 arm64

### Version

uv 0.6.14 (Homebrew 2025-04-09)

### Python version

Python 3.13.3

---

_Label `bug` added by @grahamcracker1234 on 2025-04-20 16:17_

---

_Comment by @zanieb on 2025-04-20 16:28_

Hm, I can't reproduce this

```
❯ uv init --app --package foo
Initialized project `foo` at `/Users/zb/workspace/uv/foo`
❯ cd foo
❯ uv run foo
Using CPython 3.13.3
Creating virtual environment at: .venv
Built foo @ file:///Users/zb/workspace/uv/foo
Installed 1 package in 1ms
Hello from foo!
❯ uv run foo
Hello from foo!
❯ uv run foo
Hello from foo!
❯ uv run foo
Hello from foo!
❯ uv run foo
Hello from foo!
❯ uv run foo
Hello from foo!
❯ uv run foo
Hello from foo!
❯ uv run foo
Hello from foo!
```

Can you share verbose logs for the failure?

---

_Comment by @zanieb on 2025-04-20 16:28_

Why is your package being reinstalled on each invocation?

```
      Built foo @ file://<some-directory>/foo
Uninstalled 1 package in 0.58ms
Installed 1 package in 1ms
```

I can force that, but still no error

```
❯ uv run --reinstall-package foo --refresh-package foo foo
      Built foo @ file:///Users/zb/workspace/uv/foo
Uninstalled 1 package in 0.60ms
Installed 1 package in 0.93ms
Hello from foo!
❯ uv run --reinstall-package foo --refresh-package foo foo
      Built foo @ file:///Users/zb/workspace/uv/foo
Uninstalled 1 package in 0.50ms
Installed 1 package in 1ms
Hello from foo!
❯ uv run --reinstall-package foo --refresh-package foo foo
      Built foo @ file:///Users/zb/workspace/uv/foo
Uninstalled 1 package in 0.54ms
Installed 1 package in 1ms
Hello from foo!
❯ uv run --reinstall-package foo --refresh-package foo foo
      Built foo @ file:///Users/zb/workspace/uv/foo
Uninstalled 1 package in 0.46ms
Installed 1 package in 1ms
Hello from foo!
❯ uv run --reinstall-package foo --refresh-package foo foo
      Built foo @ file:///Users/zb/workspace/uv/foo
Uninstalled 1 package in 0.63ms
Installed 1 package in 1ms
Hello from foo!
❯ uv run --reinstall-package foo --refresh-package foo foo
      Built foo @ file:///Users/zb/workspace/uv/foo
Uninstalled 1 package in 0.52ms
Installed 1 package in 1ms
Hello from foo!
```

---

_Comment by @grahamcracker1234 on 2025-04-20 16:34_

Here are the full logs with verbose

```sh
❯ uv init --app --package foo --verbose
DEBUG uv 0.6.14 (Homebrew 2025-04-09)
DEBUG Checking for Python environment at `foo/.venv`
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/Users/<user>/.local/share/uv/python`
DEBUG Found `cpython-3.13.3-macos-aarch64-none` at `/Users/<user>/.pyenv/shims/python` (first executable in the search path)
DEBUG Writing Python versions to `/Users/<user>/Documents/foo/.python-version`
Initialized project `foo` at `/Users/<user>/Documents/foo`
❯ cd foo
❯ uv run --verbose foo
DEBUG uv 0.6.14 (Homebrew 2025-04-09)
DEBUG Found project root: `/Users/<user>/Documents/foo`
DEBUG No workspace root found, using project root
DEBUG Discovered project `foo` at: /Users/<user>/Documents/foo
DEBUG Acquired lock for `/Users/<user>/Documents/foo`
DEBUG Reading Python requests from version file at `/Users/<user>/Documents/foo/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.13 in managed installations or search path
DEBUG Searching for managed installations at `/Users/<user>/.local/share/uv/python`
DEBUG Found `cpython-3.13.3-macos-aarch64-none` at `/Users/<user>/.pyenv/shims/python3.13` (first executable in the search path)
Using CPython 3.13.3 interpreter at: /Users/<user>/.pyenv/versions/3.13.3/bin/python3.13
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: /Users/<user>/.pyenv/versions/3.13.3/bin/python3.13
DEBUG Released lock at `/var/folders/9r/d484vsys2lg49tvfcsjz4q2w0000gn/T/uv-72f2d935b5cfa49a.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: foo @ file:///Users/<user>/Documents/foo
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: foo*
DEBUG Searching for a compatible version of foo @ file:///Users/<user>/Documents/foo (*)
DEBUG Tried 1 versions: foo 1
DEBUG all marker environments resolution took 0.000s
Resolved 1 package in 0.73ms
DEBUG Using request timeout of 30s
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1745166728, tv_nsec: 38300920 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1745166720, tv_nsec: 368098974 })))}
DEBUG Identified uncached distribution: foo @ file:///Users/<user>/Documents/foo
DEBUG Acquired lock for `/Users/<user>/.cache/uv/sdists-v9/editable/f0858a3ac305a09a`
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1745166728, tv_nsec: 38300920 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1745166720, tv_nsec: 368098974 })))}
DEBUG Cached revision does not match expected cache info for: foo @ file:///Users/<user>/Documents/foo
   Building foo @ file:///Users/<user>/Documents/foo
DEBUG Building: foo @ file:///Users/<user>/Documents/foo
DEBUG No workspace root found, using project root
DEBUG Using base executable for virtual environment: /Users/<user>/.pyenv/versions/3.13.3/bin/python3.13
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13.3
DEBUG Adding direct dependency: hatchling*
DEBUG Found fresh response for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.27.0 [compatible] (hatchling-1.27.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/e7/ae38d7a6dfba0533684e0b2136817d667588ae3ec984c1a4e5df5eb88482/hatchling-1.27.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.27.0: packaging>=24.2
DEBUG Adding transitive dependency for hatchling==1.27.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.27.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.27.0: trove-classifiers*
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/pluggy/
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Found fresh response for: https://pypi.org/simple/trove-classifiers/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2025.4.11.15 [compatible] (trove_classifiers-2025.4.11.15-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/70/7d/a2271b98b833680561ab3fcd60ab682478dc4f7cc023fab24991601ac8ac/trove_classifiers-2025.4.11.15-py3-none-any.whl.metadata
DEBUG Tried 5 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG marker environment resolution took 0.001s
DEBUG Installing in pluggy==1.5.0, hatchling==1.27.0, trove-classifiers==2025.4.11.15, packaging==25.0, pathspec==0.12.1 in /Users/<user>/.cache/uv/builds-v0/.tmpMUHTMY
DEBUG Registry requirement already cached: pluggy==1.5.0
DEBUG Registry requirement already cached: hatchling==1.27.0
DEBUG Registry requirement already cached: trove-classifiers==2025.4.11.15
DEBUG Registry requirement already cached: packaging==25.0
DEBUG Registry requirement already cached: pathspec==0.12.1
DEBUG Installing build requirements: pluggy==1.5.0, hatchling==1.27.0, trove-classifiers==2025.4.11.15, packaging==25.0, pathspec==0.12.1
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_editable()`
DEBUG No workspace root found, using project root
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13.3
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: editables>=0.3, <1.dev0
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.27.0 [compatible] (hatchling-1.27.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.27.0: packaging>=24.2
DEBUG Adding transitive dependency for hatchling==1.27.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.27.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.27.0: trove-classifiers*
DEBUG Found fresh response for: https://pypi.org/simple/editables/
DEBUG Searching for a compatible version of editables (>=0.3, <1.dev0)
DEBUG Selecting: editables==0.5 [compatible] (editables-0.5-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/be/0f2f4a5e8adc114a02b63d92bf8edbfa24db6fc602fca83c885af2479e0e/editables-0.5-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2025.4.11.15 [compatible] (trove_classifiers-2025.4.11.15-py3-none-any.whl)
DEBUG Tried 6 versions: editables 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG marker environment resolution took 0.000s
DEBUG Installing in pluggy==1.5.0, hatchling==1.27.0, editables==0.5, packaging==25.0, trove-classifiers==2025.4.11.15, pathspec==0.12.1 in /Users/<user>/.cache/uv/builds-v0/.tmpMUHTMY
DEBUG Requirement already installed: pluggy==1.5.0
DEBUG Requirement already installed: hatchling==1.27.0
DEBUG Registry requirement already cached: editables==0.5
DEBUG Requirement already installed: packaging==25.0
DEBUG Requirement already installed: trove-classifiers==2025.4.11.15
DEBUG Requirement already installed: pathspec==0.12.1
DEBUG Installing build requirement: editables==0.5
DEBUG Calling `hatchling.build.build_editable("/Users/<user>/.cache/uv/builds-v0/.tmpsV419z", {}, None)`
DEBUG Finished building: foo @ file:///Users/<user>/Documents/foo
      Built foo @ file:///Users/<user>/Documents/foo
DEBUG Released lock at `/Users/<user>/.cache/uv/sdists-v9/editable/f0858a3ac305a09a/.lock`
Prepared 1 package in 179ms
Installed 1 package in 1ms
 + foo==0.1.0 (from file:///Users/<user>/Documents/foo)
DEBUG Using Python 3.13.3 interpreter at: /Users/<user>/Documents/foo/.venv/bin/python
DEBUG Running `foo`
DEBUG Spawned child 51529 in process group 51442
Hello from foo!
DEBUG Command exited with code: 0
❯ uv run --verbose foo
DEBUG uv 0.6.14 (Homebrew 2025-04-09)
DEBUG Found project root: `/Users/<user>/Documents/foo`
DEBUG No workspace root found, using project root
DEBUG Discovered project `foo` at: /Users/<user>/Documents/foo
DEBUG Acquired lock for `/Users/<user>/Documents/foo`
DEBUG Reading Python requests from version file at `/Users/<user>/Documents/foo/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.13`
DEBUG Released lock at `/var/folders/9r/d484vsys2lg49tvfcsjz4q2w0000gn/T/uv-72f2d935b5cfa49a.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: foo @ file:///Users/<user>/Documents/foo
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 0.38ms
DEBUG Using request timeout of 30s
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1745166728, tv_nsec: 38300920 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1745166720, tv_nsec: 368098974 })))}
DEBUG Requirement already installed: foo==0.1.0 (from file:///Users/<user>/Documents/foo)
Audited 1 package in 6ms
DEBUG Using Python 3.13.3 interpreter at: /Users/<user>/Documents/foo/.venv/bin/python3
DEBUG Running `foo`
DEBUG Spawned child 51643 in process group 51642
Traceback (most recent call last):
  File "/Users/<user>/Documents/foo/.venv/bin/foo", line 4, in <module>
    from foo import main
ModuleNotFoundError: No module named 'foo'
DEBUG Command exited with code: 1
```

---

_Comment by @zanieb on 2025-04-20 16:38_

And after the failure, what is the result of `ls -lah .venv/lib/python3.13/site-packages` and `cat .venv/lib/python3.13/site-packages/_foo.pth`?

---

_Comment by @grahamcracker1234 on 2025-04-20 16:39_

> Why is your package being reinstalled on each invocation?

Sometimes it gets built once, sometimes it gets built twice, but it seems as soon as it stops building that may be when the error occurs.



> And after the failure, what is the result of `ls -lah .venv/lib/python3.13/site-packages` and `cat .venv/lib/python3.13/site-packages/_foo.pth`?

```sh
❯ ls -lah .venv/lib/python3.13/site-packages
total 32
drwxr-xr-x@  3 <user>  staff    96B Apr 20 12:37 __pycache__
-rw-r--r--@  1 <user>  staff    38B Apr 20 12:37 _foo.pth
-rw-r--r--@  1 <user>  staff    18B Apr 20 12:37 _virtualenv.pth
-rw-r--r--@  1 <user>  staff   4.2K Apr 20 12:37 _virtualenv.py
drwxr-xr-x@  7 <user>  staff   224B Apr 20 12:37 .
drwxr-xr-x@  3 <user>  staff    96B Apr 20 12:37 ..
drwxr-xr-x@ 10 <user>  staff   320B Apr 20 12:37 foo-0.1.0.dist-info
❯ cat .venv/lib/python3.13/site-packages/_foo.pth
/Users/<user>/Documents/foo/src%
```

---

_Comment by @zanieb on 2025-04-20 16:39_

Oh

> DEBUG Found `cpython-3.13.3-macos-aarch64-none` at `/Users/<user>/.pyenv/shims/python` (first executable in the search path)

I suspect something weird is going on with pyenv here. That shim must be changing Python interpreters behind the scenes in a weird way?

---

_Comment by @zanieb on 2025-04-20 16:39_

I'd strongly recommend not using a pyenv shim

---

_Comment by @zanieb on 2025-04-20 16:41_

You could do like `/Users/<user>/Documents/foo/.venv/bin/python3 -c "import sys; print(sys.executable)"` to try to see what pyenv is redirecting to?

---

_Comment by @grahamcracker1234 on 2025-04-20 16:50_

> I'd strongly recommend not using a pyenv shim

I'm going to try and uninstall pyenv and see if the issue persists.



> You could do like `/Users/<user>/Documents/foo/.venv/bin/python3 -c "import sys; print(sys.executable)"` to try to see what pyenv is redirecting to?

It doesn't seem to redirect, it just gives this binary file.
```sh
/Users/<user>/Documents/foo/.venv/bin/python3
```

---

_Comment by @grahamcracker1234 on 2025-04-20 16:58_

The issue persists even after a pyenv uninstall. Here are the verbose logs

```sh
❯ uv init --app --package foo --verbose
DEBUG uv 0.6.14 (Homebrew 2025-04-09)
DEBUG Checking for Python environment at `foo/.venv`
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/Users/<user>/.local/share/uv/python`
DEBUG Found `cpython-3.13.3-macos-aarch64-none` at `/opt/homebrew/bin/python3` (first executable in the search path)
DEBUG Writing Python versions to `/Users/<user>/Documents/foo/.python-version`
Initialized project `foo` at `/Users/<user>/Documents/foo`
❯ cd foo
❯ uv run --verbose foo
DEBUG uv 0.6.14 (Homebrew 2025-04-09)
DEBUG Found project root: `/Users/<user>/Documents/foo`
DEBUG No workspace root found, using project root
DEBUG Discovered project `foo` at: /Users/<user>/Documents/foo
DEBUG Acquired lock for `/Users/<user>/Documents/foo`
DEBUG Reading Python requests from version file at `/Users/<user>/Documents/foo/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.13 in managed installations or search path
DEBUG Searching for managed installations at `/Users/<user>/.local/share/uv/python`
DEBUG Found `cpython-3.13.3-macos-aarch64-none` at `/opt/homebrew/bin/python3.13` (first executable in the search path)
Using CPython 3.13.3 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: /opt/homebrew/opt/python@3.13/bin/python3.13
DEBUG Released lock at `/var/folders/9r/d484vsys2lg49tvfcsjz4q2w0000gn/T/uv-72f2d935b5cfa49a.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: foo @ file:///Users/<user>/Documents/foo
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: foo*
DEBUG Searching for a compatible version of foo @ file:///Users/<user>/Documents/foo (*)
DEBUG Tried 1 versions: foo 1
DEBUG all marker environments resolution took 0.000s
Resolved 1 package in 7ms
DEBUG Using request timeout of 30s
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1745168159, tv_nsec: 221558457 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1745168153, tv_nsec: 980062723 })))}
DEBUG Identified uncached distribution: foo @ file:///Users/<user>/Documents/foo
DEBUG Acquired lock for `/Users/<user>/.cache/uv/sdists-v9/editable/f0858a3ac305a09a`
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1745168159, tv_nsec: 221558457 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1745168153, tv_nsec: 980062723 })))}
DEBUG Cached revision does not match expected cache info for: foo @ file:///Users/<user>/Documents/foo
   Building foo @ file:///Users/<user>/Documents/foo
DEBUG Building: foo @ file:///Users/<user>/Documents/foo
DEBUG No workspace root found, using project root
DEBUG Using base executable for virtual environment: /opt/homebrew/opt/python@3.13/bin/python3.13
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13.3
DEBUG Adding direct dependency: hatchling*
DEBUG Found stale response for: https://pypi.org/simple/hatchling/
DEBUG Sending revalidation request for: https://pypi.org/simple/hatchling/
DEBUG Found not-modified response for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.27.0 [compatible] (hatchling-1.27.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/e7/ae38d7a6dfba0533684e0b2136817d667588ae3ec984c1a4e5df5eb88482/hatchling-1.27.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.27.0: packaging>=24.2
DEBUG Adding transitive dependency for hatchling==1.27.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.27.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.27.0: trove-classifiers*
DEBUG Found stale response for: https://pypi.org/simple/packaging/
DEBUG Sending revalidation request for: https://pypi.org/simple/packaging/
DEBUG Found stale response for: https://pypi.org/simple/pluggy/
DEBUG Sending revalidation request for: https://pypi.org/simple/pluggy/
DEBUG Found stale response for: https://pypi.org/simple/pathspec/
DEBUG Sending revalidation request for: https://pypi.org/simple/pathspec/
DEBUG Found stale response for: https://pypi.org/simple/trove-classifiers/
DEBUG Sending revalidation request for: https://pypi.org/simple/trove-classifiers/
DEBUG Found not-modified response for: https://pypi.org/simple/packaging/
DEBUG Found not-modified response for: https://pypi.org/simple/pluggy/
DEBUG Found not-modified response for: https://pypi.org/simple/pathspec/
DEBUG Found not-modified response for: https://pypi.org/simple/trove-classifiers/
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/70/7d/a2271b98b833680561ab3fcd60ab682478dc4f7cc023fab24991601ac8ac/trove_classifiers-2025.4.11.15-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2025.4.11.15 [compatible] (trove_classifiers-2025.4.11.15-py3-none-any.whl)
DEBUG Tried 5 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG marker environment resolution took 0.239s
DEBUG Installing in pluggy==1.5.0, hatchling==1.27.0, trove-classifiers==2025.4.11.15, packaging==25.0, pathspec==0.12.1 in /Users/<user>/.cache/uv/builds-v0/.tmp5nkmU1
DEBUG Registry requirement already cached: pluggy==1.5.0
DEBUG Registry requirement already cached: hatchling==1.27.0
DEBUG Registry requirement already cached: trove-classifiers==2025.4.11.15
DEBUG Registry requirement already cached: packaging==25.0
DEBUG Registry requirement already cached: pathspec==0.12.1
DEBUG Installing build requirements: pluggy==1.5.0, hatchling==1.27.0, trove-classifiers==2025.4.11.15, packaging==25.0, pathspec==0.12.1
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_editable()`
DEBUG No workspace root found, using project root
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13.3
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: editables>=0.3, <1.dev0
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.27.0 [compatible] (hatchling-1.27.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.27.0: packaging>=24.2
DEBUG Adding transitive dependency for hatchling==1.27.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.27.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.27.0: trove-classifiers*
DEBUG Found stale response for: https://pypi.org/simple/editables/
DEBUG Sending revalidation request for: https://pypi.org/simple/editables/
DEBUG Found not-modified response for: https://pypi.org/simple/editables/
DEBUG Searching for a compatible version of editables (>=0.3, <1.dev0)
DEBUG Selecting: editables==0.5 [compatible] (editables-0.5-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/be/0f2f4a5e8adc114a02b63d92bf8edbfa24db6fc602fca83c885af2479e0e/editables-0.5-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2025.4.11.15 [compatible] (trove_classifiers-2025.4.11.15-py3-none-any.whl)
DEBUG Tried 6 versions: editables 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG marker environment resolution took 0.058s
DEBUG Installing in pluggy==1.5.0, hatchling==1.27.0, editables==0.5, packaging==25.0, trove-classifiers==2025.4.11.15, pathspec==0.12.1 in /Users/<user>/.cache/uv/builds-v0/.tmp5nkmU1
DEBUG Requirement already installed: pluggy==1.5.0
DEBUG Requirement already installed: hatchling==1.27.0
DEBUG Registry requirement already cached: editables==0.5
DEBUG Requirement already installed: packaging==25.0
DEBUG Requirement already installed: trove-classifiers==2025.4.11.15
DEBUG Requirement already installed: pathspec==0.12.1
DEBUG Installing build requirement: editables==0.5
DEBUG Calling `hatchling.build.build_editable("/Users/<user>/.cache/uv/builds-v0/.tmpdll5DE", {}, None)`
DEBUG Finished building: foo @ file:///Users/<user>/Documents/foo
      Built foo @ file:///Users/<user>/Documents/foo
DEBUG Released lock at `/Users/<user>/.cache/uv/sdists-v9/editable/f0858a3ac305a09a/.lock`
Prepared 1 package in 507ms
Installed 1 package in 1ms
 + foo==0.1.0 (from file:///Users/<user>/Documents/foo)
DEBUG Using Python 3.13.3 interpreter at: /Users/<user>/Documents/foo/.venv/bin/python
DEBUG Running `foo`
DEBUG Spawned child 79893 in process group 79890
Hello from foo!
DEBUG Command exited with code: 0
❯ uv run --verbose foo
DEBUG uv 0.6.14 (Homebrew 2025-04-09)
DEBUG Found project root: `/Users/<user>/Documents/foo`
DEBUG No workspace root found, using project root
DEBUG Discovered project `foo` at: /Users/<user>/Documents/foo
DEBUG Acquired lock for `/Users/<user>/Documents/foo`
DEBUG Reading Python requests from version file at `/Users/<user>/Documents/foo/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.13`
DEBUG Released lock at `/var/folders/9r/d484vsys2lg49tvfcsjz4q2w0000gn/T/uv-72f2d935b5cfa49a.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: foo @ file:///Users/<user>/Documents/foo
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 0.38ms
DEBUG Using request timeout of 30s
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1745168159, tv_nsec: 221558457 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1745168153, tv_nsec: 980062723 })))}
DEBUG Requirement already installed: foo==0.1.0 (from file:///Users/<user>/Documents/foo)
Audited 1 package in 13ms
DEBUG Using Python 3.13.3 interpreter at: /Users/<user>/Documents/foo/.venv/bin/python3
DEBUG Running `foo`
DEBUG Spawned child 80009 in process group 80008
Traceback (most recent call last):
  File "/Users/<user>/Documents/foo/.venv/bin/foo", line 4, in <module>
    from foo import main
ModuleNotFoundError: No module named 'foo'
DEBUG Command exited with code: 1
```

---

_Comment by @zanieb on 2025-04-20 17:01_

Interesting, and very weird.

What about these commands? https://github.com/astral-sh/uv/issues/12990#issuecomment-2817247448

---

_Comment by @grahamcracker1234 on 2025-04-20 17:04_

> Interesting, and very weird.
> 
> What about these commands? [#12990 (comment)](https://github.com/astral-sh/uv/issues/12990#issuecomment-2817247448)

```sh
❯ ls -lah .venv/lib/python3.13/site-packages
total 32
drwxr-xr-x@  3 <user>  staff    96B Apr 20 12:56 __pycache__
-rw-r--r--@  1 <user>  staff    38B Apr 20 12:56 _foo.pth
-rw-r--r--@  1 <user>  staff    18B Apr 20 12:56 _virtualenv.pth
-rw-r--r--@  1 <user>  staff   4.2K Apr 20 12:56 _virtualenv.py
drwxr-xr-x@  7 <user>  staff   224B Apr 20 12:56 .
drwxr-xr-x@  3 <user>  staff    96B Apr 20 12:56 ..
drwxr-xr-x@ 10 <user>  staff   320B Apr 20 12:56 foo-0.1.0.dist-info
❯ cat .venv/lib/python3.13/site-packages/_foo.pth
/Users/<user>/Documents/foo/src%
```

---

_Comment by @zanieb on 2025-04-20 17:23_

Does it also fail if you `.venv/bin/foo` without uv? Does `.venv/bin/python -c "import foo"` fail too?

Sorry, I have no clue what's going on here. 

---

_Comment by @grahamcracker1234 on 2025-04-20 17:38_

I tried with python 3.9 and it works as expected

```sh
❯ uv init --app --package foo --python 3.9 --verbose
DEBUG uv 0.6.14 (Homebrew 2025-04-09)
DEBUG Writing Python versions to `/Users/<user>/Documents/foo/.python-version`
Initialized project `foo` at `/Users/<user>/Documents/foo`
❯ cd foo
❯ uv run --verbose foo
DEBUG uv 0.6.14 (Homebrew 2025-04-09)
DEBUG Found project root: `/Users/<user>/Documents/foo`
DEBUG No workspace root found, using project root
DEBUG Discovered project `foo` at: /Users/<user>/Documents/foo
DEBUG Acquired lock for `/Users/<user>/Documents/foo`
DEBUG Reading Python requests from version file at `/Users/<user>/Documents/foo/.python-version`
DEBUG Using Python request `3.9` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.9 in managed installations or search path
DEBUG Searching for managed installations at `/Users/<user>/.local/share/uv/python`
DEBUG Found `cpython-3.13.3-macos-aarch64-none` at `/opt/homebrew/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `/opt/homebrew/opt/python@3.13/bin/python3.13` from first executable in the search path: does not satisfy request `3.9`
DEBUG Found `cpython-3.9.6-macos-aarch64-none` at `/usr/bin/python3` (search path)
Using CPython 3.9.6 interpreter at: /Library/Developer/CommandLineTools/usr/bin/python3
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: /Library/Developer/CommandLineTools/usr/bin/python3
DEBUG Released lock at `/var/folders/9r/d484vsys2lg49tvfcsjz4q2w0000gn/T/uv-72f2d935b5cfa49a.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: foo @ file:///Users/<user>/Documents/foo
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.9.6
DEBUG Solving with target Python version: >=3.9
DEBUG Adding direct dependency: foo*
DEBUG Searching for a compatible version of foo @ file:///Users/<user>/Documents/foo (*)
DEBUG Tried 1 versions: foo 1
DEBUG all marker environments resolution took 0.000s
Resolved 1 package in 0.69ms
DEBUG Using request timeout of 30s
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1745170627, tv_nsec: 899886426 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1745170621, tv_nsec: 488399028 })))}
DEBUG Identified uncached distribution: foo @ file:///Users/<user>/Documents/foo
DEBUG Acquired lock for `/Users/<user>/.cache/uv/sdists-v9/editable/f0858a3ac305a09a`
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1745170627, tv_nsec: 899886426 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1745170621, tv_nsec: 488399028 })))}
DEBUG Cached revision does not match expected cache info for: foo @ file:///Users/<user>/Documents/foo
   Building foo @ file:///Users/<user>/Documents/foo
DEBUG Building: foo @ file:///Users/<user>/Documents/foo
DEBUG No workspace root found, using project root
DEBUG Using base executable for virtual environment: /Library/Developer/CommandLineTools/usr/bin/python3
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.9.6
DEBUG Solving with target Python version: >=3.9.6
DEBUG Adding direct dependency: hatchling*
DEBUG Found fresh response for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.27.0 [compatible] (hatchling-1.27.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/e7/ae38d7a6dfba0533684e0b2136817d667588ae3ec984c1a4e5df5eb88482/hatchling-1.27.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.27.0: packaging>=24.2
DEBUG Adding transitive dependency for hatchling==1.27.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.27.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.27.0: tomli{python_full_version < '3.11'}>=1.2.2
DEBUG Adding transitive dependency for hatchling==1.27.0: trove-classifiers*
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/pluggy/
DEBUG Found fresh response for: https://pypi.org/simple/tomli/
DEBUG Found fresh response for: https://pypi.org/simple/trove-classifiers/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/70/7d/a2271b98b833680561ab3fcd60ab682478dc4f7cc023fab24991601ac8ac/trove_classifiers-2025.4.11.15-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1.2.2)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG Adding transitive dependency for tomli==2.2.1: tomli==2.2.1
DEBUG Adding transitive dependency for tomli==2.2.1: tomli{python_full_version < '3.11'}==2.2.1
DEBUG Searching for a compatible version of tomli (==2.2.1)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6e/c2/61d3e0f47e2b74ef40a68b9e6ad5984f6241a942f7cd3bbfbdbd03861ea9/tomli-2.2.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.2.1)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2025.4.11.15 [compatible] (trove_classifiers-2025.4.11.15-py3-none-any.whl)
DEBUG Tried 6 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, tomli 1, trove-classifiers 1
DEBUG marker environment resolution took 0.001s
DEBUG Installing in pluggy==1.5.0, hatchling==1.27.0, trove-classifiers==2025.4.11.15, packaging==25.0, pathspec==0.12.1, tomli==2.2.1 in /Users/<user>/.cache/uv/builds-v0/.tmpSu3njH
DEBUG Registry requirement already cached: pluggy==1.5.0
DEBUG Registry requirement already cached: hatchling==1.27.0
DEBUG Registry requirement already cached: trove-classifiers==2025.4.11.15
DEBUG Registry requirement already cached: packaging==25.0
DEBUG Registry requirement already cached: pathspec==0.12.1
DEBUG Registry requirement already cached: tomli==2.2.1
DEBUG Installing build requirements: pluggy==1.5.0, hatchling==1.27.0, trove-classifiers==2025.4.11.15, packaging==25.0, pathspec==0.12.1, tomli==2.2.1
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_editable()`
DEBUG No workspace root found, using project root
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.9.6
DEBUG Solving with target Python version: >=3.9.6
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: editables>=0.3, <1.dev0
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.27.0 [compatible] (hatchling-1.27.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.27.0: packaging>=24.2
DEBUG Adding transitive dependency for hatchling==1.27.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.27.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.27.0: tomli{python_full_version < '3.11'}>=1.2.2
DEBUG Adding transitive dependency for hatchling==1.27.0: trove-classifiers*
DEBUG Found fresh response for: https://pypi.org/simple/editables/
DEBUG Searching for a compatible version of editables (>=0.3, <1.dev0)
DEBUG Selecting: editables==0.5 [compatible] (editables-0.5-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/be/0f2f4a5e8adc114a02b63d92bf8edbfa24db6fc602fca83c885af2479e0e/editables-0.5-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of packaging (>=24.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1.2.2)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG Adding transitive dependency for tomli==2.2.1: tomli==2.2.1
DEBUG Adding transitive dependency for tomli==2.2.1: tomli{python_full_version < '3.11'}==2.2.1
DEBUG Searching for a compatible version of tomli (==2.2.1)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.2.1)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2025.4.11.15 [compatible] (trove_classifiers-2025.4.11.15-py3-none-any.whl)
DEBUG Tried 7 versions: editables 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, tomli 1, trove-classifiers 1
DEBUG marker environment resolution took 0.000s
DEBUG Installing in pluggy==1.5.0, editables==0.5, hatchling==1.27.0, packaging==25.0, trove-classifiers==2025.4.11.15, pathspec==0.12.1, tomli==2.2.1 in /Users/<user>/.cache/uv/builds-v0/.tmpSu3njH
DEBUG Requirement already installed: pluggy==1.5.0
DEBUG Registry requirement already cached: editables==0.5
DEBUG Requirement already installed: hatchling==1.27.0
DEBUG Requirement already installed: packaging==25.0
DEBUG Requirement already installed: trove-classifiers==2025.4.11.15
DEBUG Requirement already installed: pathspec==0.12.1
DEBUG Requirement already installed: tomli==2.2.1
DEBUG Installing build requirement: editables==0.5
DEBUG Calling `hatchling.build.build_editable("/Users/<user>/.cache/uv/builds-v0/.tmpS18A24", {}, None)`
DEBUG Finished building: foo @ file:///Users/<user>/Documents/foo
      Built foo @ file:///Users/<user>/Documents/foo
DEBUG Released lock at `/Users/<user>/.cache/uv/sdists-v9/editable/f0858a3ac305a09a/.lock`
Prepared 1 package in 180ms
Installed 1 package in 1ms
 + foo==0.1.0 (from file:///Users/<user>/Documents/foo)
DEBUG Using Python 3.9.6 interpreter at: /Users/<user>/Documents/foo/.venv/bin/python
DEBUG Running `foo`
DEBUG Spawned child 21759 in process group 21755
Hello from foo!
DEBUG Command exited with code: 0
❯ uv run --verbose foo
DEBUG uv 0.6.14 (Homebrew 2025-04-09)
DEBUG Found project root: `/Users/<user>/Documents/foo`
DEBUG No workspace root found, using project root
DEBUG Discovered project `foo` at: /Users/<user>/Documents/foo
DEBUG Acquired lock for `/Users/<user>/Documents/foo`
DEBUG Reading Python requests from version file at `/Users/<user>/Documents/foo/.python-version`
DEBUG Using Python request `3.9` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.9`
DEBUG Released lock at `/var/folders/9r/d484vsys2lg49tvfcsjz4q2w0000gn/T/uv-72f2d935b5cfa49a.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: foo @ file:///Users/<user>/Documents/foo
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 0.55ms
DEBUG Using request timeout of 30s
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1745170627, tv_nsec: 899886426 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1745170621, tv_nsec: 488399028 })))}
DEBUG Requirement already installed: foo==0.1.0 (from file:///Users/<user>/Documents/foo)
Audited 1 package in 10ms
DEBUG Using Python 3.9.6 interpreter at: /Users/<user>/Documents/foo/.venv/bin/python3
DEBUG Running `foo`
DEBUG Spawned child 21834 in process group 21833
Hello from foo!
DEBUG Command exited with code: 0
❯ uv run --verbose foo
DEBUG uv 0.6.14 (Homebrew 2025-04-09)
DEBUG Found project root: `/Users/<user>/Documents/foo`
DEBUG No workspace root found, using project root
DEBUG Discovered project `foo` at: /Users/<user>/Documents/foo
DEBUG Acquired lock for `/Users/<user>/Documents/foo`
DEBUG Reading Python requests from version file at `/Users/<user>/Documents/foo/.python-version`
DEBUG Using Python request `3.9` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.9`
DEBUG Released lock at `/var/folders/9r/d484vsys2lg49tvfcsjz4q2w0000gn/T/uv-72f2d935b5cfa49a.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: foo @ file:///Users/<user>/Documents/foo
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 0.51ms
DEBUG Using request timeout of 30s
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1745170627, tv_nsec: 899886426 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1745170621, tv_nsec: 488399028 })))}
DEBUG Requirement already installed: foo==0.1.0 (from file:///Users/<user>/Documents/foo)
Audited 1 package in 13ms
DEBUG Using Python 3.9.6 interpreter at: /Users/<user>/Documents/foo/.venv/bin/python3
DEBUG Running `foo`
DEBUG Spawned child 21885 in process group 21884
Hello from foo!
DEBUG Command exited with code: 0
```

---

_Comment by @grahamcracker1234 on 2025-04-20 17:41_

> Does it also fail if you `.venv/bin/foo` without uv? Does `.venv/bin/python -c "import foo"` fail too?
> 
> Sorry, I have no clue what's going on here.

Both fail and succeed whenever `uv run foo` does.

---

_Comment by @grahamcracker1234 on 2025-04-20 17:52_

Interestingly python 3.9 is the latest version that appears to work

| Status    | Python Version | Notes                  |
|-----------|----------------|------------------------|
| Fails     | 3.13.3         | on 2nd or 3rd attempt  |
| Fails     | 3.12.10        | on 2nd attempt         |
| Fails     | 3.11.12        | on 4th or 5th attempt  |
| Fails     | 3.10.17        | on 2nd attempt         |
| Succeeds  | 3.9.6          |  tried at least 20 attempts                      |

Those commands from earlier for python 3.9

```sh
❯ ls -lah .venv/lib/python3.9/site-packages
total 32
-rw-r--r--@  1 <user>  staff    38B Apr 20 13:48 _foo.pth
-rw-r--r--@  1 <user>  staff    18B Apr 20 13:48 _virtualenv.pth
-rw-r--r--@  1 <user>  staff   4.2K Apr 20 13:48 _virtualenv.py
drwxr-xr-x@  6 <user>  staff   192B Apr 20 13:49 .
drwxr-xr-x@  3 <user>  staff    96B Apr 20 13:49 ..
drwxr-xr-x@ 10 <user>  staff   320B Apr 20 13:49 foo-0.1.0.dist-info
❯ cat .venv/lib/python3.9/site-packages/_foo.pth
/Users/<user>/Documents/foo/src%
```

---

_Comment by @zanieb on 2025-04-20 18:46_

Hm, Python 3.9 is the one that comes with macOS? Are the other Python versions you're testing provided by uv? Does it work with `uv run -p 3.9 --managed-python foo` too?

---

_Comment by @zanieb on 2025-04-20 18:48_

> Both fail and succeed whenever uv run foo does.

If this is the case, then this is probably not a bug with uv. We can narrow it down a bit... e.g, if you only do `uv sync` then `.venv/bin/python -c "import foo"` repeatedly, does it still fail at some point? Or does it require a repeated `uv run` invocation to start failing?

---

_Comment by @grahamcracker1234 on 2025-04-20 20:11_

Interestingly after clearing the `.venv` if I use bash to run several commands in succession I can run hundreds before it randomly starts failing after around 200-300 runs.

```bash
for i in {1..300}; do
  echo "Run #$i"
  uv run foo
done;
```

It does in fact fail with python 3.8, but if I use your `uv run -p 3.9 --managed-python foo` command it works perfectly fine.

> > Both fail and succeed whenever uv run foo does.
> 
> If this is the case, then this is probably not a bug with uv. We can narrow it down a bit... e.g, if you only do `uv sync` then `.venv/bin/python -c "import foo"` repeatedly, does it still fail at some point? Or does it require a repeated `uv run` invocation to start failing?

If fails eventually too.

---

_Comment by @grahamcracker1234 on 2025-04-20 20:25_

There is probably a cleaner/more straightforward way to determine this, but I found a way to determine if the contents of the `.venv` change between when it succeeds and when it fails.

```sh
uv init --app --package foo  # same ol' init
cd foo                       # regular
rm .gitignore                # remove .gitignore
uv sync && \                 # creates .venv
rm .venv/.gitignore && \     # removes the .venv .gitignore
git add -A && \              # stage all current files
for i in {1..500}; do        # after ~250 runs, it fails
  echo "Run #$i"
  uv run foo
done;
```

After failing, I can see that there are unstaged changes to `.venv/lib/python3.12/site-packages/__pycache__/_virtualenv.cpython-312.pyc` that were made while the `uv run foo` loop was running. Given that uv is installing this version of python in the `.venv` it would seem weird _NOT_ to be a uv bug. But of course if I'm mistaken then any more explanation would be helpful.

---

_Comment by @grahamcracker1234 on 2025-04-20 20:28_

> There is probably a cleaner/more straightforward way to determine this, but I found a way to determine if the contents of the `.venv` change between when it succeeds and when it fails.
> 
> uv init --app --package foo  # same ol' init
> cd foo                       # regular
> rm .gitignore                # remove .gitignore
> uv sync && \                 # creates .venv
> rm .venv/.gitignore && \     # removes the .venv .gitignore
> git add -A && \              # stage all current files
> for i in {1..500}; do        # after ~250 runs, it fails
>   echo "Run #$i"
>   uv run foo
> done;
> 
> After failing, I can see that there are unstaged changes to `.venv/lib/python3.12/site-packages/__pycache__/_virtualenv.cpython-312.pyc` that were made while the `uv run foo` loop was running. Given that uv is installing this version of python in the `.venv` it would seem weird _NOT_ to be a uv bug. But of course if I'm mistaken then any more explanation would be helpful.

scratch all of that actually, that doesn't seem to impact anything when I revert those changes, it still fails

---

_Comment by @zanieb on 2025-04-20 20:49_

Yeah if this is reproducible _after_ the initial sync by invoking `python` directly I think we'll need to look outside uv for the cause of this. You could also try `python -m venv .venv` then `.venv/bin/pip install -e .` then `.venv/bin/python -c "import foo"` repeatedly to try to reproduce without involving uv at all? (Similarly, you can create the virtual environment with `uv venv` or install the package with `uv pip install -e .` to try to isolate where the problem is occurring).

The `.pyc` file doesn't seem problematic, generally. You could try `PYTHONDONTWRITEBYTECODE=1` and see if the issue reproduces to rule that out.

Also, in the vein of isolating the source of the problem, you could try a different build backend, like `uv init --build-backend uv foo` or `uv init --build-backend setuptools foo` to see if the problem is related to `hatchling`.

---

_Closed by @grahamcracker1234 on 2025-05-20 18:03_

---

_Comment by @cartertemm on 2025-10-20 00:19_

Hey there,

I recognize this is a stale issue, but after spending an hour or so on it today and finding this thread, I'd like to share what worked for me in the event that someone else ends up here and feels like pulling their hair out.

In my case, the inconsistency came from iCloud drive trying to sync my files. Somehow MacOS decided that I wanted my Desktop, Documents, and Downloads folders uploaded to iCloud. I've only recently returned to MacOS, so I can't speak to whether this is default behavior.
Anyway, after about five seconds of being able to run a script, it would stop working with "ModuleNotFound" because iCloud touched a lock file. So the issue could be as simple as the cloud taking it upon itself to sync incorrectly, especially if you've run out of space. Check System Settings -> iCloud -> Drive -> Desktop and Documents folders.

Hope this helps someone!

---
