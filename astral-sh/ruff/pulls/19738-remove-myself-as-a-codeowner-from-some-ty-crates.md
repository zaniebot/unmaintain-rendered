```yaml
number: 19738
title: Remove myself as a codeowner from some ty crates
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/own-less-code
created_at: 2025-08-04T10:10:45Z
updated_at: 2025-08-04T10:23:14Z
url: https://github.com/astral-sh/ruff/pull/19738
synced_at: 2026-01-12T15:56:46Z
```

# Remove myself as a codeowner from some ty crates

---

_@AlexWaygood_

## Summary

This removes me as a CODEOWNER for several ty crates:
- ty
- ty_server
- ty_project
- ty_wasm
- ruff_db

I don't know the code in these crates particularly well and don't have a strong sense of ownership over most of it, so there are other people on the team better placed to review PRs touching these crates. This will reduce the number of notifications in my inbox, allowing me to focus on the important ones.

## Test Plan

GitHub says the CODE0WNERS file is valid, and I think it confoms with the syntax described in https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners#codeowners-syntax


---

_Label `internal` added by @AlexWaygood on 2025-08-04 10:10_

---

_@MichaReiser approved on 2025-08-04 10:17_

---

_Merged by @AlexWaygood on 2025-08-04 10:23_

---

_Closed by @AlexWaygood on 2025-08-04 10:23_

---

_Branch deleted on 2025-08-04 10:23_

---
