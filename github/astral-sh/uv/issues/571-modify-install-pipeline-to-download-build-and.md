---
number: 571
title: Modify install pipeline to download, build, and unzip in a single continuous loop
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2023-12-05T18:20:17Z
updated_at: 2023-12-11T15:42:32Z
url: https://github.com/astral-sh/uv/issues/571
synced_at: 2026-01-07T13:12:16-06:00
---

# Modify install pipeline to download, build, and unzip in a single continuous loop

---

_Issue opened by @charliermarsh on 2023-12-05 18:20_

Right now, we (install and build), and then (unzip), as two separate phases. Making this a single phase allows for greater efficiency, and also makes it less likely that we (e.g.) leave around a bunch of zipped wheels in the cache that we're unable to reuse in subsequent commands (see: #569).

---

_Label `internal` added by @charliermarsh on 2023-12-05 18:20_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-12-05 18:20_

---

_Comment by @charliermarsh on 2023-12-05 18:20_

The main downside here is that user-facing reporting becomes less granular.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-06 16:47_

---

_Comment by @charliermarsh on 2023-12-09 16:21_

I have this working, now need to benchmark.

---

_Comment by @charliermarsh on 2023-12-09 20:45_

Performance is roughly identical, maybe a small speedup.

---

_Referenced in [astral-sh/uv#605](../../astral-sh/uv/pulls/605.md) on 2023-12-10 01:31_

---

_Closed by @charliermarsh on 2023-12-11 15:42_

---
