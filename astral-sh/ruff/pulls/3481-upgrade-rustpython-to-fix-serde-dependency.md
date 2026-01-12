```yaml
number: 3481
title: Upgrade RustPython to fix Serde dependency
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/serde
created_at: 2023-03-13T12:58:26Z
updated_at: 2023-03-13T16:29:33Z
url: https://github.com/astral-sh/ruff/pull/3481
synced_at: 2026-01-12T15:55:13Z
```

# Upgrade RustPython to fix Serde dependency

---

_@charliermarsh_

## Summary

Upgrading RustPython to pull in https://github.com/RustPython/RustPython/pull/4684, which allows us to remove the "hacked" Serde dependency in some of our smaller crates.

## Test Plan

From `crates/ruff_python_ast`, run `cargo test.


---

_Label `internal` added by @charliermarsh on 2023-03-13 12:58_

---

_Comment by @github-actions[bot] on 2023-03-13 13:19_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_@MichaReiser approved on 2023-03-13 16:28_

---

_Merged by @charliermarsh on 2023-03-13 16:29_

---

_Closed by @charliermarsh on 2023-03-13 16:29_

---

_Branch deleted on 2023-03-13 16:29_

---
