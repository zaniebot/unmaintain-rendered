---
number: 4087
title: "Allow user to specify \"Python range to lock\" separate from `Requires-Python`"
type: issue
state: open
author: charliermarsh
labels: []
assignees: []
created_at: 2024-06-06T04:06:22Z
updated_at: 2024-08-20T18:23:37Z
url: https://github.com/astral-sh/uv/issues/4087
synced_at: 2026-01-07T13:12:17-06:00
---

# Allow user to specify "Python range to lock" separate from `Requires-Python`

---

_Issue opened by @charliermarsh on 2024-06-06 04:06_

There's a lot of discussion around this idea in #4071 and #4022. It makes sense to me. I want it in my own projects!

What should this be called? Just `Requires-Python` under `tool.uv`? `Constrains-Python`? `Supports-Python`? `Locks-Python`? Just `python`?

---

_Label `preview` added by @charliermarsh on 2024-06-06 04:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-06 04:06_

---

_Comment by @henryiii on 2024-06-06 04:26_

I'd not use the capitals, those are only in the metadata field. :) `python-range`? `python-lock-range`? `supported-python-range`?


---

_Comment by @charliermarsh on 2024-06-06 04:28_

Haha yes sorry. No capitals :)

---

_Referenced in [astral-sh/uv#5046](../../astral-sh/uv/issues/5046.md) on 2024-08-17 21:56_

---

_Referenced in [astral-sh/uv#6184](../../astral-sh/uv/issues/6184.md) on 2024-08-18 18:22_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---
