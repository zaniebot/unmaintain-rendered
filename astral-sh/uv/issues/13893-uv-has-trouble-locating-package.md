```yaml
number: 13893
title: uv has trouble locating package
type: issue
state: closed
author: usman-faisal
labels:
  - question
assignees: []
created_at: 2025-06-06T23:00:44Z
updated_at: 2025-06-07T10:33:41Z
url: https://github.com/astral-sh/uv/issues/13893
synced_at: 2026-01-12T16:01:39Z
```

# uv has trouble locating package

---

_@usman-faisal_

### Summary

Hello.

I have a requirements.txt that specifies `pytorch-triton==3.3.0+git96316ce5`. However, running `uv pip install -r requirements.txt` yields `× No solution found when resolving dependencies:
  ╰─▶ Because there is no version of pytorch-triton==3.3.0+git96316ce5 and you require
      pytorch-triton==3.3.0+git96316ce5, we can conclude that your requirements are
      unsatisfiable.`

Can someone help me understand the problem? It is essential for me to strictly follow the requirements.txt.

### Platform

Linux 6.8.0-60-generic x86_64 GNU/Linux

### Version

uv-self 0.6.10

### Python version

Python 3.11.11

---

_Label `bug` added by @usman-faisal on 2025-06-06 23:00_

---

_Comment by @zanieb on 2025-06-07 05:04_

Where do you expect that dependency to be installed from? Where did you get the requirements file?

The `+git96316ce5` tag isn't usually allowed on package indexes.

---

_Label `bug` removed by @zanieb on 2025-06-07 05:04_

---

_Label `question` added by @zanieb on 2025-06-07 05:04_

---

_Closed by @usman-faisal on 2025-06-07 10:33_

---
