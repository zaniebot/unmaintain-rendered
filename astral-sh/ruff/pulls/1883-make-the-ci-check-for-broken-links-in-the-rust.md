```yaml
number: 1883
title: Make the CI check for broken links in the Rust docs
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: check-docs-in-ci
created_at: 2023-01-15T03:50:02Z
updated_at: 2023-01-15T04:18:18Z
url: https://github.com/astral-sh/ruff/pull/1883
synced_at: 2026-01-12T05:36:32Z
```

# Make the CI check for broken links in the Rust docs

---

_Pull request opened by @not-my-profile on 2023-01-15 03:50_

_No description provided._

---

_Marked ready for review by @not-my-profile on 2023-01-15 04:04_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:85 on 2023-01-15 04:11_

Any reason to make this its own job? (Is this pretty fast?)

---

_@charliermarsh reviewed on 2023-01-15 04:12_

---

_Review comment by @not-my-profile on `.github/workflows/ci.yaml`:85 on 2023-01-15 04:14_

In the CI for this PR it just took 38s. I mainly put it in the `cargo test` job because `cargo doc` also needs to compile the code and after `cargo test` the code already is compiled.

---

_@not-my-profile reviewed on 2023-01-15 04:14_

---

_Merged by @charliermarsh on 2023-01-15 04:18_

---

_Closed by @charliermarsh on 2023-01-15 04:18_

---
