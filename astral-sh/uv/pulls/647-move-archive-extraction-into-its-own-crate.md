```yaml
number: 647
title: Move archive extraction into its own crate
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/extract
created_at: 2023-12-14T04:40:56Z
updated_at: 2023-12-14T04:49:10Z
url: https://github.com/astral-sh/uv/pull/647
synced_at: 2026-01-12T16:04:05Z
```

# Move archive extraction into its own crate

---

_@charliermarsh_

We have some shared utilities beyond `puffin-build` and `puffin-distribution`, and further, I want to be able to access the sdist archive extraction logic from `puffin-distribution`. This is really generic, so moving into its own crate.

---

_@charliermarsh reviewed on 2023-12-14 04:42_

---

_Review comment by @charliermarsh on `crates/puffin-extract/src/lib.rs`:78 on 2023-12-14 04:42_

Also means sdists now use the parallel unzip.

---

_Merged by @charliermarsh on 2023-12-14 04:49_

---

_Closed by @charliermarsh on 2023-12-14 04:49_

---

_Branch deleted on 2023-12-14 04:49_

---
