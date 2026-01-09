---
number: 4933
title: "`uv sync` should recreate venv when it's broken"
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - preview
assignees: []
created_at: 2024-07-09T18:09:11Z
updated_at: 2024-07-09T19:25:24Z
url: https://github.com/astral-sh/uv/issues/4933
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv sync` should recreate venv when it's broken

---

_Issue opened by @konstin on 2024-07-09 18:09_

I have a venv that's broken because i removed the python patch version it was pointing. `uv sync` then fails with the error:

> Python interpreter not found at `<project>/.venv/bin/python3`

It should instead remove and recreate the venv for me.

---

_Label `enhancement` added by @konstin on 2024-07-09 18:09_

---

_Label `preview` added by @konstin on 2024-07-09 18:09_

---

_Comment by @charliermarsh on 2024-07-09 18:13_

Makes sense, thanks.

---

_Comment by @charliermarsh on 2024-07-09 18:13_

Same with uv tools IMO.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-09 18:16_

---

_Comment by @zanieb on 2024-07-09 18:20_

Presumably this is because we're using `from_root`, I believe discovery would normally skip these.

---

_Comment by @charliermarsh on 2024-07-09 18:21_

Yeah, I think we just need to expand the fallback cases.

---

_Referenced in [astral-sh/uv#4935](../../astral-sh/uv/pulls/4935.md) on 2024-07-09 18:44_

---

_Closed by @charliermarsh on 2024-07-09 19:25_

---
