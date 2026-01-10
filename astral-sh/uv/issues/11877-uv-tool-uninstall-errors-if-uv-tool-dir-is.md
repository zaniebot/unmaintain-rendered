```yaml
number: 11877
title: "`uv tool uninstall` errors if `UV_TOOL_DIR` is subdirectory of current directory"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-03-01T03:51:45Z
updated_at: 2025-03-02T01:12:52Z
url: https://github.com/astral-sh/uv/issues/11877
synced_at: 2026-01-10T03:50:31Z
```

# `uv tool uninstall` errors if `UV_TOOL_DIR` is subdirectory of current directory

---

_Issue opened by @charliermarsh on 2025-03-01 03:51_

```
❯ export UV_TOOL_DIR=foo
❯ uv tool install flask
Resolved 7 packages in 3ms
Installed 7 packages in 8ms
 + blinker==1.9.0
 + click==8.1.8
 + flask==3.1.0
 + itsdangerous==2.2.0
 + jinja2==3.1.5
 + markupsafe==3.0.2
 + werkzeug==3.1.3
Installed 1 executable: flask
uv on  main [$] is 📦 v0.6.3 via 🐍 v3.11.6 via 🦀 v1.85.0
❯ uv tool uninstall flask
Uninstalled 1 executable: flask
error: failed to remove directory ``: No such file or directory (os error 2)
```

---

_Label `bug` added by @charliermarsh on 2025-03-01 03:51_

---

_Comment by @charliermarsh on 2025-03-01 03:52_

It's because the parent of the path is `""`, and trying to query that fails.

---

_Comment by @charliermarsh on 2025-03-01 03:53_

Also, it turns out that `uv_fs::directories` just returns an empty iterator on error. That might be bad, we should probably change it.

---

_Closed by @charliermarsh on 2025-03-02 01:12_

---
