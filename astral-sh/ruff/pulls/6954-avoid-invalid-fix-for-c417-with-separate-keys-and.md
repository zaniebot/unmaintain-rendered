```yaml
number: 6954
title: Avoid invalid fix for C417 with separate keys and values
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/C417
created_at: 2023-08-28T20:14:36Z
updated_at: 2023-08-28T20:49:42Z
url: https://github.com/astral-sh/ruff/pull/6954
synced_at: 2026-01-12T02:45:38Z
```

# Avoid invalid fix for C417 with separate keys and values

---

_Pull request opened by @charliermarsh on 2023-08-28 20:14_

Closes https://github.com/astral-sh/ruff/issues/6951.

---

_Label `bug` added by @charliermarsh on 2023-08-28 20:14_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-28 20:22_

---

_Comment by @github-actions[bot] on 2023-08-28 20:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-08-28 20:32_

We may want to consider flagging these cases in the future but this is an improvement at least over `main`.

---

_@zanieb approved on 2023-08-28 20:35_

No violation for uneccessary map here? It's still probably better to rewrite this as `dict(zip(keys, values))` but the auto-fix seems like more work than it's worth to implement.

---

_Comment by @charliermarsh on 2023-08-28 20:49_

Yeah, that was my feeling too -- we may want to add this eventually but it's low priority.

---

_Merged by @charliermarsh on 2023-08-28 20:49_

---

_Closed by @charliermarsh on 2023-08-28 20:49_

---

_Branch deleted on 2023-08-28 20:49_

---
