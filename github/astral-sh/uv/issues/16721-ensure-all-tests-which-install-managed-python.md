---
number: 16721
title: Ensure all tests which install managed Python versions use the cache
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - internal
  - testing
assignees: []
created_at: 2025-11-13T14:10:59Z
updated_at: 2025-11-13T14:16:17Z
url: https://github.com/astral-sh/uv/issues/16721
synced_at: 2026-01-07T13:12:19-06:00
---

# Ensure all tests which install managed Python versions use the cache

---

_Issue opened by @zanieb on 2025-11-13 14:10_

We have `TestContext::with_python_download_cache` which improves the performance of tests which download a managed Python version, but I'm not sure we use it everywhere. There are only a couple cases where we explicitly don't want to use this. Maybe we should have `with_managed_python_dirs` turn this on and have `without_python_download_cache` instead for the opt-out cases?

---

_Label `help wanted` added by @zanieb on 2025-11-13 14:11_

---

_Label `internal` added by @zanieb on 2025-11-13 14:11_

---

_Label `testing` added by @zanieb on 2025-11-13 14:11_

---

_Comment by @nooscraft on 2025-11-13 14:14_

@zanieb can I take a look at this?

---

_Comment by @zanieb on 2025-11-13 14:16_

Yes!

---

_Referenced in [astral-sh/uv#16723](../../astral-sh/uv/pulls/16723.md) on 2025-11-13 15:06_

---
