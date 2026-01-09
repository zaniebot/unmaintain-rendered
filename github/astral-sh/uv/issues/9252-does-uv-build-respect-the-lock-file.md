---
number: 9252
title: Does uv build respect the lock file?
type: issue
state: closed
author: idan-rahamim-lendbuzz
labels: []
assignees: []
created_at: 2024-11-19T21:42:34Z
updated_at: 2024-11-20T00:11:10Z
url: https://github.com/astral-sh/uv/issues/9252
synced_at: 2026-01-07T13:12:18-06:00
---

# Does uv build respect the lock file?

---

_Issue opened by @idan-rahamim-lendbuzz on 2024-11-19 21:42_

Is it possible to build packages with pinned dependencies from the lock file.
In poetry it's not supported
https://github.com/python-poetry/poetry/issues/2778

---

_Comment by @charliermarsh on 2024-11-19 22:57_

Are you referring to pinning the versions in the built wheel's metadata? Or using pinned dependencies _while building_ (like, pinned build dependencies)?

---

_Comment by @charliermarsh on 2024-11-20 00:11_

I believe this is a duplicate of https://github.com/astral-sh/uv/issues/8729.

---

_Closed by @charliermarsh on 2024-11-20 00:11_

---
