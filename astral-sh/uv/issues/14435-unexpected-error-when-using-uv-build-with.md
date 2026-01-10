```yaml
number: 14435
title: Unexpected error when using uv_build with namespace=true and no-editable install
type: issue
state: closed
author: hmvp
labels:
  - build-backend
assignees: []
created_at: 2025-07-03T08:51:54Z
updated_at: 2025-07-09T17:45:46Z
url: https://github.com/astral-sh/uv/issues/14435
synced_at: 2026-01-10T03:32:45Z
```

# Unexpected error when using uv_build with namespace=true and no-editable install

---

_Issue opened by @hmvp on 2025-07-03 08:51_

### Summary

Our project uses the layout that needs 
```
[tool.uv.build-backend]
namespace = true
```

When changing the build backend from setuptools to:
```
[build-system]
requires = ["uv_build>=0.7.19,<0.8.0"]
build-backend = "uv_build"
```

a normal `uv sync` works as expected. But `uv sync --no-editable` errors with:

```
$ uv sync --no-editable
Resolved 135 packages in 1ms
  × Failed to build `test_project @ file:////home/user/git/repo_folder`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `uv_build.build_wheel` failed (exit status: 1)

      [stderr]
      Error: Failed to walk source tree: ``

      Caused by:
          0: IO error for operation on /home/user/git/repo_folder/src/test_project: No such file or directory (os error 2)
          1: No such file or directory (os error 2)

      hint: This usually indicates a problem with the package or the build environment.
```

in our `src` dir we don't have a folder named `test_project` but a number of folders named after specific "components" like this:

```
src
    ├─ api
    |   ├── __init__.py
    |   └── ...
    ├─ common
    |   ├── __init__.py
    |   └── ...
    └── ...
```

### Platform

Linux 6.15.3-200.fc42.x86_64 x86_64 GNU/Linux

### Version

uv 0.7.13  (uv_build 0.7.19)

### Python version

Python 3.13.5

---

_Label `bug` added by @hmvp on 2025-07-03 08:51_

---

_Comment by @konstin on 2025-07-03 09:37_

For this layout, you currently need to set `module-name = ""`; the build backend currently mainly assumes a single root module.

---

_Label `bug` removed by @konstin on 2025-07-03 09:38_

---

_Label `build-backend` added by @konstin on 2025-07-03 09:38_

---

_Assigned to @konstin by @konstin on 2025-07-03 17:40_

---

_Closed by @zanieb on 2025-07-09 17:45_

---
