```yaml
number: 488
title: Use a sparse Metadata 2.1 representation
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/sparse
created_at: 2023-11-22T12:53:50Z
updated_at: 2023-11-22T13:25:37Z
url: https://github.com/astral-sh/uv/pull/488
synced_at: 2026-01-10T15:50:29Z
```

# Use a sparse Metadata 2.1 representation

---

_Pull request opened by @charliermarsh on 2023-11-22 12:53_

This is an optimization to avoid parsing the entire Metadata 2.1 when we only need a small subset of the fields.

Closes #175.

---

_Label `performance` added by @charliermarsh on 2023-11-22 12:53_

---

_Review requested from @konstin by @charliermarsh on 2023-11-22 13:01_

---

_@charliermarsh reviewed on 2023-11-22 13:01_

---

_Review comment by @charliermarsh on `crates/puffin-client/tests/remote_metadata.rs`:24 on 2023-11-22 13:01_

This is the primary change here -- using a different metadata field for the test. The rest is removing unwraps.

---

_Review comment by @konstin on `crates/pypi-types/src/metadata.rs`:47 on 2023-11-22 13:09_

This one we still need! We currently don't but we need to consider it for path and urls fields, but that's a bug https://github.com/astral-sh/puffin/issues/489

---

_Review comment by @konstin on `crates/pypi-types/src/metadata.rs`:46 on 2023-11-22 13:09_

This field could have been so useful, but no one uses them unfortunately

---

_@konstin approved on 2023-11-22 13:10_

---

_@charliermarsh reviewed on 2023-11-22 13:20_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/metadata.rs`:47 on 2023-11-22 13:20_

Will re-add :joy:

---

_Merged by @charliermarsh on 2023-11-22 13:25_

---

_Closed by @charliermarsh on 2023-11-22 13:25_

---

_Branch deleted on 2023-11-22 13:25_

---
