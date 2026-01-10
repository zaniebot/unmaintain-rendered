---
number: 655
title: Building source distributions leads to double-resolution
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-15T04:18:18Z
updated_at: 2023-12-15T17:27:18Z
url: https://github.com/astral-sh/uv/issues/655
synced_at: 2026-01-10T01:23:04Z
---

# Building source distributions leads to double-resolution

---

_Issue opened by @charliermarsh on 2023-12-15 04:18_

We run `resolve`, which requires a list of requirements; then, in the install step, we run the `Finder` to map each requirement to a specific distribution. But we already know the distributions, from the resolution! (It's all cached, so not that bad, but we should fix it, just like we did in `pip-install`.)

---

_Label `bug` added by @charliermarsh on 2023-12-15 04:18_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-15 04:18_

---

_Referenced in [astral-sh/uv#656](../../astral-sh/uv/pulls/656.md) on 2023-12-15 04:57_

---

_Closed by @charliermarsh on 2023-12-15 17:27_

---
