---
number: 1510
title: "`textwrap` always uses LF line endings"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2022-12-31T12:41:23Z
updated_at: 2023-05-31T16:35:24Z
url: https://github.com/astral-sh/ruff/issues/1510
synced_at: 2026-01-07T13:12:14-06:00
---

# `textwrap` always uses LF line endings

---

_Issue opened by @charliermarsh on 2022-12-31 12:41_

See #1504.

---

_Label `bug` added by @charliermarsh on 2022-12-31 13:13_

---

_Comment by @charliermarsh on 2022-12-31 13:16_

@squiddy - Looking at the source, we could just pull these out, they're self-contained functions and it's not much code.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-31 13:30_

---

_Unassigned @charliermarsh by @charliermarsh on 2022-12-31 13:30_

---

_Comment by @squiddy on 2022-12-31 15:50_

I'll look at this together with #1230.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-30 03:54_

---

_Comment by @charliermarsh on 2023-05-30 03:54_

Fixing this as part of another refactor.

---

_Referenced in [astral-sh/ruff#4731](../../astral-sh/ruff/pulls/4731.md) on 2023-05-30 17:10_

---

_Closed by @charliermarsh on 2023-05-31 16:35_

---
