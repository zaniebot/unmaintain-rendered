```yaml
number: 725
title: Restore clippy on all crates in the workspace
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: clippy-workspace
created_at: 2022-11-13T19:22:24Z
updated_at: 2022-11-13T20:33:22Z
url: https://github.com/astral-sh/ruff/pull/725
synced_at: 2026-01-12T15:55:05Z
```

# Restore clippy on all crates in the workspace

---

_@andersk_

This was incorrectly removed in #721.

---

_@charliermarsh reviewed on 2022-11-13 19:31_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:80 on 2022-11-13 19:31_

Should we change the other usages of all to workspace too? E.g. for fmt?

---

_@andersk reviewed on 2022-11-13 19:34_

---

_Review comment by @andersk on `.github/workflows/ci.yaml`:80 on 2022-11-13 19:34_

`cargo build` supports `--workspace` (and marks `--all` as deprecated too), but `cargo fmt` doesn’t yet. Weird.

---

_Merged by @charliermarsh on 2022-11-13 20:18_

---

_Closed by @charliermarsh on 2022-11-13 20:18_

---

_Branch deleted on 2022-11-13 20:33_

---
