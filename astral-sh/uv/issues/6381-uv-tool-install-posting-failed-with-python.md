```yaml
number: 6381
title: "`uv tool install posting` failed with Python version conflict"
type: issue
state: closed
author: darrenburns
labels:
  - enhancement
  - help wanted
  - uv python
  - uv tool
assignees: []
created_at: 2024-08-21T21:49:57Z
updated_at: 2025-01-08T17:38:18Z
url: https://github.com/astral-sh/uv/issues/6381
synced_at: 2026-01-12T15:59:03Z
```

# `uv tool install posting` failed with Python version conflict

---

_@darrenburns_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Firstly, thanks for the amazing work here. This project is incredibly exciting, and I hope the whole community gets onboard.

Earlier today I installed `uv` (version 0.3.0) using the recommended approach in the docs. I ran `uvx posting` and received an error roughly along the lines of "the current Python version is 3.10 and posting requires >=3.11, so the constraint could not be satisfied".

This was surprising, as I expected `uv` to automatically download and install a suitable Python for me (I have not touched any config at all - installing `uvx posting` was the first thing I tried after installing `uv`). My understanding was the "current" Python version is irrelevant.

I then ran `uv install python 3.11` and repeated `uvx posting`, and it worked as expected.

I was using an M1 Macbook Pro at the time. I cannot reproduce this issue on a different Intel Macbook Pro.

Unfortunately I don't have access to the machine I encountered this on right now and couldn't report at the time, but can probably add more information soon if required.

---

_Comment by @charliermarsh on 2024-08-21 21:51_

