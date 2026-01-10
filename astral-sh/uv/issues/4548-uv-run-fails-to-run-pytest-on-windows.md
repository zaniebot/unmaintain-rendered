---
number: 4548
title: "`uv run` fails to run `pytest` on Windows"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-06-26T13:28:03Z
updated_at: 2024-06-26T14:05:14Z
url: https://github.com/astral-sh/uv/issues/4548
synced_at: 2026-01-10T01:23:39Z
---

# `uv run` fails to run `pytest` on Windows

---

_Issue opened by @charliermarsh on 2024-06-26 13:28_

https://github.com/astral-sh/ruff-lsp/actions/runs/9680551699/job/26708978872?pr=443

---

_Label `bug` added by @charliermarsh on 2024-06-26 13:28_

---

_Label `preview` added by @charliermarsh on 2024-06-26 13:28_

---

_Comment by @charliermarsh on 2024-06-26 13:29_

Does the error indicate that it's trying to execute `pytest.exe` with Python?

---

_Comment by @charliermarsh on 2024-06-26 13:38_

Oh, interestingly, that's only in the Python 3.7 job.

---

_Comment by @charliermarsh on 2024-06-26 14:05_

It looks like this is the same as https://github.com/astral-sh/uv/issues/2445, which we marked as unsupported (we don't officially support Python 3.7).

---

_Closed by @charliermarsh on 2024-06-26 14:05_

---
