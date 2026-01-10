```yaml
number: 4253
title: "Represent build tag as `u64`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/u
created_at: 2024-06-11T21:28:45Z
updated_at: 2024-06-11T21:40:09Z
url: https://github.com/astral-sh/uv/pull/4253
synced_at: 2026-01-10T13:54:02Z
```

# Represent build tag as `u64`

---

_Pull request opened by @charliermarsh on 2024-06-11 21:28_

## Summary

The build tags in this case are like, e.g., `202206090410`. That's larger than a `u32`, so we're rejecting the wheel. In theory build tags could be even larger, but we already use `u64` for version segment so I think it's fine to keep that constraint here.

I'm going to look into surfacing these errors separately.

Closes https://github.com/astral-sh/uv/issues/4252.

## Test Plan

`cargo run pip install monailabel`


---

_Label `bug` added by @charliermarsh on 2024-06-11 21:29_

---

_Merged by @charliermarsh on 2024-06-11 21:40_

---

_Closed by @charliermarsh on 2024-06-11 21:40_

---

_Branch deleted on 2024-06-11 21:40_

---
