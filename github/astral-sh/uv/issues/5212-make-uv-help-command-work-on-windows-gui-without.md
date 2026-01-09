---
number: 5212
title: "Make ``uv help <command>`` work on Windows(GUI) without using ``more``."
type: issue
state: closed
author: FishAlchemist
labels:
  - question
  - cli
assignees: []
created_at: 2024-07-19T10:01:06Z
updated_at: 2024-07-19T14:47:12Z
url: https://github.com/astral-sh/uv/issues/5212
synced_at: 2026-01-07T13:12:17-06:00
---

# Make ``uv help <command>`` work on Windows(GUI) without using ``more``.

---

_Issue opened by @FishAlchemist on 2024-07-19 10:01_

**uv version**: build from commit 93ba676f(after uv v0.2.26)
In Windows(GUI), using the ``more`` is a bit cumbersome for me. It would be better to output everything at once and then I use the mouse to find the information I need.

![image](https://github.com/user-attachments/assets/78120cfb-81a7-43b7-a8d4-b7ef2ff10235)


---

_Renamed from "Make ``uv help <command>`` work on Windows without using ``more``." to "Make ``uv help <command>`` work on Windows(GUI) without using ``more``." by @FishAlchemist on 2024-07-19 10:03_

---

_Comment by @charliermarsh on 2024-07-19 12:42_

Defer to @zanieb.

---

_Label `cli` added by @charliermarsh on 2024-07-19 12:42_

---

_Comment by @zanieb on 2024-07-19 13:49_

There's a `--no-pager` option to disable this behavior.

Can you say a bit more about "Windows(GUI)" and why that makes it preferable to scroll?

---

_Comment by @FishAlchemist on 2024-07-19 14:27_

> There's a `--no-pager` option to disable this behavior.
> 
> Can you say a bit more about "Windows(GUI)" and why that makes it preferable to scroll?

``--no-pager`` seems to be a hidden parameter, I didn't see it in the help. <br>
But it does meet my requirements. <br>
Windows (GUI) only explicitly specifies the use of the graphical interface of Windows. <br>
Q: why that makes it preferable to scroll? 
A: Since the help of CLI is not so much that I can't scroll through it with a mouse, I let it output all at once so I can use the mouse to find what I need, instead of using ``more`` to view it line by line.

---

_Comment by @zanieb on 2024-07-19 14:33_

Yeah sorry I think there are some technical difficulties in getting it to display without displaying a bunch of other options. I've created an issue to track that at https://github.com/astral-sh/uv/issues/5221

Some of our help is really long and we only expect it to get longer. I think I'd prefer to stick to paging by default and let people opt-out as needed.

---

_Closed by @zanieb on 2024-07-19 14:47_

---

_Label `question` added by @zanieb on 2024-07-19 14:47_

---
