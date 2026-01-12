```yaml
number: 10782
title: Avoid respecting preferences from other indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ind
created_at: 2025-01-20T17:12:38Z
updated_at: 2025-01-20T17:23:02Z
url: https://github.com/astral-sh/uv/pull/10782
synced_at: 2026-01-12T16:09:29Z
```

# Avoid respecting preferences from other indexes

---

_@charliermarsh_

## Summary

The fix I shipped in https://github.com/astral-sh/uv/pull/10690 regressed an important case. If we solve a PyPI branch before a PyTorch branch, we'll end up respecting the preference, and choosing `2.2.2` instead of `2.2.2+cpu`.

This PR goes back to ignoring preferences that don't map to the current index. However, to solve https://github.com/astral-sh/uv/issues/10383, we need to special-case `requirements.txt`, which can't provide explicit indexes. So, if a preference comes from `requirements.txt`, we still respect it.

Closes https://github.com/astral-sh/uv/issues/10772.


---

_Label `bug` added by @charliermarsh on 2025-01-20 17:12_

---

_Merged by @charliermarsh on 2025-01-20 17:23_

---

_Closed by @charliermarsh on 2025-01-20 17:23_

---

_Branch deleted on 2025-01-20 17:23_

---
