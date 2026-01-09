---
number: 6808
title: "`TD` rules should respect `task-tags`"
type: issue
state: closed
author: tylerlaprade
labels: []
assignees: []
created_at: 2023-08-23T09:53:20Z
updated_at: 2023-08-25T21:34:48Z
url: https://github.com/astral-sh/ruff/issues/6808
synced_at: 2026-01-07T13:12:15-06:00
---

# `TD` rules should respect `task-tags`

---

_Issue opened by @tylerlaprade on 2023-08-23 09:53_

![image](https://github.com/astral-sh/ruff/assets/5475199/98ef7ff4-0262-4fbb-9a1d-7413dced5aa9)

I use `TODO` for long-term fixes and `FIXME` for anything that I want to remember to fix before submitting my current PR. `FIX001` is sufficient for this - I don't need the three `TD` errors. I can't disable them, because I _do_ want them applied to `TODO` comments.

---

_Comment by @charliermarsh on 2023-08-25 21:34_

Makes sense -- I think this is similar to https://github.com/astral-sh/ruff/issues/5407.

---

_Closed by @charliermarsh on 2023-08-25 21:34_

---

_Referenced in [astral-sh/ruff#5407](../../astral-sh/ruff/issues/5407.md) on 2025-02-10 19:32_

---
