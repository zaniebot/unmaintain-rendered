---
number: 14622
title: uv init
type: issue
state: closed
author: Aspirant-A
labels:
  - question
assignees: []
created_at: 2025-07-15T09:12:56Z
updated_at: 2025-07-16T13:22:10Z
url: https://github.com/astral-sh/uv/issues/14622
synced_at: 2026-01-07T13:12:18-06:00
---

# uv init

---

_Issue opened by @Aspirant-A on 2025-07-15 09:12_

### Question

When I create a virtual environment using <uv venv> in a folder, and then I use <uv init>, why can the .pythonversion file know the python version number in my virtual environment

### Platform

Windows 11x86

### Version

uv  0.52.0

---

_Label `question` added by @Aspirant-A on 2025-07-15 09:12_

---

_Comment by @zanieb on 2025-07-15 12:30_

Like you want

```
mkdir foo
cd foo
uv venv -p 3.8
uv init
```

to use the version in the venv?

---

_Comment by @Aspirant-A on 2025-07-15 12:45_

yes，Why is the content in the generated .pythonversion file 3.8。

---

_Comment by @zanieb on 2025-07-15 12:53_

Existing virtual environments aren't part of Python discovery for `uv init`, so... we use the same Python interpreter we'd find with `uv python find --system`

---

_Comment by @Aspirant-A on 2025-07-15 12:58_

OK，I see，Thank you for your answer

---

_Closed by @charliermarsh on 2025-07-16 13:22_

---
