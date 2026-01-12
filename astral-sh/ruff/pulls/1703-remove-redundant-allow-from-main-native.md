```yaml
number: 1703
title: "Remove redundant #![allow()] from main_native"
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: redundant-allow
created_at: 2023-01-07T01:04:59Z
updated_at: 2023-01-07T01:25:47Z
url: https://github.com/astral-sh/ruff/pull/1703
synced_at: 2026-01-12T05:36:32Z
```

# Remove redundant #![allow()] from main_native

---

_Pull request opened by @andersk on 2023-01-07 01:04_

`main_native.rs` is a module of `main.rs`, so the `#![allow()]`s in the latter apply to the former automatically.

---

_Comment by @charliermarsh on 2023-01-07 01:25_

Thanks!

---

_Merged by @charliermarsh on 2023-01-07 01:25_

---

_Closed by @charliermarsh on 2023-01-07 01:25_

---
