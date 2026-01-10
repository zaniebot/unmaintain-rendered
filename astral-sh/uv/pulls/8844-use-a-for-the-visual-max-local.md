```yaml
number: 8844
title: Use a + for the visual max local
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: tracking/050
head: charlie/plus
created_at: 2024-11-05T21:47:06Z
updated_at: 2024-11-06T03:51:38Z
url: https://github.com/astral-sh/uv/pull/8844
synced_at: 2026-01-10T12:00:00Z
```

# Use a + for the visual max local

---

_Pull request opened by @charliermarsh on 2024-11-05 21:47_

## Summary

We don't actually want users to see this, but we should be stripping it anyway. Without this change, we show ranges in the debug logs that look like `>=1.0.0, <1.0.0`, which is more confusing than helpful. (We may want to post-process those debug ranges to remove these.)

---

_Label `internal` added by @charliermarsh on 2024-11-05 21:47_

---

_Marked ready for review by @charliermarsh on 2024-11-06 03:42_

---

_Merged by @charliermarsh on 2024-11-06 03:51_

---

_Closed by @charliermarsh on 2024-11-06 03:51_

---

_Branch deleted on 2024-11-06 03:51_

---
