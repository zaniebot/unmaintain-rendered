---
number: 156
title: Support multiple registries
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-10-20T15:04:15Z
updated_at: 2023-10-23T03:18:32Z
url: https://github.com/astral-sh/uv/issues/156
synced_at: 2026-01-10T01:23:03Z
---

# Support multiple registries

---

_Issue opened by @charliermarsh on 2023-10-20 15:04_

How does this work with `pip`? Do you need to check every package in every registry?

---

_Comment by @charliermarsh on 2023-10-21 02:03_

So `--index-url` sets the index URL (which defaults to https://pypi.org/simple), and `--extra-index-url` augments the list of index URLs. `pip` will search them all and choose the "best" match.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-21 02:30_

---

_Referenced in [astral-sh/uv#169](../../astral-sh/uv/pulls/169.md) on 2023-10-23 03:13_

---

_Closed by @charliermarsh on 2023-10-23 03:18_

---
