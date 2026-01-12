```yaml
number: 16594
title: uv sync can not resolve version, but uv pip install can
type: issue
state: closed
author: Jerakin
labels:
  - question
assignees: []
created_at: 2025-11-04T17:22:33Z
updated_at: 2025-11-06T15:16:03Z
url: https://github.com/astral-sh/uv/issues/16594
synced_at: 2026-01-12T16:02:34Z
```

# uv sync can not resolve version, but uv pip install can

---

_@Jerakin_

### Summary

Using `uv sync` one of our packages can't be synced, it throws the error below (it also can't be pulled down with `uv add`). However a valid version exists and can be pulled down with `uv pip install mydepen`.
```
  x No solution found when resolving dependencies for split (markers: python_full_version == '3.9.*'):
  `-> Because the requested Python version (>=3.9) does not satisfy Python>=3.10,<4 and all versions of mydepen depend on Python>=3.10,<4, we can conclude that all versions of mydepen cannot be used.
      And because your project depends on mydepen, we can conclude that your project's requirements are unsatisfiable.

      hint: The `requires-python` value (>=3.9) includes Python versions that are not supported by your dependencies (e.g., all versions of mydepen only supports >=3.10, <4). Consider using a more restrictive  
      `requires-python` value (like >=3.10, <4).
