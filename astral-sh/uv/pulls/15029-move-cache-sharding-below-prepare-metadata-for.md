```yaml
number: 15029
title: "Move cache sharding below `prepare_metadata_for_build_wheel`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/o
created_at: 2025-08-02T17:47:53Z
updated_at: 2025-08-02T18:08:51Z
url: https://github.com/astral-sh/uv/pull/15029
synced_at: 2026-01-10T06:44:33Z
```

# Move cache sharding below `prepare_metadata_for_build_wheel`

---

_Pull request opened by @charliermarsh on 2025-08-02 17:47_

## Summary

No change in behavior. This logic just isn't needed until the next block, and as-written, it's hard to tell.


---

_Label `internal` added by @charliermarsh on 2025-08-02 17:47_

---

_Marked ready for review by @charliermarsh on 2025-08-02 17:47_

---

_Merged by @charliermarsh on 2025-08-02 18:08_

---

_Closed by @charliermarsh on 2025-08-02 18:08_

---

_Branch deleted on 2025-08-02 18:08_

---
