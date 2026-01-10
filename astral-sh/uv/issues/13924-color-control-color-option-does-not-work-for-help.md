---
number: 13924
title: Color control (--color) option does not work for help in the cli tool
type: issue
state: closed
author: h-mayorquin
labels:
  - bug
  - duplicate
assignees: []
created_at: 2025-06-09T16:40:36Z
updated_at: 2025-06-09T17:03:41Z
url: https://github.com/astral-sh/uv/issues/13924
synced_at: 2026-01-10T01:25:40Z
---

# Color control (--color) option does not work for help in the cli tool

---

_Issue opened by @h-mayorquin on 2025-06-09 16:40_

### Summary

When doing the following:

```
uv --color never venv --help
```

I expect the output on my terminal to be non colored but that is not the case

![Image](https://github.com/user-attachments/assets/958dbb3d-f3b4-4b80-ba81-8f00b0b20454)

It seems that the color argument is not propagated to help or I am failing to understand its purpose or use.



### Platform

Linux 6.8.0-60-generic x86_64 GNU/Linux Ubuntu 24.04.2 LTS

### Version

uv 0.7.12

### Python version

Python 3.11.9

---

_Label `bug` added by @h-mayorquin on 2025-06-09 16:40_

---

_Comment by @zanieb on 2025-06-09 16:44_

Duplicate of https://github.com/astral-sh/uv/issues/7219

---

_Label `duplicate` added by @zanieb on 2025-06-09 16:44_

---

_Comment by @h-mayorquin on 2025-06-09 16:52_

My bad, did not check before. Closing!

---

_Closed by @h-mayorquin on 2025-06-09 16:52_

---

_Comment by @zanieb on 2025-06-09 16:54_

We'd love to fix this, but I think it'll involve work in Clap?

---

_Comment by @h-mayorquin on 2025-06-09 17:03_

Yes, thanks. I read the linked PR:
https://github.com/astral-sh/uv/pull/7223

I see the problem.

---
