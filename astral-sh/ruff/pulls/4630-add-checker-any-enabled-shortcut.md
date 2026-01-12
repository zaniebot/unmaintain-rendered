```yaml
number: 4630
title: " Add Checker::any_enabled shortcut"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: any_enabled_shortcut
created_at: 2023-05-24T14:18:10Z
updated_at: 2023-05-24T15:00:04Z
url: https://github.com/astral-sh/ruff/pull/4630
synced_at: 2026-01-12T15:55:16Z
```

#  Add Checker::any_enabled shortcut

---

_@konstin_

 ## Summary

 Akin to #4625, This is a refactoring that shortens a bunch of code by replacing `checker.settings.rules.any_enabled` with `checker.any_enabled`.

 ## Test Plan

 `cargo clippy`

---

_@charliermarsh approved on 2023-05-24 14:19_

Thanks!

---

_Merged by @konstin on 2023-05-24 14:32_

---

_Closed by @konstin on 2023-05-24 14:32_

---

_Branch deleted on 2023-05-24 14:32_

---

_Comment by @github-actions[bot] on 2023-05-24 15:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
