```yaml
number: 4375
title: Fix cache-prune-test to work outside the git repository
type: pull_request
state: merged
author: mgorny
labels:
  - testing
assignees: []
merged: true
base: main
head: cache-prune-version
created_at: 2024-06-18T04:54:01Z
updated_at: 2024-06-18T07:35:06Z
url: https://github.com/astral-sh/uv/pull/4375
synced_at: 2026-01-10T13:54:02Z
```

# Fix cache-prune-test to work outside the git repository

---

_Pull request opened by @mgorny on 2024-06-18 04:54_

## Summary

Make the git commit/date part of the version string matched in cache_prune optional, so that the test also works correctly when uv is built from an unpacked release tarball rather than a git repository.

Fixes #4374

## Test Plan

Ran `cargo test` inside the git repository and inside unpacked archive for `0.2.12` release with the patch applied on top.

---

_@charliermarsh approved on 2024-06-18 07:15_

Thank you, sorry about that.

---

_Merged by @charliermarsh on 2024-06-18 07:15_

---

_Closed by @charliermarsh on 2024-06-18 07:15_

---

_Label `testing` added by @charliermarsh on 2024-06-18 07:15_

---

_Branch deleted on 2024-06-18 07:34_

---

_Comment by @mgorny on 2024-06-18 07:35_

Thanks. No harm done.

---
