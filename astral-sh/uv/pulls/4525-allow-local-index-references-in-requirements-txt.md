```yaml
number: 4525
title: "Allow local index references in `requirements.txt` files"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/local-req
created_at: 2024-06-25T17:54:05Z
updated_at: 2024-06-25T18:06:38Z
url: https://github.com/astral-sh/uv/pull/4525
synced_at: 2026-01-12T16:06:17Z
```

# Allow local index references in `requirements.txt` files

---

_@charliermarsh_

## Summary

We currently accept `--index-url /path/to/index` on the command line, but confusingly, not in `requirements.txt`. This PR just brings the two in sync.

## Test Plan

New snapshot tests.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-25 17:54_

---

_Label `compatibility` added by @charliermarsh on 2024-06-25 17:54_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-06-25 17:54_

---

_Marked ready for review by @charliermarsh on 2024-06-25 17:54_

---

_Merged by @charliermarsh on 2024-06-25 18:06_

---

_Closed by @charliermarsh on 2024-06-25 18:06_

---

_Branch deleted on 2024-06-25 18:06_

---
