---
number: 2011
title: extra-index-url is not taken into account if package exists in the index-url
type: issue
state: closed
author: atti92
labels:
  - duplicate
assignees: []
created_at: 2024-02-27T12:30:56Z
updated_at: 2024-02-27T15:47:22Z
url: https://github.com/astral-sh/uv/issues/2011
synced_at: 2026-01-07T13:12:16-06:00
---

# extra-index-url is not taken into account if package exists in the index-url

---

_Issue opened by @atti92 on 2024-02-27 12:30_

If you have a package in a private repository and you add it as an extra-index-url, the resolver will throw conflict errors

```
uv pip compile requirements.txt
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of mypkg are available:
          mypkg<1.0.0rc6
          mypkg>=1.1.dev0
      and you require mypkg>=1.0.0rc6,<1.1.dev0, we can conclude that the requirements are unsatisfiable.
```
In my case pypi had 1 yanked version (0.0.0a1) and all valid versions are in the private repo.

This behaviour is contrary to `pip` because it can still find requirements in the extra-index-url.

workaround that works:
- set pypi as extra-index-url
- set private repo as index-url

---
* The command you invoked: uv pip compile requirements.txt
* The current uv platform: Ubuntu 22.04.1
* The current uv version:  0.1.11



---

_Comment by @charliermarsh on 2024-02-27 15:35_

Thanks! This is the same as https://github.com/astral-sh/uv/issues/1377.

---

_Closed by @charliermarsh on 2024-02-27 15:35_

---

_Label `duplicate` added by @zanieb on 2024-02-27 15:47_

---

_Referenced in [astral-sh/uv#2159](../../astral-sh/uv/issues/2159.md) on 2024-03-04 17:03_

---

_Referenced in [astral-sh/uv#2205](../../astral-sh/uv/issues/2205.md) on 2024-03-05 16:42_

---
