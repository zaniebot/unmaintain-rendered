---
number: 15677
title: Why is uv trying to install packages into /usr/ ?
type: issue
state: open
author: ForceConstant
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-09-04T12:35:59Z
updated_at: 2025-09-04T13:48:09Z
url: https://github.com/astral-sh/uv/issues/15677
synced_at: 2026-01-10T01:25:58Z
---

# Why is uv trying to install packages into /usr/ ?

---

_Issue opened by @ForceConstant on 2025-09-04 12:35_

### Summary

For example if I run 'uv sync' or 'uv run main.py' after initial sync I get the following error. Why is it even trying to install packages into system? 

```
uv sync
Resolved 62 packages in 0.90ms
░░░░░░░░░░░░░░░░░░░░ [0/49] Installing wheels...                                                                                                                                                                                                                           warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
error: Failed to install: pydantic_ai_slim-0.8.1-py3-none-any.whl (pydantic-ai-slim==0.8.1)
  Caused by: Permission denied (os error 13) at path "/usr/local/lib/python3.12/dist-packages/../../../bin/.tmpuwjMCr"
```

### Platform

linux

### Version

0.8.15

### Python version

python3.12

---

_Label `bug` added by @ForceConstant on 2025-09-04 12:36_

---

_Comment by @konstin on 2025-09-04 12:46_

Can you share a reproducible example? What Python interpreter are you using? Is `VIRTUAL_ENV` set?

---

_Label `needs-mre` added by @konstin on 2025-09-04 12:46_

---

_Comment by @zanieb on 2025-09-04 13:48_

`env | grep UV` would be helpful

---
