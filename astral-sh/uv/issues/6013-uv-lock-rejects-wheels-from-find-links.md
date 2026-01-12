```yaml
number: 6013
title: "`uv lock` rejects wheels from `--find-links`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-12T00:14:36Z
updated_at: 2024-08-12T02:02:40Z
url: https://github.com/astral-sh/uv/issues/6013
synced_at: 2026-01-12T15:59:00Z
```

# `uv lock` rejects wheels from `--find-links`

---

_@charliermarsh_

```
❯ uv lock --no-index --find-links ../scripts/links
warning: `uv lock` is experimental and may change without warning
Using Python 3.12.1
Resolved 2 packages in 15ms
error: since the package `tqdm==1000.0.0 @ path+/Users/crmarsh/workspace/puffin/scripts/links` comes from a path dependency, a hash was expected but one was not found for wheel
```

---

_Label `bug` added by @charliermarsh on 2024-08-12 00:14_

---

_Label `preview` added by @charliermarsh on 2024-08-12 00:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-12 00:14_

---

_Closed by @charliermarsh on 2024-08-12 02:02_

---
