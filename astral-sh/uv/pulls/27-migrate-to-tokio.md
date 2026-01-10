```yaml
number: 27
title: "Migrate to `tokio`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/tokio
created_at: 2023-10-06T20:21:59Z
updated_at: 2023-10-06T20:31:05Z
url: https://github.com/astral-sh/uv/pull/27
synced_at: 2026-01-10T15:56:16Z
```

# Migrate to `tokio`

---

_Pull request opened by @charliermarsh on 2023-10-06 20:21_

Closes https://github.com/astral-sh/puffin/issues/26.

---

_@charliermarsh reviewed on 2023-10-06 20:22_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/lib.rs`:29 on 2023-10-06 20:22_

This is unfortunate but Reqwest uses `futures::io::AsyncRead`, and we need to convert to a Tokio `AsyncRead`.

---

_Merged by @charliermarsh on 2023-10-06 20:31_

---

_Closed by @charliermarsh on 2023-10-06 20:31_

---

_Branch deleted on 2023-10-06 20:31_

---
