```yaml
number: 13315
title: uv sync incorrectly determining dependencies
type: issue
state: closed
author: NeilGirdhar
labels:
  - bug
assignees: []
created_at: 2025-05-06T11:44:33Z
updated_at: 2025-07-08T05:04:49Z
url: https://github.com/astral-sh/uv/issues/13315
synced_at: 2026-01-10T03:32:45Z
```

# uv sync incorrectly determining dependencies

---

_Issue opened by @NeilGirdhar on 2025-05-06 11:44_

### Summary

Uv seems to be incorrectly determining dependencies.

### Minimum working example

With a `pyproject.toml`:
```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "blah"
version = "1.0"
requires-python = ">=3.13, <3.14"
dependencies = ["ray[tune]>=2.45.0"]
```
Add a subdirectory called `blah` with an empty `__init__.py`

Then:
```
❯ uv lock -U
Resolved 28 packages in 117ms
❯ uv sync
Resolved 28 packages in 1ms
  × Failed to build `pyarrow==17.0.0`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_wheel` failed (exit status: 1)

...
      CMake Error at CMakeLists.txt:266 (find_package):
        By not providing "FindArrow.cmake" in CMAKE_MODULE_PATH this project has
        asked CMake to find a package configuration file provided by "Arrow", but
        CMake did not find one.

        Could not find a package configuration file provided by "Arrow" with any of
        the following names:

          ArrowConfig.cmake
          arrow-config.cmake

        Add the installation prefix of "Arrow" to CMAKE_PREFIX_PATH or set
        "Arrow_DIR" to a directory containing one of the above files.  If "Arrow"
        provides a separate development package or SDK, be sure it has been
        installed.

      
      error: command '/usr/bin/cmake' failed with exit code 1

      hint: This usually indicates a problem with the package or the build environment.
  help: `pyarrow` (v17.0.0) was included because `blah` (v1.0) depends on `ray[tune]` (v2.45.0) which depends on `pyarrow`
```
But why do we have pyarrow 17 required?  Ray only depends on pyarrow <=17 for the Darwin platform as per its `setup.py`:
```py
    pyarrow_deps = [
        "pyarrow >= 9.0.0",
        "pyarrow <18; sys_platform == 'darwin' and platform_machine == 'x86_64'",
    ]
```
Since I'm on Ubuntu, I expect it to install pyarrow 20, which uv has no problem installing (`uv pip install pyarrow==20`).

### Platform

Ubuntu 25.04

### Version

0.6.17

### Python version

3.13

---

_Label `bug` added by @NeilGirdhar on 2025-05-06 11:44_

---

_Comment by @charliermarsh on 2025-05-06 12:30_

We typically try to minimize the number of selected versions for a given package. So we'd attempt to use `pyarrow` 17 here in the resolver. You can add a constraint to force uv to select the higher version on non-macOS.

---

_Comment by @NeilGirdhar on 2025-05-06 12:35_

@charliermarsh I tried that, but it still didn't work:
```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "blah"
version = "1.0"
requires-python = ">=3.13, <3.14"
dependencies = [
    "ray[tune]>=2.45.0",
    "pyarrow>=19.0",
]
```
Gives:
```
❯ uv lock -U
  × No solution found when resolving dependencies for split (platform_machine == 'x86_64' and sys_platform == 'darwin'):
```
I tried to figure out how to block darwin in the pyproject, but there doesn't seem to be a way to do that.

---

_Comment by @charliermarsh on 2025-05-06 12:52_

Yeah that correctly doesn't work, because now on Intel macOS you're asking for `pyarrow>=19` and `pyarrow<18`.

I think you want this:

```
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "blah"
version = "1.0"
requires-python = ">=3.13, <3.14"
dependencies = [
    "ray[tune]>=2.45.0",
]

[tool.uv]
constraint-dependencies = [
    "pyarrow>=19.0 ; platform_machine != 'x86_64' or sys_platform != 'darwin'"
]
```

---

_Comment by @charliermarsh on 2025-05-06 12:53_

Another option (tell uv to only support Linux):

```
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "blah"
version = "1.0"
requires-python = ">=3.13, <3.14"
dependencies = [
    "ray[tune]>=2.45.0",
]

[tool.uv]
environments = ["sys_platform == 'linux'"]
```

---

_Comment by @NeilGirdhar on 2025-05-06 13:07_

Thanks!  It's unfortunate that the solution is uv-specific.  Why doesn't the resolver choose the latest version on linux?  Can setup.py for Ray be changed in some way to force it to do that?

---

_Comment by @charliermarsh on 2025-05-06 13:09_

You can just do this if you want, it's not uv-specific but it means you're encoding a direct dependency on `pyarrow`:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "blah"
version = "1.0"
requires-python = ">=3.13, <3.14"
dependencies = [
    "ray[tune]>=2.45.0",
    "pyarrow>=19.0 ; platform_machine != 'x86_64' or sys_platform != 'darwin'"
]
```

Yes, Ray could be changed to increase their lower-bound from 9.0.0 to something higher.

---

_Comment by @NeilGirdhar on 2025-05-06 13:14_

Perfect, thanks!

---

_Closed by @NeilGirdhar on 2025-05-06 13:14_

---

_Comment by @charliermarsh on 2025-05-06 13:16_

Np!

---

_Comment by @NeilGirdhar on 2025-07-08 05:04_

> We typically try to minimize the number of selected versions for a given package. So we'd attempt to use `pyarrow` 17 here in the resolver. You can add a constraint to force uv to select the higher version on non-macOS.

Can this heuristic be changed to choose newer versions if possible?  Ray is refusing to bump their minimum versions, and the situations is basically broken.

---
