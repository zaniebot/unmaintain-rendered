```yaml
number: 825
title: Simplify ranges in pre-release hints
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/simplift
created_at: 2024-01-07T02:32:22Z
updated_at: 2024-01-07T17:40:23Z
url: https://github.com/astral-sh/uv/pull/825
synced_at: 2026-01-12T16:04:13Z
```

# Simplify ranges in pre-release hints

---

_@charliermarsh_

Closes https://github.com/astral-sh/puffin/issues/807.

---

_Review requested from @zanieb by @charliermarsh on 2024-01-07 02:32_

---

_Label `error messages` added by @charliermarsh on 2024-01-07 02:32_

---

_@charliermarsh reviewed on 2024-01-07 02:32_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/report.rs`:150 on 2024-01-07 02:32_

I removed this struct entirely, and just made `hints` a method on `PubGrubReportFormatter`, since now the hints needs access to `available_versions` and `simplify_set`.

---

_@zanieb approved on 2024-01-07 17:39_

Thanks!

---

_Merged by @charliermarsh on 2024-01-07 17:40_

---

_Closed by @charliermarsh on 2024-01-07 17:40_

---

_Branch deleted on 2024-01-07 17:40_

---
