```yaml
number: 3936
title: Stop workspace discovery for git and other cache entries at cache entry root
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2024-05-31T12:10:52Z
updated_at: 2024-10-30T19:12:25Z
url: https://github.com/astral-sh/uv/issues/3936
synced_at: 2026-01-12T15:58:46Z
```

# Stop workspace discovery for git and other cache entries at cache entry root

---

_@konstin_

Currently, workspace discovery walks from the project `pyproject.toml` to the file system root to find a workspace pyproject.toml. For cache entries such as git checkout and temporary directories, we should only walk up to the cache entry boundary; we don't want to consider any `pyproject.toml` files the user may have above the cache root.

---

_Label `preview` added by @konstin on 2024-05-31 12:10_

---

_Comment by @charliermarsh on 2024-05-31 16:10_

What are Cargo's rules here?

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---

_Closed by @charliermarsh on 2024-10-30 19:12_

---

_Closed by @charliermarsh on 2024-10-30 19:12_

---
