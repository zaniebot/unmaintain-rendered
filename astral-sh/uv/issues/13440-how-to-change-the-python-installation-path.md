```yaml
number: 13440
title: How to change the Python installation path
type: issue
state: closed
author: wzr0108
labels:
  - question
assignees: []
created_at: 2025-05-14T02:10:30Z
updated_at: 2025-05-14T02:34:05Z
url: https://github.com/astral-sh/uv/issues/13440
synced_at: 2026-01-12T16:01:29Z
```

# How to change the Python installation path

---

_@wzr0108_

### Question

I create a new environment using ```uv venv -p 3.10```
UV installs Python interpreters to /root/.local/share/uv/python/cpython-[version]-[platform] by default. 
How to change the Python installation path. 
I tried setting UV_CACHE_DIR to a different path, but it had no effect


### Platform

Ubuntu 20.04

### Version

uv 0.7.3

---

_Label `question` added by @wzr0108 on 2025-05-14 02:10_

---

_Comment by @charliermarsh on 2025-05-14 02:18_

Check out https://docs.astral.sh/uv/configuration/environment/#uv_python_install_dir.

---

_Closed by @wzr0108 on 2025-05-14 02:34_

---
