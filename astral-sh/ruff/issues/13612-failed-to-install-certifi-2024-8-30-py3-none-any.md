```yaml
number: 13612
title: "Failed to install: certifi-2024.8.30-py3-none-any.whl"
type: issue
state: closed
author: Pdsantos73
labels: []
assignees: []
created_at: 2024-10-03T17:23:28Z
updated_at: 2024-10-04T04:28:19Z
url: https://github.com/astral-sh/ruff/issues/13612
synced_at: 2026-01-12T15:54:53Z
```

# Failed to install: certifi-2024.8.30-py3-none-any.whl

---

_@Pdsantos73_

Trying to install fastapi "uv add fastapi[standard]" is showing the following error:


error: Failed to install: certifi-2024.8.30-py3-none-any.whl (certifi==2024.8.30)
  Caused by: failed to hardlink file from C:\Users\paulodo\AppData\Local\uv\cache\archive-v0\ez_A-yAgnUYbpxYugeHkX\certifi\py.typed to C:\Users\paulodo\OneDrive - Genesys Telecommunications Laboratories, Inc\DeveloperProject\Python\GenesysClould\middleware_a\.venv\Lib\site-packages\certifi\py.typed
  Caused by: The cloud operation cannot be performed on a file with incompatible hardlinks. (os error 396)

---

_Comment by @dhruvmanila on 2024-10-04 04:28_

Duplicate of https://github.com/astral-sh/uv/issues/7906 where the issue belongs

---

_Closed by @dhruvmanila on 2024-10-04 04:28_

---
