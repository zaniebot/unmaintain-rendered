---
number: 157
title: Improve performance of wheel tag matching
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2023-10-21T02:22:27Z
updated_at: 2023-11-09T14:01:05Z
url: https://github.com/astral-sh/uv/issues/157
synced_at: 2026-01-07T13:12:16-06:00
---

# Improve performance of wheel tag matching

---

_Issue opened by @charliermarsh on 2023-10-21 02:22_

Very low priority, but I have to imagine that we can make this much more efficient? Right now, we iterate over all tags, but can we use some kind of bit set or hash set to reduce the number of operations?

---

_Comment by @charliermarsh on 2023-10-24 01:10_

On a cached resolution of `flyte.in`, this is 25-30% of the time.

---

_Label `performance` added by @charliermarsh on 2023-10-24 01:18_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-10-24 19:21_

---

_Referenced in [astral-sh/uv#367](../../astral-sh/uv/pulls/367.md) on 2023-11-08 19:36_

---

_Closed by @BurntSushi on 2023-11-09 14:01_

---
