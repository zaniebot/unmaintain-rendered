---
number: 4584
title: pip compile --universal does not retain environment markers for dependencies
type: issue
state: closed
author: houbie
labels:
  - bug
  - lock
assignees: []
created_at: 2024-06-27T11:07:00Z
updated_at: 2024-06-27T21:30:11Z
url: https://github.com/astral-sh/uv/issues/4584
synced_at: 2026-01-10T01:23:39Z
---

# pip compile --universal does not retain environment markers for dependencies

---

_Issue opened by @houbie on 2024-06-27 11:07_

**Steps to reproduce:**
`echo poetry | uv pip compile --universal -`
adding `--no-strip-markers` yields the same result.

**Outcome:**
```
...
xattr==1.1.0
    # via poetry
```
xattr is not available for windows, so the generated requirements will fail to install on windows.

**Expected outcome:**
```
...
xattr==1.1.0; sys_platform == "darwin"
    # via poetry
```
From poetry's sdist: `Requires-Dist: xattr (>=1.0.0,<2.0.0) ; sys_platform == "darwin"`


---

_Comment by @zanieb on 2024-06-27 11:10_

Thanks for the report, looks like a bug but someone who's working on the lock file will check in.

---

_Label `bug` added by @zanieb on 2024-06-27 11:10_

---

_Label `lock` added by @zanieb on 2024-06-27 11:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-27 17:40_

---

_Comment by @charliermarsh on 2024-06-27 17:42_

Thanks, not immediately clear why it's being omitted there, looking now.

---

_Comment by @charliermarsh on 2024-06-27 17:43_

(`uv lock` does correctly include the marker.)

---

_Comment by @charliermarsh on 2024-06-27 17:49_

Ahh ok, the problem is that the dependency graph has a cycle, because `poetry-plugin-export` has a dependency on `poetry`. Very interesting... Will fix.

---

_Referenced in [astral-sh/uv#4595](../../astral-sh/uv/pulls/4595.md) on 2024-06-27 18:19_

---

_Comment by @charliermarsh on 2024-06-27 18:20_

Fixed in #4595.

---

_Comment by @houbie on 2024-06-27 19:27_

that was super responsive 🚀
I just replaced pip with uv in [pyprojectx](https://github.com/pyprojectx/pyprojectx) and the integration tests became 4x faster 🥳 
Looking forward to the next release

---

_Comment by @charliermarsh on 2024-06-27 19:31_

Awesome, thanks!

---

_Closed by @charliermarsh on 2024-06-27 21:30_

---

_Closed by @charliermarsh on 2024-06-27 21:30_

---

_Referenced in [pyprojectx/pyprojectx#95](../../pyprojectx/pyprojectx/issues/95.md) on 2024-06-28 11:38_

---
