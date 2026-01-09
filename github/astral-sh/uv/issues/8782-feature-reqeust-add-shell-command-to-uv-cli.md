---
number: 8782
title: "Feature Reqeust: Add shell command to uv cli"
type: issue
state: closed
author: tleonhardt
labels:
  - duplicate
assignees: []
created_at: 2024-11-03T20:12:10Z
updated_at: 2024-11-03T21:25:09Z
url: https://github.com/astral-sh/uv/issues/8782
synced_at: 2026-01-07T13:12:18-06:00
---

# Feature Reqeust: Add shell command to uv cli

---

_Issue opened by @tleonhardt on 2024-11-03 20:12_

I love how fast `uv` is and I am beginning to use it as a replacement for `pipenv`. The one thing I really miss is the ability to do something like `uv shell` to quickly and easily activate the venv.  This could be a very simple command that takes no arguments and just runs `source .venv/bin/activate` where `.venv` is the path to the virtual env created by `uv`.

---

_Comment by @charliermarsh on 2024-11-03 21:23_

Thanks! I believe this is the same as #1910 -- you can track that issue instead.

---

_Closed by @charliermarsh on 2024-11-03 21:23_

---

_Label `duplicate` added by @charliermarsh on 2024-11-03 21:23_

---

_Comment by @tleonhardt on 2024-11-03 21:24_

@charliermarsh Ah sorry, I searched but didn't see that one.  Thanks!

---

_Comment by @charliermarsh on 2024-11-03 21:25_

No prob!

---
