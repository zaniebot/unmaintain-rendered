```yaml
number: 1229
title: Avoid race condition in clone file replacement
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/r
created_at: 2024-02-01T15:29:38Z
updated_at: 2024-02-01T15:55:24Z
url: https://github.com/astral-sh/uv/pull/1229
synced_at: 2026-01-12T16:04:31Z
```

# Avoid race condition in clone file replacement

---

_@charliermarsh_

## Summary

I've never seen this in practice but in theory it is possible, and we have the same guardrail in the hardlink path.

---

_Label `bug` added by @charliermarsh on 2024-02-01 15:29_

---

_@konstin approved on 2024-02-01 15:31_

Does this have a perf impact (do we have a platform to test - i could try if this works with my btrfs partition)

---

_Comment by @charliermarsh on 2024-02-01 15:50_

@konstin - Yeah but it should be quite minimal I think? It only applies when two packages contain overlapping files.

---

_Merged by @charliermarsh on 2024-02-01 15:55_

---

_Closed by @charliermarsh on 2024-02-01 15:55_

---

_Branch deleted on 2024-02-01 15:55_

---
