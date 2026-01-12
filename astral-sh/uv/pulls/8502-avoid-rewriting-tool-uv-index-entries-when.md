```yaml
number: 8502
title: "Avoid rewriting `[[tool.uv.index]]` entries when credentials are provided"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/creds
created_at: 2024-10-23T14:31:43Z
updated_at: 2024-10-23T14:57:15Z
url: https://github.com/astral-sh/uv/pull/8502
synced_at: 2026-01-12T16:08:21Z
```

# Avoid rewriting `[[tool.uv.index]]` entries when credentials are provided

---

_@charliermarsh_

## Summary

Instead of creating a new entry, we should reuse the existing entry (to preserve decor); similarly, we should avoid overwriting fields that are already "correct".

Closes https://github.com/astral-sh/uv/issues/8483.


---

_Label `bug` added by @charliermarsh on 2024-10-23 14:31_

---

_Merged by @charliermarsh on 2024-10-23 14:57_

---

_Closed by @charliermarsh on 2024-10-23 14:57_

---

_Branch deleted on 2024-10-23 14:57_

---
