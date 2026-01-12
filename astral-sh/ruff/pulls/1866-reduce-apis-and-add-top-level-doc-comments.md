```yaml
number: 1866
title: Reduce APIs and add top-level doc comments
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: reduce-apis-and-document
created_at: 2023-01-14T07:31:33Z
updated_at: 2023-01-14T15:11:30Z
url: https://github.com/astral-sh/ruff/pull/1866
synced_at: 2026-01-12T05:36:32Z
```

# Reduce APIs and add top-level doc comments

---

_Pull request opened by @not-my-profile on 2023-01-14 07:31_

Test by running:

    cargo doc --no-deps --all --open

---

_@charliermarsh reviewed on 2023-01-14 12:23_

---

_Review comment by @charliermarsh on `ruff_dev/src/main.rs`:1 on 2023-01-14 12:23_

This is a little bit of a nit but how are we choosing to stylize "ruff" in these various docs? In the docs, I tend to do "Ruff" when referring to the project by name and `ruff` when referring to the application binary.

---

_@not-my-profile reviewed on 2023-01-14 13:34_

---

_Review comment by @not-my-profile on `ruff_dev/src/main.rs`:1 on 2023-01-14 13:34_

Makes sense ... changed to Ruff.

---

_Merged by @charliermarsh on 2023-01-14 15:11_

---

_Closed by @charliermarsh on 2023-01-14 15:11_

---
