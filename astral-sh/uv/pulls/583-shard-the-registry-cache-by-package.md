```yaml
number: 583
title: Shard the registry cache by package
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/shard
created_at: 2023-12-06T21:50:42Z
updated_at: 2023-12-07T05:02:47Z
url: https://github.com/astral-sh/uv/pull/583
synced_at: 2026-01-10T15:44:44Z
```

# Shard the registry cache by package

---

_Pull request opened by @charliermarsh on 2023-12-06 21:50_

## Summary

This PR modifies the cache structure in a few ways. Most notably, we now shard the set of registry wheels by package, and index them lazily when computing the install plan.

This applies both to built wheels:

<img width="989" alt="Screen Shot 2023-12-06 at 4 42 19 PM" src="https://github.com/astral-sh/puffin/assets/1309177/0e8a306f-befd-4be9-a63e-2303389837bb">

And remote wheels:

<img width="836" alt="Screen Shot 2023-12-06 at 4 42 30 PM" src="https://github.com/astral-sh/puffin/assets/1309177/7fd908cd-dd86-475e-9779-07ed067b4a1a">

For other distributions, we now consistently cache using the package name, which is really just for clarity and debuggability (we could consider omitting these):

<img width="955" alt="Screen Shot 2023-12-06 at 4 58 30 PM" src="https://github.com/astral-sh/puffin/assets/1309177/3e8d0f99-df45-429a-9175-d57b54a72e56">

Obliquely closes https://github.com/astral-sh/puffin/issues/575.


---

_Review requested from @konstin by @charliermarsh on 2023-12-06 21:50_

---

_Label `internal` added by @charliermarsh on 2023-12-06 21:50_

---

_Review comment by @konstin on `crates/puffin-installer/src/index/registry_wheel_index.rs`:42 on 2023-12-06 22:51_

I think just calling it `get` and documenting the index is lazy is fine

---

_@konstin approved on 2023-12-06 23:09_

---

_Comment by @charliermarsh on 2023-12-06 23:15_

I also need to update the documentation prior to merging.

---

_@zanieb approved on 2023-12-06 23:16_

---

_Merged by @charliermarsh on 2023-12-07 05:02_

---

_Closed by @charliermarsh on 2023-12-07 05:02_

---

_Branch deleted on 2023-12-07 05:02_

---
