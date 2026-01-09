---
number: 12539
title: how to use same dependency with diffierent source in diffierent group(or extra)
type: issue
state: closed
author: ryomahan
labels:
  - question
assignees: []
created_at: 2025-03-28T23:58:02Z
updated_at: 2025-03-29T19:59:42Z
url: https://github.com/astral-sh/uv/issues/12539
synced_at: 2026-01-07T13:12:18-06:00
---

# how to use same dependency with diffierent source in diffierent group(or extra)

---

_Issue opened by @ryomahan on 2025-03-28 23:58_

### Question

There is a custom lib in my project, when I coding in local, I want install the custom lib from local path(fixed relative path). And when I deploy it online, I want install it from github repo.

How should I organise my pyproject.toml?

### What I try

```toml
[project.optional-dependencies]
local = ["base"]
online = ["base"]

[tool.uv]
conflicts = [
    [
      { extra = "local" },
      { extra = "online" },
    ],
]

[tool.uv.sources]
base = [
    { path = "../base/python/django", extra = "local" },
    { git = "ssh://git@github.com/xxx/base", branch = "main", subdirectory = "python/django", extra = "online" }
]
```

This config is working in local, but report error in online like this:

```shell
 $ uv sync --extra online
error: Distribution not found at: file:///path/to/project/../base/python/django
```

I just want use online optional-dependencies, but uv always check all optional-dependencies.

### any other way?

Am I getting into a misunderstanding and are there better engineering practices in this matter.

### Platform

_No response_

### Version

latest

---

_Label `question` added by @ryomahan on 2025-03-28 23:58_

---

_Closed by @ryomahan on 2025-03-29 19:59_

---

_Referenced in [astral-sh/uv#9258](../../astral-sh/uv/issues/9258.md) on 2025-04-23 17:14_

---

_Referenced in [astral-sh/uv#13073](../../astral-sh/uv/issues/13073.md) on 2025-04-23 19:27_

---
