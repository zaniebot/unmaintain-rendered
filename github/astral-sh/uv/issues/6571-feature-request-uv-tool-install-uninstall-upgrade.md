---
number: 6571
title: "Feature Request: `uv tool install/uninstall/upgrade` supports multiple packages"
type: issue
state: closed
author: pplmx
labels:
  - enhancement
  - help wanted
  - uv tool
assignees: []
created_at: 2024-08-24T07:00:36Z
updated_at: 2024-09-04T21:18:53Z
url: https://github.com/astral-sh/uv/issues/6571
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature Request: `uv tool install/uninstall/upgrade` supports multiple packages

---

_Issue opened by @pplmx on 2024-08-24 07:00_

As title


---

_Comment by @zanieb on 2024-08-26 13:39_

And what do you expect to happen if you do `uv tool install --with foo black httpx`?

---

_Label `enhancement` added by @zanieb on 2024-08-26 13:40_

---

_Label `uv tool` added by @zanieb on 2024-08-26 13:40_

---

_Comment by @pplmx on 2024-08-26 13:48_

Why not run `uv tool install ruff commitizen pre-commit`? I hope these three can be installed.


---

_Comment by @zanieb on 2024-08-26 14:01_

Sorry that's not answering my question — what do you expect ot happen when you use `--with` or other flags with multiple install targets?

---

_Comment by @pplmx on 2024-08-26 15:05_

It seems that `--with` is intended for managing executables that require additional dependencies, whereas the aforementioned feature request focuses on installing, upgrading, or uninstalling multiple independent executables at the same time.
(Maybe the FR desc is not very clear, I'm sorry ><)


---

_Comment by @zanieb on 2024-08-26 18:12_

So the problem here is `uv tool install tool1 tool2` is fine but once other flags are included, e.g., `--with plugin1` it doesn't make sense if `plugin1` should be applied to both tools or what. We're not excited about complicating the install interface to support multiple packages in this way.

However, I think `uv tool uninstall` and `uv tool upgrade` could definitely support multiple names.

---

_Label `help wanted` added by @zanieb on 2024-08-26 18:12_

---

_Referenced in [astral-sh/uv#7037](../../astral-sh/uv/pulls/7037.md) on 2024-09-04 17:20_

---

_Closed by @charliermarsh on 2024-09-04 21:18_

---

_Closed by @charliermarsh on 2024-09-04 21:18_

---

_Referenced in [astral-sh/uv#13750](../../astral-sh/uv/issues/13750.md) on 2025-05-31 07:20_

---
