---
number: 8003
title: .netrc not working on Windows without having HOME environment variable
type: issue
state: closed
author: delreluca
labels:
  - external
assignees: []
created_at: 2024-10-08T11:45:21Z
updated_at: 2024-10-09T10:41:12Z
url: https://github.com/astral-sh/uv/issues/8003
synced_at: 2026-01-07T13:12:17-06:00
---

# .netrc not working on Windows without having HOME environment variable

---

_Issue opened by @delreluca on 2024-10-08 11:45_

I have a `.netrc` file on Windows under my user profile. It works fine with pip.

But it does not work with uv.

After reading https://github.com/astral-sh/uv/issues/3471 I decided to define the environment variable `%HOME%` (setting it to match `%USERPROFILE%` which is `C:\Users\username` on my machine) and then it worked.

I don’t think this variable is defined by default on Windows. Is it possible to use `%USERPROFILE%` instead?

---

_Comment by @delreluca on 2024-10-08 20:52_

Looks like this was fixed by https://github.com/gribouille/netrc/pull/3 (together with the underscore issue from #6809), but there has been no release since

---

_Label `upstream` added by @zanieb on 2024-10-08 21:13_

---

_Referenced in [astral-sh/uv#8021](../../astral-sh/uv/pulls/8021.md) on 2024-10-08 21:17_

---

_Closed by @zanieb on 2024-10-08 22:15_

---

_Closed by @zanieb on 2024-10-08 22:15_

---

_Comment by @delreluca on 2024-10-09 10:41_

Many thanks!

---

_Referenced in [gribouille/netrc#7](../../gribouille/netrc/issues/7.md) on 2024-10-16 14:41_

---

_Referenced in [astral-sh/uv#8424](../../astral-sh/uv/pulls/8424.md) on 2024-10-21 18:32_

---
