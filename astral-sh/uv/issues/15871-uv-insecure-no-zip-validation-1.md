```yaml
number: 15871
title: UV_INSECURE_NO_ZIP_VALIDATION=1
type: issue
state: closed
author: saifjarboui
labels:
  - bug
assignees: []
created_at: 2025-09-15T14:20:41Z
updated_at: 2025-09-17T14:34:51Z
url: https://github.com/astral-sh/uv/issues/15871
synced_at: 2026-01-10T03:23:54Z
```

# UV_INSECURE_NO_ZIP_VALIDATION=1

---

_Issue opened by @saifjarboui on 2025-09-15 14:20_

### Summary

uv still throws a 
` ZIP file contains multiple entries with different contents for:`
even after setting `UV_INSECURE_NO_ZIP_VALIDATION=1`


### Platform

Windows 11

### Version

0.8.17 (10960bc13 2025-09-10)

### Python version

python 3.11

---

_Label `bug` added by @saifjarboui on 2025-09-15 14:20_

---

_Comment by @konstin on 2025-09-15 15:59_

Can you share the package or more context to how the problem occurs?

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-17 14:20_

---

_Closed by @charliermarsh on 2025-09-17 14:34_

---
