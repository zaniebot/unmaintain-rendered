---
number: 2863
title: Always return unzipped wheels from distribution database
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2024-04-07T22:24:22Z
updated_at: 2024-04-08T18:07:18Z
url: https://github.com/astral-sh/uv/issues/2863
synced_at: 2026-01-07T13:12:17-06:00
---

# Always return unzipped wheels from distribution database

---

_Issue opened by @charliermarsh on 2024-04-07 22:24_

We already do this in most cases, and in the others, we just immediately unzip them. It would let us remove some complexity to just preemptively unzip in the remaining cases (which are: local wheels, and built wheels -- remote wheels are already unzipped while downloading).

Should quickly benchmark to make sure there's no regression.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-07 22:24_

---

_Label `internal` added by @charliermarsh on 2024-04-07 22:24_

---

_Comment by @charliermarsh on 2024-04-07 22:25_

This will also make hash-checking a bit easier for unrelated reasons (roughly: we have a more uniform flow of data out of the database).

---

_Referenced in [astral-sh/uv#2905](../../astral-sh/uv/pulls/2905.md) on 2024-04-08 17:40_

---

_Closed by @charliermarsh on 2024-04-08 18:07_

---
