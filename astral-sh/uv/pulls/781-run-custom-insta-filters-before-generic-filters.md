```yaml
number: 781
title: Run custom insta filters before generic filters
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: custom-filters-before-generic-filters
created_at: 2024-01-04T14:57:29Z
updated_at: 2024-01-04T15:40:29Z
url: https://github.com/astral-sh/uv/pull/781
synced_at: 2026-01-12T16:04:11Z
```

# Run custom insta filters before generic filters

---

_@konstin_

I've noticed some non-deterministic test failures when a temp dir looks like a timestamp (https://github.com/astral-sh/puffin/actions/runs/7410022542/job/20161416805). Running the custom filters for e.g. the temp dirs before the generic time filters should fix that.

---

_@charliermarsh approved on 2024-01-04 15:33_

---

_Label `internal` added by @charliermarsh on 2024-01-04 15:33_

---

_Merged by @konstin on 2024-01-04 15:40_

---

_Closed by @konstin on 2024-01-04 15:40_

---

_Branch deleted on 2024-01-04 15:40_

---