Thanks Darren! This is an interesting one. We should probably find a way respect the `requires-python` of the declared dependencies here (there's a little bit of a chicken-and-egg problem here). Right now, we'd install Python if you did something like `uvx --python 3.11 posting`, or had a `.python-version` file in the directory -- but we wouldn't install Python after resolving the dependencies like that. I agree that as a user, you should expect this to work though.

---

_Comment by @zanieb on 2024-08-21 21:52_

Ah interesting. We determine the interpreter to use _before_ we resolve the package because we need the markers to perform resolution.

---

_Comment by @zanieb on 2024-08-21 22:41_

I guess we could just... download the version and try again with the known requirements instead of failing. I don't think it'd be too slow since it's a rare case.

---

_Label `enhancement` added by @zanieb on 2024-08-21 22:41_

---

_Label `uv python` added by @zanieb on 2024-08-21 22:41_

---

_Label `uv tool` added by @zanieb on 2024-08-21 22:41_

---

_Comment by @peppedilillo on 2024-09-12 21:11_

+1, this is important for me too! i encountered this problem with `uv tool install`, which I was expecting to install and manage a python instance. Ty for the great work so far!

---

_Comment by @my1e5 on 2024-10-16 09:55_

@charliermarsh 

> Right now, we'd install Python if you did something like uvx --python 3.11 posting, or had a .python-version file in the directory

I think I'm experiencing a bug with this in uv 0.4.21. For me, uvx is not respecting the `.python-version` file? I've posted a MRE in https://github.com/astral-sh/uv/issues/8206.

But it can be boiled down to the following:
```bash
$ uv init --app --package example-packaged-app --python 3.11

$ cd example-packaged-app/

$ uv run example-packaged-app
Using CPython 3.11.4 interpreter at: C:\Users\User\AppData\Local\Programs\Python\Python311\python.exe
Creating virtual environment at: .venv
   Built example-packaged-app @ file:///C:/Users/User/Projects/test/example-packaged-app
Installed 1 package in 16ms
Hello from example-packaged-app!
```
```
$ uvx -v --from  file:///C:/Users/User/Projects/test/example-packaged-app  example-packaged-app
DEBUG uv 0.4.21
DEBUG Searching for default Python interpreter in managed installations, system path, or `py` launcher
DEBUG Searching for managed installations at `C:\Users\User\AppData\Roaming\uv\data\python`
DEBUG Found managed installation `cpython-3.13.0-windows-x86_64-none`
DEBUG Found `cpython-3.13.0-windows-x86_64-none` at `C:\Users\User\AppData\Roaming\uv\data\python\cpython-3.13.0-windows-x86_64-none\python.exe` (managed installations)
DEBUG Using request timeout of 30s
DEBUG Found PEP 621 metadata for C:\Users\User\Projects\test\example-packaged-app in `pyproject.toml` (example-packaged-app)
DEBUG Acquired lock for `C:\Users\User\AppData\Roaming\uv\data\tools`
DEBUG Released lock at `C:\Users\User\AppData\Roaming\uv\data\tools\.lock`
DEBUG Caching via interpreter: `C:\Users\User\AppData\Roaming\uv\data\python\cpython-3.13.0-windows-x86_64-none\python.exe`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: example-packaged-app @ file:///C:/Users/User/Projects/test/example-packaged-app
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: example-packaged-app*
DEBUG Searching for a compatible version of example-packaged-app @ file:///C:/Users/User/Projects/test/example-packaged-app (*)
DEBUG Tried 1 versions: example-packaged-app 1
DEBUG Split specific environment resolution took 0.001s
Resolved 1 package in 4ms
DEBUG Ignoring empty directory
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: example-packaged-app @ file:///C:/Users/User/Projects/test/example-packaged-app
DEBUG Acquired lock for `C:\Users\User\AppData\Local\uv\cache\sdists-v4\path\9142387dc7aebe60`
DEBUG Building: example-packaged-app @ file:///C:/Users/User/Projects/test/example-packaged-app
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: hatchling*
DEBUG Found fresh response for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.25.0 [compatible] (hatchling-1.25.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.25.0: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.25.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.25.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.25.0: trove-classifiers*
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/pluggy/
DEBUG Found fresh response for: https://pypi.org/simple/trove-classifiers/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.10.13 [compatible] (trove_classifiers-2024.10.13-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4b/5c/65ae209acac6038f9576893e983b1abcdd6268cab7f5ab124103f4dde701/trove_classifiers-2024.10.13-py3-none-any.whl.metadata
DEBUG Tried 5 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG Split specific environment resolution took 0.007s
DEBUG Installing in hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.10.13 in C:\Users\User\AppData\Local\uv\cache\builds-v0\.tmpmfKAzV
DEBUG Requirement already cached: hatchling==1.25.0
DEBUG Requirement already cached: packaging==24.1
DEBUG Requirement already cached: pathspec==0.12.1
DEBUG Requirement already cached: pluggy==1.5.0
DEBUG Requirement already cached: trove-classifiers==2024.10.13
DEBUG Installing build requirements: hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.10.13
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_wheel()`
DEBUG Calling `hatchling.build.build_wheel("C:\\Users\\User\\AppData\\Local\\uv\\cache\\sdists-v4\\path\\9142387dc7aebe60\\w5EB4DN8W0LMM-1ydBKWg\\.tmpkXklQ7", {}, None)`
DEBUG Finished building: example-packaged-app @ file:///C:/Users/User/Projects/test/example-packaged-app
DEBUG Released lock at `C:\Users\User\AppData\Local\uv\cache\sdists-v4\path\9142387dc7aebe60\.lock`
Prepared 1 package in 1.84s
Installed 1 package in 13ms
 + example-packaged-app==0.1.0 (from file:///C:/Users/User/Projects/test/example-packaged-app)
DEBUG Running `example-packaged-app`
DEBUG Looking at `.dist-info` at: C:\Users\User\AppData\Local\uv\cache\archive-v0\h2OOAHfgH4IIxkLSk4BZW\Lib\site-packages\example_packaged_app-0.1.0.dist-info
Hello from example-packaged-app!
DEBUG Command exited with code: 0
```
Note how it uses Python 3.13.0, even though the `.python-version` file is 3.11 and a downloaded copy of 3.11 does already exist on my system. 

---

_Comment by @notatallshaw on 2024-11-25 01:04_

I assumed uv at least looked at the `pyproject.toml` or the `Requires-Python` in the wheel of the primary package it was installing to check for any restrictions, but apparently not, OP pointed me to this issue from Reddit.

I uninstalled Python 3.13 and then for a project which specifies `Requires-Python: >=3.13` it failed to install:

```
uv tool install  bagels
  × No solution found when resolving dependencies:
  ╰─▶ Because the current Python version (3.11.1) does not satisfy Python>=3.13 and all versions of bagels depend on Python>=3.13, we can
      conclude that all versions of bagels cannot be used.
      And because only the following versions of bagels are available:
          bagels==0.1.3
          bagels==0.1.4
          bagels==0.1.5
          bagels==0.1.6
          bagels==0.1.7
          bagels==0.1.8
          bagels==0.1.9
          bagels==0.1.10
          bagels==0.1.11
      and you require bagels, we can conclude that your requirements are unsatisfiable.
```

Seems like a small quality of life improvement to check at least the tool being installed (even if not all it's dependencies) and download the appropriate Python? But maybe I'm oversimplifying the work that would be required.

---

_Comment by @charliermarsh on 2024-11-25 01:08_

Yeah we should fix this.

---

_Comment by @zanieb on 2024-11-25 23:02_

@notatallshaw that's roughly the idea that was pursued in https://github.com/astral-sh/uv/pull/7827

---

_Label `help wanted` added by @zanieb on 2024-11-25 23:02_

---

_Comment by @charliermarsh on 2024-12-31 02:51_

I think the "holistic" solution is probably something like: do a solve with _no_ Python constraints, then install a version in the specified range.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-31 03:27_

---

_Closed by @charliermarsh on 2025-01-08 17:38_

---

_Closed by @charliermarsh on 2025-01-08 17:38_

---
