---
number: 5879
title: Update tests to use exclude newer environment variable
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - testing
assignees: []
created_at: 2024-08-07T17:24:50Z
updated_at: 2024-08-09T13:28:43Z
url: https://github.com/astral-sh/uv/issues/5879
synced_at: 2026-01-10T01:23:53Z
---

# Update tests to use exclude newer environment variable

---

_Issue opened by @zanieb on 2024-08-07 17:24_

Instead of using the `--exclude-newer` flag, we should use the `UV_EXCLUDE_NEWER` environment variable so we can replace all the "without_exclude_newer` methods with a simple `.clear_env("UV_EXCLUDE_NEWER")` in the relevant tests?

---

_Label `help wanted` added by @zanieb on 2024-08-07 17:24_

---

_Label `testing` added by @zanieb on 2024-08-07 17:24_

---

_Referenced in [astral-sh/uv#5946](../../astral-sh/uv/pulls/5946.md) on 2024-08-09 00:15_

---

_Closed by @konstin on 2024-08-09 07:53_

---

_Closed by @konstin on 2024-08-09 07:53_

---

_Reopened by @zanieb on 2024-08-09 13:25_

---

_Closed by @zanieb on 2024-08-09 13:28_

---
