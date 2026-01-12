```yaml
number: 4278
title: "Use `FxHashMap` for available versions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/index-map
created_at: 2024-06-12T16:03:31Z
updated_at: 2024-06-12T17:16:38Z
url: https://github.com/astral-sh/uv/pull/4278
synced_at: 2026-01-12T16:06:07Z
```

# Use `FxHashMap` for available versions

---

_@charliermarsh_

## Summary

I don't think we ever iterate over these in-order so I'd rather use `FxHash` to avoid creating the appearance that the order matters.

---

_Label `internal` added by @charliermarsh on 2024-06-12 16:03_

---

_@zanieb approved on 2024-06-12 16:14_

---

_Merged by @charliermarsh on 2024-06-12 17:16_

---

_Closed by @charliermarsh on 2024-06-12 17:16_

---

_Branch deleted on 2024-06-12 17:16_

---
