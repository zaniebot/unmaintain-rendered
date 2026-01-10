---
number: 6245
title: "`uv pip` inconsistencies with win32 platform"
type: issue
state: closed
author: schlamar
labels: []
assignees: []
created_at: 2024-08-20T06:55:27Z
updated_at: 2024-08-20T15:03:34Z
url: https://github.com/astral-sh/uv/issues/6245
synced_at: 2026-01-10T01:23:57Z
---

# `uv pip` inconsistencies with win32 platform

---

_Issue opened by @schlamar on 2024-08-20 06:55_

Follow up of #6198

Right now, you can install Python packages with win32 platform tag when running uv from a 32bit Python interpreter.

However, you cannot target win32 platform from uv running in a 64bit Python interpreter, as there is no corresponding target triple in uv. Please add a target triple for Python's win32 platform tag.

Generally, I wonder why win32 isn't an officially supported platform as it is in [Python](https://peps.python.org/pep-0011/#tier-1) **and** [Rust](https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-1-with-host-tools) tier 1 list? 

---

_Referenced in [astral-sh/uv#6198](../../astral-sh/uv/issues/6198.md) on 2024-08-20 07:00_

---

_Comment by @charliermarsh on 2024-08-20 13:45_

We can add this!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-20 13:45_

---

_Referenced in [astral-sh/uv#6252](../../astral-sh/uv/pulls/6252.md) on 2024-08-20 13:52_

---

_Closed by @charliermarsh on 2024-08-20 14:06_

---

_Comment by @schlamar on 2024-08-20 15:03_

Wow, thanks for that really fast reaction.

---
