```yaml
number: 841
title: Avoid duplicated source distribution handling in url
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/dupe
created_at: 2024-01-08T21:04:25Z
updated_at: 2024-01-08T21:19:56Z
url: https://github.com/astral-sh/uv/pull/841
synced_at: 2026-01-12T16:04:13Z
```

# Avoid duplicated source distribution handling in url

---

_@charliermarsh_

## Summary

Right now, both the callback _and_ the "We have no compatible wheel" paths have a lot of repeated code. This PR changes the callback to _just_ remove all the wheels and handle the download, and the rest of the method following the callback is responsible for finding and building any wheels.

---

_Label `internal` added by @charliermarsh on 2024-01-08 21:11_

---

_@charliermarsh reviewed on 2024-01-08 21:14_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/source/mod.rs`:218 on 2024-01-08 21:14_

I think this also meant we were unnecessarily re-requesting, though not sure how much of this was done lazily.

---

_Merged by @charliermarsh on 2024-01-08 21:19_

---

_Closed by @charliermarsh on 2024-01-08 21:19_

---

_Branch deleted on 2024-01-08 21:19_

---
