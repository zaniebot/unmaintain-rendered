---
number: 15516
title: 🐛 Possible metadata mismatch between uv pip show and pip show
type: issue
state: closed
author: DhavalGojiya
labels:
  - bug
assignees: []
created_at: 2025-08-25T14:56:18Z
updated_at: 2025-08-25T16:43:37Z
url: https://github.com/astral-sh/uv/issues/15516
synced_at: 2026-01-07T13:12:19-06:00
---

# 🐛 Possible metadata mismatch between uv pip show and pip show

---

_Issue opened by @DhavalGojiya on 2025-08-25 14:56_

### Summary

Hi team 👋  

I noticed that `uv pip show` and `pip show` return different metadata for the same installed package.  

---

### 1. Output of `uv pip show django-tasks-scheduler`
```bash
Name: django-tasks-scheduler
Version: 4.0.6
Location: /home/dhaval/my_project/.venv/lib/python3.12/site-packages
Requires: click, croniter, django
Required-by:
````

---

### 2. Output of `pip show django-tasks-scheduler`

```bash
Name: django-tasks-scheduler
Version: 4.0.6
Summary: An async job scheduler for django using redis/valkey brokers
Home-page: https://github.com/django-commons/django-tasks-scheduler
Author: 
Author-email: Daniel Moran <daniel@moransoftware.ca>
License-Expression: MIT
Location: /home/dhaval/my_project/.venv/lib/python3.12/site-packages
Requires: click, croniter, django
Required-by:
```

---

### ❓ Question

As you can see, `pip show` displays additional metadata fields (`Summary`, `Home-page`, `Author`, `Author-email`, `License-Expression`) while `uv pip show` only shows the basics.

Is this difference **intentional** (simplified output by design) or is it a **bug/limitation** in `uv pip show`?

### Platform

Linux 6.8.0-45-generic x86_64 GNU/Linux

### Version

uv 0.8.13

### Python version

Python 3.12.11

---

_Label `bug` added by @DhavalGojiya on 2025-08-25 14:56_

---

_Comment by @charliermarsh on 2025-08-25 16:43_

I think this is the same as https://github.com/astral-sh/uv/issues/2526. We just haven't implemented all of the fields.

---

_Closed by @charliermarsh on 2025-08-25 16:43_

---
