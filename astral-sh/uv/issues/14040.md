```yaml
number: 14040
title: "uv cannot find resolution for required-environment `sys_platform == 'win32' and platform_machine == 'AMD64'`"
type: issue
state: closed
author: RazerM
labels:
  - bug
assignees: []
created_at: 2025-06-15T00:45:54Z
updated_at: 2025-07-07T11:23:45Z
url: https://github.com/astral-sh/uv/issues/14040
synced_at: 2026-01-10T03:32:45Z
```

# uv cannot find resolution for required-environment `sys_platform == 'win32' and platform_machine == 'AMD64'`

---

_Issue opened by @RazerM on 2025-06-15 00:45_

### Summary

[pywin32](https://pypi.org/project/pywin32/#files) is a package without source distributions, so I'm trying to follow the recommendation to ensure the lock covers the environments I expect.

To reproduce, run `uv init` and update the `pyproject.toml` to this:

```toml
[project]
name = "uvtestwin32"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.9"
dependencies = [
    "pywin32>=310; sys_platform == 'win32'",
]

[tool.uv]
required-environments = [
    "sys_platform == 'win32' and platform_machine == 'AMD64'",
    "sys_platform == 'win32' and platform_machine == 'x86'",
]
```

The problem seems to be the environment with `platform_machine == 'AMD64'`, because it works if that is commented out. I tested on a Windows machine that this is the right value of platform_machine on Windows running 64-bit Python.

```
  × No solution found when resolving dependencies for split (platform_machine == 'AMD64' and sys_platform == 'win32'):
  ╰─▶ Because only pywin32{sys_platform == 'win32'}<=310 is available and pywin32{sys_platform == 'win32'}==310 has no `platform_machine == 'AMD64' and sys_platform == 'win32'`-compatible wheels, we can conclude that
      pywin32{sys_platform == 'win32'}>=310 cannot be used.
      And because your project depends on pywin32{sys_platform == 'win32'}>=310, we can conclude that your project's requirements are unsatisfiable.

      hint: `pywin32` was found on https://pypi.org/simple, but not at the requested version (pywin32>310). A compatible version may be available on a subsequent index (e.g., https://pypi.org/simple). By
      default, uv will only consider versions that are published on the first index that contains a given package, to avoid dependency confusion attacks. If all indexes are equally trusted, use `--index-strategy
      unsafe-best-match` to consider all versions from all indexes, regardless of the order in which they were defined.

      hint: The resolution failed for an environment that is not the current one, consider limiting the environments with `tool.uv.environments`.
```

<details>
<summary>Verbose log</summary>

```
DEBUG uv 0.7.13 (62ed17b23 2025-06-12)
DEBUG Found workspace root: `/Users/frazer/projects/personal/uvtestwin32`
DEBUG Adding root workspace member: `/Users/frazer/projects/personal/uvtestwin32`
DEBUG Reading Python requests from version file at `/Users/frazer/projects/personal/uvtestwin32/.python-version`
DEBUG Using Python request `3.10` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.10`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: uvtestwin32 @ file:///Users/frazer/projects/personal/uvtestwin32
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.10.13
DEBUG Solving with target Python version: >=3.9
DEBUG Adding direct dependency: uvtestwin32*
DEBUG Searching for a compatible version of uvtestwin32 @ file:///Users/frazer/projects/personal/uvtestwin32 (*)
DEBUG Adding direct dependency: pywin32{sys_platform == 'win32'}>=310
DEBUG Found fresh response for: https://pypi.org/simple/pywin32/
DEBUG Searching for a compatible version of pywin32{sys_platform == 'win32'} (>=310)
DEBUG Forking on required platform `platform_machine == 'AMD64' and sys_platform == 'win32'` for pywin32==310 (split `platform_machine == 'AMD64' and sys_platform == 'win32'`, split `platform_machine != 'AMD64' or sys_platform != 'win32'`)
DEBUG Solving split (platform_machine != 'AMD64' or sys_platform != 'win32') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.9" }]), range: RequiresPythonRange(LowerBound(Included("3.9")), UpperBound(Unbounded)) })
DEBUG Searching for a compatible version of pywin32{sys_platform == 'win32'} (>=310)
DEBUG Selecting: pywin32==310 [compatible] (pywin32-310-cp310-cp310-win32.whl)
DEBUG Adding transitive dependency for pywin32==310: pywin32==310
DEBUG Adding transitive dependency for pywin32==310: pywin32{sys_platform == 'win32'}==310
DEBUG Searching for a compatible version of pywin32 (==310)
DEBUG Selecting: pywin32==310 [compatible] (pywin32-310-cp310-cp310-win32.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/95/da/a5f38fffbba2fb99aa4aa905480ac4b8e83ca486659ac8c95bce47fb5276/pywin32-310-cp310-cp310-win32.whl.metadata
DEBUG Searching for a compatible version of pywin32{sys_platform == 'win32'} (==310)
DEBUG Selecting: pywin32==310 [compatible] (pywin32-310-cp310-cp310-win32.whl)
DEBUG Tried 2 versions: pywin32 1, uvtestwin32 1
DEBUG split `platform_machine != 'AMD64' or sys_platform != 'win32'` resolution took 0.000s
DEBUG Solving split (platform_machine == 'AMD64' and sys_platform == 'win32') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.9" }]), range: RequiresPythonRange(LowerBound(Included("3.9")), UpperBound(Unbounded)) })
DEBUG Searching for a compatible version of pywin32{sys_platform == 'win32'} (>=310)
DEBUG Searching for a compatible version of pywin32{sys_platform == 'win32'} (>310)
DEBUG No compatible version found for: pywin32{sys_platform == 'win32'}
DEBUG Recording unit propagation conflict of pywin32{sys_platform == 'win32'} from incompatibility of (uvtestwin32)
DEBUG Searching for a compatible version of uvtestwin32 @ file:///Users/frazer/projects/personal/uvtestwin32 (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: uvtestwin32
  × No solution found when resolving dependencies for split (platform_machine == 'AMD64' and sys_platform == 'win32'):
  ╰─▶ Because only pywin32{sys_platform == 'win32'}<=310 is available and pywin32{sys_platform == 'win32'}==310 has no `platform_machine == 'AMD64' and sys_platform == 'win32'`-compatible wheels, we can conclude that
      pywin32{sys_platform == 'win32'}>=310 cannot be used.
      And because your project depends on pywin32{sys_platform == 'win32'}>=310, we can conclude that your project's requirements are unsatisfiable.

      hint: `pywin32` was found on https://pypi.org/simple, but not at the requested version (pywin32>310). A compatible version may be available on a subsequent index (e.g., https://pypi.org/simple). By
      default, uv will only consider versions that are published on the first index that contains a given package, to avoid dependency confusion attacks. If all indexes are equally trusted, use `--index-strategy
      unsafe-best-match` to consider all versions from all indexes, regardless of the order in which they were defined.

      hint: The resolution failed for an environment that is not the current one, consider limiting the environments with `tool.uv.environments`.
```

</details>

### Platform

macOS 15 arm64

### Version

uv 0.7.13 (62ed17b23 2025-06-12)

### Python version

3.9-3.13

---

_Label `bug` added by @RazerM on 2025-06-15 00:45_

---

_Closed by @charliermarsh on 2025-06-16 02:37_

---

_Comment by @rcghpge on 2025-07-07 10:58_

whoa. Windows is running into this as well? Running tests on FreeBSD. Is there a discussion chat for this. lets brinstorm chat


---

_Comment by @konstin on 2025-07-07 11:23_

@rcghpge please do not comment on closed issues. If you have a specific problem, please open a new issue following https://github.com/astral-sh/uv/issues/9452.

---
