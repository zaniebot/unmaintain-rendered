```yaml
number: 8387
title: Use correct indentation when project table contains open bracket comment
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/open
created_at: 2024-10-20T17:11:00Z
updated_at: 2024-10-20T19:56:38Z
url: https://github.com/astral-sh/uv/pull/8387
synced_at: 2026-01-12T16:08:17Z
```

# Use correct indentation when project table contains open bracket comment

---

_@charliermarsh_

## Summary

Now, we use four space (rather than one space) for cases like:

```toml
dependencies = [ # comment 0
    # comment 1
    "anyio==3.7.0", # comment 2
    # comment 3
]
```

---

_Label `bug` added by @charliermarsh on 2024-10-20 17:11_

---

_Merged by @charliermarsh on 2024-10-20 19:56_

---

_Closed by @charliermarsh on 2024-10-20 19:56_

---

_Branch deleted on 2024-10-20 19:56_

---
