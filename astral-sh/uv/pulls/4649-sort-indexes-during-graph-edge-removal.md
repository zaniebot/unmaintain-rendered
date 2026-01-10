```yaml
number: 4649
title: Sort indexes during graph edge removal
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-06-29T17:14:44Z
updated_at: 2024-06-29T17:31:56Z
url: https://github.com/astral-sh/uv/pull/4649
synced_at: 2026-01-10T13:48:28Z
```

# Sort indexes during graph edge removal

---

_Pull request opened by @charliermarsh on 2024-06-29 17:14_

## Summary

`remove_edge` will invalidate the last index in the graph, so we need to ensure that each index we look at is "earlier" than the last.



---

_Label `bug` added by @charliermarsh on 2024-06-29 17:14_

---

_Comment by @charliermarsh on 2024-06-29 17:14_

Will be made redundant with #4645.

---

_Comment by @bluss on 2024-06-29 17:17_

could have a co-authored-by byline, github knows those :)

---

_Merged by @charliermarsh on 2024-06-29 17:31_

---

_Closed by @charliermarsh on 2024-06-29 17:31_

---

_Branch deleted on 2024-06-29 17:31_

---
