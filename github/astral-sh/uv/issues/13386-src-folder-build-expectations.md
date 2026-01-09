---
number: 13386
title: /src folder build expectations
type: issue
state: closed
author: svdimitrov
labels:
  - question
assignees: []
created_at: 2025-05-11T14:31:10Z
updated_at: 2025-05-21T11:59:41Z
url: https://github.com/astral-sh/uv/issues/13386
synced_at: 2026-01-07T13:12:18-06:00
---

# /src folder build expectations

---

_Issue opened by @svdimitrov on 2025-05-11 14:31_

### Question

Is the `/src/{project}` folder structure fixed or can it be changed? I currently have a `/backend` folder with a `pyproject.toml` inside. Here is a minimal version:

```
[project]
name = "backend"
version = "0.1.0"
description = "FastAPI backend for Next.js web application"
requires-python = ">=3.9,<4.0"

[project.scripts]
start = "backend.scripts:start"

[build-system]
requires = ["uv_build>=0.7.3,<0.8.0"]
build-backend = "uv_build"
```

This forces me to create a `/backend/src/backend` structure in which I have to put the whole app logic.

Here is the error I get when I try to remove the `/src/backend` structure:

```
  × Failed to build `backend @ /file:******/backend`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `uv_build.build_editable` failed (exit status: 1)

      [stderr]
      Error: Missing source directory at: `src`

      hint: This usually indicates a problem with the package or the build environment.
```

Desired state: Have uv look for everything in the root `/backend` folder.

### Platform

Windows 11 + WSL Ubuntu 22 LTS

### Version

0.7.3

---

_Label `question` added by @svdimitrov on 2025-05-11 14:31_

---

_Comment by @konstin on 2025-05-12 09:51_

You can use the [module-root](https://docs.astral.sh/uv/reference/settings/#build-backend_module-root) setting to use a flat layout without `src`.

---

_Comment by @cpaniaguam on 2025-05-13 17:44_

What if the package name "foo" is already taken but I still want to use `src/foo` for the source code? If I make the project name "foo-bar" then `uv` tries look for files under `src/foo_bar`.

---

_Comment by @konstin on 2025-05-13 18:25_

I recommend picking a unique name, otherwise there can be hard-to-debug conflicts on installation. If you still want to use a different name for the module than for the package, you can use the [`module-name`](https://docs.astral.sh/uv/reference/settings/#build-backend_module-name) setting.


---

_Closed by @charliermarsh on 2025-05-21 11:59_

---
