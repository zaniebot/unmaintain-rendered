---
number: 6668
title: "Export `uv.lock` to `requirements.txt`"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2024-08-27T02:45:21Z
updated_at: 2024-08-29T17:46:44Z
url: https://github.com/astral-sh/uv/issues/6668
synced_at: 2026-01-10T01:24:04Z
---

# Export `uv.lock` to `requirements.txt`

---

_Issue opened by @charliermarsh on 2024-08-27 02:45_

_No description provided._

---

_Label `enhancement` added by @charliermarsh on 2024-08-27 02:45_

---

_Comment by @mikeleppane on 2024-08-27 08:09_

+1 

After using Poetry for years, I've decided to try uv. And it's great 👏. Here are some thoughts I had while using uv. Some of these may have been discussed already.
- export only dev-dependencies from uv.lock to requirements-dev.txt (this is useful at least in the CI); _poetry export --without-hashes --without-urls  --only=dev -o requirements-dev.txt_
- _uv sync/pip install_ only dev-dependencies (like above useful in the CI) 
- show the latest version but only for outdated packages; _poetry show --outdated_ / _poetry show --outdated --only=dev_ has been useful. 
- how do get info about the current virtual env? E.g. Poetry has _poetry env info_ command to display info about current env
- is it possible to only update dev-dependencies like _poetry update --only=dev_?

Thanks!


    




---

_Referenced in [astral-sh/uv#6778](../../astral-sh/uv/pulls/6778.md) on 2024-08-28 21:49_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-28 23:11_

---

_Closed by @charliermarsh on 2024-08-29 17:46_

---

_Closed by @charliermarsh on 2024-08-29 17:46_

---

_Referenced in [astral-sh/uv#7245](../../astral-sh/uv/issues/7245.md) on 2024-09-10 08:12_

---

_Referenced in [astral-sh/uv#11575](../../astral-sh/uv/issues/11575.md) on 2025-02-17 11:25_

---
