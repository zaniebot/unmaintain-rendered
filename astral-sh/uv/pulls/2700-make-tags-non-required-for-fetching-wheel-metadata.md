```yaml
number: 2700
title: Make tags non-required for fetching wheel metadata
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/dist-db
created_at: 2024-03-27T23:55:28Z
updated_at: 2024-03-28T00:06:26Z
url: https://github.com/astral-sh/uv/pull/2700
synced_at: 2026-01-10T14:49:08Z
```

# Make tags non-required for fetching wheel metadata

---

_Pull request opened by @charliermarsh on 2024-03-27 23:55_

## Summary

This looks like a big change but it really isn't. Rather, I just split `get_or_build_wheel` into separate `get_wheel` and `build_wheel` methods internally, which made `get_or_build_wheel_metadata` capable of _not_ relying on `Tags`, which in turn makes it easier for us to use the `DistributionDatabase` in various places without having it coupled to an interpreter or environment (something we already did for `SourceDistributionBuilder`).

---

_Label `internal` added by @charliermarsh on 2024-03-27 23:55_

---

_Marked ready for review by @charliermarsh on 2024-03-27 23:55_

---

_Merged by @charliermarsh on 2024-03-28 00:06_

---

_Closed by @charliermarsh on 2024-03-28 00:06_

---

_Branch deleted on 2024-03-28 00:06_

---
