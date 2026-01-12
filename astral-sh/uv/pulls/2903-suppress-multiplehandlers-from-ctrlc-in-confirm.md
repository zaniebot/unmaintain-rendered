```yaml
number: 2903
title: Suppress MultipleHandlers from ctrlc in confirm
type: pull_request
state: merged
author: slafs
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-2900-supress-multiple-ctrlc-handlers
created_at: 2024-04-08T15:16:30Z
updated_at: 2024-04-08T18:58:16Z
url: https://github.com/astral-sh/uv/pull/2903
synced_at: 2026-01-12T16:05:17Z
```

# Suppress MultipleHandlers from ctrlc in confirm

---

_@slafs_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
This is AFAICR my first contribution to a Rust-based codebase 🎉 . Be gentle please 😅.

## Summary

Fixes #2900

## Test Plan

Tried reproducing the steps described in #2900,
but with `cargo run -- pip ...` and it didn't crash 😄.


---

_@slafs reviewed on 2024-04-08 15:17_

---

_Review comment by @slafs on `crates/uv-requirements/src/confirm.rs`:22 on 2024-04-08 15:17_

I should maybe mention something about _why_ we're doing this 🤔 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-08 16:07_

---

_Label `bug` added by @charliermarsh on 2024-04-08 16:07_

---

_@charliermarsh approved on 2024-04-08 17:07_

Thanks!

---

_@charliermarsh reviewed on 2024-04-08 17:07_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/confirm.rs`:22 on 2024-04-08 17:07_

👍 I added a note.

---

_Merged by @charliermarsh on 2024-04-08 17:18_

---

_Closed by @charliermarsh on 2024-04-08 17:18_

---

_Branch deleted on 2024-04-08 18:58_

---
