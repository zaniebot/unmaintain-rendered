---
number: 4719
title: "`requires-python` normalization is unintuitive"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-07-02T02:29:47Z
updated_at: 2024-07-04T20:06:53Z
url: https://github.com/astral-sh/uv/issues/4719
synced_at: 2026-01-07T13:12:17-06:00
---

# `requires-python` normalization is unintuitive

---

_Issue opened by @charliermarsh on 2024-07-02 02:29_

E.g., `python_version <= '3.9' or python_version > '3.9'` doesn't simplify out, nor does `python_version < '3.9' or python_version >= '3.9'`.

Related to #4714.

---

_Label `bug` added by @charliermarsh on 2024-07-02 02:29_

---

_Comment by @charliermarsh on 2024-07-02 02:30_

If this were a Python package, it would actually be correct not to normalize these, due to pre-release handling and other details of PEP 440. But we generally want to ignore that stuff with `requires-python`.

---

_Referenced in [astral-sh/uv#4536](../../astral-sh/uv/issues/4536.md) on 2024-07-02 02:31_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-02 02:31_

---

_Comment by @charliermarsh on 2024-07-02 12:19_

I think this can be solved alongside https://github.com/astral-sh/uv/issues/4714.

---

_Comment by @ibraheemdev on 2024-07-02 14:51_

Is this a duplicate of https://github.com/astral-sh/uv/issues/4272?

---

_Comment by @charliermarsh on 2024-07-02 20:15_

@ibraheemdev - Yeah, thank you.

---

_Referenced in [astral-sh/uv#4794](../../astral-sh/uv/pulls/4794.md) on 2024-07-03 23:50_

---

_Closed by @charliermarsh on 2024-07-04 20:06_

---

_Closed by @charliermarsh on 2024-07-04 20:06_

---
