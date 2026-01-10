```yaml
number: 317
title: Use separate representations for canonical repository vs. commit
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dual
created_at: 2023-11-04T15:35:11Z
updated_at: 2023-11-04T15:46:43Z
url: https://github.com/astral-sh/uv/pull/317
synced_at: 2026-01-10T15:50:28Z
```

# Use separate representations for canonical repository vs. commit

---

_Pull request opened by @charliermarsh on 2023-11-04 15:35_

Given `https://github.com/pypa/package.git#subdirectory=pkg_a` and `https://github.com/pypa/package.git#subdirectory=pkg_b`, we want these to map to the same shared _resource_ (for locking and cloning), but different _packages_ (for determining whether the wheel already exists in the cache). As such, we need two distinct concepts for "canonical equality".

Closes #316.


---

_Label `bug` added by @charliermarsh on 2023-11-04 15:35_

---

_@zanieb reviewed on 2023-11-04 15:38_

---

_Review comment by @zanieb on `crates/puffin-cache/src/canonical_url.rs`:145 on 2023-11-04 15:38_

What about an inequal case?

---

_@charliermarsh reviewed on 2023-11-04 15:46_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/canonical_url.rs`:145 on 2023-11-04 15:46_

Good call

---

_Merged by @charliermarsh on 2023-11-04 15:46_

---

_Closed by @charliermarsh on 2023-11-04 15:46_

---

_Branch deleted on 2023-11-04 15:46_

---
