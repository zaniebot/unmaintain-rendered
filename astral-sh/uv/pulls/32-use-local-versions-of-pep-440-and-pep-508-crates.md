```yaml
number: 32
title: Use local versions of PEP 440 and PEP 508 crates
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pep
created_at: 2023-10-06T23:53:09Z
updated_at: 2023-10-07T00:16:45Z
url: https://github.com/astral-sh/uv/pull/32
synced_at: 2026-01-10T15:50:28Z
```

# Use local versions of PEP 440 and PEP 508 crates

---

_Pull request opened by @charliermarsh on 2023-10-06 23:53_

This PR modifies the PEP 440 and PEP 508 crates to pass CI, primarily by fixing all lint violations.

We're also now using these crates in the workspace via `path`. (Previously, we were still fetching them from Cargo.)

---

_Merged by @charliermarsh on 2023-10-07 00:16_

---

_Closed by @charliermarsh on 2023-10-07 00:16_

---

_Branch deleted on 2023-10-07 00:16_

---
