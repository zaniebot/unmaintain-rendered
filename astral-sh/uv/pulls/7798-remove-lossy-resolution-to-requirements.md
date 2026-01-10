```yaml
number: 7798
title: Remove lossy resolution-to-requirements conversion in install plan
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/plan
created_at: 2024-09-30T01:03:43Z
updated_at: 2024-09-30T14:13:11Z
url: https://github.com/astral-sh/uv/pull/7798
synced_at: 2026-01-10T12:53:56Z
```

# Remove lossy resolution-to-requirements conversion in install plan

---

_Pull request opened by @charliermarsh on 2024-09-30 01:03_

## Summary

This is a longstanding piece of technical debt. After we resolve, we have a bunch of `ResolvedDist` entries. We then convert those to `Requirement` (which is lossy -- we lose information like "the index that the package was resolved to"), and then back to `Dist`.

---

_Marked ready for review by @charliermarsh on 2024-09-30 01:03_

---

_Label `internal` added by @charliermarsh on 2024-09-30 01:04_

---

_@charliermarsh reviewed on 2024-09-30 01:04_

---

_Review comment by @charliermarsh on `crates/uv-dispatch/src/lib.rs`:256 on 2024-09-30 01:04_

Getting rid of this is so good.

---

_Comment by @charliermarsh on 2024-09-30 01:08_

This will enable us to fix some very subtle bugs in future PRs.

---

_@konstin approved on 2024-09-30 14:09_

Less type conversions :+1:

---

_Merged by @charliermarsh on 2024-09-30 14:13_

---

_Closed by @charliermarsh on 2024-09-30 14:13_

---

_Branch deleted on 2024-09-30 14:13_

---
