```yaml
number: 11664
title: Resolution error for pywin32 with tool.uv.environments constraints
type: issue
state: closed
author: Winand
labels:
  - bug
assignees: []
created_at: 2025-02-20T12:19:21Z
updated_at: 2025-02-24T15:33:20Z
url: https://github.com/astral-sh/uv/issues/11664
synced_at: 2026-01-10T03:50:31Z
```

# Resolution error for pywin32 with tool.uv.environments constraints

---

_Issue opened by @Winand on 2025-02-20 12:19_

### Question

I cannot understand why uv fails to add _pywin32_ in this case:
```
[project]
name = "pywin32-prj"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "~=3.12.0"
dependencies = []

[tool.uv]
environments = [
    "sys_platform == 'linux' and platform_machine == 'x86_64'",
    "sys_platform == 'win32' and platform_machine == 'AMD64'",
]
```

I try to add pywin32 `uv add "pywin32; sys_platform == 'win32'"` and get the following output:
<details>
  <summary>error: Distribution `pywin32==308 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform</summary>

```
DEBUG uv 0.6.2 (6d3614eec 2025-02-19)
DEBUG Found project root: `C:\Users\User\pywin32-prj`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `C:\Users\User\pywin32-prj`
DEBUG Reading Python requests from version file at `C:\Users\User\pywin32-prj\.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.12`
DEBUG Released lock at `C:\Users\User\AppData\Local\Temp\uv-87f42d72bfbcde40.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: pywin32-prj @ file:///C:/Users/User/pywin32-prj      
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.0, <3.13
DEBUG Solving split (platform_machine == 'x86_64' and sys_platform == 'linux') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.12.0" }, VersionSpecifier { operator: LessThan, version: "3.13" }]), range: RequiresPythonRange(LowerBound(Included("3.12.0")), UpperBound(Excluded("3.13"))) })
DEBUG Adding direct dependency: pywin32-prj*
DEBUG Searching for a compatible version of pywin32-prj @ file:///C:/Users/User/pywin32-prj (*)
DEBUG Tried 1 versions: pywin32-prj 1
DEBUG split `platform_machine == 'x86_64' and sys_platform == 'linux'` resolution took 0.001s
DEBUG Solving split (platform_machine == 'AMD64' and sys_platform == 'win32') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.12.0" }, VersionSpecifier { operator: LessThan, version: "3.13" }]), range: RequiresPythonRange(LowerBound(Included("3.12.0")), UpperBound(Excluded("3.13"))) })
DEBUG Adding direct dependency: pywin32-prj*
DEBUG Searching for a compatible version of pywin32-prj @ file:///C:/Users/User/pywin32-prj (*)
DEBUG Adding direct dependency: pywin32{sys_platform == 'win32'}*
DEBUG Acquired lock for `C:\Users\User\AppData\Local\uv\cache\simple-v15\pypi\pywin32.lock`
DEBUG Found fresh response for: https://pypi.org/simple/pywin32/
DEBUG Released lock at `C:\Users\User\AppData\Local\uv\cache\simple-v15\pypi\pywin32.lock`
DEBUG Searching for a compatible version of pywin32{sys_platform == 'win32'} (*)
DEBUG Selecting: pywin32==308 [compatible] (pywin32-308-cp312-cp312-win32.whl)
DEBUG Adding transitive dependency for pywin32==308: pywin32==308
DEBUG Adding transitive dependency for pywin32==308: pywin32{sys_platform == 'win32'}==308
DEBUG Searching for a compatible version of pywin32 (==308)
DEBUG Selecting: pywin32==308 [compatible] (pywin32-308-cp312-cp312-win32.whl)
DEBUG Acquired lock for `C:\Users\User\AppData\Local\uv\cache\wheels-v4\pypi\pywin32\pywin32-308-cp312-cp312-win32.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/7c/d00d6bdd96de4344e06c4afbf218bc86b54436a94c01c71a8701f613aa56/pywin32-308-cp312-cp312-win32.whl.metadata
DEBUG Released lock at `C:\Users\User\AppData\Local\uv\cache\wheels-v4\pypi\pywin32\pywin32-308-cp312-cp312-win32.lock`
DEBUG Searching for a compatible version of pywin32{sys_platform == 'win32'} (==308)
DEBUG Selecting: pywin32==308 [compatible] (pywin32-308-cp312-cp312-win32.whl)
DEBUG Tried 2 versions: pywin32 1, pywin32-prj 1
DEBUG split `platform_machine == 'AMD64' and sys_platform == 'win32'` resolution took 0.005s
INFO Solved your requirements for 2 environments
DEBUG Distinct solution for split (platform_machine == 'x86_64' and sys_platform == 'linux') with 1 packages
DEBUG Distinct solution for split (platform_machine == 'AMD64' and sys_platform == 'win32') with 2 packages
Resolved 2 packages in 15ms
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: pywin32-prj @ file:///C:/Users/User/pywin32-prj
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched requirements for: `pywin32-prj==0.1.0`
  Requested: {Requirement { name: PackageName("pywin32"), extras: [], groups: [], marker: sys_platform == 'win32', source: Registry { specifier: 
VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "308" }]), index: None, conflict: None }, origin: None }}
  Existing: {Requirement { name: PackageName("pywin32"), extras: [], groups: [], marker: sys_platform == 'win32', source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.0, <3.13
DEBUG Solving split (python_full_version == '3.12.*' and platform_machine == 'x86_64' and sys_platform == 'linux') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.12.0" }, VersionSpecifier { operator: LessThan, version: "3.13" }]), range: RequiresPythonRange(LowerBound(Included("3.12.0")), UpperBound(Excluded("3.13"))) })
DEBUG Adding direct dependency: pywin32-prj*
DEBUG Searching for a compatible version of pywin32-prj @ file:///C:/Users/User/pywin32-prj (*)
DEBUG Tried 1 versions: pywin32-prj 1
DEBUG split `python_full_version == '3.12.*' and platform_machine == 'x86_64' and sys_platform == 'linux'` resolution took 0.001s
DEBUG Solving split (python_full_version == '3.12.*' and platform_machine == 'AMD64' and sys_platform == 'win32') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.12.0" }, VersionSpecifier { operator: LessThan, version: "3.13" }]), range: RequiresPythonRange(LowerBound(Included("3.12.0")), UpperBound(Excluded("3.13"))) })
DEBUG Adding direct dependency: pywin32-prj*
DEBUG Searching for a compatible version of pywin32-prj @ file:///C:/Users/User/pywin32-prj (*)
DEBUG Adding direct dependency: pywin32{sys_platform == 'win32'}>=308
DEBUG Searching for a compatible version of pywin32{sys_platform == 'win32'} (>=308)
DEBUG Selecting: pywin32==308 [preference] (pywin32-308-cp312-cp312-win32.whl)
DEBUG Adding transitive dependency for pywin32==308: pywin32==308
DEBUG Adding transitive dependency for pywin32==308: pywin32{sys_platform == 'win32'}==308
DEBUG Searching for a compatible version of pywin32 (==308)
DEBUG Selecting: pywin32==308 [preference] (pywin32-308-cp312-cp312-win32.whl)
DEBUG Searching for a compatible version of pywin32{sys_platform == 'win32'} (==308)
DEBUG Selecting: pywin32==308 [preference] (pywin32-308-cp312-cp312-win32.whl)
DEBUG Tried 2 versions: pywin32 1, pywin32-prj 1
DEBUG split `python_full_version == '3.12.*' and platform_machine == 'AMD64' and sys_platform == 'win32'` resolution took 0.003s
INFO Solved your requirements for 2 environments
DEBUG Distinct solution for split (python_full_version == '3.12.*' and platform_machine == 'x86_64' and sys_platform == 'linux') with 1 packages 
DEBUG Distinct solution for split (python_full_version == '3.12.*' and platform_machine == 'AMD64' and sys_platform == 'win32') with 2 packages  
DEBUG Reverting changes to `pyproject.toml`
DEBUG Removing `uv.lock`
error: Distribution `pywin32==308 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
```
</details>

It's important that the package is added successfully, if i remove platform_machine constraint:
```
[tool.uv]
environments = [
    "sys_platform == 'linux' and platform_machine == 'x86_64'",
    "sys_platform == 'win32'",
]
```
or if I add the package without `sys_platform` constraint: `uv add pywin32` (though it's not possible as pywin32 is a transitive dependency from locust package in my real project).

Is it a bug?

---
And an additional question.
I want to limit supported environments to 64-bit versions of Linux and Windows so my lock file is smaller.
Also I don't want uv to deal with wheels for MacOS. But when I tried to run `uv sync` against my private (buggy and very slow) PyPI mirror on GitLab, I've noticed that it still tries to access macos wheels:
```
$ uv sync --default-index $PYPI_MIRROR/simple
Using CPython 3.12.9 interpreter at: /usr/local/bin/python3
Creating virtual environment at: .venv
error: Failed to fetch: `http://my.pypi.mirror/packages/scikit-learn/1.4.2/scikit_learn-1.4.2-cp312-cp312-macosx_10_9_x86_64.whl#sha256=90378e1747949f90c8f385898fff35d73193dfcaec3dd75d6b542f90c4e89755`
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (http://my.pypi.mirror/packages/scikit-learn/1.4.2/scikit_learn-1.4.2-cp312-cp312-macosx_10_9_x86_64.whl#sha256=90378e1747949f90c8f385898fff35d73193dfcaec3dd75d6b542f90c4e89755)
  Caused by: operation timed out
```
Is it supposed to access those wheels even with `tool.uv.environments` set to Linux and Windows?

### Platform

Windows 10 x64

### Version

uv 0.6.2

---

_Label `question` added by @Winand on 2025-02-20 12:19_

---

_Renamed from "Resolution error when environments are limited" to "Resolution error for pywin32 with tool.uv.environments constraints" by @Winand on 2025-02-20 12:46_

---

_Comment by @zanieb on 2025-02-20 14:16_

Thanks for all the details.

You might want to use `required-environments`? https://docs.astral.sh/uv/concepts/resolution/#required-environments

It's intended to address cases like "can't be installed because it doesn't have a source distribution or wheel for the current platform"

> But when I tried to run uv sync against my private (buggy and very slow) PyPI mirror on GitLab, I've noticed that it still tries to access macos wheels:

We might read macOS wheels for metadata, even if they won't become part of the lockfile, because we assume metadata is constant across all wheels for a package.

---

_Comment by @Winand on 2025-02-20 14:40_

@zanieb I still don't get it.

Now I've set the following rules:
```
[tool.uv]
environments = [
    "sys_platform == 'linux' and platform_machine == 'x86_64'",
    "sys_platform == 'win32' and platform_machine == 'AMD64'",
]
required-environments = [
    "sys_platform == 'linux' and platform_machine == 'x86_64'",
    "sys_platform == 'win32' and platform_machine == 'AMD64'",
]
```
Then I've tried to add pywin32==308 `uv add "pywin32==308; sys_platform == 'win32'"` and got the following error:
<details>
  <summary>pywin32{sys_platform == 'win32'}==308 has no `platform_machine == 'AMD64' and sys_platform == 'win32'`-compatible wheels</summary>

```
uv add "pywin32==308; sys_platform == 'win32'"
DEBUG uv 0.6.2 (6d3614eec 2025-02-19)
DEBUG Found project root: `C:\Users\User\pywin32-prj`
DEBUG No workspace root found, using project root
TRACE Checking lock for `C:\Users\User\pywin32-prj` at `C:\Users\User\AppData\Local\Temp\uv-87f42d72bfbcde40.lock`
DEBUG Acquired lock for `C:\Users\User\pywin32-prj`
DEBUG Reading Python requests from version file at `C:\Users\User\pywin32-prj\.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
TRACE Cached interpreter info for Python 3.12.7, skipping probing: .venv\Scripts\python.exe
DEBUG The virtual environment's Python version satisfies `3.12`
DEBUG Released lock at `C:\Users\User\AppData\Local\Temp\uv-87f42d72bfbcde40.lock`
DEBUG Using request timeout of 30s
DEBUG Ignoring existing lockfile due to change in supported environments: `[]` vs. `[platform_machine == 'x86_64' and sys_platform == 'linux', platform_machine == 'AMD64' and sys_platform == 'win32']`
DEBUG Found static `pyproject.toml` for: pywin32-prj @ file:///C:/Users/User/pywin32-prj
DEBUG No workspace root found, using project root
TRACE Performing lookahead for pywin32-prj @ file:///C:/Users/User/pywin32-prj
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.0, <3.13
DEBUG Solving split (platform_machine == 'x86_64' and sys_platform == 'linux') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.12.0" }, VersionSpecifier { operator: LessThan, version: "3.13" }]), range: RequiresPythonRange(LowerBound(Included("3.12.0")), UpperBound(Excluded("3.13"))) })
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: pywin32-prj*
INFO add_decision: Id::<PubGrubPackage>(0) @ 0a0.dev0 without checking dependencies
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: pywin32-prj. remaining choices:
DEBUG Searching for a compatible version of pywin32-prj @ file:///C:/Users/User/pywin32-prj (*)
TRACE Skipping pywin32==308 ; sys_platform == 'win32' because of split `platform_machine == 'x86_64' and sys_platform == 'linux'`
INFO add_decision: Id::<PubGrubPackage>(1) @ 0.1.0 without checking dependencies
TRACE Assigned packages: root==0a0.dev0, pywin32-prj==0.1.0
DEBUG Tried 1 versions: pywin32-prj 1
DEBUG split `platform_machine == 'x86_64' and sys_platform == 'linux'` resolution took 0.003s
DEBUG Solving split (platform_machine == 'AMD64' and sys_platform == 'win32') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.12.0" }, VersionSpecifier { operator: LessThan, version: "3.13" }]), range: RequiresPythonRange(LowerBound(Included("3.12.0")), UpperBound(Excluded("3.13"))) })
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: pywin32-prj*
INFO add_decision: Id::<PubGrubPackage>(0) @ 0a0.dev0 without checking dependencies
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: pywin32-prj. remaining choices:
DEBUG Searching for a compatible version of pywin32-prj @ file:///C:/Users/User/pywin32-prj (*)
DEBUG Adding direct dependency: pywin32{sys_platform == 'win32'}>=308, <308+
TRACE Fetching metadata for pywin32 from https://pypi.org/simple/pywin32/
INFO add_decision: Id::<PubGrubPackage>(1) @ 0.1.0 without checking dependencies
TRACE Assigned packages: root==0a0.dev0, pywin32-prj==0.1.0
TRACE Checking lock for `C:\Users\User\AppData\Local\uv\cache\simple-v15\pypi\pywin32.lock` at `C:\Users\User\AppData\Local\uv\cache\simple-v15\pypi\pywin32.lock`
TRACE Chose package for decision: pywin32{sys_platform == 'win32'}. remaining choices:
DEBUG Acquired lock for `C:\Users\User\AppData\Local\uv\cache\simple-v15\pypi\pywin32.lock`
TRACE Cached request https://pypi.org/simple/pywin32/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/pywin32/
DEBUG Released lock at `C:\Users\User\AppData\Local\uv\cache\simple-v15\pypi\pywin32.lock`
TRACE Received package metadata for: pywin32
DEBUG Searching for a compatible version of pywin32{sys_platform == 'win32'} (>=308, <308+)
TRACE Selecting candidate for pywin32 with range >=308, <308+ with 16 remote versions
TRACE Found candidate for package pywin32 with range >=308, <308+ after 1 steps: 308 version
TRACE Returning candidate for package pywin32 with range >=308, <308+ after 1 steps
TRACE Assigned packages: root==0a0.dev0, pywin32-prj==0.1.0
TRACE Chose package for decision: pywin32{sys_platform == 'win32'}. remaining choices:
DEBUG Searching for a compatible version of pywin32{sys_platform == 'win32'} (>308, <308+)
TRACE Selecting candidate for pywin32 with range >308, <308+ with 16 remote versions
TRACE Exhausted all candidates for package pywin32 with range >308, <308+ after 0 steps
DEBUG No compatible version found for: pywin32{sys_platform == 'win32'}
INFO Start conflict resolution because incompat satisfied:
   pywin32{sys_platform == 'win32'} >308, <308+ is forbidden
INFO prior cause: pywin32{sys_platform == 'win32'} >=308, <308+ is forbidden
INFO prior cause: pywin32-prj ==0.1.0 is forbidden
INFO backtrack to DecisionLevel(1)
DEBUG Recording unit propagation conflict of pywin32{sys_platform == 'win32'} from incompatibility of (pywin32-prj)
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: pywin32-prj. remaining choices:
DEBUG Searching for a compatible version of pywin32-prj @ file:///C:/Users/User/pywin32-prj (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: pywin32-prj
INFO Start conflict resolution because incompat satisfied:
   pywin32-prj <0.1.0 | >0.1.0 is forbidden
INFO prior cause: pywin32-prj * is forbidden
INFO prior cause: root ==0a0.dev0 is forbidden
DEBUG Reverting changes to `pyproject.toml`
DEBUG Reverting changes to `uv.lock`
  × No solution found when resolving dependencies for split (platform_machine == 'AMD64' and sys_platform == 'win32'):
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on pywin32-prj*
  term pywin32-prj*
    term pywin32-prj==0.1.0
      pywin32-prj==0.1.0 depends on pywin32{sys_platform == 'win32'}==308
      pywin32{sys_platform == 'win32'}==308 no `platform_machine == 'AMD64' and sys_platform == 'win32'`-compatible wheels
    no versions of pywin32-prj<0.1.0 | >0.1.0
TRACE Resolver derivation tree after reduction
term pywin32-prj==0.1.0
  pywin32-prj==0.1.0 depends on pywin32{sys_platform == 'win32'}==308
  pywin32{sys_platform == 'win32'}==308 no `platform_machine == 'AMD64' and sys_platform == 'win32'`-compatible wheels
  ╰─▶ Because pywin32{sys_platform == 'win32'}==308 has no `platform_machine == 'AMD64' and sys_platform == 'win32'`-compatible wheels and your
      project depends on pywin32{sys_platform == 'win32'}==308, we can conclude that your project's requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```
</details>

What does it mean? There's `pywin32-308-cp312-cp312-win_amd64.whl` wheel in PyPI obviously.

---

_Comment by @zanieb on 2025-02-20 15:22_

Indeed, that's surprising. There might be a bug here?

It could also be `win` vs `win32`?

This fails

```
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.0"
dependencies = ["pywin32==308; sys_platform == 'win32'"]


[tool.uv]
required-environments = [
    "sys_platform == 'linux' and platform_machine == 'x86_64'",
    "sys_platform == 'win32' and platform_machine == 'AMD64'",
]
```

but this seems to work?

```
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.0"
dependencies = ["pywin32==308; sys_platform == 'win32'"]


[tool.uv]
required-environments = [
    "sys_platform == 'linux' and platform_machine == 'x86_64'",
    "sys_platform == 'win' and platform_machine == 'AMD64'",
]
```

Which is confusing to me.

@charliermarsh may know what's going on.

---

_Comment by @charliermarsh on 2025-02-20 16:39_

I can take a look later today.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-20 16:39_

---

_Comment by @charliermarsh on 2025-02-20 22:10_

I think the issue is that you're using `"sys_platform == 'win32' and platform_machine == 'AMD64'"` when we expect `"sys_platform == 'win32' and platform_machine == 'amd64'"`.

If you change it, the lockfile includes the wheel you want:

```toml
[[package]]
name = "pywin32"
version = "308"
source = { registry = "https://pypi.org/simple" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/21/27/0c8811fbc3ca188f93b5354e7c286eb91f80a53afa4e11007ef661afa746/pywin32-308-cp312-cp312-win_amd64.whl", hash = "sha256:00b3e11ef09ede56c6a43c71f2d31857cf7c54b0ab6e78ac659497abd2834f47", size = 6543015 },
]
```

---

_Comment by @charliermarsh on 2025-02-20 22:11_

I think this is a bug on our end, though.

---

_Label `question` removed by @charliermarsh on 2025-02-20 22:13_

---

_Label `bug` added by @charliermarsh on 2025-02-20 22:13_

---

_Closed by @charliermarsh on 2025-02-20 22:27_

---

_Closed by @charliermarsh on 2025-02-20 22:27_

---

_Comment by @Winand on 2025-02-21 06:42_

> I think the issue is that you're using `"sys_platform == 'win32' and platform_machine == 'AMD64'"` when we expect `"sys_platform == 'win32' and platform_machine == 'amd64'"`.

@charliermarsh i've changed _platform_machine_ to lowercase "amd64", but that didn't work:
<details>
<summary>error: The current Python platform is not compatible with the lockfile's supported environments: `platform_machine == 'x86_64' and sys_platform == 'linux'`, `platform_machine == 'amd64' and sys_platform == 'win32'`</summary>

```
DEBUG Searching for a compatible version of pywin32{sys_platform == 'win32'} (==308)
DEBUG Selecting: pywin32==308 [compatible] (pywin32-308-cp312-cp312-win32.whl)
DEBUG Tried 2 versions: pywin32 1, pywin32-prj 1
DEBUG split `platform_machine == 'amd64' and sys_platform == 'win32'` resolution took 0.006s
INFO Solved your requirements for 2 environments
DEBUG Distinct solution for split (platform_machine == 'x86_64' and sys_platform == 'linux') with 1 packages
DEBUG Distinct solution for split (platform_machine == 'amd64' and sys_platform == 'win32') with 2 packages
Resolved 2 packages in 16ms
DEBUG Reverting changes to `pyproject.toml`
DEBUG Removing `uv.lock`
error: The current Python platform is not compatible with the lockfile's supported environments: `platform_machine == 'x86_64' and sys_platform == 'linux'`, `platform_machine == 'amd64' and sys_platform == 'win32'`
```
</details>

Though this configuration seems to fix the issue in 0.6.2:
```
[tool.uv]
environments = [
    "sys_platform == 'linux' and platform_machine == 'x86_64'",
    "sys_platform == 'win32' and (platform_machine == 'amd64' or platform_machine == 'AMD64')",
]
```
---
Also I'd like to mention that specifying `platform_machine == 'AMD64'"` (uppercase) breaks only _pywin32_ for me.

In another project I don't have any issues with that marker:
```
dependencies = [
    "fastapi>=0.115.5",
    "gunicorn>=22.0.0",
    "numpy>=1.26.1",
    "pandas>=2.1.1",
    "prometheus-client>=0.21.0",
    "pyyaml>=6.0.1",
    "uvicorn>=0.23.2",
]

[dependency-groups]
dev = [
    "httpx>=0.26.0",
    "pip-licenses>=4.3.3",
    "pytest-cov>=5.0.0",
    "pytest>=7.4.3",
]
build = [
    "editables>=0.5",
    "hatchling>=1.26.3",
]

[tool.uv]
environments = [
    "sys_platform == 'linux' and platform_machine == 'x86_64'",
    "sys_platform == 'win32' and platform_machine == 'AMD64'",
]
```

---

_Comment by @Winand on 2025-02-21 17:05_

I've built _uv_ 88aa6e2 and can confirm that now `platform_machine == 'AMD64'` is handled correctly when processing _pywin32_.

But
>we expect "sys_platform == 'win32' and platform_machine == 'amd64'"

No, with lowercase 'amd64' it still doesn't work. Though case-insensitive markers could be a handy feature.

---

_Comment by @charliermarsh on 2025-02-21 17:14_

I don’t expect it to work with the lowercase version, hence the bug fix. I was explaining _why_ we were handling it incorrectly.

---

_Comment by @zanieb on 2025-02-24 15:33_

Thanks for fixing it Charlie :)

---
