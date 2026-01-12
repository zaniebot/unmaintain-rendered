```yaml
number: 15526
title: How to force not install package on one OS and install on other?
type: issue
state: open
author: theLastOfCats
labels:
  - question
assignees: []
created_at: 2025-08-26T10:40:08Z
updated_at: 2025-08-26T11:50:28Z
url: https://github.com/astral-sh/uv/issues/15526
synced_at: 2026-01-12T16:02:12Z
```

# How to force not install package on one OS and install on other?

---

_@theLastOfCats_

### Question

Trying to install dependencies on Darwin platform:

Repr:
```
[project]
name = "inot"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.13"
dependencies = [
    "pyinotify==0.9.6 ; sys_platform == 'linux'",
]
environments = [
    "sys_platform == 'darwin'",
    "sys_platform == 'linux'",
]

```

Got an error:
```
(inot) shaziganshin@sc-mac-01156 inot % uv sync
  × Failed to build `pyinotify==0.9.6`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stderr]
      inotify is not available on macosx-11.0-arm64

      hint: This usually indicates a problem with the package or the build environment.
  help: `pyinotify` (v0.9.6) was included because `inot` (v0.1.0) depends on `pyinotify==0.9.6`

```

On Linux platform installs correctly. Is this proper behavior?

### Platform

Darwin 24.6.0 arm64

### Version

uv 0.8.13 (Homebrew 2025-08-21)

---

_Label `question` added by @theLastOfCats on 2025-08-26 10:40_

---

_Comment by @charliermarsh on 2025-08-26 11:50_

In this case, I think uv is trying to resolve the dependencies of `pyinotify`, which doesn't publish static metadata. So we need to build it to get the dependencies. Typically, you'd solve this by checking in the `uv.lock` file that you generated on Linux; then we can avoid resolving the dependencies on macOS, and can just reuse the lockfile.

Alternatively, you can declare the metadata statically for that package, to avoid the need to build it:

```toml
[project]
name = "inot"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.13"
dependencies = [
    "pyinotify==0.9.6 ; sys_platform == 'linux'",
]
environments = [
    "sys_platform == 'darwin'",
    "sys_platform == 'linux'",
]

[[tool.uv.dependency-metadata]]
name = "pyinotify"
version = "0.9.6"
requires-dist = []
```



---
