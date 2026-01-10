```yaml
number: 13261
title: Only rebuild download JSON on change
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/uv-python-build.rs-invalidation
created_at: 2025-05-02T09:41:30Z
updated_at: 2025-05-02T12:49:35Z
url: https://github.com/astral-sh/uv/pull/13261
synced_at: 2026-01-10T11:10:41Z
```

# Only rebuild download JSON on change

---

_Pull request opened by @konstin on 2025-05-02 09:41_

By default, Cargo runs the build script if any file in the package changes (https://doc.rust-lang.org/cargo/reference/build-scripts.html#change-detection). In our case, we only need to rerun it if `download-metadata.json` changed.

---

_Label `internal` added by @konstin on 2025-05-02 09:41_

---

_Review requested from @Gankra by @konstin on 2025-05-02 09:41_

---

_@zanieb approved on 2025-05-02 12:49_

---

_Merged by @zanieb on 2025-05-02 12:49_

---

_Closed by @zanieb on 2025-05-02 12:49_

---

_Branch deleted on 2025-05-02 12:49_

---
