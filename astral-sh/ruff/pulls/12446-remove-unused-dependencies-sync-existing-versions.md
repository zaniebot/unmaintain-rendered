```yaml
number: 12446
title: Remove unused dependencies, sync existing versions
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/fuzz-deps
created_at: 2024-07-22T03:54:37Z
updated_at: 2024-07-22T05:19:07Z
url: https://github.com/astral-sh/ruff/pull/12446
synced_at: 2026-01-12T15:55:41Z
```

# Remove unused dependencies, sync existing versions

---

_@dhruvmanila_

## Summary

This PR removes unused dependencies from `fuzz` crate and syncs the `similar` crate to the workspace version. This will help in resolve https://github.com/astral-sh/ruff/pull/12442.

## Test Plan

Build the fuzz crate:

For Mac (it requires the nightly build):
```
cargo +nightly fuzz build
```


---

_Label `internal` added by @dhruvmanila on 2024-07-22 03:54_

---

_@MichaReiser approved on 2024-07-22 04:49_

---

_Merged by @dhruvmanila on 2024-07-22 05:19_

---

_Closed by @dhruvmanila on 2024-07-22 05:19_

---

_Branch deleted on 2024-07-22 05:19_

---
