```yaml
number: 7406
title: "uv sync --no-sources doesn't install dev dependencies"
type: issue
state: closed
author: GalOzRlz
labels:
  - bug
assignees: []
created_at: 2024-09-15T13:05:54Z
updated_at: 2024-09-16T02:29:05Z
url: https://github.com/astral-sh/uv/issues/7406
synced_at: 2026-01-10T04:45:10Z
```

# uv sync --no-sources doesn't install dev dependencies

---

_Issue opened by @GalOzRlz on 2024-09-15 13:05_

Hi! Maybe this is a feature I'm missing.

uv sync --no-sources is supposed to remove dev dependencies? ( those that are set in [tool.uv.dev-dependencies])

when I run a normal uv sync it does install them - but with the --no-sources it removes them (acts like I have the --no-dev flag) --  but I don't want to do this inside my docker.

I'm running uv 0.4.10 (690716484 2024-09-13)



---

_Label `bug` added by @charliermarsh on 2024-09-15 15:20_

---

_Comment by @charliermarsh on 2024-09-15 15:20_

I think I consider this a bug. Thanks for pointing it out.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-15 16:45_

---

_Closed by @charliermarsh on 2024-09-16 02:29_

---

_Closed by @charliermarsh on 2024-09-16 02:29_

---
