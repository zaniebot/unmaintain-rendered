---
number: 1480
title: Nest cache entries under package version subdirectories
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2022-12-30T15:53:20Z
updated_at: 2023-05-08T22:26:11Z
url: https://github.com/astral-sh/ruff/issues/1480
synced_at: 2026-01-07T13:12:14-06:00
---

# Nest cache entries under package version subdirectories

---

_Issue opened by @charliermarsh on 2022-12-30 15:53_

Right now, the package version is included in the cache key. But we should just use it to create a subdirectory for each version. This would let us automatically clean up old cache entries.

---

_Label `enhancement` added by @charliermarsh on 2022-12-30 15:53_

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:11_

---

_Label `internal` added by @charliermarsh on 2022-12-31 18:11_

---

_Closed by @charliermarsh on 2023-05-08 22:26_

---
