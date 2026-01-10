```yaml
number: 9185
title: Sort distributions when computing hash
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-11-18T02:30:51Z
updated_at: 2024-11-18T02:43:05Z
url: https://github.com/astral-sh/uv/pull/9185
synced_at: 2026-01-10T12:00:00Z
```

# Sort distributions when computing hash

---

_Pull request opened by @charliermarsh on 2024-11-18 02:30_

## Summary

The distributions used to be stored in a `BTreeMap`, keyed by name. They're now stored in a graph... so iteration isn't guaranteed to produce a deterministic hash!

This fixes a "flaky" test, though it's actually a real bug. The test was right!

Closes #9137.


---

_Label `bug` added by @charliermarsh on 2024-11-18 02:30_

---

_Marked ready for review by @charliermarsh on 2024-11-18 02:30_

---

_Merged by @charliermarsh on 2024-11-18 02:43_

---

_Closed by @charliermarsh on 2024-11-18 02:43_

---

_Branch deleted on 2024-11-18 02:43_

---