```


It's an internal package so unfortunately I can't share a reproduction case, but hopefully the log can help.
<details><summary>uv sync --verbose</summary>
<p>

```
PS D:\projects\test-project> uv sync --verbose
DEBUG uv 0.9.7 (0adb44480 2025-10-30)
DEBUG Acquired shared lock for `C:\Users\jerakin\AppData\Local\uv\cache`
DEBUG Found project root: `D:\projects\test-project`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `D:\projects\test-project`
DEBUG No Python version file found in workspace: D:\projects\test-project
DEBUG Using Python request `>=3.9` from `requires-python` metadata
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python >=3.9`
DEBUG Released lock at `C:\Users\jerakin\AppData\Local\Temp\uv-ceddaac331f3092f.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-project @ file:///D:/projects/test-project
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.9.21
DEBUG Solving with target Python version: >=3.9
DEBUG Adding direct dependency: test-project*
DEBUG Searching for a compatible version of test-project @ file:///D:/projects/test-project (*)
DEBUG Adding direct dependency: mydepen*
DEBUG Acquired lock for `C:\Users\jerakin\AppData\Local\uv\cache\simple-v18\index\e0c21cce6dd9ebe2\mydepen.lock`
DEBUG Found stale response for: https://company.com/api/pypi/pypi/simple/mydepen/
DEBUG Sending revalidation request for: https://company.com/api/pypi/pypi/simple/mydepen/
DEBUG Found modified response for: https://company.com/api/pypi/pypi/simple/mydepen/
DEBUG Released lock at `C:\Users\jerakin\AppData\Local\uv\cache\simple-v18\index\e0c21cce6dd9ebe2\mydepen.lock`
DEBUG Searching for a compatible version of mydepen (*)
DEBUG Forking Python requirement `>=3.9` on `~=3.10` for mydepen==2020.3.7 (split `python_full_version == '3.9.*'`, split `python_full_version >= '3.10'`)
DEBUG Narrowed `requires-python` bound to: >=3.9, <3.10
DEBUG Narrowed `requires-python` bound to: >=3.10
DEBUG Solving split (markers: python_full_version >= '3.10') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.10" }]), range: RequiresPythonRange(LowerBound(Included("3.10")), UpperBound(Unbounded)) })
DEBUG Searching for a compatible version of mydepen (*)
DEBUG Selecting: mydepen==2020.3.7 [compatible] (mydepen-2020.3.7-cp310-cp310-win_amd64.whl)
DEBUG Acquired lock for `C:\Users\jerakin\AppData\Local\uv\cache\wheels-v5\index\e0c21cce6dd9ebe2\mydepen\mydepen-2020.3.7-cp310-cp310-win_amd64.lock`
DEBUG Found fresh response for: https://company.com/api/pypi/pypi/mydepen/2020.3.7/mydepen-2020.3.7-cp310-cp310-win_amd64.whl
DEBUG Released lock at `C:\Users\jerakin\AppData\Local\uv\cache\wheels-v5\index\e0c21cce6dd9ebe2\mydepen\mydepen-2020.3.7-cp310-cp310-win_amd64.lock`
DEBUG Tried 2 versions: mydepen 1, test-project 1
DEBUG split `python_full_version >= '3.10'` resolution took 0.002s
DEBUG Solving split (markers: python_full_version == '3.9.*') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.9" }, VersionSpecifier { operator: LessThan, version: "3.10" }]), range: RequiresPythonRange(LowerBound(Included("3.9")), UpperBound(Excluded("3.10"))) })
DEBUG Searching for a compatible version of mydepen (*)
DEBUG No compatible version found for: Python
DEBUG Recording unit propagation conflict of Python from incompatibility of (mydepen)
DEBUG Searching for a compatible version of mydepen (<2020.3.7 | >2020.3.7)
DEBUG Recording unit propagation conflict of Python from incompatibility of (mydepen)
DEBUG Searching for a compatible version of mydepen (<2020.3.4 | >2020.3.4, <2020.3.7 | >2020.3.7)
DEBUG Recording unit propagation conflict of Python from incompatibility of (mydepen)
DEBUG Searching for a compatible version of mydepen (<2020.3.2 | >2020.3.2, <2020.3.4 | >2020.3.4, <2020.3.7 | >2020.3.7)
DEBUG Recording unit propagation conflict of Python from incompatibility of (mydepen)
DEBUG Searching for a compatible version of mydepen (<2020.3.1 | >2020.3.1, <2020.3.2 | >2020.3.2, <2020.3.4 | >2020.3.4, <2020.3.7 | >2020.3.7)
DEBUG No compatible version found for: mydepen
DEBUG Recording unit propagation conflict of mydepen from incompatibility of (test-project)
DEBUG Searching for a compatible version of test-project @ file:///D:/projects/test-project (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: test-project
  x No solution found when resolving dependencies for split (markers: python_full_version == '3.9.*'):
  `-> Because the requested Python version (>=3.9) does not satisfy Python>=3.10,<4 and all versions of mydepen depend on Python>=3.10,<4, we can conclude that all versions of mydepen cannot be used.
      And because your project depends on mydepen, we can conclude that your project's requirements are unsatisfiable.

      hint: The `requires-python` value (>=3.9) includes Python versions that are not supported by your dependencies (e.g., all versions of mydepen only supports >=3.10, <4). Consider using a more restrictive  
      `requires-python` value (like >=3.10, <4).
DEBUG Released lock at `D:\projects\test-project\.venv\.lock`
DEBUG Released lock at `C:\Users\jerakin\AppData\Local\uv\cache\.lock`
```

</p>
</details> 

These are the available versions on the registry.
<details><summary>package registry</summary>
<p>

```
mydepen-2020.3.1-cp27-cp27m-win_amd64.whl
mydepen-2020.3.1-cp310-cp310-win_amd64.whl
mydepen-2020.3.1-cp37-cp37m-win_amd64.whl
mydepen-2020.3.1-cp39-cp39-win_amd64.whl
mydepen-2020.3.2-cp27-cp27m-win_amd64.whl
mydepen-2020.3.2-cp310-cp310-win_amd64.whl
mydepen-2020.3.2-cp311-cp311-win_amd64.whl
mydepen-2020.3.2-cp37-cp37m-win_amd64.whl
mydepen-2020.3.2-cp38-cp38-win_amd64.whl
mydepen-2020.3.2-cp39-cp39-win_amd64.whl
mydepen-2020.3.4-cp310-cp310-win_amd64.whl
mydepen-2020.3.4-cp311-cp311-win_amd64.whl
mydepen-2020.3.4-cp37-cp37m-win_amd64.whl
mydepen-2020.3.4-cp38-cp38-win_amd64.whl
mydepen-2020.3.4-cp39-cp39-win_amd64.whl
mydepen-2020.3.7-cp310-cp310-win_amd64.whl
mydepen-2020.3.7-cp311-cp311-win_amd64.whl
mydepen-2020.3.7-cp37-cp37m-win_amd64.whl
mydepen-2020.3.7-cp38-cp38-win_amd64.whl
mydepen-2020.3.7-cp39-cp39-win_amd64.whl
```

