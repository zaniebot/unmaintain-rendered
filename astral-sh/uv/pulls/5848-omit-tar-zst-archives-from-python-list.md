```yaml
number: 5848
title: "Omit `.tar.zst` archives from Python list"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/stripped
created_at: 2024-08-07T02:33:15Z
updated_at: 2024-08-07T02:37:16Z
url: https://github.com/astral-sh/uv/pull/5848
synced_at: 2026-01-12T16:07:03Z
```

# Omit `.tar.zst` archives from Python list

---

_@charliermarsh_

## Summary

We don't support installing these (because we don't support zstandard), so I don't think we need to include them right now. We could add zst support instead if we want.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-07 02:33_

---

_Comment by @charliermarsh on 2024-08-07 02:37_

Wait nevermind, I'm wrong, we do support this.

---

_Closed by @charliermarsh on 2024-08-07 02:37_

---

_Branch deleted on 2024-08-07 02:37_

---
