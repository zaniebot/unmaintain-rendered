---
number: 11819
title: "`preview = true` in `pyproject.toml` doesn't work if build backend is using a different version of uv"
type: issue
state: closed
author: DetachHead
labels:
  - question
assignees: []
created_at: 2025-02-27T03:07:06Z
updated_at: 2025-03-17T08:11:05Z
url: https://github.com/astral-sh/uv/issues/11819
synced_at: 2026-01-07T13:12:18-06:00
---

# `preview = true` in `pyproject.toml` doesn't work if build backend is using a different version of uv

---

_Issue opened by @DetachHead on 2025-02-27 03:07_

### Question

from my understanding there are three different ways to enable preview mode:

- `UV_PREVIEW` environment variable
- `--preview` cli argument
- `preview = true` in `pyproject.toml`

i'm attempting to use the experimental uv build backend which requires preview mode to be enabled. in uv 0.6.2, this used to work if i set `preview = true` in `pyproject.toml`:

```toml
[tool.uv]
preview = true
```

but as of 0.6.3, this no longer seems to work unless i set the `UV_PREVIEW` environment variable:

```
> uv sync --reinstall-package project
Resolved 148 packages in 35ms
  × Failed to build `project @ file:///C:/Users/user/project`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `uv.build_editable` failed (exit code: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
          wheel_filename = backend.build_editable("C:\\Users\\user\\AppData\\Local\\uv\\cache\\builds-v0\\.tmpm5b0zU", {}, None)
                           ^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\user\AppData\Local\uv\cache\builds-v0\.tmpKvrqnM\Lib\site-packages\uv\__init__.py", line 45, in __getattr__
          raise AttributeError(err)
      AttributeError: Using `uv.build_editable` is not allowed. The uv build backend requires preview mode to be enabled, e.g., via the `UV_PREVIEW=1` environment variable.       

      hint: This usually indicates a problem with the package or the build environment.
```

the wording of this error message says "e.g.", which implies that there are multiple ways to enable preview mode, but it seems like that method is now the only way.

also, in #8779 it says the following:

> Currently, the build backend is in preview. You need to set the `UV_PREVIEW=1` environment variable and use the `--preview` flag with all CLI commands.

this sounds like you need both `UV_PREVIEW=1` and the `--preview` cli argument. i can't seem to find any mention of the `--preview` argument in the docs other than for [`uv python install`](https://docs.astral.sh/uv/concepts/python-versions/#installing-python-executables). i don't think it has any effect on commands like `uv sync`

### Platform

_No response_

### Version

0.6.3

---

_Label `question` added by @DetachHead on 2025-02-27 03:07_

---

_Comment by @charliermarsh on 2025-02-27 03:08_

\cc @konstin 

---

_Assigned to @Gankra by @charliermarsh on 2025-02-27 03:08_

---

_Assigned to @konstin by @charliermarsh on 2025-02-27 03:08_

---

_Unassigned @Gankra by @charliermarsh on 2025-02-27 03:08_

---

_Comment by @DetachHead on 2025-02-27 03:24_

so it looks like the issue is that `preview = true` doesn't work if the version of uv being used in the backend is different to the version you're using for the frontend. for example i have uv 0.6.3 installed but had the build backend set to use 0.6.2:

```toml
[build-system]
requires = ["uv==0.6.2"]
build-backend = "uv"
```

changing it to 0.6.3 fixes it

---

_Renamed from "what's the correct way to enable "preview" mode?" to "`preview = true` in pyproject.toml doesn't work if build backend is using a different version of uv" by @DetachHead on 2025-02-27 03:25_

---

_Renamed from "`preview = true` in pyproject.toml doesn't work if build backend is using a different version of uv" to "`preview = true` in `pyproject.toml` doesn't work if build backend is using a different version of uv" by @DetachHead on 2025-02-27 03:25_

---

_Comment by @konstin on 2025-02-27 08:38_

This will be fixed by #11576

---

_Referenced in [astral-sh/uv#11576](../../astral-sh/uv/pulls/11576.md) on 2025-02-27 08:38_

---

_Closed by @konstin on 2025-03-17 08:11_

---
