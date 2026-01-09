---
number: 2809
title: "New rule: warning accidental garbage collection of `asyncio.create_task`"
type: issue
state: closed
author: Glyphack
labels:
  - rule
assignees: []
created_at: 2023-02-12T12:21:32Z
updated_at: 2023-02-15T20:19:05Z
url: https://github.com/astral-sh/ruff/issues/2809
synced_at: 2026-01-07T13:12:14-06:00
---

# New rule: warning accidental garbage collection of `asyncio.create_task`

---

_Issue opened by @Glyphack on 2023-02-12 12:21_

I wanted to purpose a new lint rule. The `asyncio.create_task` function has a caveat, which is when you create a task in order to make sure it finishes you need to take a reference to it.
![image](https://textual.textualize.io/blog/images/async-create-task.jpeg)


I first read about this in [this post](https://textual.textualize.io/blog/2023/02/11/the-heisenbug-lurking-in-your-async-code/). Which also sites a lot of projects that are doing this wrong.

Is this something that ruff wants to support? I could give it a try to implement it in that case.


---

_Comment by @edgarrmondragon on 2023-02-12 15:51_

This was requested in `flake8-async`: https://github.com/cooperlees/flake8-async/issues/5

---

_Comment by @charliermarsh on 2023-02-12 15:53_

We could implement it as part of `RUF` for now. I'd support it!

---

_Label `rule` added by @charliermarsh on 2023-02-12 16:58_

---

_Comment by @samuelcolvin on 2023-02-15 17:11_

amazing!

---

_Comment by @charliermarsh on 2023-02-15 17:15_

Lol, I can do this today, I didn't realize there were so many upvotes on it.

---

_Comment by @zunda-arrow on 2023-02-15 18:01_

I didn't know I needed this lol

---

_Referenced in [astral-sh/ruff#2935](../../astral-sh/ruff/pulls/2935.md) on 2023-02-15 18:54_

---

_Closed by @charliermarsh on 2023-02-15 20:19_

---
