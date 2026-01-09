---
number: 491
title: Nest cache directories to avoid too large directories
type: issue
state: closed
author: konstin
labels:
  - performance
assignees: []
created_at: 2023-11-22T19:06:20Z
updated_at: 2024-07-09T12:37:34Z
url: https://github.com/astral-sh/uv/issues/491
synced_at: 2026-01-07T13:12:16-06:00
---

# Nest cache directories to avoid too large directories

---

_Issue opened by @konstin on 2023-11-22 19:06_

Instead of e.g. placing all pypi metadata in a directory, we should consider adding subdirectory levels to avoid large directories with multiple thousand entries that slow down fs operations.  cache and git e.g. do this by using the first to letter of the files as additional directory name.

The first task is to identify which directly are likely to even become too large, and then find a good and consistent scheme for subdividing them. 

---

_Comment by @charliermarsh on 2023-11-22 19:39_

We could also just use cacache for this, maybe?

---

_Label `performance` added by @charliermarsh on 2023-11-25 17:27_

---

_Added to milestone `Future` by @charliermarsh on 2023-11-25 17:27_

---

_Comment by @konstin on 2024-06-27 11:16_

In practice, did this turn out to be a problem / do we still plan to change this?

---

_Removed from milestone `Future` by @konstin on 2024-07-09 12:37_

---

_Closed by @konstin on 2024-07-09 12:37_

---
