```yaml
number: 1558
title: Allow repeated dependencies when installing
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/conflict
created_at: 2024-02-17T01:19:19Z
updated_at: 2024-02-17T01:33:41Z
url: https://github.com/astral-sh/uv/pull/1558
synced_at: 2026-01-10T15:33:24Z
```

# Allow repeated dependencies when installing

---

_Pull request opened by @charliermarsh on 2024-02-17 01:19_

## Summary

It turns out that it's not uncommon to end up with repeated packages in requirements files when running `pip-sync`, e.g., you might have `anyio==4.0.0` specified multiple times. This PR relaxes our assertions in the install plan to allow such repeated packages, as long as the requirement markers are exactly the same (i.e., they are truly duplicates).

Closes https://github.com/astral-sh/uv/issues/1552.


---

_Label `bug` added by @charliermarsh on 2024-02-17 01:19_

---

_Merged by @charliermarsh on 2024-02-17 01:33_

---

_Closed by @charliermarsh on 2024-02-17 01:33_

---

_Branch deleted on 2024-02-17 01:33_

---