</p>
</details> 

Of course the project doesn't exist so this pyproject won't work, but this is how it looks.
<details><summary>pyproject.toml</summary>
<p>

```toml
[project]
name = "test-project"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.9"
dependencies = [
    "mydepen"
]

[[tool.uv.index]]
name = "company"
url = "https://company.com/api/pypi/pypi/simple"
publish-url = "https://company.com/api/pypi/pypi"
default = true
```

</p>
</details> 

### Platform

Windows 11

### Version

v0.6.13, v0.9.7

### Python version

python 3.9.21

---

_Label `bug` added by @Jerakin on 2025-11-04 17:22_

---

_Comment by @charliermarsh on 2025-11-04 19:04_

I think this is correct? You're saying that you want to support Python 3.9 and later, but that dependency requires at least Python 3.10. So when we try to generate the lockfile (and therefore generate a resolution for all supported Python versions), we fail when trying to find a version of `mydepen` that would work for Python 3.9.

`uv pip install` would work as long as you're using Python 3.10 or later, but I assume `uv pip install --python 3.9` would also fail.

---

_Label `bug` removed by @charliermarsh on 2025-11-05 17:03_

---

_Label `question` added by @charliermarsh on 2025-11-05 17:03_

---

_Comment by @Jerakin on 2025-11-06 08:15_

The dependency doesn't require `3.10` . The package registry contains wheels for `2.7`, `3.7`, `3.8`, and `3.9` (As well as `3.10` and `3.11`).  

`uv pip install` is run in an active environment of python `3.9`

---

_Comment by @zanieb on 2025-11-06 12:51_

What is the `Requires-Python` in the `METADATA` for those wheels? Does it vary?

---

_Comment by @Jerakin on 2025-11-06 14:11_

They all seem correct.

| package | version |
| --- | --- |
| mydepen-2020.3.2-cp27-cp27m-win_amd64.whl | Requires-Python: ~=2.7
| mydepen-2020.3.2-cp37-cp37m-win_amd64.whl | Requires-Python: ~=3.7
| mydepen-2020.3.2-cp38-cp38-win_amd64.whl | Requires-Python: ~=3.8
| mydepen-2020.3.2-cp39-cp39-win_amd64.whl | Requires-Python: ~=3.9
| mydepen-2020.3.2-cp310-cp310-win_amd64.whl | Requires-Python: ~=3.10
| mydepen-2020.3.2-cp311-cp311-win_amd64.whl | Requires-Python: ~=3.11

---

_Comment by @Jerakin on 2025-11-06 14:19_

Is there a difference in how `uv sync` and `uv pip install` searches for compatible version(s)?

---

_Comment by @charliermarsh on 2025-11-06 14:34_

It's generally not recommended to vary your `Requires-Python` in that way. Python ABI compatibility is covered by the wheel tag, not the `Requires-Python`, which is intended to be the minimum compatible Python version for the source code.

uv, like many other tools, assumes that metadata is consistent within a release, and so `uv sync` (which generates a `uv.lock` and therefore must perform a universal resolution) will use the `Requires-Python` from one of the wheels and assume it applies to the entire release. See: https://docs.astral.sh/uv/reference/internals/resolver/#metadata-consistency.

`uv pip install` does differ in that it only has to resolve for a single platform, so it won't even look at wheels that aren't relevant for the current platform.

I'd recommend releasing with a consistent `Requires-Python`. If you can't re-release or re-build the dependency, you could consider providing [static metadata](https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata) as a workaround.

---

_Comment by @Jerakin on 2025-11-06 15:02_

That's awesome. Thank you so much for helping narrowing it down!

---

_Closed by @Jerakin on 2025-11-06 15:16_

---
