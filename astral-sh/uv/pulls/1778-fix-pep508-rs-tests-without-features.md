```yaml
number: 1778
title: Fix pep508-rs tests without features
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/pep
created_at: 2024-02-20T19:27:18Z
updated_at: 2024-02-20T19:35:37Z
url: https://github.com/astral-sh/uv/pull/1778
synced_at: 2026-01-10T15:33:24Z
```

# Fix pep508-rs tests without features

---

_Pull request opened by @charliermarsh on 2024-02-20 19:27_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2024-02-20 19:27_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:785 on 2024-02-20 19:27_

This was missing `#[cfg(feature = "non-pep508-extensions")]`.

---

_@charliermarsh reviewed on 2024-02-20 19:27_

---

_Marked ready for review by @charliermarsh on 2024-02-20 19:27_

---

_Merged by @charliermarsh on 2024-02-20 19:35_

---

_Closed by @charliermarsh on 2024-02-20 19:35_

---

_Branch deleted on 2024-02-20 19:35_

---
