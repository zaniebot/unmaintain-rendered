```yaml
number: 7985
title: sys_platform seems to be ignored
type: issue
state: closed
author: mikkelam
labels:
  - question
  - error messages
assignees: []
created_at: 2024-10-07T18:22:15Z
updated_at: 2024-10-21T21:55:45Z
url: https://github.com/astral-sh/uv/issues/7985
synced_at: 2026-01-12T15:59:18Z
```

# sys_platform seems to be ignored

---

_@mikkelam_

sys_platform seems to be ignored. I am on a macos ARM64. This package is only available for linux

Reproduce
```
~/projects/uvtest
❯ uv init
Initialized project `uvtest`

~/projects/uvtest
❯ uv add 'picamera2>=0.3.18; sys_platform == "linux"'
Using CPython 3.12.4 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtual environment at: .venv
⠹ av==13.1.0
error: Failed to download and build `python-prctl==1.8.1`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
This module only works on linux
---

~/projects/uvtest
❯ uv --version
uv 0.4.17 (Homebrew 2024-09-27)
```

---

_Comment by @zanieb on 2024-10-07 18:27_

We do support `sys_platform`, but to create a lockfile we need to read metadata from this package and if you look at the [package index](https://inspector.pypi.io/project/python-prctl/1.8.1/) they only publish a source distribution and it does not statically define its dependencies so we need to build the project to know what its dependencies are.

---

_Label `question` added by @zanieb on 2024-10-07 18:27_

---

_Comment by @mikkelam on 2024-10-07 18:36_

> We do support `sys_platform`, but to create a lockfile we need to read metadata from this package and if you look at the [package index](https://inspector.pypi.io/project/python-prctl/1.8.1/) they only publish a source distribution and it does not statically define its dependencies so we need to build the project to know what its dependencies are.

Fair enough, that makes sense. Should `uv` gracefully handle such examples? Or is this a rare occurrence?

---

_Comment by @zanieb on 2024-10-07 18:39_

It's fairly rare, especially with projects using modern metadata standards, but we've definitely seen other reports.  How would we handle it? We can't know it won't build on another platform unless we try, in which case the build could fail for arbitrary reasons.

---

_Comment by @mikkelam on 2024-10-07 18:49_

> It's fairly rare, especially with projects using modern metadata standards, but we've definitely seen other reports. How would we handle it? We can't know it won't build on another platform unless we try, in which case the build could fail for arbitrary reasons.

I'm not sure it's worth your time, but a successful handling could be to simply inform the user what's happening here. i.e. `uv` is unable to create a lockfile on the host platform. 

It would avoid bug reports from annoything users like me😇 

---

_Label `error messages` added by @zanieb on 2024-10-07 19:16_

---

_Comment by @zanieb on 2024-10-07 19:17_

cc @konstin / @charliermarsh if you have any ideas

It'd be great if we could have a hint, I'm just not sure we have enough information to explain the situation. Maybe we can hint about dynamic metadata and a lack of wheels to explain why we're building a package?

---

_Comment by @konstin on 2024-10-08 13:22_

There are some categories of common build failures which are hard to detect algorithmically due to the intermediate state in which they occur.

Below is a draft i wrote compiling a number of user reports. After polishing it a bit, we can add it to the docs and link it for build failures.

---

# Build failure FAQ

This page lists common reasons why resolution and installation fails with a build error and how to fix them.

### Why does uv build a package?

When generating the cross-platform lockfile, uv needs to determine the dependencies of all packages, even those only installed on other platforms. uv tries to avoid package builds during resolution. It uses any wheel if exist for that version, then tries to find static metadata in the source distribution (mainly pyproject.toml with static project.version, project.dependencies and project.optional-dependencies or METADATA of at least version 2.2). Only if all of that fails, it builds the package.

When installing, uv needs to have a wheel for the current platform for each package. If no matching wheel exists in the index, uv tries to build the source distribution.

You can check which wheels exist for a pypi project under “Download Files”, e.g. https://pypi.org/project/numpy/2.1.1/#files. Wheels with `...-py3-none-any.whl` filenames work everywhere, others have the operating system and platform in the filename. For the linked numpy version, you can see that Python 3.10 to 3.13 on MacOS, Linux and Windows are supported.

### Fixes and Workarounds

- If the build error mentions a missing header or library, there is often a matching package in your system package manager.
- If the build error mentions a failing import, consider [deactivating build isolation](https://docs.astral.sh/uv/concepts/projects/#build-isolation).
- If a package fails to build during resolution and the version that failed to build is older then the version you want to use, try adding a [constraint](https://docs.astral.sh/uv/reference/settings/#constraint-dependencies) with a lower bound (e.g. `numpy>=1.17`). Sometimes, due to algorithmic limitations, the uv resolver tries to find a fitting version using unreasonably old packages, which can be prevented by using lower bounds.
- Consider using a different Python version for locking and/or installation (`-p`). If you are using an older Python version, you may need to use an older version of certain packages with native code too, especially for scientific code.
- If locking fails due to building a package from a platform you do not support, consider [declaring resolver environments](https://docs.astral.sh/uv/reference/settings/#environments) with your supported platforms.
- If you support a large range of Python versions versions, consider using markers, e.g.:

```
numpy>=1.23; python_version >= "3.10"
numpy<1.23; python_version < "3.10"
```

- If locking fails due to building a package from a different platform, as an escape hatch you can [provide dependency metadata manually](https://docs.astral.sh/uv/reference/settings/#dependency-metadata). As uv can not verify this information, it is important to specify correct metadata in this override.

---

_Comment by @MicahLyle on 2024-10-11 17:28_

Just documenting my use case

```
    "psycopg[binary]==3.2.3; (sys_platform == 'win32' or sys_platform == 'darwin')", # https://github.com/psycopg/psycopg
    "psycopg[c]==3.2.3; (sys_platform != 'win32' and sys_platform != 'darwin')", # https://github.com/psycopg/psycopg
```

In this case I wouldn't want to try and compile the c version unless I'm on a production Linux system. But for developers running natively within Windows or Mac I just use the binary

---

_Closed by @zanieb on 2024-10-21 21:55_

---
