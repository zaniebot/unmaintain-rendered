```yaml
number: 6896
title: "Add source distribution support to `uv-build` crate"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/sdist
created_at: 2024-08-31T18:05:56Z
updated_at: 2024-09-02T18:14:51Z
url: https://github.com/astral-sh/uv/pull/6896
synced_at: 2026-01-10T12:53:36Z
```

# Add source distribution support to `uv-build` crate

---

_Pull request opened by @charliermarsh on 2024-08-31 18:05_

## Summary

Just exposes the correct PEP 517 hooks.


---

_Label `internal` added by @charliermarsh on 2024-08-31 18:06_

---

_Review requested from @konstin by @charliermarsh on 2024-08-31 18:06_

---

_Label `internal` removed by @charliermarsh on 2024-08-31 18:06_

---

_Label `enhancement` added by @charliermarsh on 2024-08-31 18:06_

---

_@charliermarsh reviewed on 2024-08-31 18:07_

---

_Review comment by @charliermarsh on `crates/uv-build/src/lib.rs`:725 on 2024-08-31 18:07_

I also changed this from canonicalize to absolute, since the error isn't very good if the directory doesn't exist.

---

_Review comment by @konstin on `crates/uv-build/src/lib.rs`:823 on 2024-09-02 07:57_

This is duplicated across the branches, we should move it above the `match`

---

_Review comment by @konstin on `crates/uv-build/src/lib.rs`:882 on 2024-09-02 07:58_

Word missing; Can we use `self.build_kind` and also move this out of the match?  

---

_@konstin approved on 2024-09-02 07:59_

---

_@charliermarsh reviewed on 2024-09-02 17:14_

---

_Review comment by @charliermarsh on `crates/uv-build/src/lib.rs`:882 on 2024-09-02 17:14_

I thought we wanted it to show `wheel` when it's editable, but otherwise yes.

---

_Merged by @charliermarsh on 2024-09-02 18:14_

---

_Closed by @charliermarsh on 2024-09-02 18:14_

---

_Branch deleted on 2024-09-02 18:14_

---
