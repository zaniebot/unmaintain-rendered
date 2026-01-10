---
number: 650
title: Add custom error messages for package name-less requirements
type: issue
state: closed
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2023-12-14T14:39:36Z
updated_at: 2023-12-16T21:14:36Z
url: https://github.com/astral-sh/uv/issues/650
synced_at: 2026-01-10T01:23:04Z
---

# Add custom error messages for package name-less requirements

---

_Issue opened by @charliermarsh on 2023-12-14 14:39_

Given a dependency in a `requirements.txt` file like:

```
file://...
```

That is, a dependency that omits a package name (unlike `black @ file://...`), we should _at least_ show the user a dedicated error message asking them to add the package name. (If we support these bare URLs, this would then be moot, although bare URLs aren't supported in `pyproject.toml` files so... we may still want it.)

---

_Label `error messages` added by @charliermarsh on 2023-12-14 14:39_

---

_Comment by @zanieb on 2023-12-14 14:45_

name-less requirements?

---

_Comment by @charliermarsh on 2023-12-14 14:48_

I wrote this during a 1:1. It'll be filled in later.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-15 04:17_

---

_Referenced in [astral-sh/uv#669](../../astral-sh/uv/pulls/669.md) on 2023-12-16 21:11_

---

_Closed by @charliermarsh on 2023-12-16 21:14_

---
