```yaml
number: 2532
title: Preserve hashes for pinned packages
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/hash
created_at: 2024-03-19T00:14:18Z
updated_at: 2024-03-19T01:02:20Z
url: https://github.com/astral-sh/uv/pull/2532
synced_at: 2026-01-12T16:05:05Z
```

# Preserve hashes for pinned packages

---

_@charliermarsh_

## Summary

When a user runs with `--output-file` and `--generate-hashes`, we should _only_ update the hashes if the pinned version itself changes.

Closes https://github.com/astral-sh/uv/issues/1530.


---

_Marked ready for review by @charliermarsh on 2024-03-19 00:39_

---

_Label `bug` added by @charliermarsh on 2024-03-19 00:40_

---

_Label `compatibility` added by @charliermarsh on 2024-03-19 00:40_

---

_Merged by @charliermarsh on 2024-03-19 01:02_

---

_Closed by @charliermarsh on 2024-03-19 01:02_

---

_Branch deleted on 2024-03-19 01:02_

---
